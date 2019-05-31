## mysql连接flush-hosts问题处理

### 上网查到解决方案

【错误】Host is blocked because of many connection errors; unblock with 'mysqladmin flush-hosts'；

【原因】同一个ip在短时间内产生太多(超过mysql数据库max_connection_errors的最大值)中断的数据库连接而导致的阻塞；

【解决】

#### 1、提高允许的max_connection_errors数量（治标不治本）：

 - ① 进入Mysql数据库查看max_connection_errors： `show variables like "max_connection_errors";`

 - ② 修改max_connection_errors的数量为1000： `set global max_connect_errors = 1000;`

  - ③ 修改 max_connections 的数量为1000 ：`set global max_connections = 1000;`

#### 2、使用 `mysql> flush-hosts;` 命令清理一下hosts文件；

  - ① 最简单的方法是root登录后，直接使用 `mysql> flush hosts;` 命令;


备注：其中端口号，用户名，密码都可以根据需要来添加和修改。
 
 **不过这些方法都是治标不治本的，本质原因是由于程序中创建了过多的mysql连接，通常情况下，程序开始的运行的时候建立与数据库的连接，运行期间进行数据库的一些增删改查操作，程序关闭的时候，断开与数据库的连接。**