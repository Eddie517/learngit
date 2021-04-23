##Step1. git clone 下載 mininet
先安裝 git
```shell
$sudo apt install git
```
![1](./png/1.png)`
下载最新版本的mininet 
```shell
$ git clone git://github.com/mininet/mininet.git 
```
![2](./png/2.png)
##Step2. 安裝 mininet
-a 全部安裝
-h 可列出所有可用選項
-s mydir 可指定安裝資料夾，需放在所有選項的最前面
###Step2.1先安装OpenVSwitch交换机
![3](./png/3.png)
1.切入root用户
```shell
$ sudo su
```
![4](./png/4.png)
2.安装系统组件及库文件以作为OVS正确运行的环境依赖
```shell
apt-get install -y build-essential
```
![5](./png/5.png)
```shell
apt-get install libssl-dev
```
![6](./png/6.png)
```shell
apt-get install libcap-ng-dev
```
![7](./png/7.png)
```shell
apt-get install autoconf
```
![8](./png/8.png)
```shell
apt-get install automake
```
![9](./png/9.png)
```shell
apt-get install libtool
```
![10](./png/10.png)
3.下载并解压OVS 2.3.0安装包（还可以下载其他安装包，如OVS 2.7.0安装包）
```shell
wget http://openvswitch.org/releases/openvswitch-2.3.0.tar.gz
```
![11](./png/11.png)
```shell
tar -xzvf openvswitch-2.3.0.tar.gz
```
4.构建基于Linux内核的交换机
```shell
cd openvswitch-2.3.0
 ./boot.sh  #生成配置文件
```
![12](./png/12.png)
```
./configure -with-linux=/lib/modules/$(uname -r)/build #配置
```
![13](./png/13.png)
```shell
./configure
```

5.编译并安装OVS
```shell
make clean
make && make install
```
6.使用ovsdb工具初始化配置数据库
```shell
mkdir -p /usr/local/etc/openvswitch
ovsdb-tool create /usr/local/etc/openvswitch/conf.db vswitchd/vswitch.ovsschema  2>/dev/null
```
7.  启动ovsdb-server配置数据库
```
apt install openvswitch-common
ovsdb-server -v --remote=punix:/usr/local/var/run/openvswitch/db.sock --remote=db:Open_vSwitch,Open_vSwitch,manager_options --private-key=db:Open_vSwitch,SSL,private_key --certificate=db:Open_vSwitch,SSL,certificate --bootstrap-ca-cert=db:Open_vSwitch,SSL,ca_cert --pidfile --detach
```
8.首次用ovsdb-tool创建数据库时需用ovs-vsctl命令初始化下数据库
```shell
ovs-vsctl --no-wait init
```
9.启动OVS主进程
```shell
ovs-vswitchd --pidfile --detach
```
![14](./png/14.png)
10.如下命令查看所安装OVS的版本号
```shell
ovs-vsctl --version
```
![15](./png/15.png)
编写OVS启动脚本
OpenVSwitch每次启动都需要输入一堆命令，建议写一个启动脚本
```shell
apt install vim 
vim start-ovs.sh
```
1.添加内容如下：
```shell

ovsdb-server -v --remote=punix:/usr/local/var/run/openvswitch/db.sock --remote=db:Open_vSwitch,Open_vSwitch,manager_options --private-key=db:Open_vSwitch,SSL,private_key --certificate=db:Open_vSwitch,SSL,certificate --bootstrap-ca-cert=db:Open_vSwitch,SSL,ca_cert --pidfile --detach
ovs-vsctl --no-wait init
ovs-vswitchd --pidfile --detach
```
![16](./png/16.png)
2.启动OVS
```shell
sh start-ovs.sh
```
![17](./png/17.png)

Step3. 測試 Mininet
```shell
Apt install mininet
$ sudo mn --test pingall
```
![18](./png/18.png)
```shell
$ mininet/util/install.sh -n3V 2.5.0
```
![19](./png/19.png)

