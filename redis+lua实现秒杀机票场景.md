## 场景
 用户前端下单、后台扣减库存。1000张机票，大约用户10w+.

## 思路

### 业务逻辑使用lua脚本方案
* 1.将所有机票信息推送到redis；
* 2.用户请求合法性校验 lua脚本调用redis现在userId是否是第一次请求；
* 3.Nginx通过Lua查询redis缓存数量、与用户下单数量比较，返回 1成功 2库存不足 3.非首次请求；
* 4.后台获取redis信息 异步新建订单、更新db库存；

### 业务逻辑不使用lua脚步方案，使用[nsq+redis](https://github.com/kgtom/back-end/blob/master/%E9%AB%98%E5%B9%B6%E5%8F%91.md)
* 1.接入层 使用 nginx 中ngx_http_limit_conn_module模块限制ip请求次数；
* 2.网关层 redis 使用incr记录ip请求次数；
* 3.服务层 nsq消息队列 只放入10000次请求；
* 4.数据库层 获取nsq请求，更新redis(读redis、写redis 后更新db库存)、库存库存信息
## 代码

```
秒杀成功 返回1；已秒杀 返回0；低于库存，不能秒杀 返回2，每人限购至多2张，超限购返回3
```

~~~lua
--检查用户是否秒杀过该机票，如果~=0,则返回 0说明已秒杀
    local hasSecKill=redis.call('sismember',KEYS[1],ARGV[1])       
    if hasSecKill ~=0 then
        return 0;
    end

 
--检查机票库存,如果库存<购买的，表示库存不足，返回1,每个用户限购2张及2张以下，超过限购返回3

    local airStoreCount=tonumber(redis.call("get",ARGV[1]) or "0");
    local airBuyCount=tonumber(ARGV[2])
    if airBuyCount>2 then
        return 3
    end 
    if airStoreCount-airBuyCount<0 then
        return 2;
    end


--扣库存,每人限量一张机票

    redis.call('DECRBY',ARGV[1],airBuyCount);


-- 记录秒杀成功的用户
    redis.call('sadd',KEYS[1],ARGV[1]);

-- 返回1 秒杀成功
return 1;
~~~


请求：
* 1.预设库存机 票id12的还有库存5张机票
~~~
127.0.0.1:6379> set air_id:12 5
OK
127.0.0.1:6379> get air_id:12
"5"
~~~
* 2 key:用户id,两个参数 一个是机票id,另一个购买张数
~~~
# tom @ tom-pc in ~/luaprojects [23:01:12]
$ redis-cli --eval test.lua user_id:101 , air_id:12  2
(integer) 1

# tom @ tom-pc in ~/luaprojects [23:01:22]
$ redis-cli --eval test.lua user_id:101 , air_id:12  2
(integer) 0
# tom @ tom-pc in ~/luaprojects [23:01:24]
$ redis-cli --eval test.lua user_id:102 , air_id:12  2
(integer) 1


# tom @ tom-pc in ~/luaprojects [23:01:47]
$ redis-cli --eval test.lua user_id:103 , air_id:12  3
(integer) 3

# tom @ tom-pc in ~/luaprojects [23:02:56]
$ redis-cli --eval test.lua user_id:103 , air_id:12  2
(integer) 2
~~~
* 3.查询机票id 12库存
~~~
127.0.0.1:6379> get air_id:12
"1"
~~~
解释：
* 1.用户id 101第一次秒杀成功，第二次因为已秒杀，不允许再次秒杀
* 2.用户id 102 成功秒杀
* 3.用户id 103 返回3，超出至多2张
* 4.用户id 103 返回2，库存不足

## 总结
* Redis 保证脚本会以原子性(atomic)的方式执行，当某个脚本正在运行的时候，不会有其他脚本或 Redis 命令被执行。
* lua 使用 incr、sadd、实现复杂的令牌桶算法。
* 从key、参数索引1开始。多个key或者参数，用空格隔开
