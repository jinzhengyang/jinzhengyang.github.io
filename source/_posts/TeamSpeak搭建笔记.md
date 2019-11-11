---
title: TeamSpeak搭建笔记
date: 2018-12-16 00:06:24
tags:
categories:
- 搭建游戏
---
TeamSpeak 虽然说是比较老的东西了 但很使用 很方便 最重要的是它可以自建服务器 是YY的一个很好的替代品 音质优秀 防监听 畅所欲言 并能够为你和朋友构建更好的游戏语音平台 。  
你可以来我的服务尝试它
<!-- more -->

``` bash
server : voice.zhengyang.kim
password : 123
```

### 客户端下载
TeamSpeak 客户端推荐从官网下载 https://www.teamspeak.com/en/  
国内那个teamspeak是魔改版已经很久没有更新了 也无法正常使用。  

### 环境搭建

阿里云学生机  
centos7.4  
安全组放行 **TCP/UDP:9987  TCP:10011 TCP:30033**  

安装依赖库  
``` bash
$ yum update -y
$ yum install vim wget perl net-tools bzip2 -y
```

创建TeamSpeak用户  
``` bash
$ useradd ts
```

修改ts密码  
``` bash
$ passwd ts
```

### 下载TeamSpeak服务器端

进入ts用户  
``` bash
$ su - ts
```

下载teamspeak服务端  
``` bash
$ wget https://files.teamspeak-services.com/releases/server/3.5.0/teamspeak3-server_linux_amd64-3.5.0.tar.bz2
```

解压  
``` bash
$ tar -xjvf teamspeak3-server_linux_amd64-3.5.0.tar.bz2
$ cd teamspeak3-server_linux_amd64
```

### 运行teamspeak

启动  
``` bash
$ touch .ts3server_license_accepted
$ ./ts3server_startscript.sh start    
```

首次启动保存 Token、serveradmin 和serverpass  
第一次客户端连接时会要求使用token 使用token的用户是管理员用户  

### 开机自启

``` bash
$ crontab -e
```

在文本中操作  
*shift-i* 进入文本写入模式  
将下面这句话复制进文本   
``` bash
@reboot /home/ts/teamspeak3-server_linux_amd64/ts3server_startscript.sh start
```
*esc*  
*:wq* 保存并退出  

查看是否成功  
``` bash
$ crontab -l
```

### 参考

teamspeak 官网：https://www.teamspeak.com/en/  
机智的狐狸菌： https://www.smartfox.cc/index.php/archives/3986/
