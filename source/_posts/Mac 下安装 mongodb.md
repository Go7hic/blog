---
layout: post
title: Mac 下安装 mongodb
date: 2015-01-29 11:21:04
tags: mongodb
---

我用的是 brew 安装的，简单记录一下安装过。

1.`brew update`

2.`brew install mongodb`

3.安装好之后创建一个放数据库的地方
  `mkdir -p /data/db`

4.添加到环境变量里
  `subl ~/.bash_profile`
我是直接在终端通过命令来打开 sublimeText 来编辑这个文件打开之后把
  `export PATH=/usr/local/Cellar/mongodb/2.4.9/bin:${PATH}`
添加到里面去

5. 然后就是修改 mongo 的配置信息，在 /usr/local/etc 的 mongo.conf 里面，用你的编辑器打开之后，修改成下面这样：

```
systemLog:
destination: file
path: /usr/local/var/log/mongodb/mongo.log
logAppend: true
storage:
dbPath: /data/db
net:
bindIp: 127.0.0.1
```

6.然后在命令行用 mongod 启动，发现最后一行提示退出了，那时你还没有权限，需要简单的修改一下权限

7.<code> sudo chown `id -u` /data/db </code>通过这个命令修改权限，然后再试试 mongod ，启动成功

8.你也可以用 `mongo` 打开你的 mongodb 客户端



