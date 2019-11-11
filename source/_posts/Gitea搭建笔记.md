---
title: Gitea搭建笔记
date: 2018-12-13 12:47:14
tags:
categories:
- 搭建游戏
---
### 环境搭建

阿里云学生机
centos7.4

#### Git安装

``` bash
yum install git
```

#### Mysql5.6安装

``` bash
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
rpm -ivh mysql-community-release-el7-5.noarch.rpm
yum install mysql-server
```

#### 配置Mysql

``` bash
chown -R root:root /var/lib/mysql
service mysqld restart
```

设置MySQL密码

``` bash
mysql -u root //直接回车进入mysql控制台
mysql>use mysql;
mysql>update user set password=password('123456') where user='root';
mysql>exit;
```

#### 创建gitea数据库

``` bash
service mysqld restart
mysql -uroot -p<password>
mysql>create database gitea;
mysql>exit;
```

### Gitea搭建

#### 下载gitea

``` bash
wget -O gitea https://dl.gitea.io/gitea/1.3.2/gitea-1.3.2-linux-amd64
```

#### 运行gitea

``` bash
chmod +x gitea
./gitea web
```

gitea会运行在服务器 3000端口 在安全组中打开端口
通过 ip:3000 访问

#### 设置gitea

第一次访问会让你进行一些设置  

按照安装数据库时候的信息填进去  

因为我们通过 **yum** 直接安装的 **git** 版本并没有达到 **2.1.2** 所以需要关闭 lfs 即那一项不填即可

最后设置 **gitea管理用户** 就可以安装了

#### 服务器端运行

安装 **screen** 并进入gitea
``` bash
yum install screen
screen -S gitea
```

最后运行 ``` ./gitea web ```

你就可以通过 **ip:3000** 在任何地方使用 **gitea**


### 绑定域名及其他操作

购买域名 绑定你的服务器ip

你可以使用 **nginx反向代理** 或者 在 **~/custom/config/app.ini** 中 设置 **80端口**

下载宝塔不失为一种好的选择

当然你也可以使用我的 **gitea**

http://git.zhengyang.kim:3000 欢迎您的使用
