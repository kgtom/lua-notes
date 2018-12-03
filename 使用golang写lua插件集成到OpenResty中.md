## 一、OpenResty 是什么
OpenResty = nginx + lua（lua语言逻辑+lua插件）

## 二、OpenResty 目标
OpenResty 的目标是让你的 Web 服务直接跑在 Nginx 服务内部,充分利用 Nginx 的非阻塞 I/O 模型,不仅仅对 HTTP 客户端请求,甚至于对远程后端诸如 MySQL,PostgreSQL,~Memcaches 以及 ~Redis 等都进行一致的高性能响应。

## 三、应用
所以对于一些高性能的服务来说，可以直接使用 OpenResty 访问 Mysql或Redis等，而不需要通过第三方语言（PHP、Python、Ruby）等来访问数据库再返回，这大大提高了应用的性能。


## 四、与golang服务 处理相比
1，openresty： nginx 接收请求，匹配url 调用lua虚拟机处理请求，nginx 返回结果给客户端。 
2，golang 服务： nginx 接收请求，匹配url，根据配置，代理请求到后端服务(网络通信时间，内存开销)，golang Handler 接收请求，controller、services 层级调用， 处理完成返回给nginx，nginx返回结果给客户端。

## 五、使用golang 写lua插件

### 1.golang 微服务与lua插件选择

### 2.如何使用golang 写lua插件



>reference:
* [zhihu](https://zhuanlan.zhihu.com/p/50937409)
* [openresty](http://openresty.org/en/)
