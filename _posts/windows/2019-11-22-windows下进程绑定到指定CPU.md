---
layout: post
title:  "windows下进程绑定到指定CPU"
date:   2019-11-22 17:31:01 +0800
categories: windows
---
#### 一、C语言实现
```java
#include <stdio.h>
#include <windows.h>
void main()
{
    //0x00000001代表第一个CPU
    //0x00000002代表第二个CPU
    //0x00000004代表第三个CPU
    //0x00000008代表第四个CPU
    //0x00000003代表第一、二个CPU
    //0x00000007代表第一、二、三个CPU
    //0x0000000f代表第一、二、三、四个CPU
	int dwThreadAffinityMask = 0x00000002;
    SetThreadAffinityMask(GetCurrentThread(), dwThreadAffinityMask);
    //do somthing
}
```
#### 二、手动实现（推荐）
1. 打开资源管理器->进程,随便找一个进程右击，点击转到详细信息
   
    <figure>
        <img src="{{ site.baseurl }}/images/windows_cpu1.png" alt="image">
        <figcaption>
        </figcaption>
    </figure>

2. 右击 设置相关性，弹出 处理器相关窗口

    <figure>
        <img src="{{ site.baseurl }}/images/windows_cpu2.png" alt="image">
        <figcaption>
        </figcaption>
    </figure>
    
    <figure>
        <img src="{{ site.baseurl }}/images/windows_cpu3.png" alt="image">
        <figcaption>
        </figcaption>
    </figure>
    
3. 勾选想指定的CPU，完结撒花。  
   如果想让某一CPU占满，只需写一个bat脚本，具体步骤如下：
   - 新建loop.bat文件
   - 将下面的内容copy到文件中
        
     ```cmd
       :loop
       goto loop
     ```
     
   - 双击loop.bat
   - 按照上面的步骤绑定到指定CPU
   
#### 另：linux下指定CPU(待完善)
1. Shell脚本

    ```cmd
    tastset -p cpu pid
    ```   

2. 内核接口

    ```cmd
    setaffinity 
    getaffinity 
    ```