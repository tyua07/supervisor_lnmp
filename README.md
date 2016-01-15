## Supervisor LNMP环境 配置信息

### 效果图

![效果图](https://raw.githubusercontent.com/tyua07/supervisor_lnmp/master/images/supervisor.png)


### 安装 Supervisor

* yum -y install python-setuptools
* easy_install supervisor
* mkdir -p /etc/supervisor/conf.d
* echo_supervisord_conf > /etc/supervisor/supervisord.conf

### 修改配置文件
* /etc/supervisor/supervisord.conf

```
[inet_http_server]         ; inet (TCP) server disabled by default
port=127.0.0.1:9501        ; (ip_address:port specifier, *:port for all iface)
username=yangyifan              ; (default is no username (open server))
password=fengqianqian               ; (default is no password (open server))

```
开启 web终端操作

port|username|password
----|--------|-------
ip和端口，如果是外网访问，则需要改成正式ip，prot则自己配置|登录名|登陆密码

```
[include]
files = /etc/supervisor/conf.d/*.conf
```

引入 conf文件夹下面的全部配置文件（这样比较好管理一点，不同的服务，在不同的配置文件，然后一并引入当前的 ``` supervisord.conf ``` 就好了）

### 创建 webserver.conf demo

* 创建 nginx 

```
[program:nginx]
command=/usr/local/nginx/sbin/nginx -g "daemon off;"
autostart=true
autorestart=true
```

* 创建 php

```
[program:php]
user = root
command = /usr/local/php5.6/sbin/php-fpm --nodaemonize
stdout_logfile=/var/log/supervisor/webserver.log
stderr_logfile=/var/log/supervisor/webserver.error.log
autostart=true
autorestart=true
```

* 创建 mysql 

```
[program:mysql]
user = root
command = /usr/local/mysql/bin/mysqld --basedir=/usr/local/mysql --datadir=/data/mysql/data --plugin-dir=/usr/local/mysql/lib/plugin --user=mysql --log-error=/data/mysql/data/zaiseouldeMacBook-Pro.local.err --pid-file=/data/mysql/data/zaiseouldeMacBook-Pro.local.pid --socket=/tmp/mysql.sock --port=3306
stdout_logfile=/var/log/supervisor/webserver.log
stderr_logfile=/var/log/supervisor/webserver.error.log
autostart=true
autorestart=true
```

* 创建 redis

```
[program:redis]
command = /usr/local/redis/bin/redis-server /usr/local/redis/redis.conf
stdout_logfile=/var/log/supervisor/webserver.log
stderr_logfile=/var/log/supervisor/webserver.error.log
autostart=true
autorestart=true
user=root
```




