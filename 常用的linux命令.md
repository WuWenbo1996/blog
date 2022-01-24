## Linux

1. 了解Linux的基本概念，会敲常用命令来应对工作
2. 能理解Linux，尤其是其内核的设计思想；并且将其思想应用到系统的架构设计中（架构师；底层开发）
3. 熟知linux的使用思想和细节；推陈出新，自主创造新的系统

## 常用命令

- pwd：查看当前目录

- ifconfig：查看网络信息

- free -h： 查看服务器内存

- df -lh：查看磁盘空间

- mkdir xxx：新建目录

- cd xxx：进入目录

- git clone xxx：拉取代码

- ls：查看当前目录文件

- du -sh *：查看项目占用空间

- cat xxx：查看文件内容

- uname -a：查看系统版本

- java -version：查看java版本号

- which java：查看安装路径

- wget xxx：下载软件压缩包

- tar -zxvf xxx：解压

- mvn --help：查看帮助手册

- find name `'*.jar*'`：查找文件

- cp xxx xxx：复制文件

- mv xxx xxx：移动文件；改名

- java -jar code-nav.jar：启动项目

- nohup xxx &：后台启动程序

  `nohup python -u sock.py > nohup.log 2>&1 &` -> 存储日志

  `tail -f nohup.log` -> 实时查看日志内容

  `tail -n 10 nohup.log` -> 查看最近10条日志内容

- jobs：查看后台任务

- ps -ef：查看系统进程情况

- ps -ef|grep 'java'：通过管道符，配合grep命令，只筛选出java进程

- netstat -ntlp：查看占用端口

- curl localhost:8082/dog：访问

- tail -n 10 error.log：查看最新10行

- sz xxx：下载到本地

- vim xxx：使用`vim`对文件内容进行修改

- kill -9 %1：杀死进程

  kill %1这条命令表示杀死一个后台程序，这个后台程序的“工作号码（jobnumber）”是1号。

  这条命令往往是用在输入如下命令之后：jobs -l

  jobs用来查看目前的后台工作状态，显示结果里面最前面的数字号就是对应进程的jobnumber，然后就可以用kill %jobnumber的格式来杀死jobnumber对应的后台工作。

- top：查看系统进程状态

- vim start.sh：编写启动shell脚本文件

  ```shell
  mvn install
  nohup java -jar code-nav.jar &
  echo "success"
  ```

  如果出现`Permission denied`，说明文件没有权限，`chmod a+x start.sh`给文件加上可执行权限

- history：查看历史执行的命令

- rm -rf /*：删除所有内容

- linux后台自动执行命令nohup与日志查看

  **1. 后台自动执行**

  ```bash
  nohup [命令] &
  # nohup（no hang up）：可以让程序不挂断执行。
  # & ：可以让程序在后台执行。
  ```

  **2. 日志输出**

  ```bash
  nohup [命令] > nohup.log 2>&1 &
  # “> nohup.log”： 表示将日志输出到nohup.log文件上。
  # 2>&1：表示将正确日志、错误日志都输出到正确日志指定的文件（nohup.log文件）上。
  # 对于运行python文件，需要python -u xxx.py
  # python中标准错误（std.err）和标准输出(std.out)的输出规则（标准输出默认需要缓存后再输出到屏幕，而标准错误则直接打印到屏幕）。print()调用stdout实现输出。-u(unbuffered)参数强制标准输出不缓存。
  ```

