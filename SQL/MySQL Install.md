# WINDOWS系统下安装MySQL

## MySQL下载

https://dev.mysql.com/downloads/mysql/

下载免安装版

![image-20220428225811899](D:\notes\SQL\image-20220428225811899.png)

完成后解压即可

> 链接：https://pan.baidu.com/s/1k4UjQi1qb4mkctetFrvErQ 
> 提取码：ou0d 



## MySQL配置

### 启动服务端和客户端

先以管理员身份打开命令行

- 进入MySQL目录

```bash
cd D:\Environment\mysql-8.0.29-winx64
```

- 安装MySQL服务`mysqld --install`

```bash
D:\Environment\mysql-8.0.29-winx64\bin>mysqld --install
Service successfully installed.
```

- 初始化MySQL`mysqld --initialize --console`

```bash
D:\Environment\mysql-8.0.29-winx64\bin>mysqld --initialize --console
2022-04-28T14:48:28.477250Z 0 [System] [MY-013169] [Server] D:\Environment\mysql-8.0.29-winx64\bin\mysqld.exe (mysqld 8.0.29) initializing of server in progress as process 20644
2022-04-28T14:48:28.490901Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2022-04-28T14:48:28.973232Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2022-04-28T14:48:29.890031Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: d&5yr?orhtWC
```

结尾为密码，例如此处的`d&5yr?orhtWC`为生成的密码

- 开启MySQL服务 `net start mysql`

```bash
D:\Environment\mysql-8.0.29-winx64\bin>net start MySQL
MySQL 服务正在启动 .
MySQL 服务已经启动成功。
```

- 进入客户端验证是否安装启动成功

```bash
D:\Environment\mysql-8.0.29-winx64\bin>mysql -u root -p
Enter password: ************
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.29
```



### 修改初始密码

使用`alter user 'root'@'localhost' identified by 'xxxx';`修改密码为xxxx

```bash
mysql> alter user 'root'@'localhost' identified by 'root';
Query OK, 0 rows affected (0.01 sec)
```

使用`exit`退出后重新进入

```bash
D:\Environment\mysql-8.0.29-winx64\bin>mysql -u root -p
Enter password: ************
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.29
```



### 添加配置文件

在mysql目录下新建my.ini文件

```
[client]
port=3306
default-character-set=utf8

[mysql] 
default-character-set=utf8 

[mysqld] 
port = 3306 
basedir=D:\Environment\mysql-8.0.29-winx64
datadir=D:\Environment\mysql-8.0.29-winx64\data
collation-server = utf8_unicode_ci
init-connect='SET NAMES utf8'
character-set-server = utf8
max_connections=200 
default-storage-engine=INNODB 
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES 
```









