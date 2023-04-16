# MI9 Pixel Experience &#43; Magisk


## 前言

MI 9 最近突发恶疾，每天都会突然黑屏，开机卡 MIUI 界面，LOG 还变黄，经常需要多次尝试不同姿势重启才能开机，就把已经用了万年的 MIUI 10 更新到了 MIUI 12，但遗憾的是更新了以后故障依旧。百度了一下，可能是烧了什么芯片或者 CPU 电源管理芯片虚焊。遂决定喜迎新机，毕竟 MI 9 陪伴我也有 4 年了。既然 MI 9 闲下来了，准备解锁刷个 ROM。先玩着，等故障发生比较频繁了去售后看看能不能修。

在大佬指点和百度下选择了 Pixel Experience 系统，顺便 root 了，装上 Magisk 和  LSPosed。

本文整理记录一下大致过程，只供参考，如果对您有帮助那就太好了。

## 文件

```powershell
├─Apk
│  │  FoxMMM-v1.1.0-default-arm64-v8a-release.apk
│  │  MT2.13.3-CoolApk-21048-o_1grsjcuebsig1bdl1g4jvmq1cgp13-uid-394584.apk
│  │
│  ├─Magisk
│  │  │  Magisk-v26.1.apk
│  │  │
│  │  └─Mode
│  │          CustomPinyinDictionary_Gboard_Magisk_20230413.zip
│  │          Font_Manager-v5.7.6-127.zip
│  │          LSPosed-v1.8.6-6712-zygisk-release.zip
│  │          Shamiko-v0.7-161-release.zip
│  │
│  └─Sufboard
│          mobile-arm64-v8a-release.apk
│          smallstrawberry.conf
│
├─Image
│  │  cepheus_eea_global_images_V12.5.2.0.RFAEUXM_20211112.0000.00_11.0_eea_655e654e8a.tgz	# EU版MIUI
│  │  cepheus_images_V12.5.6.0.RFACNXM_20211125.0000.00_11.0_cn_8d261e6804.tgz	# 国行版MIUI
│  │  PixelExperience_cepheus-13.0-20221127-0218-OFFICIAL.zip	# PixelExperience
│  │  xiaomi.eu_multi_MI9_V12.5.6.0.RFACNXM_v12-11.zip	# EU版MIUI
│  │
│  └─Recovery
│          PixelExperience_cepheus-13.0-20221127-0218-OFFICIAL.img	# PixelExperience配套rec
│          twrp-3.5.2_9-0-cepheus.img	# 第三方rec
│
└─Tools
        MiFlash2020-3-14-0.rar	# XIAOMI 线刷工具
        miflash_unlock-6.5.406.31.zip	# XIAOMI 解锁工具（这里面自带 Fastboot）
        platform-tools_r34.0.1-windows.zip	# ADB
```



## 过程

### 解锁 Bootloader

小米手机解锁需要小米账号，和小米官方的解锁工具。

