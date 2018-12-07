## 题目
实现一个访问频率控制，记录某个ip在短时间内频繁访问记录。要求 3s内不超5次。
在redis客户端机器上，新建一个文件 rate_userIp.lua，内容如下

~~~ lua

local key=KEYS[1] --限流key 
local limit =tonumber(ARGV[2]) --限流大小
-- current+1:+1加上本次请求
local current=tonumber(redis.call("get",key) or "0")+1
if current > limit then -- 如果超过限流大小
    return 0
else -- 请求数+1,并设置过期时间3s
    redis.call("incrby",key,"1")
    redis.call("expire",key,ARGV[1])
    return 1
end

~~~

在redis客户端机器上，rate_userIp.lua 所在目录执行代码如下：

~~~
redis-cli --eval rate_userIp.lua ip_limiting:127.0.0.1 , 5 3
~~~

** 解释 **
* ip_limiting:127.0.0.1 ，作为key,通过KEYS[1]获取
* 5:请求次数不超过5次，通过ARGV[1]获取
* 3：3s内，通过ARGV[2]获取
* 注意 , 前后有空格


## 总结
在Redis中使用lua,可以减少网络请求，使用lua脚本完成原子性相关操作。
