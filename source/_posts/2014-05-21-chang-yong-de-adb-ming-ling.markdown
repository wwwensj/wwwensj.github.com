---
layout: post
title: "常用的 ADB 命令"
date: 2014-05-21 23:46:05 +0800
comments: true
categories: 脚本&工具
---
最近在搞 Andorid 开发，在调用的过程中，经常会用到 `ADB`，这里简单做个此命令使用频率较高的用法。

`ADB` 全称是 **Android Debug Bridge**, 是 Android SDK 里的一个工具, 用它可以直接操作管理 Android 模拟器或者 Andriod 设备。

它的主要功能有:

> * 运行设备的 shell 命令行
> * 管理模拟器或设备的端口映射
> * 计算机和设备之间上传/下载文件
> * 将本地 apk 软件安装至模拟器或 Android 设备

------

## 常用的 ADB 命令

*  1、**adb help** 显示帮助信息

*  2、**adb version** 显示 ADB 命令版本号

*  3、**adb devices** 查看当前连接的设备,包括真机和模拟器

*  4、**adb start-server** 启动计算机 adb 服务进程，也可使用 adb devices 命令时自动开启

*  5、**adb kill-server** 关闭计算机 adb 服务进程，有时在正在使用的 adb，然后想删除 adb，那这时你得先关闭进程才了删除，就要用到它了

*  6、**adb install [-r] [-s]** 安装软件

	**－r**强制安装（在某些情况下可以已有些应用程序在运行或不可写，可加上此参数强制安装）

	**－s** 将 apk 文件安装在 SD-Card

*  7、**adb uninstall [-k] <软件名>** 卸载软件， -k 参数表示为卸载软件但是保留配置和缓存文件

*  8、**adb push <本地路径> <远程路径>** 从电脑上发送文件到设备

	例：adb push recovery.img /sdcard/recovery.img，将本地目录中的 recovery.img 文件传送手机的 SD 卡中并取同样的文件名

*  9、**adb pull <远程路径> <本地路径>** 从设备上下载文件到电脑

*  10、**adb reboot [bootloader|recovery]** 重启设备

	1）**adb reboot** 直接重启设备回到使用界面 ；

	2）**adb reboot-bootloader** 或 **adb reboot bootloader** 重启设备到 bootloader 引导模式：

	3）**adb reboot recovery**，重启到 **recovery** 刷机模式

*  11、**adb get-state** 返回设备状态，有三种结果：关机，引导模式，设备在线

*  12、**adb get-serialno** 返回设备序列号SN值

*  13、**adb remount** 获取设备的 ROOT 权限

	通过这个命令就可以获取设备的 ROOT 权限一样的通 adb 操作 /system 等目录的，如 adb push xx.app /system/app 即可将 app 应用直接放入系统目录，这个操作必须设备已解锁并 ROOT 过。
	
*  14、**adb shell pm clear <包名>** 清除应用数据，跟在设备上打开设置--应用--清除数据 是一样的效果。

------

[文章参考: http://www.izhangheng.com/android-debug-bridge][1]

[1]: http://www.izhangheng.com/android-debug-bridge
