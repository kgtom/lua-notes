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


## 总结
* Redis 保证脚本会以原子性(atomic)的方式执行，当某个脚本正在运行的时候，不会有其他脚本或 Redis 命令被执行。
* lua本身也是一种脚步语言，使用它实现复杂的令牌桶或漏桶算法。
