
### github Hexo搭建个人博客
<!--more-->
#### 准备环境

node  Hexo git

#### 安装node

官网地址：https://nodejs.org

wget https://nodejs.org/dist/v12.18.3/node-v12.18.3-linux-x64.tar.xz

mkdir -p /usr/local

tar -xvf node-v12.14.0-linux-x64.tar.xz -C /usr/local

mv node-v12.14.0-linux-x64 node

环境变量的配置

vim /root/.bash_profile

PATH=$PATH:$HOME/bin:/usr/local/node/bin

source /root/.bash_profile

检测环境是否安装

node -v 

npm -v

 git version

#### 安装Hexo

官网文档：https://hexo.io/zh-cn/

npm install -g hexo-cli

hexo init myBlog

npm install 

目录文件详细解释

_config.yml			#网站的配置信息

package.json

scaffolds			#模板文件夹

source				#资源文件夹

​	_drafts			#草稿文件

​	_posts			#文章markdown文件

themes			  #主题文件夹

#### 启动Hexo

hexo g			#生成

hexo s			#部署到本地

hexo g -d 	  #部署到远程github

访问地址：http://IP:4000

#### 更换主题

https://github.com/removeif/hexo-theme-amazing

#### 本文章参考：

https://segmentfault.com/a/1190000017986794

http://xiaoli.love/
