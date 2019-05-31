## Windows下 nodejs 进程的查看、删除

1.准备工作
任务管理器 以管理员身份 运行cmd

2.查看指定端口号下的进程 ：netstat -ano|findstr "端口号"
例子：3000端口

```
netstat -ano|findstr "3000"
```

3.根据PID获取进程名称 ：tasklist|findstr PID

```
tasklist |findstr 5948
```

4.根据进程名称结束进程 TASKKILL 具体参数查看帮助文件

```
taskkill /f /t /im node.exe
```

