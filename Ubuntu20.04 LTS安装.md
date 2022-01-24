# 1. 安装Ubuntu20.04 LTS

从[Ubuntu官网](https://ubuntu.com/download/desktop)下载ISO文件，并且使用U盘制作系统盘

![Ubuntu系统下载](https://cdn.jsdelivr.net/gh/SCUTCS-WWB/pic-bed@master//project/Ubuntu系统下载.png)

- 进入Boot界面，选择对应的USB介质。需要注意的是，更新选项**不要勾选**下载更新以及安装第三方软件

- 安装类型：选择清除整个磁盘并安装Ubuntu

# 2. 软件源更换

安装完系统后，首先要做的就是更换软件源，毕竟原来的默认软件源太慢了，这里使用的是阿里云开源镜像以及清华大学镜像

预先配置

```bash
sudo visudo
# 在ALL=(ALL:ALL) 后面加上 NOPASSWD: ALL
# ctrl+o 保存文件
sudo apt-get update
sudo apt-get install vim # 安装vim
```

软件源备份

```bash
# 备份原来的软件源
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
# 在/etc/apt/sources.list文件下添加以下条目
sudo vim /etc/apt/sources.list
```

软件源

```ini
#添加阿里源
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
#添加清华源
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse multiverse
```

更新源

```bash
sudo apt-get update
sudo apt-get upgrade
```

# 3. zsh安装配置

```bash
sudo apt-get install zsh
chsh -s /bin/zsh	# 设置默认shell
# 官方配置脚本
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# gitee镜像脚本
git clone https://gitee.com/mirrors/oh-my-zsh.git
cd oh-my-zsh
cd tools
./install.sh
```

- 安装autojump插件(不安装)

```bash
# autojump：自动跳转
git clone https://github.com/wting/autojump.git
cd autojump
python ./install.py
```

- 安装zsh-autosuggestions

```bash
# zsh-autosuggestions：自动补全
# github下载
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
# gitee下载
git clone https://gitee.com/pocmon/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

- 安装zsh-syntax-highlighting

```bash
# zsh0syntax-highlighting：语法高亮
# github下载
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
# gitee下载
git clone https://gitee.com/Annihilater/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

修改`~/.zshrc`

```ini
# plugins
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)
```

更新配置

```bash
source ~/.zshrc
```

# 4. 蓝灯安装配置

进入[蓝灯github下载](https://github.com/getlantern/download)，得到`lantern-installer.deb`

```
sudo dpkg -i lantern-installer.deb
```

# 5. 安装显卡驱动

[nvidia驱动程序官网](https://www.nvidia.cn/Download/index.aspx?lang=cn)安装nvidia驱动

```bash
# 安装lightdm桌面
sudo apt install lightdm
# 删除旧驱动
sudo apt-get purge nvidia*
# 禁用自带的 nouveau nvidia驱动
sudo gedit /etc/modprobe.d/blacklist.conf
# 在文件末尾写入
blacklist nouveau
options nouveau modeset=0
# 更新系统修改
sudo update-initramfs -u
# 重启
sudo reboot
# 验证nouveau是否已经禁用
lsmod | grep nouveau
# 将下载后的run文件放在初始目录位置
# ctrl+alt+f1到6其中一个进入命令行界面
# 关闭图形界面
sudo service lightdm stop
# 卸载系统中存在的显卡驱动
sudo apt-get remove nvidia-*
# 给文件权限
sudo chmod a+x NVIDIA-Linux-x86_64-xxx.run
# 运行run文件
sudo apt-get install gcc	# 预先安装gcc
sudo apt-get install make	#	预先安装make
sudo ./NVIDIA-Linux-x86_64-xxx.run -no-x-check -no-nouveau-check -no-opengl-files
# -no-x-check：安装驱动时关闭X服务  -no-nouveau-check：安装驱动时禁用nouveau   -no-opengl-files：只安装驱动文件，不安装OpenGL文件

# 1.The distribution-provided pre-install script failed! Are you sure you want to continue? “Yes”
# 2.Would you like to register the kernel module souces with DKMS? This will allow DKMS to automatically build a new module, if you install a different kernel later? “No”
# 3.Nvidia’s 32-bit compatibility libraries? “No”
# 4.Would you like to run the nvidia-xconfigutility to automatically update your x configuration so that the NVIDIA x driver will be used when you restart x? Any pre-existing x confile will be backed up. “Yes”

# 重启图形界面，安装成功后，在命令行输入
sudo service lightdm start
# Ctrl+Alt+F7返回图形界面
# 检测是否安装成功
nvidia-smi
nvidia-setting
```

设置显卡风扇转速

```bash
cd /etc/X11	# 查看是否有xorg.conf。如果没有，可用sudo nvidia-xconfig新建
# 设置多gpu模式
sudo nvidia-xconfig --enable-all-gpus
# 设置每个gpu风扇转速可以调节
sudo nvidia-xconfig --cool-bits=4
sudo reboot
```

设置显卡功耗

```bash
# enable persistance mode
sudo nvidia-smi -pm 1
# set power limit to 125W
sudo nvidia-smi -pl 280
# lock the gpu clock, 500 is generally safe, you can try higher values
sudo nvidia-smi -lgc 500,500
```



# 6. 安装向日葵远程控制

接下来就是安装向日葵远程控制软件，这里选择对应的**Ubuntu**版本

![向日葵下载](https://cdn.jsdelivr.net/gh/SCUTCS-WWB/pic-bed@master//project/向日葵下载.png)

```bash
sudo mv sunloginclient-11.0.0.36662-amd64.deb /usr/local/sunloginclient.deb
cd /usr/local
sudo dpkg -i sunloginclient.deb
```

# 7. CUDA+cuDNN安装

[CUDA官网](https://developer.nvidia.com/cuda-downloads)下载cuda文件

![cuda下载](https://cdn.jsdelivr.net/gh/SCUTCS-WWB/pic-bed@master//project/cuda下载.png)

```bash
sudo wget https://developer.download.nvidia.com/compute/cuda/11.3.1/local_installers/cuda_11.3.1_465.19.01_linux.run
sudo sh cuda_11.3.1_465.19.01_linux.run
```

在安装过程中，不要安装Driver，一定要安装`CUDA Toolkit`

```bash
# 设置环境变量 sudo vim ~/.zshrc
PATH=/usr/local/cuda-11.3/bin${PATH:+:${PATH}}
LD_LIBRARY_PATH=/usr/local/cuda-11.3/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
# 更新环境变量
source ~/.zshrc
# 验证
nvcc -V
```

---

[cuDNN官网](https://developer.nvidia.com/rdp/cudnn-download)选择与CUDA对应的版本，下载cuDNN

![cuDNN下载](https://cdn.jsdelivr.net/gh/SCUTCS-WWB/pic-bed@master//project/cuDNN下载.png)

```bash
# 下载cuDNN Library for Linux(x86_64)
# 解压
tar -zxvf xxx
# 覆盖文件
sudo cp cuda/include/cudnn.h /usr/local/cuda-11.3/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda-11.3/lib64
# 权限
sudo chmod a+r /usr/local/cuda-11.3/include/cudnn.h 
sudo chmod a+r /usr/local/cuda-11.3/lib64/libcudnn*
# 测试
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
```

# 8. 安装Anaconda

[清华源镜像](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)下载`Anaconda`镜像文件

```bash
# 安装anaconda
sudo sh Anaconda3-2021.05-Linux-x86_64.sh
# 安装位置填写/usr/local/anaconda3
# 修改环境变量
sudo vim ~/.zshrc
PATH=/usr/local/anaconda3/bin${PATH:+:${PATH}}	# 添加
```

创建并修改`.condarc`文件

```ini
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

# 9. 安装Pycharm

[PyCharm官网](https://www.jetbrains.com/pycharm/)下载`Community`安装包

```bash
# 解压
sudo tar -zxvf pycharm-community-2021.1.2.tar.gz
# 设置桌面图标
sudo gedit /usr/share/applications/Pycharm.desktop

[Desktop Entry]
Type=Application
Name=Pycharm
GenericName=Pycharm3
Comment=Pycharm3:The Python IDE
Exec=sh /home/xxx/pycharm-2018.3.2/bin/pycharm.sh
Icon=/home/xxx/pycharm-2018.3.2/bin/pycharm.png
Terminal=pycharm
Categories=Pycharm;

# 设置权限为可执行文件
# 右击图标，选择“允许启动”
```

# 10. 安装Pytorch

[Pytorch官网](https://pytorch.org/)下载对应的版本

![Pytorch安装](https://cdn.jsdelivr.net/gh/SCUTCS-WWB/pic-bed@master//project/Pytorch安装.png)

```bash
# conda
conda install pytorch torchvision torchaudio cudatoolkit=11.1
# pip
pip3 install torch==1.9.0+cu111 torchvision==0.10.0+cu111 torchaudio==0.9.0 -f https://download.pytorch.org/whl/torch_stable.html
```

安装验证测试

```python
# 环境测试
import torch

print(torch.__version__)               # PyTorch version
print(torch.version.cuda)              # Corresponding CUDA version
print(torch.backends.cudnn.version())  # Corresponding cuDNN version
print(torch.cuda.get_device_name(0))   # GPU type
print(torch.cuda.is_available())       # Check whether GPU support
```

# 11. 安装Ubuntu主题和图标（不推荐使用）

安装主题和图标

```bash
sudo add-apt-repository ppa:daniruiz/flat-remix
sudo apt update
sudo apt install flat-remix flat-remix-gnome flat-remix-gtk
sudo reboot	# 密码右下角选择该主题
```

安装插件

1. Dash to dock插件，用于去除Gnome原始的dock（太难用了，也不好看）
2. Hide to bar 插件，用于智能隐藏Gnome上面的bar，这样应用就能全屏了。如果需要 唤出bar，只要用鼠标快速滑到屏幕上边缘就可以了
3. Dynamic to bar插件，可以让bar变成半透明状态。

# 12. 安装git

包含`git`安装过程以及免密配置

```bash
# 安装git
sudo apt-get install git
# 免密(全局配置)
git config --global credential.helper store
```

# 13. ssh远程登录

查看SSH Server是否安装

```bash
dpkg -l|grep ssh
```

安装SSH Server 

```bash
sudo apt-get install openssh-server
```

查看是否安装成功&查看是否启动成功

```bash
dpkg -l|grep ssh
```

如果没有启动，可通过以下两种方式启动

```bash
# 方式1
sudo /etc/init.d/ssh start
# 方式2
sudo service ssh start
```

SSH Server相关配置

```bash
# 配置文件位置 /etc/ssh/sshd_config
# 可设置SSH Server端口，默认是22
# 重启SSH Server
sudo /etc/init.d/ssh stop
sudo /etc/init.d/ssh start
```

sakurafrp内网穿透

```bash
frpc -f userKey:channelID
```

# 14. 翻墙软件

- 首先安装`clash`软件

  clash下载地址：https://github.com/Dreamacro/clash

  系统信息可以通过`uname -a`查看

  ```bash
  wget https://github.com/Dreamacro/clash/releases/download/v1.8.0/clash-linux-amd64-v1.8.0.gz
  ```

  下载后改权限，解压，放到/opt/clash文件夹

- 下载配置信息`AgentNeo`

  ```bash
  wget -O config.yaml https://neofeed.co/files/HiLjecmFjG/clash.yml
  ```

  clash的配置文件放在`~/.config/clash/config.yaml`

  修改外部控制设置（external-controller）地址为：0.0.0.0:9990，使内外网都可以访问这个地址

- 设置系统代理

  ```bash
  sudo vim /etc/environment
  ```

  修改文件加入以下三行

  ```ini
  export http_proxy="http://127.0.0.1:7890"
  export https_proxy="http://127.0.0.1:7890"
  export no_proxy="localhost, 127.0.0.1"
  ```

  修改sudo文件

  ```bash
  sudo visudo
  ```

  加入下行

  ```ini
  Defaults env_keep+="http_proxy https_proxy no_proxy"
  ```

  重启

  ```bash
  reboot
  ```

- 设置部分程序代理

  有些程序不走系统代理，需要单独配置，下面以git为例

  ```bash
  git config --global http.proxy 'http://127.0.0.1:7890'
  ```

  shell最好也设一下，以zsh为例

  ```ini
  # .zshrc最后加入
  set proxy
  export http_proxy="http://127.0.0.1:7890"
  export https_proxy="http://127.0.0.1:7890"
  ```

  **设置外部控制ui：**

  ```bash
  git clone https://github.com/Dreamacro/clash-dashboard.git
  cd clash-dashboard
  git checkout -b gh-pages origin/gh-pages
  ```

  在~/.config/clash/config.yaml中设置好ui地址和访问密码

- 设置clash开机自启

  将配置文件移动到/etc

  ```bash
  sudo mv ~/.config/clash /etc
  ```

  添加启动信息

  ```bash
  sudo vim /etc/systemd/system/clash.service
  ```

  输入以下内容，clash -d的意思是指定配置文件路径，这里已经改成了/etc/clash

  ```bash
  [Unit]
  Description=clash daemon
  
  [Service]
  Type=simple
  User=root
  ExecStart=/opt/clash/clash -d /etc/clash/
  Restart=on-failure
  
  [Install]
  WantedBy=multi-user.target
  ```

  重新加载systemctl daemon

  ```bash
  sudo systemctl daemon-reload
  ```

  启动Clash

  ```bash
  sudo systemctl start clash.service
  ```

  设置Clash开机自启动

  ```bash
  sudo systemctl enable clash.service
  ```

  以下为Clash相关的管理命令

  ```bash
  启动Clash
  sudo systemctl start clash.service
  重启Clash
  sudo systemctl restart clash.service
  查看Clash运行状态
  sudo systemctl status clash.service
  ```

  **配置定时更新订阅：**

  先撸个脚本，别忘了设可执行权限

  ```ini
  #!/bin/bash
  
  # 设置clash路径
  clash_path="/opt/clash"
  
  # 停止clash
  systemctl stop clash.service
  
  # 取消代理
  unset https_proxy
  
  # 如果配置文件存在，备份后下载，如果不存在，直接下载
  if [ -e $clash_path/config.yaml ]; then
  	mv $clash_path/config.yaml $clash_path/configbackup.yaml
  	wget -O $clash_path/config.yaml "[你的订阅链接]"
  else
  	wget -O $clash_path/config.yaml "[你的订阅链接]"
  fi
  
  # 重启clash
  systemctl restart clash.service
  
  # 重设代理
  export https_proxy="http://127.0.0.1:7890"
  ```

  设置定时任务：

  ```bash
  sudo crontab -e
  ```

  填入以下内容

  ```bash
  //每月1号和15号的4点30分开始更新
  30 4 1,15 * * sh [脚本目录]/[脚本名称]
  ```

  重启crontab，使配置生效

  ```bash
  sudo systemctl restart cron.service
  ```

  查看代理是否正常工作：

  ```bash
  curl www.google.com
  ```
