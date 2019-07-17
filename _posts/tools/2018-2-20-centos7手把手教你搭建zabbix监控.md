---
layout: post
title:  "centos7手把手教你搭建zabbix监控"
date:   2018-2-10 19:31:01 +0800
categories: tools
---
#### 经验之谈
- 官方安装文档点[这里](https://www.zabbix.com/documentation/3.4/zh/manual/installation/install) 
- 中文乱码处理方式：
   ```cmd
   #1.将Windows/Fonts/simkai.ttf复制到zabbix-server主机上
   [guoyanjun@guoyanjun-pc omc]$ scp /run/media/guoyanjun/Windows7/Windows/Fonts/simkai.ttf root@192.168.122.190:/var/www/html/zabbix/fonts/
   #2.在server主机上替换默认字体文件
   [root@collectd-server fonts]# cd /var/www/html/zabbix/fonts/
   [root@collectd-server fonts]# mv DejaVuSans.ttf DejaVuSans.ttf.bak
   [root@collectd-server fonts]# mv simkai.ttf DejaVuSans.ttf
   #3.刷新页面
   ```

1、安装前需要先关闭selinux和firewall.

```cmd
[root@zabbix ~]# vi /etc/selinux/config 
#将SELINUX=enforcing改为SELINUX=disabled 
```
设置后需要重启才能生效

```cmd
[root@zabbix ~]# setenforce 0       #临时关闭
[root@zabbix ~]# getenforce         #检测selinux是否关闭，Disabled 为关闭
```
```cmd
[root@zabbix ~]# firewall-cmd --state    #查看默认防火墙状态
not running           #关闭后显示not running，开启后显示running
[root@zabbix ~]# systemctl stop firewalld.service    #临时关闭firewall
[root@zabbix ~]# systemctl disable firewalld.service       #禁止firewall开机启动
```

2、配置zabbix 程序安装

- 2.1配置zabbix 的yum源
```cmd
[root@zabbix ~]# rpm -ivh http://repo.zabbix.com/zabbix/3.4/rhel/7/x86_64/zabbix-release-3.4-2.el7.noarch.rpm
```

- 2.2安装zabbix程序包，安装mysql、zabbix-agent
```cmd
[root@zabbix ~]# yum install -y zabbix-server-mysql zabbix-web-mysql zabbix-agent mariadb-server
```
- 2.3 启动mariadb(数据库)并设置开机启动，创建数据库实例，授权

    ```cmd
    [root@zabbix ~]# systemctl start mariadb         #启动mariadb
    [root@zabbix ~]# systemctl enable mariadb        #开机时启动mariadb
    [root@zabbix ~]# mysql                         #登入数据库
    MariaDB [(none)]> create database zabbix character set utf8 collate utf8_bin;  
    
    #创建数据库实例
    Query OK, 1 row affected (0.00 sec)
    MariaDB [(none)]> grant all privileges on zabbix.* to zabbix@'%' identified by 'zabbix';  
    
    #授权所有主机访问数据库实例zabbix，用户名/密码：zabbix/zabbix
    Query OK, 0 rows affected (0.00 sec)
    MariaDB [(none)]> grant all privileges on zabbix.* to zabbix@localhost identified by 'zabbix';   
    
    #授权localhost主机名访问数据库实例zabbix，用户名/密码：zabbix/zabbix
    Query OK, 0 rows affected (0.00 sec)
    MariaDB [(none)]> grant all privileges on zabbix.* to zabbix@localhost.localdomain identified by 'zabbix';  
    
    #授权localhost.localdomain主机访问数据库实例zabbix，用户名/密码：zabbix/zabbix
    Query OK, 0 rows affected (0.00 sec)
    ```

   导入初始模式和数据  
   ```cmd
   [root@zabbix ~]# cd /usr/share/doc/zabbix-server-mysql-3.4.5/  #进入create.sql.gz所在目录
   [root@zabbix zabbix-server-mysql-3.4.5]# zcat create.sql.gz |mysql -uroot zabbix  #导入出事模式
   ```

   注：这里的数据库没有设置密码的

- 2.4、启动zabbix-server

    2.4.1 配置zabbix-server r 配置文件zabbix-server.conf

    ```cmd
    root@zabbix zabbix-server-mysql-3.4.5]# vi /etc/zabbix/zabbix_server.conf
    DBHost=localhost          # 数据主机名
    DBName=zabbix            # 数据库实例
    DBUser=zabbix              # 用户名
    DBPassword=zabbix      # 密码
    ```
    
    2.4.2 启动zabbix-server服务
    
    ```cmd
    [root@zabbix ~]# systemctl start zabbix-server   #启动zabbix-server服务
    [root@zabbix ~]# systemctl enable zabbix-server   #开机启动zabbix-server服务。
    ```

- 2.5编辑Apache的配置文件，消注释设置正确的时区
    
    ```cmd
    [root@zabbix ~]# vi /etc/httpd/conf.d/zabbix.conf
    
    php_value max_execution_time 300
    
    php_value memory_limit 128M
    
    php_value post_max_size 16M
    
    php_value upload_max_filesize 2M
    
    php_value max_input_time 300
    
    php_value always_populate_raw_post_data -1
    
    php_value date.timezone Asia/Shanghai
    ```
    启动httpd服务，设置开机启动httpd服务

    ```cmd
    [root@zabbix ~]# systemctl start httpd     #启动httpd服务
    [root@zabbix ~]# systemctl enable httpd    #设置开机启动httpd服务
    ```

3、启动zabbix-agent并设置开机自启动

```cmd
[root@zabbix ~]# systemctl start zabbix-agent  #启动zabbix-agent服务
[root@zabbix ~]# systemctl enable zabbix-agent   #设置zabbix-agent服务开机自动启动
```    

4、zabbix web 网页安装
- 4.1.在浏览器输入地址http://服务器ip/zabbix/setup.php，出现欢迎界面，点击下一步；
- 4.2.出现必要条件检测界面，正常都是OK，点击下一步
- 4.3.配置DB连接，与zabbix_server.conf文件中主机、数据库名称、用户名、密码保持一致，点击下一步

        <figure>
            <img src="{{ site.baseurl }}/images/zabbix-mysql.png" alt="image">
            <figcaption>
                zabbix-mysql
            </figcaption>
        </figure>

- 4.4.zabbix服务器详细信息，点击下一步
- 4.5.安装前汇总，检查信息无误，点击下一步安装
- 4.6.安装成功

5、zabbix网页登录

在浏览器输入http://zabbix服务器ip/zabbix/index.php，输入管理员用户名Admin(区分大小写)，默认密码zabbix，点击登入即可。

6、设置zabbix 中文

<figure>
    <img src="{{ site.baseurl }}/images/zabbix-中文.png" alt="image">
    <figcaption>
        zabbix-中文
    </figcaption>
</figure>

7、解决中文在图形界面上的乱码

- 7.1 一般情况下还是会出现中文乱码的情况

    <figure>
        <img src="{{ site.baseurl }}/images/zabbix-中文乱码展示.png" alt="image">
        <figcaption>
            zabbix-中文乱码展示
        </figcaption>
    </figure>

- 7.2 因为zabbix自身对中文简体的支持不完善，需要我们手动的去上传新的字体进行替换：

    在C:\Windows\Fonts中复制想要的字体，后缀为ttf，把文件复制到桌面。
    
    上传至zabbix服务器的/usr/share/zabbix/fonts 目录中，把文件上传在linux系统中我们可以使用winSCP 这个软件。在这里我直接使用 rz -y 这个命令上传。
    
    ```cmd
    [root@zabbix fonts]# yum install lrzsz -y   #安装命令
    [root@zabbix fonts]# rz -y
    [root@zabbix fonts]# mv graphfont.ttf  graphfont.ttf.bak   #把graphfont.ttf备份
    [root@zabbix fonts]# mv simkai.ttf graphfont.ttf           #把simkai.ttf 改名为graphfont.ttf
    ```    

    然后刷新下网页就可以了。
    
8、zabbix-agent 客户端安装与配置（windows操作系统）

目前已安装好了zabbix-server 服务端，接下来我们需要添加客户端的操作。

现在添加监控的对象是server 2012操作系统 64位。

- 8.1下载zabbix-agent 监控客户端软件安装包（windows操作系统）
  
  官方下载地址：http://www.zabbix.com/download
  
- 8.2 关闭监控主机windows server 2008防火墙或防火墙入放行zabbix_agentd客户端口号  10050 (TPC/UDP)。

- 8.3 下载后解压zabbix_agents_3.4.0.win.zip压缩包。里面有两个文件夹，一个是bin文件夹，另一个是conf文件夹。

  Bin文件夹里面有两个文件夹，一个是win32文件夹里存放zabbix_agentd安装程序应用于windows 32位操作系统，一个是win64文件夹里存放zabbix_agentd安装程序应用于windows 64位操作系统。

  Conf文件夹里存放是配置文件zabbix_agentd.win.conf
  
- 8.4 在windows server 2012 操作系统下， C盘目录下创建一个zabbix文件夹，把刚下载的zabbix_agentd压缩包里的win64位文件夹复制到zabbix文件夹里。把conf文件夹zabbix_agentd.win.conf复制到新创的zabbix目录下。

- 8.5 右键以文本格式编辑zabbix_agentd.win.conf 配置文件，使用Notrpad++编辑。
 
  修改下面几项:
  ```properties
    EnableRemoteCommands=1           #允许在本地执行远程命令
    LogRemoteCommands=1               #执行远程命令是否保存操作日志
    Server=192.168.3.50                       #填写zabbix-server服务器IP地址
    ServerActive=192.168.3.50             #填写zabbix-server服务器IP地址
    Hostname=server2012                    #zabbix_agent客户端计算机名 (被监控主机)
  ```
- 8.6 打开DOS命令窗口---- 输入以下两条命令进行zabbix客户端安装。(必须要以管理员身份运行打开DOS命令窗口)

    ```cmd
    C:\zabbix\zabbix_agentd.exe -i -c C:\zabbix\zabbix_agentd.win.conf
    
    #安装zabbix客户端
    C:\zabbix\zabbix_agentd.exe -s -c C:\zabbix\zabbix_agentd.win.conf
    
    #启动zabbix服务
    ```  

- 8.7  在zabbix服务端操作

    8.7.1  选择配置 ---- 主机 ---- 创建主机。

    8.7.2  输入客户端计算机名 --- 可见名称自定义 ---- 群组自行选择 ---- 输入客户端计算IP地址 ---- 勾选已启用 ---- 选择添加。
    
    <figure>
        <img src="{{ site.baseurl }}/images/zabbix-增加服务器.png" alt="image">
        <figcaption>
            zabbix-增加服务器
        </figcaption>
    </figure>
    
    8.7.3 添加 zabbix_agentd 客户端监控模版。
     
    <figure>
        <img src="{{ site.baseurl }}/images/zabbix_agentd添加监控模板.png" alt="image">
        <figcaption>
            zabbix_agentd添加监控模板
        </figcaption>
    </figure>
    
    <figure>
        <img src="{{ site.baseurl }}/images/zabbix_agentd添加监控模板2.png" alt="image">
        <figcaption>
            zabbix_agentd添加监控模板2
        </figcaption>
    </figure>
    
    8.7.3 检查zabbix-agent服务是否开启或直接重启zabbix-agent 服务。
    
    <figure>
        <img src="{{ site.baseurl }}/images/zabbix_agentd服务查看.png" alt="image">
        <figcaption>
            zabbix_agentd服务查看
        </figcaption>
    </figure>
    
9、zabbix 邮件告警提示
- 9.1 创建自定义媒介，和邮件脚本

    ```cmd
    [root@zabbix ~]# vi /etc/zabbix/zabbix_server.conf
    AlertScriptsPath=/usr/lib/zabbix/alertscripts   #修改配置文件
    
    [root@zabbix ~]# cd /usr/lib/zabbix/alertscripts/
    [root@zabbix alertscripts]# vi zabbix-email.py 
    
    #!/usr/bin/python
    
    #coding:utf-8
    
    import smtplib
    
    from email.mime.text import MIMEText
    
    import sys
    
    mail_host = 'smtp.163.com'
    
    mail_user = '15626866674'
    
    mail_pass = '15626866674.'
    
    mail_postfix = '163.com'
    
    def send_mail(to_list,subject,content):
    
        me = "zabbix3.4监控告警平台"+"<"+mail_user+"@"+mail_postfix+">"
    
        msg = MIMEText(content, 'plain', 'utf-8')
    
        msg['Subject'] = subject
    
        msg['From'] = me
    
        msg['to'] = to_list
    
        try:
    
            s = smtplib.SMTP()
    
            s.connect(mail_host)
    
            s.login(mail_user,mail_pass)
    
            s.sendmail(me,to_list,msg.as_string())
    
            s.close()
    
            return True
    
        except Exception,e:
    
            print str(e)
    
            return False
    
    if __name__ == "__main__":
    
    send_mail(sys.argv[1], sys.argv[2], sys.argv[3])
    
    #添加上面内容，并修改邮箱。
    
    [root@zabbix alertscripts]# chmod +x zabbix-email.py  #修改权限
    ```    
    
- 9.2 管理---报警媒介类型---创建媒体类型

    ```properties
    名称：zabbix-email
    
    类型：脚本
    
    脚本名称：zabbix-email.py
    
    脚本参数：{ALERT.SENDTO}
    
              {ALERT.SUBJECT}
    
              {ALERT.MESSAGE}
    
               {ALERT.URL}
    ```

- 9.3 管理---用户，点击admin,选择报警媒介并添加
    
    <figure>
        <img src="{{ site.baseurl }}/images/zabbix-告警媒介添加.png" alt="image">
        <figcaption>
            zabbix-告警媒介添加
        </figcaption>
    </figure>

- 9.4 配置—动作，编辑动作，然后添加操作，添加恢复操作。    

    操作
    ```properties
    时间：60s
    
    接收人：问题警告: {TRIGGER.NAME}
    
    默认信息：问题警告 started at {EVENT.TIME} on {EVENT.DATE}
    
    问题警告对象: {TRIGGER.NAME}
    
    Host: {HOST.NAME}:{HOST.CONN}
    
    Severity: {TRIGGER.SEVERITY}
    
     
    
    Original problem ID: {EVENT.ID}
    
    {TRIGGER.URL}
    
    注：记得添加发送到用户
    ```
    
    恢复操作
    
    ```properties
    接收人：告警已恢复: {TRIGGER.NAME}
    
    默认信息：
    
    告警已恢复 at {EVENT.RECOVERY.TIME} on {EVENT.RECOVERY.DATE}
    
    告警恢复对象: {TRIGGER.NAME}
    
    Host: {HOST.NAME}:{HOST.CONN}
    
    Severity: HEALTH
    
    Original problem ID: {EVENT.ID}
    
    {TRIGGER.URL}
    
    注：记得添加发送到用户
    ```    
-9.4 测试    

  ①记得客户端Server-IP要指向服务器的IP
  ```cmd
    vim /usr/local/zabbix/etc/zabbix_agentd.conf
    Server=10.0.0.137
  ```  
   ②往往邮箱收不到邮件的原因是没打开邮箱设置里面的POP3服务