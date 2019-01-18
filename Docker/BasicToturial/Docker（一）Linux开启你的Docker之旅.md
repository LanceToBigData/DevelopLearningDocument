### 前言 ###
Docker容器最早受到RHEL完善的支持是从最近的CentOS 7.0开始的，官方说明是只能运行于64位  
架构平台,内核版本为2.6.32-431及以上（即 >= CentOS 6.5，运行docker时实际提示3.10.0及  
以上）。 需要注意的是CentOS 6.5与7.0的安装是有一点点不同的，CentOS 6.x上Docker的安装  
包叫docker-io，并且来源于Fedora epel库这个仓库维护了大量的没有包含在发行版中的软件，所  
以先要安装EPEL，而CentOS 7.x的Docker直接包含在官方镜像源的Extras仓库（CentOS-Base.repo  
下的[extras]节enable=1启用）。  
**备注：**本文的安装环境是CentOS 7.3，不适用CentOS 6.x的相关环境，切记！  
## 一、Docker安装 ##
1. Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS版本  
是否支持 Docker 。  
通过 uname -r 命令查看你当前的内核版本  
```
uname -r
```
2. 使用 root 权限登录 Centos。确保 yum 包更新到最新。  
```
yum update
```
3. 卸载旧版本(如果安装过旧版本的话)  
```
sudo yum remove docker  docker-common docker-selinux docker-engine
```  
4. 安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的  
```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```
5. 设置yum源  
```
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```
6. 可以查看所有仓库中所有docker版本，并选择特定版本安装  
```
yum list docker-ce --showduplicates | sort -r
```
![](https://i.imgur.com/Zi9Ge36.png)
7. 安装Docker  
```
sudo yum install <FQPN>  # 例如：sudo yum install docker-ce-18.03.1.ce
```
8. 启动并加入开机启动  
```
sudo systemctl start docker
sudo systemctl enable docker
```
9. 验证安装是否成功(有client和service两部分表示docker安装启动都成功了)  
```
docker version
```
## 二、Docker国内镜像源设置 ##
docker pull 国内网络链接失败或很卡慢，一般都需要更换至国内 （需要下载 最新的 18版本）  
比较常用的是网易的镜像中心和daocloud镜像市场：  
```
网易镜像中心：https://c.163.com/hub#/m/home/   
daocloud镜像市场：https://hub.daocloud.io/
```  
1. 配置网易镜像中心  
创建或修改 /etc/docker/daemon.json 文件，修改为如下形式  
```
{
 
"registry-mirrors": ["http://hub-mirror.c.163.com"]
 
} 
```  
 
重启docker：systemctl restart docker.service  
2. 配置DaoCloud镜像中心  
[https://www.daocloud.io/mirror](https://www.daocloud.io/mirror "daocloud镜像中心")  
![](https://i.imgur.com/GkmIYvK.png)   
由于docker的版本不同和操作系统。使用的方法也有差异。我这里使用的是centos7.2和docker1.12的。  
```
docker version
cat /etc/redhat-release 
CentOS Linux release 7.2.1511 (Core)
```    

修改配置文件/etc/docker/daemon.json 为:  
```
{
    "registry-mirrors": ["http://ef017c13.m.daocloud.io"],
    "live-restore": true
}
```  
可以手动vim添加，也可以使用daocloud给出的命令直接更改（建议使用命令）  
```
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://f1361db2.m.daocloud.io
```  
更改后重启docker  
```
service docker restart
```  
相关详细文档说明：  
1)详情请参考daocloud的说明文档  
[http://guide.daocloud.io/dcs/daocloud-9153151.html ](http://guide.daocloud.io/dcs/daocloud-9153151.html )  
2)docker官方文档  
[https://docs.docker.com/engine/admin/](https://docs.docker.com/engine/admin/)