&gt; [MIUI 解锁工具](https://www.miui.com/unlock/download.html)

手机需要登录小米账号并在有 SIM 卡的情况下使用数据流量在“**设置 -&gt; 开发者选项 -&gt; 设备解锁状态**”中绑定设备。

**注意事项：**

1. 去 “**我的设备 -&gt; 全部参数**” 重复点击 “**MIUI 版本**” 开启开发者模式。
2. MI 9 进入 Fastboost 方式：关机后，同时按住电源键和音量键下直到出现 “**MI**” 标志。
3. 如果提示因为账号问题无法解锁，只能等待限制时间到期，或者更换账号。
4. 解锁完毕后，及时退出手机的小米账号和谷歌账号。

### 重新刷入国行 MIUI

中间我直接清空了所有分区装上了 Pixel Experience 发现有一些问题，百度了一下决定先重新刷个 MIUI，开始刷了 EU 版 MIUI 好多系统软件没有懒得搞其他的了，于是又重新刷了国行包。

去小米官方网站下载线刷工具在以下网站查找 MI 9（需要代理到国外访问）

&gt; https://c.mi.com/global/miuidownload/index

找到“**MI 9 -&gt; Flashing Guide -&gt; Fastboot Update -&gt; Download MIUI ROM Flashing Tool(Size: 66.7 MB)**”

&gt; [MIUI ROM Flashing Tool 下载](https://cdn.alsgp0.fds.api.mi-img.com/micomm/MiFlash2020-3-14-0.rar)

在 XiaomiROM 直接搜索小米 9 (cepheus)下载国行线刷包。

&gt; [XiaomiROM 网站](https://xiaomirom.com/)

开始刷机，提前退出小米账号和谷歌账号。

&gt;  运行 XiaoMiFlash.exe -&gt; 手机进入 Fastboost 模式 -&gt; 手机插入电脑- &gt; 点击Driver -&gt; 点击安装（默认把所有需要的驱动装入电脑）
&gt;
&gt; 回到 XiaoMiFlash 首页 -&gt; 点击选择 -&gt; 选择解压的文件夹 -&gt; 点击加载设备 -&gt; 点击加载出的设备 -&gt; 点击右下角全部删除 -&gt; 刷机

一定要选择“**全部删除**”，默认是删除加锁。

稍作等待，刷完后将小米的系统软件全部安装更新一遍，更新完毕后退出小米账号即可。

### 刷入 Pixel Experience

到 Pixel Experience 官网，点击 devices 按照机型查找相关 ROM。

&gt; [Pixel Experience 官网](https://get.pixelexperience.org/)
&gt;
&gt; [Pixel Experience 官网 - Xiaomi Mi 9](https://get.pixelexperience.org/cepheus)

选择最新的版本（13）将镜像和 Recovery 都下载下来。

#### 使用 Fastboot 安装 Pixel Experience Recovery

Pixel Experience Recovery 是第三方（非小米）的恢复程序。在这里使用这个配套刷入系统。

手机进入 Fastboot 模式（电源键 &#43; 音量下）并将手机与电脑连接，将 Pixel Experience Recovery 放到和 fastboot 工具一个目录下，输入命令：

```powershell
.\fastboot.exe flash recovery  PixelExperience_cepheus-13.0-20221127-0218-OFFICIAL.img
```

fastboot 工具在 XIAOMI 的解锁工具里有，我直接在解锁工具的目录里用的。

#### 从 Recovery 安装Pixel Experience

&gt;  MI 9 进入 Recovery 方式：关机后，同时按住电源键和音量键上直到出现 “MI” 标志。

进入 Recovery 点击 `Factory Reset` ，然后点击 `Format data/` 格式化数据分区。

格式化完成后点击左上角返回按钮，返回到主菜单，点击 `Apply from ADB` 并将手机与电脑连接。

电脑下载 ADB 工具。

&gt; Android SDK Platform-Tools 是 Android SDK 的一个组件。它包含与 Android 平台进行交互的工具，主要是 [`adb`](https://developer.android.google.cn/studio/command-line/adb?hl=zh-cn) 和 [`fastboot`](https://android.googlesource.com/platform/system/core/&#43;/master/fastboot/#fastboot)。
&gt;
&gt; [ANDROID STUDIO SDK 平台工具](https://developer.android.google.cn/studio/releases/platform-tools?hl=zh-cn)

在电脑上，将 Pixel Experience 包放到ADB 工具的目录下，使用命令：

```powershell
./Adb sideload PixelExperience_cepheus-13.0-20221127-0218-OFFICIAL.zip
```

&gt; **提示：**通常情况下，adb 会报告`Total xfer: 1.00x`，但在某些情况下，即使进程成功，输出也会停止在 47% 并报告`Total xfer: 0.98x`or `adb: failed to read command: Success`。在某些情况下，它会报告`adb: failed to read command: No error`or`adb: failed to read command: Undefined error: 0`也可以。

然后解决一下开机启用页面设置，连接 WIFI 无法跳过的问题，输入命令修改 FRP 标志位：

```powershell
./Adb shell
dd if=/dev/zero of=/dev/block/bootdevice/by-name/frp
```

等待完成后，手机返回到主菜单，点击 `Reboot system now` 即可。

重启之后，成功进入系统开始设置。

### 安装 Magisk

先到 GitHub 下载最新的 **Magisk**（记录时最新版本为 v26.1）。

&gt; [Magisk - GitHub](https://github.com/topjohnwu/Magisk)

手机安装一下，完成后进去，`Ramdisk` 已经是 `Yes` 了，接下来修补一下 **boot.img**。

boot.img 从 **PixelExperience_cepheus-13.0-20221127-0218-OFFICIAL.zip**，解压出来即可。

将 boot.img  放到手机中，在 **Magisk** 的主页面的 `Magisk`（也就是第一个）选择 `Install`，方式为 选择并修补一个文件，选择刚刚放进去的 boot.img 选中，完成后重启。

重启后就可以设置 Magisk 并继续安装 LSPosed 等等了。

## 写在末尾

过程基本就是这样，接下来看看有什么模块比较好用有意思来试一试了。

## 参考链接

&gt;https://zhuanlan.zhihu.com/p/408114647
&gt;
&gt;https://wiki.pixelexperience.org/devices/cepheus/install/
&gt;
&gt;https://topjohnwu.github.io/Magisk/install.html

## 附录

&gt; https://github.com/LSPosed/LSPosed
&gt;
&gt; https://github.com/wuhgit/CustomPinyinDictionary
&gt;
&gt; https://github.com/Fox2Code/FoxMagiskModuleManager
&gt;
&gt; https://github.com/DJ131452DJ/Shamiko_for_Magisk


---

> 作者: 度绛河  
> URL: https://purplered-river.github.io/mi9%E5%88%B7%E6%9C%BA%E6%8C%87%E5%8C%97/  

