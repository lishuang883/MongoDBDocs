文件存入
========
1.由于MongoDB的文档结构为BJSON格式（BJSON全称：Binary JSON），而BJSON格式本身就支持保存二进制格式的数据，因此可以把文件的二进制格式的数据直接保存到MongoDB的文档结构中。取的时候再以二进制格式取，这样文档就能实现无损保存。

2.数据库支持以BSON格式保存二进制对象。 但是MongoDB中BSON对象最大不能超过4MB。GridFS 规范提供了一种透明的机制，可以将一个大文件分割成为多个较小的文档。原理：
GridFS在数据库中，默认使用fs.chunks和fs.files来存储文件。（GridFS不自动处理md5相同的文件）

 * fs.files集合存放文件的信息；
 * fs.chunks存放文件数据；

进入到MongoDB的安装目录的bin目录中，找到mongofiles.exe，并输入下面的代码：
::
 mongofiles.exe -d gridfs put song.mp3
GridFS 是存储文件的数据名称。如果不存在该数据库，MongoDB会自动创建。Song.mp3 是音频文件名。

使用以下命令来查看数据库中文件的文档：
::
 >db.fs.files.find()
以上命令执行后返回以下文档数据：
::
 {
   _id: ObjectId('534a811bf8b4aa4d33fdf94d'), 
   filename: "song.mp3", 
   chunkSize: 261120, 
   uploadDate: new Date(1397391643474), md5: "e4f53379c909f7bed2e9d631e15c1c41",
   length: 10401959 
 }
我们可以看到 fs.chunks 集合中所有的区块，以下我们得到了文件的 _id 值，我们可以根据这个 _id 获取区块(chunk)的数据： 
::
 >db.fs.chunks.find({files_id:ObjectId('534a811bf8b4aa4d33fdf94d')})
以上实例中，查询返回了 40 个文档的数据，意味着mp3文件被存储在40个区块中。

3. 使用mongoimport 在命令行执行如下： 
::
 mongoimport -d test -c students students.dat -d test 
指的是导入test 数据库(database)
