---
layout: post
title:  "java执行shell脚本"
date:   2019-4-2 11:01:01 +0800
categories: shell
---

#### 执行本地命令
```java
    /**
     * 执行本地命令
     * @param command 命令字符串
     * @return
     */
    public boolean execLocalCommand(String command) throws IOException{
        Runtime rt = Runtime.getRuntime();
        logger.info("开始执行本地命令：" +  command);
        Process proc = rt.exec(command,null,null);
        logger.info("本地命令执行完毕！");
        return true;
    }
```

#### 执行远程命令
加入依赖包
```xml
    <dependency>
        <groupId>ch.ethz.ganymed</groupId>
        <artifactId>ganymed-ssh2</artifactId>
        #注意，如果version是没有带build的高版本，比如262，其中的SCPClient的put方法参数变多且无法传输，原因未知
        <version>build210</version>
    </dependency>
```
```java
    private Connection conn;

    private void connect(String ip,String loginName, String passwd) throws IOException {
        conn = new Connection(ip);
        conn.connect();
        boolean isAuthenticated = conn.authenticateWithPassword(loginName, passwd);
        if (!isAuthenticated) {
            throw new IOException("Authentication failed.");
        }
    }
    
    /**
     * 执行命令
     * @param command 命令字符串
     * @return
     */
    public boolean execCommand(String command) {
        boolean flag = false;
        Session sess = null;
        BufferedReader br = null;

        if (conn == null){
            throw new IllegalStateException("没有建立连接");
        }

        logger.info("开始执行命令：" +  command);
        int length = -1;
        byte[] buffer = new byte[1024];
        long start = System.currentTimeMillis();
        StringBuilder sb = new StringBuilder();
        try {
            sess = conn.openSession();
            sess.execCommand(command);
            InputStream is = sess.getStderr();
            while ((length = is.read(buffer)) > -1) {
                sb.append(new String(buffer, 0, length));
            }
            logger.info(sb.toString()); // 实时打印命令执行情况
            flag = true;
            logger.info("命令执行成功，耗时：秒"+ (System.currentTimeMillis() - start) / 1000.0 + "秒");
        } catch (IOException e) {
            logger.error("execCommand error",e);
        } finally {
            try {
                if (sess != null) {
                    sess.close();
                }
                if (null != br) {
                    br.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return flag;
    }
    
    /**
     * 执行scp命令可以用封装好的客户端
     * @return
     */
    public void scp(String localPath,String remotePath) throws IOException {
        SCPClient client = new SCPClient(conn);
        //将本地文件scp到远程
        client.put(localPath, remotePath);
        //将远程文件scp到本地
        client.get(remotePath, localPath);
    }
    
    /**
     * 下载文件 
     * 注：该方法下载超过100M的文件容易丢包，慎用，原因未知
     * @param pathname  FTP服务器文件目录 *
     * @param filename  文件名称 *
     * @param localpath 下载后的文件路径 *
     * @return
     */
    public boolean downloadFile(String pathname,String filename, String localpath) {
        boolean flag = false;
        OutputStream os = null;
        try {
            createDirectory(localpath);
            logger.info("开始下载文件");
            //切换FTP目录
            ftpClient.changeWorkingDirectory(pathname);
            FTPFile[] ftpFiles = ftpClient.listFiles();
            for (FTPFile file : ftpFiles) {
                if (fileHashName.equalsIgnoreCase(file.getName())) {
                    File localFile = new File(localpath + "/" + filename);
                    os = new FileOutputStream(localFile);
                    ftpClient.retrieveFile(file.getName(), os);
                    os.close();
                }
            }
            ftpClient.logout();
            flag = true;
            logger.info("下载文件成功");
        } catch (Exception e) {
            logger.error("下载文件失败",e);
        } finally {
            if (ftpClient.isConnected()) {
                try {
                    ftpClient.disconnect();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (null != os) {
                try {
                    os.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        return flag;
    }
    
```