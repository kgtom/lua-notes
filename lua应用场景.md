### 1.Nginx +lua 限流

  * 接入层限流： 对于接入层限流，nginx可以通过ngx_http_limit_conn_module 网络连接数 和ngx_http_limit_req_module 漏桶算法进行限流，还可以使用nginx 第三方 OpenResty 提高的Lua脚本 lua-resty-limit-traffic ，
 对更复杂的限流场景
 * 分布式限流
 [详细]()


### 2.Redis+lua

* 分布式限流
 [详细]()

### 3.wrk+lua 性能测试

模拟POST请求
新建 post.lua

~~~
wrk.method = "POST"
wrk.body = "foo=a&baz=b"
wrk.headers["Content-Type"] = "application/x-www-form-urlencode"
wrk.headers["User-Agent"] = "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
~~~
请求:
~~~
$ wrk -t 4 -c 100 -d 30s --timeout 10 --script=post.lua --latency http://127.0.0.1/post
~~~

通过在脚本中记录日志的方式可以看到$_POST数据和$_SERVER中的字段为lua脚本中设置的值
