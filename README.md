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

##### 安装 nodejs 环境
* `http://nodejs.cn/` 在该网站找到源代码版 .tar.gz 文件下载地址
* `wget https://npm.taobao.org/mirrors/node/v10.13.0/node-v10.13.0.tar.gz` 使用该命令下载压缩包--地址可替换为最新版 nodejs 源码地址
* `tar xvf node-v10.13.0.tar.gz` 解压出文件--文件名可能不一样
* `cd node-v10.13.0/` 进入解压后的文件路径
* `./configure`
* `make` 开始编译文件--很慢
* `sudo make install` 开始安装 nodejs
* `node -v` 验证是否安装成功