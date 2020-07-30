---
layout: post
title:  "服务器压测工具stress及批处理命令pssh的使用"
date:   2020-7-30 15:00:01 +0800
categories: linux
---

### 一. 将目标服务器的CPU、内存等利用率提升到指定值，以便观察服务器性能
- 使用工具：stress
- 安装方式：  
    1. yum安装  
        ```cmd
        yum install -y epel-release
        yum install -y stress  
       ```
     2. tar包安装
        ```cmd
        #下载地址https://fossies.org/linux/privat/stress-1.0.4.tar.gz/
        tar -zxvf stress-1.0.4.tar.gz
        cd stress-1.0.4
        ./configure
        ```
- 使用方式
    1. 打满指定个数的CPU
        ```cmd
        stress -c 1 
        ```
    2. 内存
        ```cmd
       #新增4个io进程，10个内存分配进程，每次分配大小1G，分配后不释放，测试100S
        stress –i 4 –vm 10 –vm-bytes 1G –vm-hang 100 –timeout 100s 
       ```  
    3. 磁盘I/O
        ```cmd
       #新增1个写进程，每次写3G文件块
       stress –d 1 --hdd-bytes 3G 
       ```      
### 二. 被观察服务器数量很多时，批量执行第1步
- 使用工具：pssh（依赖python）
- 安装方式：https://pypi.org/project/pssh/#files 下载tar包
    ```cmd
    tar xf pssh-2.3.tar.gz
    cd pssh-2.3
    python setup.py build
    python setup.py install
    ```
- 增加软链到/usr/bin
    ```cmd
    cd /usr/bin
    ln -s pssh-2.3/bin/pssh pssh
    ```  
- 使用方式
    1. pssh执行命令
        ```cmd
        # 将多台服务器的IP放入到文件中，每行一个ip,也可以加用户名
            vi hostlist.txt
            192.168.122.1
            192.168.122.2
            root@192.168.122.3
        # 执行命令
             pssh -h hostlist.txt -A -O StrictHostKeyChecking=no -i stress -c 1 
            #-A 手动输入密码
            #-O 后接StrictHostKeyChecking=no表示不用提示公钥连接，否则会卡住
            #-h  后面接主机ip文件,文件数据格式[user@]host[:port]
            #-P  显示输出内容，建议用-i
        ```  
    2. pssh执行文件
        ```cmd
         # 将命令放入test.sh文件中，执行如下命令
            pssh -h hostlist.txt -P -I < test.sh
        ```
    3. pnuke终结进程
        ```cmd
            #最后stress为关键词
            pnuke  -h hostlist.txt -A -O StrictHostKeyChecking=no stress
        ```
    4. pscp批量传输文件
        ```cmd
            #传文件,不支持远程新建目录
            pscp -h ip.txt test.py /tmp/dir1/
            #传目录
            pscp -r -h ip.txt test/ /tmp/dir1/
        ```
    


