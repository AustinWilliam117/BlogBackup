---
title: Linux 安装多个版本JDK并设置默认版本
date: 2021-11-03 20:32:36
tags: 
categories: ["Linux"]
---

> 一台设备不能同事运行两个 Java 版本

1. 官网下载 JDK 版本，`jdk-8u181-linux-x64.tar.gz`

2. 使用 ssh 工具上传安装包到 Linux 系统，传至：`/usr/local`

3. Linux 用户安装的程序一般放在 `/usr/local`下

<!--more-->

4. 使用命令解压：tar zxcf jdk-8u181-linux-x64.tar.gz， 解压安装包到 /usr/local/下

5. 配置环境变量

   用管理员用户root对 /etc/profile进行编辑，添加如下内容：

   ```shell
   export JAVA_HOME=/usr/local/jdk1.8.0_181   -- jdk所在目录
   export JRE_HOME=/usr/local/jdk1.8.0_181/jre -- jre所在目录
   export CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
   export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
   ```

6. 退出vim编辑器，使环境设置立即生效 `source /etc/profile`

7. 使用`java -version`命令查看档期那系统正在使用的java版本

   ```shell
   [root@CentOS ~]# java -version
   java version "1.7.0"
   Java(TM) SE Runtime Environment (build 1.7.0)
   Java HotSpot(TM) 64-Bit Server VM (build 25.170, mixed mode)
   ```

   发现并不是新安装的jdk版本，使用命令更改当前系统使用的jdk版本

   `alternatives --config java`

   ```shell
   [root@CentOS ~]# alternatives --config java
   
   There are 3 programs which provide 'java'.
   
     Selection    Command
   -----------------------------------------------
   *+ 1           /usr/lib/jvm/jre-1.7.0-openjdk.x86_64/bin/java
      2           /usr/lib/jvm/jre-1.6.0-openjdk.x86_64/bin/java
      3           /usr/lib/jvm/jre-1.5.0-gcj/bin/java
   ```

   没有新安装的1.8.1版本jdk，需要使用命令将新安装的jdk放入到java bin中

   `alternatives --install /usr/bin/java java /usr/local/jdk1.8.0_181/bin/java 4`

   再使用`alternatives --config java`，此时可以看到新安装的 版本，然后选择即可：

   ```shell
   [root@CentOS ~]# alternatives --config java
   
   There are 4 programs which provide 'java'.
   
     Selection    Command
   -----------------------------------------------
   *+ 1           /usr/lib/jvm/jre-1.7.0-openjdk.x86_64/bin/java
      2           /usr/lib/jvm/jre-1.6.0-openjdk.x86_64/bin/java
      3           /usr/lib/jvm/jre-1.5.0-gcj/bin/java
      4           /usr/local/jdk1.8.0_181/bin/java
   
   Enter to keep the current selection[+], or type selection number: 4
   [root@CentOS ~]# java -version
   java version "1.8.0_181"
   Java(TM) SE Runtime Environment (build 1.8.0_181-b13)
   Java HotSpot(TM) 64-Bit Server VM (build 25.181-b13, mixed mode)
   ```

