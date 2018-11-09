## 题目
实现一个访问频率控制，记录某个ip在短时间内频繁访问记录。要求 10s内不超3次。
在redis客户端机器上，新建一个文件ratelimiting.lua，内容如下

~~~ lua
local times = redis.call('incr',KEYS[1])

if times == 1 then
    redis.call('expire',KEYS[1], ARGV[1])
end

if times > tonumber(ARGV[2]) then
    return 0
end
return 1

~~~

在redis客户端机器上，执行代码如下：

~~~
redis-cli --eval ratelimiting.lua ip_limiting:127.0.0.1 , 5 3
~~~

** 解释 **
* ip_limiting ，作为key,通过KEYS[1]获取
* 3:请求次数不超过3次，通过ARGV[2]获取
* 5：5s内，通过ARGV[1]获取
* 注意 , 前后有空格


## 总结
在Redis中使用lua,可以减少网络请求，使用lua脚本完成相关逻辑。
