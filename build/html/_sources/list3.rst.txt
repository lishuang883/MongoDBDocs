安装MongoDB
===========


安装
----
你可以在mongodb官网下载该安装包，MongoDB 预编译二进制包下载地址：https://www.mongodb.com/download-center#community

根据你的系统下载 32 位或 64 位的 .msi 文件，下载后双击该文件，按操作提示安装即可。

安装过程中，你可以通过点击 "Custom(自定义)" 按钮来设置你的安装目录。

运行
----
创建数据目录

MongoDB将数据目录存储在 db 文件夹下。但是这个数据目录不会主动创建，我们在安装完成后需要创建它。请注意，数据目录应该放在根目录下（(如： C:\ 或者 D:\ 等 )。创建一个data的文件夹，然后在data文件夹里创建db文件夹。

为了从命令提示符（cmd）下运行 MongoDB 服务器，你必须从 MongoDB 目录的 bin 目录中执行 mongod.exe 文件。

输入如下的命令启动mongodb服务：（我是安装到D盘了）
::
 D:/mongodb/bin>mongod --dbpath D:\mongodb\data\db

如果执行成功，会输出如下信息：
::  
 2015-09-25T15:54:09.212+0800 I CONTROL  Hotfix KB2731284 or later update is not
 installed, will zero-out data files  
 2015-09-25T15:54:09.229+0800 I JOURNAL  [initandlisten] journal dir=c:\data\db\j
 ournal 
 2015-09-25T15:54:09.237+0800 I JOURNAL  [initandlisten] recover : no journal fil
 es present, no recovery needed 
 2015-09-25T15:54:09.290+0800 I JOURNAL  [durability] Durability thread started 
 2015-09-25T15:54:09.294+0800 I CONTROL  [initandlisten] MongoDB starting : pid=2
 488 port=27017 dbpath=c:\data\db 64-bit host=WIN-1VONBJOCE88 
 2015-09-25T15:54:09.296+0800 I CONTROL  [initandlisten] targetMinOS: Windows 7/W
 indows Server 2008 R2 
 2015-09-25T15:54:09.298+0800 I CONTROL  [initandlisten] db version v3.0.6 
 …… 

要看下是否开启成功，mongodb采用27017端口，那么我们就在浏览器里面键入http://localhost:27017/。
::
 It looks like you are trying to access MongoDB over HTTP on the native driver port

如果你需要进入MongoDB后台管理，你需要先打开mongodb装目录的下的bin目录，然后执行mongo.exe文件，MongoDBShell是MongoDB自带的交互式Javascript shell,用来对MongoDB进行操作和管理的交互式环境。

当你进入mongoDB后台后，它默认会链接到 test 文档（数据库）

将MongoDB设置成Windows服务
--------------------------
1. 在d:\mongodb\data下新建文件夹log（存放日志文件）并且新建文件mongodb.log

2. 在d:\mongodb新建文件mongo.config
	用记事本打开mongo.config输入：

	dbpath=D:\\mongodb\\data\\db

	logpath=D:\\mongodb\\data\\log\\mongodb.log 
3. 用管理员身份打开cmd命令行，进入D:\mongodb\bin目录，输入如下的命令：
::
 mongod --config D:\mongodb\mongo.config --install --serviceName "MongoDB"
如图结果存放在日志文件中，查看日志发现已经成功。如果失败有可能没有使用管理员身份，遭到拒绝访问。

4. 打开cmd输入services.msc查看服务可以看到MongoDB服务，点击可以启动。

GUI
---
 * 网页式,由Django和jQuery所构成。
 * 一个CouchDB Futon web的mongodb山寨版。
 * Ruby写成。
 * 适用于OSX的应用程序。
 * 一个基于浏览器的MongoDB控制台, 由PHP撰写而成。
 * Windows的mongodb管理工具
 * 最好的PHP语言的MongoDB管理工具，轻量级, 支持多国语言.
教程给的是Rockmongo 下载地址：http://rockmongo.com/downloads 

而我用的是Robo 3T         https://robomongo.org/。