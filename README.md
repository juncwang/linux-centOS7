### linux-centos7

##### 系统安装
* `https://www.centos.org/` 下载 centos7.x 版本
* 下载安装虚拟机 `vmware`
* 运行虚拟机安装 `centos7`

##### 网络配置
* 安装完成后使用 vi 打开 `/etc/sysconfig/network-scripts/ifcfg-ens33`
* 打开后 将 `ONBOOT=no` 改为 `ONBOOT=yes`
* 使用 `wq` 命令保存
* 使用 `sudo service network restart` 命令重启服务
* 使用 `ip addr` 查看本机ip
* 使用软件 `Xshell 5` 连接 `centos`
    * 连接时需要配置 ip 地址及端口号
    * 连接时需要配置用户名及密码

##### 安装基础软件
* `yum -y install wget` 安装 wget 用于下载文件

* 更换 yum 源到阿里
* `mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup` 备份
* `wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo` centos7 阿里源
* `yum clean all`
* `yum makecache`

* `sudo yum install gcc gcc-c++` 安装 c++ 编辑器
* `gcc -v` 验证是否安装成功

* `yum install lrzsz -y` 安装上传下载插件, 支持 `Xshell 5`
    * 该方法只适用于 windows
    * `rz` 上传
    * `sz` 下载

* `yum -y install vim*` 安装 vim
    * 基本操作
        * `:q` 直接退出
        * `:wq` 保存退出
        * `:set nu` 显示行号

##### mac 链接服务器方法
* `ssh username@servername` 链接服务器 用户名加服务器 ip
* `scp username@servername:/serverPath/filename /localPath(本地目录)`  把服务器上的文件下载到本地目录
* `scp /localPath/filename username@servername:/serverPath` 把本地文件上传到服务器目录
* `scp -r ... ...` 上传下载整个文件目录及内容

##### 基本命令
* `ps -ef|grep softName` 查看进程 id
* `ps aux|grep softName` 查看进程 id
* `kill id` 根据 id 直接关闭进程

* `mkdir filePath` 添加文件路径
* `touch fileName` 添加文件
* `rm -rf fileName` 删除文件及文件夹

* `systemctl start firewalld` 启用防火墙
* `systemctl stop firewalld` 关闭防火墙
* `firewall-cmd --zone=public --add-port=80/tcp --permanent` 在防火墙上开启端口（--permanent永久生效，没有此参数重启后失效）
* `firewall-cmd --zone= public --remove-port=80/tcp --permanent` 删除开启的端口

* `whereis softName` 查询安装的软件路径

##### 设置开机启动运行
* `vi /etc/rc.local` 编辑文件
* 在末尾添加需要启动程序的操作指令
* `chmod 755 rc.local` 增加权限

##### 设置定时任务
* 创建一个 shFileName.sh 文件
```sh
# !/bin/bash
# 定义变量
LOGPATH=/usr/local/nginx/logs/access.log
BASEPATH=/data
bak=$BASEPATH/$(date -d yesterday +%Y%m%d%H%M).access.log

# linux 基本操作
mv $LOGPATH $bak
touch $LOGPATH

kill -USR1 `cat /usr/local/nginx/logs/nginx.pid`
```
* 执行 `crontab -e` 并写入
    * `*/1 * * * * sh /shFilePaht/shFileName.sh` 

##### 安装 nodejs 环境
* `http://nodejs.cn/` 在该网站找到源代码版 .tar.gz 文件下载地址
* `wget https://npm.taobao.org/mirrors/node/v10.13.0/node-v10.13.0.tar.gz` 使用该命令下载压缩包--地址可替换为最新版 nodejs 源码地址
* `tar xvf node-v10.13.0.tar.gz` 解压出文件--文件名可能不一样
* `cd node-v10.13.0/` 进入解压后的文件路径
* `./configure`
* `make` 开始编译文件--很慢
* `sudo make install` 开始安装 nodejs
* `node -v` 验证是否安装成功
* `npm install -g cnpm --registry=https://registry.npm.taobao.org` 换源安装 cnpm

* `npm install -g pm2` 安装 pm2 实现后台运行 nodejs 程序
* `pm2 start server.js` 启动 nodejs 后台服务程序

##### 安装 mongo
* 在安装时均使用 root 用户，如果非 root 用户则在命令前加 sudo 命令，用来以 root 身份运行
* `https://www.mongodb.com` 获取下载路径压缩包

```conf
# 1.在usr/local/下创建mongodb文件夹
cd /usr/local/
mkdir mongodb

# 2.解压文件
tar -xzvf mongodb-linux-x86_64-rhel70-4.0.1.tgz

# 3.将解压后的文件下所有内容移动到mongodb文件夹下
# 注意这里不是将mongodb-linux-x86_64-rhel70-4.0.1文件夹移动到创建好的mongodb下，而是文件下的内容
mv mongodb-linux-x86_64-rhel70-4.0.1/*  /usr/local/mongodb/
```

* 经过上述步骤，基本的配置已经完成了，接下来创建mongodb数据文件和日志文件的存放位置，并且对启动项进行配置，启动项配置其中包含数据库文件路径和日志文件路径
* 填写上述将要创建的文件夹或文件路径。具体步骤如下：

