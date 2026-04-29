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

直接在MCP的客户端中配置
配置文件：`~/.mcp/servers.json`

```json
{
  "mcpServers": {
    "mysql": {
      "command": "npx",
      "args": ["-y", "@fhuang/mcp-mysql-server"],
      "env": {
        "MYSQL_HOST": "localhost",
        "MYSQL_USER": "your_username",
        "MYSQL_PASSWORD": "your_password",
        "MYSQL_DATABASE": "your_database"
      }
    }
  }
}
```

# 连接数据库

在Java项目中连接数据库

```java
// 导入JDBC相关类

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.util.Arrays;

/**
 * 向 t_user 表插入多条数据（Maven/普通Java项目通用）
 */
public class DBConnectionTest {
    // 1. 配置数据库连接参数（替换为你的实际配置）
    private static final String JDBC_URL = "jdbc:mysql://localhost:3306/test_db?useSSL=false&serverTimezone=Asia/Shanghai";
    private static final String DB_USER = "root";
    private static final String DB_PASSWORD = "123456";

    public static void main(String[] args) {
        // 2. try-with-resources：自动关闭 Connection 和 PreparedStatement 资源
        try (
                // 获取数据库连接
                Connection conn = DriverManager.getConnection(JDBC_URL, DB_USER, DB_PASSWORD);
                // 预编译插入SQL（? 是占位符，对应 username 和 age 字段，id自增、create_time自动填充，无需手动传入）
                PreparedStatement pstmt = conn.prepareStatement("INSERT INTO t_user (username, age) VALUES (?, ?)")
        ) {
            System.out.println("数据库连接成功！🎉");

            // 方式1：单条插入（插入第一条数据）
            pstmt.setString(1, "张三");  // 第一个占位符：username（String类型）
            pstmt.setInt(2, 20);         // 第二个占位符：age（int类型）
            int singleAffectedRows = pstmt.executeUpdate(); // 执行单条插入
            System.out.println("单条数据插入成功，受影响行数：" + singleAffectedRows);

            // 方式2：批量插入（插入多条数据，效率更高）
            // 清空上一次的占位符参数
            pstmt.clearParameters();
            pstmt.setString(1, "李四");
            pstmt.setInt(2, 22);
            pstmt.addBatch(); // 添加到批量任务队列

            pstmt.clearParameters();
            pstmt.setString(1, "王五");
            pstmt.setInt(2, 25);
            pstmt.addBatch(); // 添加到批量任务队列

            pstmt.clearParameters();
            pstmt.setString(1, "赵六");
            pstmt.setInt(2, 28);
            pstmt.addBatch(); // 添加到批量任务队列

            // 执行批量插入，返回每条数据的受影响行数
            int[] batchAffectedRows = pstmt.executeBatch();
            System.out.println("批量数据插入成功，每条数据受影响行数：" + Arrays.toString(batchAffectedRows));
            System.out.println("累计插入数据条数：" + (1 + batchAffectedRows.length)); // 1条单条 + 批量条数

        } catch (Exception e) {
            // 捕获异常，打印报错信息
            System.err.println("数据库操作失败！❌");
            e.printStackTrace();
        }
    }
}
```

# 完成
