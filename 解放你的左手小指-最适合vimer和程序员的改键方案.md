程序员用的最多的键想必是ctrl CV了:), 我从网瘾少年到打工人的过程中, 对计算机的了解在不断加深, 对这几个键的依赖却一直没有减少, 接触vim后, 还一直在探索最佳的返回normal模式方案.  
而如今我已经找到且用了很久了, 趁着重装系统特此分享一波

打游戏以及刚开始编程的那些时间, 因为ctrl键奇葩的位置导致要用到的时候左手总是要扭到一个及其别扭的位置, 心中不爽却无奈, 但没有多想应该怎么解决, 直到开始系统学习vim之后才花时间去了解, 在那之前, 不改键的情况下返回normal模式的几种方案我也基本都尝试过:

1. `ctrl + [` 需要两只手配合, 不够简练
2. `ctrl + c` 大概一年的时间都在使用, 但是语义是发送SIGINT, 少数情况下可能会导致预期意外的结果, 不完美
3. `esc` 直接使用距离太远

最后捣鼓Arch的时候发现了这个宝藏仓库, 效果是将capslock改键, 单独按是esc, 组合键按是ctrl:


<https://gitlab.com/interception/linux/plugins/caps2esc>

里面的这张古董键盘的图片也让我恍然大悟为什么ctrl这个常用的键会在那么奇葩的位置:
![caps2esc](https://pic3.zhimg.com/80/8f28311a26dcccca7323237fea9e532c_1440w.jpeg)

也就是说capslock这个键就是万恶之源, 所以要么改键要么把这个键扣掉吧:)

了解了一下很多年前就有类似的解决方案, 但很多不可用了, 这个caps2esc也有些bug, 有时需要ctrl加鼠标的组合, 并不支持这个操作, 但是interception tools这个仓库另外一个插件完美解决了这个问题:

<https://gitlab.com/interception/linux/plugins/dual-function-keys>

接下来记录我配置的过程

Arch Linux直接AUR安装:

`yay -S interception-dual-function-keys`

其他发行版参考仓库地址编译安装

安装后, 只需简单几个配置:

编辑改键配置的yaml文件, 我放在`/etc/interception/capslock2ctrlesc.yaml`:
```
TIMING:
    TAP_MILLISEC: 200 # 单击时长判定
    DOUBLE_TAP_MILLISEC: 150 # 可选配置

MAPPINGS:
    - KEY: KEY_CAPSLOCK
      TAP: KEY_ESC
      HOLD: KEY_LEFTCTRL
```

然后是任务配置yaml, 文件位置固定在`/etc/interception/udevmon.d/dual-function-keys.yaml`:
```
- JOB: "intercept -g $DEVNODE | dual-function-keys -c /etc/interception/capslock2ctrlesc.yaml | uinput -d $DEVNODE"
  DEVICE:
    NAME: "AT Translated Set 2 keyboard" # 任务作用的设备
- JOB: "intercept -g $DEVNODE | dual-function-keys -c /etc/interception/capslock2ctrlesc.yaml | uinput -d $DEVNODE"
  DEVICE:
    NAME: "FILCO Bluetooth Keyboard"
```

如果你想把改键配置放在其他路径, 记得修改上面配置相同的路径  
`NAME`参数的设备名称通过这个命令确定`sudo uinput -p -d /dev/input/by-id/X`, `X`是路径下类似keyboard名称的设备, 如果这个路径下没有, 则需要在`/dev/input`目录下慢慢找了

配置写好后启动进程即可
```
$ systemctl enable --now udevmon.service
```
至此大功告成, capslock点击是esc, 长按是ctrl, 因为是以systemctl的方式启动, 也不会像kde之类桌面环境自带的改键一样在TTY下不生效

另外windows下也有类似的解决方案, 用一个AHK脚本即可:
[CapsLockCtrlEscape.ahk](https://gist.github.com/sedm0784/4443120)

vim用户用这套改键方案是最爽的, 当然一开始可能有点不喜欢, 稍微适应一下就可以了, 本人大概一两天就习惯了停不下来:)
