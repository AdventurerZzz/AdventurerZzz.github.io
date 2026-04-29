---
title: "Java环境搭建与mcp连接数据库"
date: 2025-12-26 16:38:08
tags:
---

## 本文用于记录 Java 环境搭建与 mcp 连接数据库的步骤

# 安装 Java

## JDK

### 1.下载地址 ：[jdk17](https://download.oracle.com/java/17/archive/jdk-17_windows-x64_bin.exe)

### 2.安装步骤：

1. 按照安装向导的提示进行安装

2. 右击【我的电脑】->【属性】->【高级系统设置】->【高级】->【环境变量】
   ![环境变量](/img/jdkpath.png "环境变量")
   新增环境变量：

   - 变量名：JAVA_HOME
     ![环境变量](/img/newJavahome.png "环境变量")
   - 变量值：D:\java\jdk-17 （根据自己的安装路径修改）
   - 变量名：PATH
   - 变量值：%JAVA_HOME%\bin;
     ![环境变量](/img/javahomepath.png "环境变量")

3. 打开命令行，在地址栏输入 `cmd` ，打开命令行
   ![命令行](/img/jdkCmd.png "命令行")

4. 在命令行中输入 `java -version` ，如果出现 `java` 命令，则说明安装成功
   ![命令行](/img/jdk.png "命令行")

## maven

### 1.下载地址 ：[maven](https://maven.apache.org/download.cgi)

![maven](/img/mavenDown.png "maven")

### 2.安装步骤：

1. 将解压后的文件夹复制到 D:\java 目录下

![maven](/img/mavenurl.png "maven")

2. 新增环境变量：

- 变量名：M2_HOME
- 变量值：D:\java\apache-maven-3.9.12
  ![环境变量](/img/mavenM2.png "环境变量")
- 变量名：PATH
- 变量值：%M2_HOME%\bin;
  ![环境变量](/img/m2mavenpath.png "环境变量")

3. 打开命令行，在地址栏输入 `cmd` ，打开命令行，输入 `mvn -v` ，如果出现 `mvn` 命令，则说明安装成功
   ![命令行](/img/mvn_version.png "命令行")

4. 在 D:\java\apache-maven-3.9.12 下找到 conf 文件夹，打开，找到 settings.xml 文件：
   ![settings.xml](/img/mvnsetting.png "settings.xml")
   找到 localRepository 标签，在注释外添加：

   ```xml
   <localRepository>D:\java\apache-maven-3.9.12\repository</localRepository>
   ![settings.xml](/img/mvnlocal.png "settings.xml")
   ```

5. 配置国内镜像：
   ![settings.xml](/img/mvnjx.png "settings.xml")
   找到 mirrors 标签，在注释外添加：

   ```xml
   <!-- 阿里云仓库 -->
   <mirror>
   <id>alimaven</id>
   <mirrorOf>central</mirrorOf>
   <name>aliyun maven</name>
   <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
   </mirror>
   ```

6. 配置 JDK 版本:
   ![settings.xml](/img/mvnjdk.png "settings.xml")
   找到 profiles 标签，在注释外添加：

   ```xml
   <profile>
   <id>jdk-17</id>
   <activation>
   <jdk>17</jdk>
   </activation>
   <properties>
   <maven.compiler.release>17</maven.compiler.release>
   </properties>
   </profile>
   ```

7. 配置完成后，保存文件，关闭命令行，重新打开命令行，输入 `mvn help:system` ，如果出现很多文件的页面，则说明配置成功
   ![命令行](/img/mvnhelp.png "命令行")

# 安装 mcp

# 连接数据库

# 测试连接

# 完成
