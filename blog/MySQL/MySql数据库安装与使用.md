## MySQL数据库操作

### MySQL 安装

**Linux/UNIX 上安装 MySQL**

Linux平台上推荐使用RPM包来安装Mysql,MySQL AB提供了以下RPM包的下载地址：

- MySQL - MySQL服务器。你需要该选项，除非你只想连接运行在另一台机器上的MySQL服务器。
- MySQL-client - MySQL 客户端程序，用于连接并操作Mysql服务器。
- MySQL-devel - 库和包含文件，如果你想要编译其它MySQL客户端，例如Perl模块，则需要安装该RPM包。
- MySQL-shared - 该软件包包含某些语言和应用程序需要动态装载的共享库(libmysqlclient.so*)，使用MySQL。
- MySQL-bench - MySQL数据库服务器的基准和性能测试工具。

安装前，我们可以检测系统是否自带安装 MySQL:

	rpm -qa | grep mysql

如果你系统有安装，那可以选择进行卸载:

	rpm -e mysql　　// 普通删除模式
	rpm -e --nodeps mysql　　// 强力删除模式，如果使用上面命令删除时，提示有依赖的其它文件，则用该命令可以对其进行强力删除

**Windows 上安装 MySQL**
下载完后，我们将 zip 包解压到相应的目录，这里我将解压后的文件夹放在 C:\web\mysql-8.0.11 下。

接下来我们需要配置下 MySQL 的配置文件

打开刚刚解压的文件夹 C:\web\mysql-8.0.11 ，在该文件夹下创建 my.ini 配置文件，编辑 my.ini 配置以下基本信息：

	[mysql]
	# 设置mysql客户端默认字符集
	default-character-set=utf8
	 
	[mysqld]
	# 设置3306端口
	port = 3306
	# 设置mysql的安装目录
	basedir=C:\\web\\mysql-8.0.11
	# 设置 mysql数据库的数据的存放目录，MySQL 8+ 不需要以下配置，系统自己生成即可，否则有可能报错
	# datadir=C:\\web\\sqldata
	# 允许最大连接数
	max_connections=20
	# 服务端使用的字符集默认为8比特编码的latin1字符集
	character-set-server=utf8
	# 创建新表时将使用的默认存储引擎
	default-storage-engine=INNODB

**接下来我们来启动下 MySQL 数据库：**

以管理员身份打开 cmd 命令行工具，切换目录：

	cd C:\web\mysql-8.0.11\bin

初始化数据库：

	mysqld --initialize --console

执行完成后，会输出 root 用户的初始默认密码，如：

	...
	2018-04-20T02:35:05.464644Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: APWCY5ws&hjQ
	...
**`APWCY5ws&hjQ`** 就是初始密码，后续登录需要用到，你也可以在登陆后修改密码。

输入以下安装命令：

	mysqld install

启动输入以下命令即可：

	net start mysql

**登录 MySQL**

当 MySQL 服务已经运行时, 我们可以通过 MySQL 自带的客户端工具登录到 MySQL 数据库中, 首先打开命令提示符, 输入以下格式的命名:

	mysql -h 主机名 -u 用户名 -p

参数说明：

- -h : 指定客户端所要登录的 MySQL 主机名, 登录本机(localhost 或 127.0.0。1)该参数可以省略;
- -u : 登录的用户名;
- -p : 告诉服务器将会使用一个密码来登录, 如果所要登录的用户名密码为空, 可以忽略此选项。

如果我们要登录本机的 MySQL 数据库，只需要输入以下命令即可：

	mysql -u root -p

按回车确认, 如果安装正确且 MySQL 正在运行, 会得到以下响应:

	Enter password:

若密码存在, 输入密码登录, 不存在则直接按回车登录。登录成功后你将会看到 Welecome to the MySQL monitor... 的提示语。

然后命令提示符会一直以 mysq> 加一个闪烁的光标等待命令的输入, 输入 exit 或 quit 退出登录。

### MySQL 连接

**使用mysql二进制方式连接**

您可以使用MySQL二进制方式进入到mysql命令提示符下来连接MySQL数据库。

实例
以下是从命令行中连接mysql服务器的简单实例：

	[root@host]# mysql -u root -p
	Enter password:******

退出 mysql> 命令提示窗口可以使用 exit 命令，如下所示：

	mysql> exit
	Bye

**使用 node 脚本连接 MySQL**

待续。。。

### MySQL 创建数据库
> CREATE DATABASE 数据库名;

以下命令简单的演示了创建数据库的过程，数据名为 RUNOOB:
	
	[root@host]# mysql -u root -p   
	Enter password:******  # 登录后进入终端
	
	mysql> create DATABASE RUNOOB;

**使用 mysqladmin 创建数据库**

使用**普通用户**，你可能需要特定的权限来创建或者删除 MySQL 数据库。

所以我们这边使用root用户登录，root用户拥有最高权限，可以使用 mysql mysqladmin 命令来创建数据库。

以下命令简单的演示了创建数据库的过程，数据名为 RUNOOB:

	[root@host]# mysqladmin -u root -p create RUNOOB
	Enter password:******

以上命令执行成功后会创建 MySQL 数据库 RUNOOB。

### MySQL UPDATE 查询
如果我们需要修改或更新 MySQL 中的数据，我们可以使用 SQL UPDATE 命令来操作。.

	UPDATE table_name SET field1=new-value1, field2=new-value2
	[WHERE Clause]


- 你可以同时更新一个或多个字段。
- 你可以在 WHERE 子句中指定任何条件。
- 你可以在一个单独表中同时更新数据。


**当你需要更新数据表中指定行的数据时 WHERE 子句是非常有用的。**

通过命令提示符更新数据
以下我们将在 SQL UPDATE 命令使用 WHERE 子句来更新 runoob_tbl 表中指定的数据：

**实例**

    mysql> UPDATE runoob_tbl SET runoob_title='学习 C++' WHERE runoob_id=3;
	Query OK, 1 rows affected (0.01 sec)
	 
	mysql> SELECT * from runoob_tbl WHERE runoob_id=3;
	+-----------+--------------+---------------+-----------------+
	| runoob_id | runoob_title | runoob_author | submission_date |
	+-----------+--------------+---------------+-----------------+
	| 3         | 学习 C++   | RUNOOB.COM    | 2016-05-06      |
	+-----------+--------------+---------------+-----------------+
	1 rows in set (0.01 sec)

