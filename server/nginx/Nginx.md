# Nginx

## 参考 

深入理解 Nginx 模块开发与架构解析  --  陶辉

https://nginx.weebly.com/

http://nginx.taohui.org.cn/

# 第一章 研究 Nginx 前的准备工作

## 1.2  why we choose Nginx

1. 更快 
2. 高扩展性
3. 高可靠性
4. 低内存消耗  10000个HTTP Keep-Alive 连接仅消耗 2.5M 内存,这是 nginx 支持高并发连接的基础.
5. 单机支持10万以上的并发连接
6. 热部署 

	- master 管理进程与 worker 工作进程的分离设计,使得 nginx 能够提供热部署功能
	- 它支持不停止服务就更新配置项,更换日志文件等功能

7. 最自由的 BSD 许可协议

## 1.3 准备工作

### 查看内核版本

```uname -a```

> Linux iZbp193yy46icaga1srlt6Z 3.10.0-693.2.2.el7.x86_64 #1 SMP Tue Sep 12 22:26:13 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux

### 内核参数优化

```/etc/sysctl.conf```

### nginx 配置地址

nginx的配置文件在  ```/etc/nginx/nginx.conf```

自定义的配置文件放在 ```/etc/nginx/conf.d```

项目文件存放在 ```/usr/share/nginx/html/```

日志文件存放在 ```/var/log/nginx/```

还有一些其他的安装文件都在 ```/etc/nginx```

### nginx 启动

#### 默认启动

nginx

#### 另行制定配置文件启动

-c

#### 另行制定安装目录的启动方式

-p

#### 另行制定全局配置的启动方式

-g 

#### 测试配置文件是否有错误

nginx -t 

使用 -q 参数可以不把 error 级别以下的信息输出到屏幕

nginx -t -q

### nginx 版本

nginx -v

nginx -V 

### 快速地停止服务

nginx -s stop ???

### 查看 nginx 进程

ps -ef | grep nginx

#### kill it

root      4362  4164  0 22:29 pts/1    00:00:00 grep --color=auto nginx

kill -s SIGINT 4362   ---> 无效??

### 查看帮助

```nginx -h``` 或者 ```nginx -?```

```
Usage: nginx [-?hvVtTq] [-s signal] [-c filename] [-p prefix] [-g directives]

Options:
  -?,-h         : this help
  -v            : show version and exit
  -V            : show version and configure options then exit
  -t            : test configuration and exit
  -T            : test configuration, dump it and exit
  -q            : suppress non-error messages during configuration testing
  -s signal     : send signal to a master process: stop, quit, reopen, reload
  -p prefix     : set prefix path (default: /usr/share/nginx/)
  -c filename   : set configuration file (default: /etc/nginx/nginx.conf)
  -g directives : set global directives out of configuration file
```

# 第二章 nginx 的配置

## 运行中的 nginx 进程间的关系


部署 nginx 时,都是使用一个 master 进程来管理多个 worker 进程,一般情况下, worker 进程的数量与服务器上的 cpu 核心数相等.

worker 进程之间通过共享内存,院子操作等一些进程间通信机制来实现负载均衡.

### 为什么要在产品环境下按照 master-worker 方式配置同时启动多个进程?

1. master 进程不会对用户请求提供服务,只用于管理 worker 进程.

2. 多个 worker 进程处理互联网请求,可以提高服务的健壮性.当一个 worker 进程出错,其他 worker 进程仍然可以正常提供服务.

3. apache 的每个进程,在一个时刻只处理一个请求. 当一台服务器拥有几百个工作进程后,大量的进程间切换,将带来无谓的系统资源消耗.

4. nginx中一个 worker 进程可以同时处理的请求数,只受限于内存大小. 不同 worker 进程之间处理并发请求时,几乎没有同步锁的限制, worker 进程通常不会进入睡眠状态.当 nginx 上的进程数与 cpu 核心数相等时,进程间切换的代价最小.














