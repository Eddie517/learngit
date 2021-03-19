#OpenStack介绍
OpenStack是一个开源的云计算管理平台项目，是一系列软件开源项目的组合。由NASA(美国国家航空航天局)和Rackspace合作研发并发起，以Apache许可证（Apache软件基金会发布的一个自由软件许可证）授权的开源代码项目。 

OpenStack为私有云和公有云提供可扩展的弹性的云计算服务。项目目标是提供实施简单、可大规模扩展、丰富、标准统一的云计算管理平台。 
#安装环境
工具：VMware Workstation 16 Pro

操作系统：Ubuntu 20.10

参考版权声明：本文为CSDN博主「小白典」的原创文章
原文链接：https://blog.csdn.net/Q0717168/article/details/114328885

#系统配置
将yum源换成华为源
```
sudo cp -a /etc/apt/sources.list /etc/apt/sources.list.bak
sudo sed -i "s@http://.*archive.ubuntu.com@http://repo.huaweicloud.com@g" /etc/apt/sources.list
sudo sed -i "s@http://.*security.ubuntu.com@http://repo.huaweicloud.com@g" /etc/apt/sources.list
sudo apt-get update
```
将PyPI源换成华为源
```
# 新建.pip目录
sudo mkdir .pip
#安装vim
sudo su root
sudo apt-get install vim
su wyq
# 在.pip目录下创建pip.conf文件
sudo vim .pip/pip.conf
# 将以下内容填入pip.conf文件中
[global]
index-url = https://repo.huaweicloud.com/repository/pypi/simple
trusted-host = repo.huaweicloud.com
```
#开始安装
安装软件包
```
sudo apt-get install bridge-utils git python3-pip -y
#安装前后可以先查看一下
# 查看pip(V是大写)
pip -V 或 pip3 -V
# 查看git
git --version
```
添加stack用户
```
# 新增stack用户
sudo useradd -s /bin/bash -d /opt/stack -m stack
# 授予stack用户sudo权限
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
# 切换到stack用户
sudo su - stack
```
下载devstack
```
# 使用git下载devstack
# git clone https://opendev.org/openstack/devstack
git clone -b stable/rocky https://git.openstack.org/openstack-dev/devstack
# 下载完成后切换到devstack目录下
cd devstack
```
添加local.conf文件
```
# 在devstack根目录下添加local.conf文件
vim local.conf
# 将以下内容添加到local.conf文件中
[[local|localrc]]
ADMIN_PASSWORD=duanyd
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
```
开始安装
```
# 在devstack目录下执行stack.sh脚本
./stack.sh
报错：
#[ERROR] ./stack.sh:227 If you wish to run this script anyway run with FORCE=yes
解决方法:  FORCE=yes ./stack.sh
#table `broute' is incompatible, use 'nft' tool
Encountered this on Ubuntu 20.04 as well, workaround was to change to iptable-legacy and attempt stacking again.
apt-get update
apt-get upgrade
apt-get install iptable
apt-get install arptables
apt-get install ebtablesupdate-alternatives --set iptables /usr/sbin/iptables-legacy || true
update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy || true
update-alternatives --set arptables /usr/sbin/arptables-legacy || true
update-alternatives --set ebtables /usr/sbin/ebtables-legacy || true
table `broute’ is incompatible, use ‘nft’ tool
Solution:
Try Switching to iptable-legacy
sudo apt update && sudo apt upgrade
sudo apt -y install iptables arptables ebtables
sudo update-alternatives --set iptables /usr/sbin/iptables-legacy || true
sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy || true
sudo update-alternatives --set arptables /usr/sbin/arptables-legacy || true
sudo update-alternatives --set ebtables /usr/sbin/ebtables-legacy || true

```
```
ubuntu更换国内源
1.备份原始源文件source.list
桌面打开终端，执行命令：sudo  cp   /etc/apt/sources.list   /etc/apt/sources.list.bak

2.修改源文件sources.list
（1）终端执行命令：sudo  chmod  777  /etc/apt/sources.list   更改文件权限使其可编辑；

（2）执行命令：  sudo  gedit   /etc/apt/sources.list    打开文件进行编辑；

（3）删除原来的文件内容（esc 999dd），复制下面的任意一个到其中并保存（常用的是阿里源和清华源，推荐阿里源）；

3.更新源
桌面终端执行命令：sudo apt update更新软件列表，换源完成。
```
```
清华源：
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse

deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse

deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse

deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse

deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse

deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse

deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse

deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```