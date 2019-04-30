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
    * `rz` 上传命令
    * `sz` 下载命令

##### 安装 nodejs 环境
* `http://nodejs.cn/` 在该网站找到源代码版 .tar.gz 文件下载地址
* `wget https://npm.taobao.org/mirrors/node/v10.13.0/node-v10.13.0.tar.gz` 使用该命令下载压缩包--地址可替换为最新版 nodejs 源码地址
* `tar xvf node-v10.13.0.tar.gz` 解压出文件--文件名可能不一样
* `cd node-v10.13.0/` 进入解压后的文件路径
* `./configure`
* `make` 开始编译文件--很慢
* `sudo make install` 开始安装 nodejs
* `node -v` 验证是否安装成功

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

# 4.添加mongodb的环境变量
vi /etc/profile

# 5.在文件末尾插入如下内容
export MONGODB_HOME=/usr/local/mongodb  
export PATH=$PATH:$MONGODB_HOME/bin

# 6.修改保存后要重启系统配置，执行命令如下
source /etc/profile
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
logpath=/data/db/journal/mongodb.log　　（这些都是可以自定义修改的）

# 以追加方式写入日志
logappend=true

# 是否以守护进程方式运行
fork = true

# 默认27017
#port = 27017

# 数据库文件位置
dbpath=/data/db

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