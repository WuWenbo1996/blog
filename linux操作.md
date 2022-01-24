## 禁止ip访问

在服务器上进行如下命令操作进行规则设置即可

```bash
#iptables -A INPUT -s ip段/网络位数 -j DROP
#例如：禁止121.40.224.197/24网段访问服务器，直接在服务器上用命令就可以实现
sudo iptables -A INPUT -s 121.40.224.197/24 -j DROP
#/etc/rc.d/init.d/iptables save （保存规则）
#service iptables restart （重启iptables服务以便升效）
```

# Ubuntu下iptables打开端口并重启后自动生效

```bash
sudo apt-get install iptables-persistent
sudo netfilter-persistent save
sudo netfilter-persistent reload
```

限制容器向外请求，iptables限制转发。这样限制后，容器的网卡还是会有向外的请求但是被本机转发限制了，流量不会发送到被限制的外部ip。

```bash
iptables -I FORWARD -p tcp -m iprange --dst-range 被限制的外部ip -j DROP
```

# 进程操作

只有第9种信号(SIGKILL)才可以无条件终止进程，其他信号进程都有权利忽略。 下面是常用的信号：

HUP    1    终端断线

INT     2    中断（同 Ctrl + C）

QUIT    3    退出（同 Ctrl + \）

TERM   15    终止

KILL    9    强制终止

CONT   18    继续（与STOP相反， fg/bg命令）

STOP    19    暂停（同 Ctrl + Z）
