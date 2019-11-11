---
title: Python爬虫_一言
date: 2018-12-20 01:07:14
tags:
categories:
- 代码笔记
---
最近新学 **python** 的 **requests** 闲来无事就简单的爬去 一言 https://hitokoto.cn/

<!-- more -->
### 一言 分析

一言没啥可以分析的 最简单的 使用他的api接口 https://hitokoto.cn/api 会返回一串json格式的数据
![](http://ww1.sinaimg.cn/large/a2dcb7e1gy1fycjm7ntecj20ce044745.jpg)

### 爬取

导入库
``` bash
import requests
import json
import pymysql
import time
```

直接使用get得到数据 并把它保存到变量 最后将参数们放入数组
并返回
``` bash
def getyiyan(i):
    r = requests.get('https://v1.hitokoto.cn/')
    s = json.loads(r.text)
    id = s["id"]
    contents = s["hitokoto"]
    type = s["type"]
    froms = s["from"]
    print("第 ", i, " 次 ", id)
    yiyan = [id,contents,type,froms]
    return yiyan
```

### 数据库

数据库基本参数
``` bash
host = 'localhost'
user = 'root'
password = '******'
port = 3306
db = 'yiyan'
dbname = 'yiyan'
```

数据库调用
``` bash
def mysql(sql):
    db = pymysql.connect(host='localhost', user='root', password='******', port=3306, db='yiyan')
    cursor = db.cursor()
    try:
        result = cursor.execute(sql)
        db.commit()
    except:
        print(db.rollback())
    db.close()
    return result
```

存储到数据库
``` bash
def intomysql(yiyan):
    id = yiyan[0]
    contents = yiyan[1]
    type = yiyan[2]
    froms = yiyan[3]
    sql = "INSERT INTO %s(`id`,`contents`,`type`,`from`) VALUES(%s,'%s','%s','%s')" % (dbname,id,contents,type,froms)
    if mysql(sql):
        print(yiyan,"保存到数据库成功 ")
```

数据库查重
``` bash
def isinsql(yiyan):
    id = yiyan[0]
    print("正在检查id",id)
    sql = "Select * FROM %s WHERE `id`=%s" %(dbname,id)
    if mysql(sql):
        print(id,"已存在于数据库")
        return -1
    else:
        return 1
```

### 运行
``` bash
if __name__ == '__main__':
    i = 1
    while i>0:
        print("--------------------------------")
        # time.sleep(1)
        data = getyiyan(i)
        if isinsql(data)<0 :
            continue
        intomysql(data)
        i += 1
```


### 源代码
``` bash
import requests
import json
import pymysql
import time


host = 'localhost'
user = 'root'
password = ''
port = 3306
db = 'yiyan'
dbname = 'yiyan'



### 数据库调用
def mysql(sql):
    db = pymysql.connect(host='localhost', user='root', password='', port=3306, db='yiyan')
    cursor = db.cursor()
    try:
        result = cursor.execute(sql)
        db.commit()
    except:
        print(db.rollback())
    db.close()
    return result

#爬取一言数据
def getyiyan(i):
    r = requests.get('https://v1.hitokoto.cn/')
    s = json.loads(r.text)
    id = s["id"]
    contents = s["hitokoto"]
    type = s["type"]
    froms = s["from"]
    print("第 ", i, " 次 ", id)
    yiyan = [id,contents,type,froms]
    return yiyan
#存储到数据库
def intomysql(yiyan):
    id = yiyan[0]
    contents = yiyan[1]
    type = yiyan[2]
    froms = yiyan[3]
    sql = "INSERT INTO %s(`id`,`contents`,`type`,`from`) VALUES(%s,'%s','%s','%s')" % (dbname,id,contents,type,froms)
    if mysql(sql):
        print(yiyan,"保存到数据库成功 ")



#数据库查重
def isinsql(yiyan):
    id = yiyan[0]
    print("正在检查id",id)
    sql = "Select * FROM %s WHERE `id`=%s" %(dbname,id)
    if mysql(sql):
        print(id,"已存在于数据库")
        return -1
    else:
        return 1



if __name__ == '__main__':
    i = 1
    while i>0:
        print("--------------------------------")
        # time.sleep(1)
        data = getyiyan(i)
        if isinsql(data)<0 :
            continue
        intomysql(data)
        i += 1
```

### 数据库参数信息

![](http://ww1.sinaimg.cn/large/a2dcb7e1gy1fycju69agyj208m02v0sk.jpg)
