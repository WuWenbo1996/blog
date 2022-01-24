>  因为最近有对windows系统进行远程登录的需求，对`密钥`以及`密码`登录两种方式进行了梳理

# SSH安装

1. 设置-应用-应用和功能-管理可选功能-添加功能
2. 安装OpenSSH服务器和客户端
3. 验证安装

```bash
(base) codewu@codeWudeMac-mini .ssh %ssh
usage: ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface]
           [-b bind_address] [-c cipher_spec] [-D [bind_address:]port]
           [-E log_file] [-e escape_char] [-F configfile] [-I pkcs11]
           [-i identity_file] [-J [user@]host[:port]] [-L address]
           [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port]
           [-Q query_option] [-R address] [-S ctl_path] [-W host:port]
           [-w local_tun[:remote_tun]] destination [command]
```

4. 开启SSH服务

```bash
net start sshd
OpenSSH SSH Server 服务正在启动
OpenSSH SSH Server 服务已经成功启动
net stop sshd
OpenSSH SSH Server 服务已成功停止
```

5. 远程登录

```bash
# ssh 用户名@用户ip
ssh username@127.0.0.1
```

# 密码登录

这种方式需要账户设置有密码，尝试过使用空密码进行登录，但是登录的时候一直提示登录失败，设置了密码后才成功

```ini
PermitEmptyPasswords yes
```

设置了这项为`yes`，但是依然无用

在完成ssh安装后，直接密码登录即可

# 密钥登录

使用密码登录其实不太安全，容易出现密码泄漏等严重安全问题。一个比较好的解决方法是使用密钥进行系统登录。

1. 在登录设备端生成一对公钥密钥，这里默认不添加`passphrase`

```bash
ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/user/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/user/.ssh/id_rsa.
Your public key has been saved in /home/user/.ssh/id_rsa.pub.
The key fingerprint is:
2e:34:7c:3a:be:e4:3b:93:2f:1d:32:4f:2d:fe:13:a1 user@master 
The key's randomart image is:
+--[RSA 2048]----+|
|				  |
|   			  |
|				  |
|	  .    .      |
|      + So .     |
|     .o=E o      |
|      =O.o .     |
|	  +=o+ .      |
|      =B....     |
+-----------------+
```

2. 将设备端生成的`id_rsa.pub`存放在服务器端`C:\Users\your_userName\.ssh\authorized_keys`内，如果没有`.ssh`文件夹，可通过`ssh-keygen -t rsa`生成
3. 修改服务器端的sshd_config文件（**重点**）

```ini
确保以下3条没有被注释
PubkeyAuthentication yes
AuthorizedKeysFile	.ssh/authorized_keys
PasswordAuthentication no

确保以下2条有注释掉
#Match Group administrators
#       AuthorizedKeysFile __PROGRAMDATA__/ssh/administrators_authorized_keys
```

4. 重启sshd服务

5. 设备端在连接服务器时，需要将本地的`id_rsa`作为`Private key`制作`Key`，添加到`Keychain`中。在登录服务器时，则无需再输入密码

   ![截屏2021-08-20 下午9.13.19](https://cdn.jsdelivr.net/gh/SCUTCS-WWB/pic-bed@master//settings/ssh密钥设置.png)

# Mac设置

1. 本地生成一对公钥密钥

```bash
ssh-keygen -t rsa
```

存放位置在`/Users/user/.ssh/`，完成后生成`id_rsa`和`id_rsa.pub`两个文件

2. 设置权限

```bash
chmod 600 /Users/user/.ssh/id_rsa.pub
```

3. 将公钥放到远程服务器上

```bash
ssh-copy-id user@ip.com
```

4. 配置保存ssh服务器信息，修改`~/.ssh/config`

```ini
#自定义主机名称，写上好记的就行了。
Host goodjob
#SSH连接的地址，IP或者是域名
HostName ip.cn
#SSH远程登录的名称
User user
#SSH的端口默认是22
Port 22
#指向私钥的位置,这里写你自己的地址。
IdentityFile /Users/user/.ssh/id_rsa
```

5. 登录ssh

```bash
ssh goodjob
```

# 实验室ssh控制

首先退出开机自启的代理，防止`connection refused`

```bash
unset http_proxy
unset ftp_proxy
unset https_proxy
```

开启隧道

```bash
sudo frpc -f veop0rd69jhlqz1b:2075200
```

windows后台运行程序

```bash
@echo off
if "%1" == "h" goto begin 
start mshta vbscript:createobject("wscript.shell").run("%~nx0 h",0)(window.close)&&exit 
:begin
```

