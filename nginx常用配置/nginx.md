# nginx

## 1. 防火墙指令

* 查看开放的端口号

```bash
$ firewall-cmd --list-all
```

* 设置开放的端口号

```bash
$ firewall-cmd --add-service=http --permanent
$ fire-cmd --add-post=80/tcp --permanent
```

* 重启防火墙

```bash
$ firewall-cmd --reload
```

## 2. nginx常用命令

* 启动nginx

```bash
$ nginx
```

* 停止nginx

```bash
$ nginx -s stop
```

* 重新加载nginx

```bash
$ nginx -s reload
```

## 3. nginx配置文件内容

* 全局快： 配置服务器整体运行的配置指令

  例如**worker_process	1;** // 处理并发数的配置

* events块：影响nginx服务器与用户的网络连接

  比如**worker_connections	1024;** //支持最大连接数为1024

* http块:

  一个**http*块可以包含多个**server**块

  一个**server**块可以包含多个**location**块

## 4. 反向代理配置

location指令说明

```bash
location [ = | ~ | ~* | ^~] uri {

}
```

​	=：用于不含正则表达式的uri前，要求请求字符串与uri严格匹配

​	~： 用于表示uri包含正则表达式，并且区分大小写

​	~*：用于表示uri包含正则表达式，并且不区分大小写

​	^~： 用于不含正则表达式的uri前，要求nginx服务器找到标识uri和请求字符串匹配度最高的location后，立即处理请求，不在继续匹配

```bash
server {
	listen	80;
	server_name	ip地址;
	
	# 此处若是想转发到多个端口，只需要配置多个location即可
	location / {
		root 		html;
		proxy_pass	 http://127.0.0.1:8080;	// 将80端口请求转发到8080端口
		index		index.html index.htm;
	}
}
```

## 5.负载均衡

* 轮询

```bash
upstream myserver {
	server	192.168.17.129:8080;
	server 	192.168.17.128:8001;
}
server {
	listen	80;
	server_name	192.168.17.129;
	
	location / {
		proxy_pass	http://myserver;
		root	html;
		index	index.html index.htm;
	}
}
```

* 权重

```bash
upstream myserver {
	server	192.168.17.129:8080 weight=10;
	server 	192.168.17.128:8001 weight=11;
}
```

* ip_hash

  每个请求按访问ip的hash分配，每个方可固定访问一个后端服务器，解决session问题

```bash
upstream myserver {
	ip_hash;
	server	192.168.17.129:8080;
	server 	192.168.17.128:8001;
}
```

* fari

  按后端服务器的响应时间来分配请求，响应时间短的有限分配

```bash
upstream myserver {
	fair;
	server	192.168.17.129:8080;
	server 	192.168.17.128:8001;
}
```

## 6.动静分离

​	将动态资源请求与静态资源请求分开

```bash
server {
	listen	80;
	server_name	ip地址;
	
	location /www/ {
		root	/data/;
		index	index.html index.htm;
	}
	location /image/ {
		root	/data/;
		autoindex	on;
	}
}
```

