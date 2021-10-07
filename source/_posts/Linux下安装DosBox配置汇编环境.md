---
title: Linux下安装DosBox配置汇编环境
date: 2021-02-01 19:26:14
tags: ["汇编"]
categories: ["计算机基础"]
---

转载：https://blog.csdn.net/qq_35572368/article/details/104638739

### 一、

**首先我们去DosBox官网下载DosBox-0.73
或者直接启用终端命令行输入以下代码**

```
sudo pacman -S dosbox
```
其他版本也可以在[dosbox官网下载](https://www.dosbox.com/download.php?main=1)

<!--more-->

**这样我们就下载安装好了DosBox**

### 二、

**然后我们还需要DEBUG.exe、LINK.exe、MASM等，这些的文件我在下面这个连接提供压缩包：**
链接：https://www.lanzous.com/i9wnzed

### 三、

**接着我们构建目录，在命令行依次输入以下代码**

```
~$ mkdir ms-dos
~$ cd ms-dos
~$ mkdir MASM
~$ mkdir ASM
~$ mkdir FILE
```

**然后我们将下载的文件解压，将里面的文件全部复制到新建的MASM文件夹内，或者直接将解压的MASM文件夹连同里面的文件一同复制到ms-dos内，选择合并。**

### 四、


**(1) 然后我们来配置环境和自动挂载，首先在终端命令行输入以下代码：**

```
~$ vim .dosbox/dosbox-0.74-3.conf
```

**(2) 如未报错则会打开dosbox-0.74.conf文件，然后我们在文件末尾添加如下代码：**

```
mount c ~/ms-dos
path=%path%;\MASM
c:
```

**(3) 接着我们打开DosBox,输入如下代码：**

```
c:>config -writeconf .dosbox/dosbox-0.74-3.conf

```

**这样我们便配置成功！**

