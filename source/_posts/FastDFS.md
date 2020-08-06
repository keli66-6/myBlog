### FastDFS

参考视频：https://www.bilibili.com/video/BV1ta4y1v7Kw?from=search&seid=7274534228530086861

参考文档：https://blog.csdn.net/qq_34301871/article/details/80060235

### 一、FastDFS入门
<!--more-->
#### 1、分布式文件系统

常见的分布式文件：FastDFS、GFS、HDFS、Lustre、GrldFS、nogileFS、TFS

传统方式存储的缺点：宕机、磁盘损坏、磁盘空间小

分布式文件存储的优点：解决单点故障、数据备份

开源地址：https://github.com/happyfish100

#### 2、架构

​					用户                                        Client

​			分布式文件系统							Tracker Server       跟踪器

文件系统	文件系统	文件系统			Storage Server	  存储节点

文件系统    文件系统	文件系统          （数据备份）

#### 3、FastDFS 环境搭建

##### 1、安装前的准备

###### i、检查Linux上是否安装了 gcc、libevent、libevent-devel

yum list installed | grep gcc

yum list installed | grep libevent

yum list installed | grep libevent-devel

###### ii、如果没有，就安装

yum install gcc libevent libevent-devel -y

##### 2、相关源码

fastdfs: 分布式文件系统（DFS）

git clone https://github.com/happyfish100/fastdfs.git

fastdfs-client-java:  FastDFS Java客户端SDK

 https://github.com/happyfish100/fastdfs-client-java.git

libfastcommon:  FastDFS中提取的c公共函数库

git clone https://github.com/happyfish100/libfastcommon.git

libshmcache:  libshmcache是共享内存中用于多个进程的本地缓存

git clone https://github.com/happyfish100/libshmcache.git

fastdfs-nginx-module:    FastDFS Nginx模块

git clone https://github.com/happyfish100/fastdfs-nginx-module.git

fastdht: FastDHT是基于键值对的高性能分布式哈希表（DHT）

git clone https://github.com/happyfish100/fastdht.git

##### 3、安装libfastcommon 库

git clone https://github.com/happyfish100/libfastcommon.git

cd  libfastcommon

./make.sh && ./make.sh install     

##### 4、安装fastdfs 分布式

git clone https://github.com/happyfish100/fastdfs.git

cd fastdfs

./make.sh && ./make.sh install

##### 5、存放位置

命令：/usr/bin

配置文件：/etc/fdfs

cd /root/fastdfs/conf

cp http.conf /etc/fdfs/

cp mime.types /etc/fdfs/

#### 4、FastDFS的配置与启动

cp storage.conf.sample storage.conf

cp tracker.conf.sample tracker.conf

mkdir -p /opt/fastdfs/{tracker,storage}

vim tracker.conf

base_path = /opt/fastdfs/tracker              #日志文件存放路径

vim storage.conf

base_path = /opt/fastdfs/storage

store_path0 =  /opt/fastdfs/storage/files          #磁盘存放路径

tracker_server = IP:22122                      # tracker服务器的ip

启动命令：fdfs_tracker <config_file> [start | stop | restart]

启动命令：fdfs_storage <config_file> [start | stop | restart]

查看进程：ps -ef |grep fdfs

（config_file   =   /etc/fdfs/*.conf)

#### 5、FastDFS测试

cp client.conf.sample client.conf

vim client.conf

base_path = /opt/fastdfs/client

tracker_server = IP:22122                      # tracker服务器的ip

##### 测试上传：

fdfs_test /etc/fdfs/client.conf upload test.txt

返回数据：保存

group_name=组名，IP，端口

group_name=组名，运程存储位置

example file url: 访问路径文件     #默认不能访问

##### 测试下载：

fdfs_test /etc/fdfs/client.conf download 组名 存储路径

下载到当前路径

##### 测试删除：

fdfs_test /etc/fdfs/client.conf delete 组名 存储路径

#### 6、HTTP访问

##### 1、下载模块

git clone https://github.com/happyfish100/fastdfs-nginx-module.git

路径：/root/soft/fastdfs-nginx-module/src

##### 2、下载nginx

nginx下载：https://nginx.org/en/download.html

进入nginx目录

./configure --prefix=/usr/local/nginx_fdfs --add-module=/root/soft/fastdfs-nginx-module-master/src

注意：HTTP报错

网址：https://blog.csdn.net/hybaym/article/details/50929958

yum -y install pcre-devel

yum -y install openssl openssl-devel

##### 3、编译安装

make && make install

##### 4、配置

vim mod_fastdfs.conf

base_path=/opt/fastdfs/nginx_mod       #基础路径

 tracker_server=IP:22122                         #跟踪服务ip

url_have_group_name = true                   #组名

store_path0=/opt/fastdfs/storage/files  #文件存放路径

cp mod_fastdfs.conf /etc/fdfs/

mkdir -p /opt/fastdfs/nginx_mod

cd /usr/local/nginx_fdfs/conf

vim nginx.conf

location ~ /group[1-9]/M0[0-9] {

​			ngx_fastdfs_module;

}

测试配置：/usr/local/nginx_fdfs/sbin/nginx -c /usr/local/nginx_fdfs/conf/nginx.conf -t

启动：/usr/local/nginx_fdfs/sbin/nginx -c /usr/local/nginx_fdfs/conf/nginx.conf

软连接：ln -i /usr/local/nginx_fdfs/sbin/nginx /usr/local/sbin

检测：ps -ef | grep nginx

#### 7、流程图

![image-20200717172533963](C:\Users\Administrator\Pictures\Camera Roll\7.png)

#### 8、FastDFS 在java 项目中开发使用