```conf
# 1.创建数据库文件存放路径
cd /usr/local/mongodb
mkdir -p data/db
chmod -r 777 data/db

# 2.创建日志文件
cd /usr/local/mongodb
mkdir logs
cd logs
touch mongodb.log

# 3.创建启动文件
cd /usr/local/mongodb/bin
touch mongodb.conf

# 4.编辑启动文件
vi mongodb.conf

# 5.在文件中插入如下内容
#日志文件位置
logpath=/usr/local/mongodb/logs/mongodb.log　

# 以追加方式写入日志
logappend=true

# 是否以守护进程方式运行
fork = true

# 默认27017
#port = 27017

# 数据库文件位置
dbpath=/usr/local/mongodb/data/db

# 启用定期记录CPU利用率和 I/O 等待
#cpu = true

# 是否以安全认证方式运行，默认是不认证的非安全方式
#noauth = true
#auth = true

# 详细记录输出
#verbose = true

# Inspect all client data for validity on receipt (useful for
# developing drivers)用于开发驱动程序时验证客户端请求
#objcheck = true

# Enable db quota management
# 启用数据库配额管理
#quota = true
# 设置oplog记录等级
# Set oplogging level where n is
#   0=off (default)
#   1=W
#   2=R
#   3=both
#   7=W+some reads
#diaglog=0

# Diagnostic/debugging option 动态调试项
#nocursors = true

# Ignore query hints 忽略查询提示
#nohints = true
# 禁用http界面，默认为localhost：28017
#nohttpinterface = true

# 关闭服务器端脚本，这将极大的限制功能
# Turns off server-side scripting.  This will result in greatly limited
# functionality
#noscripting = true
# 关闭扫描表，任何查询将会是扫描失败
# Turns off table scans.  Any query that would do a table scan fails.
#notablescan = true
# 关闭数据文件预分配
# Disable data file preallocation.
#noprealloc = true
# 为新数据库指定.ns文件的大小，单位:MB
# Specify .ns file size for new databases.
# nssize =

# Replication Options 复制选项
# in replicated mongo databases, specify the replica set name here
#replSet=setname
# maximum size in megabytes for replication operation log
#oplogSize=1024
# path to a key file storing authentication info for connections
# between replica set members
#指定存储身份验证信息的密钥文件的路径
#keyFile=/path/to/keyfile
```

* 启动数据库
* 经过配置后即可启动数据库了，启动数据库文件在bin目录下执行下术命令

```conf
# 切换到bin目录下
cd /usr/local/mongodb/bin

# 启动数据库
./mongod --config mongodb.conf

# 访问数据库
./mongo
```

##### 安装nginx

* yum 安装 nginx 方式
    * `yum install epel-release`
    * `yum install nginx` 安装 nginx
    * `systemctl start nginx` 启动 nginx

    * `systemctl enable nginx` 设置开机启动
    * `systemctl disable nginx` 禁止开机启动
    * `systemctl status nginx` 查看运行状态
    * `systemctl restart nginx` 重启

* 通过下载编译方式安装
    * `yum install gcc-c++` 
    * `yum install -y pcre pcre-devel`
    * `yum install -y zlib zlib-devel`
    * `yum install -y openssl openssl-devel`

* 安装包下载地址 `https://nginx.org/en/download.html`
    * `wget -c https://nginx.org/download/nginx-1.10.1.tar.gz` 使用该命令进行下载
    * `tar -zxvf nginx-1.10.1.tar.gz` 解压
    * `cd nginx-1.10.1` 进入文件夹
    * `./configure` 使用默认配置
    * `make` 开始编译
    * `make install` 开始安装
    * `whereis nginx` 获取安装路径
    * `cd /usr/local/nginx/sbin/` 进入 nginx 路径
        * 启动、停止
            * `kill -INT id` 根据 id 直接关闭进程
            * `kill -QUIT id` 根据 id 等待进程任务结束关闭进程
            * `kill -HUP id` 根据 id 等待进程任务结束重启进程
            * `kill -USR1 id` 重读日志
            * `kill -USR2 id` 平滑升级
            * `kill -WINCH id` 等待旧进程任务结束后关闭旧进程
            
            * `./nginx` 启动
            * `./nginx -s stop` 停止, 直接杀死进程
            * `./nginx -s quit` 停止, 等到进程处理完毕后停止
            * `./nginx -s reload` 重启
            * `./nginx -s reopen` 重读日志
        * 查看进程 
            * `ps aux|grep nginx`

* 配置 nginx
    * `cd /usr/local/nginx/conf` 进入配置路径
    * `vi nginx.conf` 编辑配置文件

```conf
# 全局区域

# 配置有几个工作进程 - 一般设置为 cpu*核数
worker_processes  1;

# 一般是配置 nginx 连接的特性
# 如几个同时工作
events {
    #这是指一个子进程最大允许连 1024 个链接
    worker_connections  1024;
}

# 这是配置 http 服务器的主要段
http {

    # 日志格式
    # log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                   '$status $body_bytes_sent "$http_referer" '
    #                   '"$http_user_agent" "$http_x_forwarded_for"';

    # 说明该 server 塔的访问日志文件是 logs/access.log, 使用的格式为 main
    # access_log  logs/access.log  main;

    # 这是虚拟主机段 ( 可以有多个 )
    server {
        # 需要监听的端口
        listen       80;
        # 需要监听的域名或ip
        server_name  localhost;

        # 单独为该服务配置一个日志, 使用格式为 main
        # access_log logs/localhost.log main

        # 定位, 把特殊的路径或文件再次定位, 如 image 目录单独处理 ( 可以有多个 )
        location / {
            # 文件路径
            root   html;
            # 默认取哪一个页面作为主页
            index  index.html index.htm;
            # 所有其他网页参数都返回给 index.html 进行处理
            # try_files $uri $uri/ /index.html;
        }

        # 添加访问目录为/apis的代理配置
        # location /apis { 
		# 	rewrite  ^/apis/(.*)$ /$1 break;
		# 	proxy_pass   http://192.168.78.113:5000;
        # }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        # location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        # }

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        # location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        # }

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        # location ~ /\.ht {
        #    deny  all;
        # }
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    # server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    # }


    # HTTPS server
    #
    # server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    # }

```