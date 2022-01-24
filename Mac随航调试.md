# Mac系统随航调试

闲来无事，想到未来可能需要出差，Mac Mini在工作现场无法找到屏幕，所以尝试使用Mac Mini随航功能，将iPad Pro作为应急使用的屏幕

## 初始步骤【简约版】

1. 打开**Mac**自带的脚本编辑器，复制以下代码

```bash
tell application "System Preferences"
	
	activate
	delay 2
	set the current pane to pane id "com.apple.preference.sidecar"
	delay 2
	
	tell application "System Events"
		tell application process "System Preferences"
			tell window "随航"
				click menu button "选择设备"
				click menu item "403 Forbidden" of menu "选择设备" of menu button "选择设备" of window "随航" of application process "System Preferences" of application "System Events"
			end tell --403 Forbidden应该修改为你自己的iPad的名称
		end tell
	end tell
	
end tell
```

上述是来自[知乎大佬写的一段代码](https://zhuanlan.zhihu.com/p/328476489)（大佬太强了）

里面还包含错误提示以及开机自启的改进代码，可根据自己需求取用

2. 存储为应用程序文件

![截屏2021-08-07 下午9.26.07](https://cdn.jsdelivr.net/gh/SCUTCS-WWB/pic-bed@master//settings/MacMini随航脚本.png)

3. 设置权限

允许应用程序使用**辅助功能**

![截屏2021-08-07 下午9.28.25](https://cdn.jsdelivr.net/gh/SCUTCS-WWB/pic-bed@master//settings/MacMini随航程序权限.png)

到此为止，已经可以直接使用app进行随航啦～～～

## 使用ssh控制Mac Mini启动随航【进阶版】

考虑到网上现有的代码使用了开机自启功能，个人认为使用体验可能不佳（也有可能只有我这么觉得哈哈哈～～）。第一点担心在不使用ipad时，Mac Mini开机也会唤醒ipad，使用方式不太优美；第二点在于如果随航不慎断开，在没有额外显示器的情况下，无法重联。因此，想到了使用ssh控制解决这两个问题，做到随时随地都能让Mac Mini打开随航。以下对具体步骤进行说明。

1. 进入系统偏好设置

![截屏2021-08-07 下午9.38.19](https://cdn.jsdelivr.net/gh/SCUTCS-WWB/pic-bed@master//settings/进入共享.png)

2. 打开**远程登录权限**，并允许远程用户对磁盘进行完全访问

![截屏2021-08-07 下午11.51.41](https://cdn.jsdelivr.net/gh/SCUTCS-WWB/pic-bed@master//settings/远程登录权限.png)

3. 在快捷指令中设置好Mac Mini对应的ip地址和端口（默认是22），以及用户名和密码

![IMG_0002](https://cdn.jsdelivr.net/gh/SCUTCS-WWB/pic-bed@master//settings/ssh设置.PNG)

3. 输入SSH运行脚本命令

```bash
osacript ~/your document position 
-- 使用osacript命令，运行Mac Mini上的app文件
```

完成しました！现在就能使用快捷指令启用ssh打开随航啦。在b站看到有个大佬使用语音打开快捷指令使用随航功能，感觉很酷，后面感觉也可以做下，（就是有点社恐，在人多的地方使用语音控制有种公开处刑的感觉。。。

## 在Mac Mini上设置内网穿透【终极版】

虽然能用ssh登录了，但如果要带Mac Mini出门，连到了别的网络里，那就不知道它ip变成什么样了。为了今后能有一个固定的ip和端口给ssh登录使用。这里使用了内网穿透，将Mac Mini的网络地址固定下来。在这里我使用了**sakuraFrp**（一看首页就知道老二次元了），连接速度可以，主要是每天可以签到领流量（抽奖形式，最近每天都能领到3GB），纯白嫖。下面对部署过程进行介绍，跟着官网走就行了。

- https://doc.natfrp.com/#/frpc/usage/macos【Mac OS frpc安装】
- https://doc.natfrp.com/#/frpc/manual?id=%e6%99%ae%e9%80%9a%e7%94%a8%e6%88%b7【启动内网穿透】

![截屏2021-08-07 下午10.07.45](https://cdn.jsdelivr.net/gh/SCUTCS-WWB/pic-bed@master//settings/内网穿透.png)

这时候把快捷指令里的ip和端口改成终端内对应的地址就行了

最后，还需要设置内网穿透开机自启。

![截屏2021-08-07 下午10.12.22](https://cdn.jsdelivr.net/gh/SCUTCS-WWB/pic-bed@master//settings/内网穿透开机自启.png)

这样就大功告成啦。

---

最近更新了Mac最新版本`Monterey`，发现设置中已经取消“随航”，上述步骤失效，因此建议不要升级系统。不知道后面Apple会不会修复～