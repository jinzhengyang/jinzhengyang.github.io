---
title: Hexo搭建笔记
date: 2018-12-11 01:00:30
categories:
- 搭建游戏
---
当前使用的就是hexo博客引擎，捣鼓了蛮久写个教程
官网地址 : https://hexo.io/zh-cn/  
官方文档 : https://hexo.io/zh-cn/docs/

<!-- more -->

## 本地+github部署

### 安装环境

Git 下载: https://git-scm.com/  
Node.js 下载: https://nodejs.org/en/

### 安装hexo

命令行运行
``` bash
$ npm install -g hexo-cli
```

安装 Hexo 完成后，请执行下列命令
``` bash
$ hexo init <文件夹>
$ cd <文件夹>
$ npm install
```

然后可以试运行，在hexo文件夹中
``` bash
$ hexo server
```

打开浏览器 地址栏输入 localhost:4000 端口查看运行是否成功

### 配置到github

网址 https://github.com/
注册并登陆账号
新建一个存储库 名字是 <username>.github.io 就可以看到你的网页了

在本地打开git-bash 输入
``` bash
$ git config --global user.name "username"
$ git config --global user.email "username@example.com"
$ ssh-keygen -t rsa -C "youremail@example.com"
```
打开本地文件夹C:\Users\<your computer name>\.ssh  

使用记事本或其他工具打开 id_rsa.pub  

将内容复制 打开你的github个人设置页 保存到你的ssh
 keys  
 ![](http://ww1.sinaimg.cn/large/a2dcb7e1gy1fy2pdohcnij20ta0h3ab5.jpg)

打开本地hexo文件夹中博客配置文件_config.yml

复制你 <username>.github.io 的 SshKey

保存到你_config.yml中的deploy标签中 如下所示
![](http://ww1.sinaimg.cn/large/a2dcb7e1gy1fy2pqdtoeij20qo02lglo.jpg)

打开cmd并进入本地hexo文件夹 输入
``` bash
$ hexo clean
$ hexo g
$ hexo d
```

最后打开浏览器 访问 <username>.github.io 就可以看到你的网页了
![](http://ww1.sinaimg.cn/large/a2dcb7e1gy1fy2ptek1fkj20ud0jm3zr.jpg)

至此本地+github就完成了。


## Hexo配置到服务器

百度云服务器  
centos7+
下载安装Xshell6 并连接到你的服务器

依此安装

### Node.js
``` bash
$ wget -qO- https://raw.github.com/creationix/nvm/v0.33.11/install.sh | sh
$ reboot
$ nvm install stable
(node -v --- v11.4.0)
```

### Git
``` bash
$ sudo yum install git-core
(git version --- git version 1.8.3.1)
```

### hexo
``` bash
$ npm install -g hexo-cli
$ hexo init hexo
$ cd hexo
$ npm install
$ npm install hexo-server --save
$ hexo server
```
### 调试

打开服务器端口4000

你就可以通过你的服务器ip+':4000' 访问你的博客了


### 安装并运行Screen
``` bash
$ yum install screen
$ screen -S hexo
```

### 再次运行
``` bash
$ hexo server
```

### 改变运行端口
域名解析之后你会发现在浏览器输入你的域名并不能直接访问你的博客   
此时你需要改变端口 你可以 下载nignx 研究一番   
也可以直接改变hexo运行端口为80 使用如下语句运行你的hexo server

``` bash
$ hexo server -p 80
```

### 关闭xshell
这样你就可以直接访问你的网站了

如果需要写文章和更改皮肤等可以访问hexo官网，附录中会放几份我自己翻阅的教程

## 附录：

### Screen
Screen是可以让你的服务器在关闭ssh连接之后依旧自主运行的软件
附一份screen简单命令表
``` bash
$ screen -S yourname  (新建一个叫yourname的session)
$ screen -r yourname  (回到yourname这个session)
$ screen -d yourname  (远程detach某个session)
$ screen -d -r yourname  (结束当前session并回到yourname这个session)
```

### 其他教程
[GitHub+Hexo（NexT主题）搭建博客](https://www.jianshu.com/p/7b5702d3f072)
[github+hexo 搭建属于自己的博客网站【2017完整版】](https://www.jianshu.com/p/e6662ca7e283)  
[2018最新版Hexo博客Next主题6.0配置优化](https://blog.csdn.net/qq_32454537/article/details/79482896)  
[绝配：hexo+next主题及我走过的坑](https://www.jianshu.com/p/21c94eb7bcd1)

当然最推荐翻阅的还是官方的教程 当然官方的教程会有点晦涩 并且中文版并不是最新的 这时候就需要用到"度娘"了

**官方有一份hexo的中文视频教程**
