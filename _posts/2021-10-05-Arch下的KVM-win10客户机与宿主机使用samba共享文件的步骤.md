---
layout: post
title:  "Arch下的KVM-win10客户机与宿主机使用samba共享文件的步骤"
date:   2021-10-05 16:18:01 +0800
categories: jekyll update
---
**背景**：Arch Linux下的各种国产必备软件（微信，qq，再加上打工人需要的钉钉，企业微信之流）确实都有大佬打包的deepin-wine版本去用，无奈工作时需要使用的一些功能，语音会议，屏幕共享等要么还是不稳定，要么直接不能用，影响到了办公，所以决定还是用KVM做个win10虚拟机去使用这些软件了，使用过程中宿主机与客户机的剪切板共享可以传输字符，屏幕截图，却传输不了文件。稍微了解了一下用samba共享文件是最通用的解决方案了，这里记录一下配置过程。

参考[Arch Wiki 上的Samba配置说明](https://wiki.archlinux.org/title/Samba_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))：

安装samba：

`yay -S samba`

创建配置文件，我用nvim将[默认配置](https://git.samba.org/samba.git/?p=samba.git;a=blob_plain;f=examples/smb.conf.default;hb=HEAD)写到这个路径：

`nvim /etc/samba/smb.conf`

创建samba 日志文件夹：

`mkdir -p /usr/local/samba/var/`

添加samba 访问用户并设置密码，smb_user为你自己设置的用户名：

`smbpasswd -a smb_user`

`smb.conf`中配置你想要共享的文件夹，我将用户目录共享出去了：
```
[homes] # 文件夹名称
   comment = Home Directories # 注释
   path = /home/smb_user/ # 共享的路径
   browseable = yes # 是否显示文件夹
   writable = no # 是否可写
```

之后启动服务：

`$ systemctl enable --now smb.service nmb.service`

win10客户机下点网络，输入宿主机的ip即可访问（这里是最坑的地方，默认不会在网络里显示），第一次访问输入上面设置的用户名和密码即可：
![共享文件夹](https://pic3.zhimg.com/80/v2-d8ccbf288cede67e3760ad59c7c315e5_1440w.png)

之后使用就跟win10下的共享文件夹一样了，最后日常吐槽一下国内软件垃圾的linux生态:)
