---
layout: post
title:  "Windows下载安装MySQL"
date:   2019-02-18 10:31:01 +0800
categories: MySQL
---
#### 1.下载MySQL
官网下载地址[https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/)

#### 2.安装MySQL
- 打开下载文件解压到指定文件目录。如：D:\mysql-5.7.21-winx64
- 打开解压后的MySql文件在根目录下创建my.ini，内容如下：
    ```properties
    [mysql]
     
    # 设置mysql客户端默认字符集
     
    default-character-set=utf8
     
    [mysqld]
     
    #设置3306端口
     
    port = 3306
     
    # 设置mysql的安装目录
     
    basedir=D:\mysql-5.7.21-winx64
     
    # 设置mysql数据库的数据的存放目录
     
    datadir=D:\mysql-5.7.21-winx64\data
     
    # 允许最大连接数
     
    max_connections=200
     
    # 服务端使用的字符集默认为8比特编码的latin1字符集
     
    character-set-server=utf8
     
    # 创建新表时将使用的默认存储引擎
     
    default-storage-engine=INNODB
    
    ```
#### 3.运行
- 找到CMD命令提示符，右键以管理员身份运行
- 进入mysql子目录bin
- 依次输入:
    ```cmd
    mysqld --install 
    mysqld --initialize
    net start mysql
    ```    
#### 4.查看MySql登录密码
  进入D:\mysql-5.7.21-winx64\data，查看.err文件，里面有如下一行：
  ```properties
    A temporary password is generated for root@localhost: xxxxxxxx 
  ```
  登录成功后必须重设密码，否则会一直提示下面这行错误
  ```cmd
    You must reset your password using ALTER USER statement before executing this statement.
  ```
  
  ```cmd
  #MySQL
  alter user 'root'@'localhost' identified by 'youpassword';
  #MariaDB
  SET PASSWORD = PASSWORD('youpassword');
  ```
  
     