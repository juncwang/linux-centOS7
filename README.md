### linux-centos7

##### 系统安装
* `https://www.centos.org/` 下载 centos7.x 版本
* 下载安装虚拟机 `vmware`
* 运行虚拟机安装 `centos7`

##### 网络配置
* 安装完成后使用 vi 打开 `/etc/sysconfig/network-scripts/ifcfg-ens33`
* 打开后 将 `ONBOOT=no` 改为 `ONBOOT=yes`
* 使用 `wq` 命令保存
* 使用 `sudo service net work restart` 命令重启服务
* 使用 `ip addr` 查看本机ip