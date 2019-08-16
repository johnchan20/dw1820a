

### how通update clover
in hackintools

[latest clover](https://sourceforge.net/projects/cloverefiboot/files/Bootable_ISO/)


### need to test today 2019/8/16
## test the 10.14.5 HDMI1.4 destrop
>and I get the full range of resolutions!
>some one tested failed in mobile notbook
```
no "Inject Intel"
empty ig-platform-id;
simply install the latest Lilu and Whatevergreen to /L/E
only FakeSMC in other/
only use hdmi
```
```
Connector Type: HDMI
Device Type: Digital Television
1920 x 1080 (60p Hz)
1920 x 1080 (30p Hz)
1920 x 1080 (24p Hz)
1920 x 1080 (50i Hz)
1920 x 1080 (60i Hz)
1920 x 1080 (50p Hz)
1920 x 1080 (25p Hz)
3840 x 2160 (24p Hz)
3840 x 2160 (30p Hz)
3840 x 2160 (25p Hz)
4096 x 2160 (24p Hz)
```
[to find lilu or whatevergreen is load](https://www.tonymacx86.com/threads/how-can-i-tell-if-lilu-and-whatevergreen-are-active-loaded.262385/)

>only loaded and started 
```
 "About This Mac" > "Overview" > "System Report..." > "Software" > "Extensions"
```

>use IOreg  all loaded include loaded but not started 
```
Look in ioreg. You should find both Lilu and WhateverGreen under the IOResources node.
```



## for hd4600 hdmi 4k 60HZ for lower version macos such as under 10.13.5 and under 10.14.2

[hd4600_4k_60HZ](https://www.reddit.com/r/hackintosh/comments/3zt8ti/4k_uhd_with_hd_4600_displayport/)
[hd4600_4k](https://www.tonymacx86.com/threads/solved-4k-display-no-4k-resolution-available.196942/)


4K UHD with HD 4600 DisplayPort

>After some tinkering with toleda's framebuffer patch script and a config file with framebuffer edits in his >audio_CloverHDMI repository on github, I finally got it working. I fine-tuned the patches I would set in Clover >with Piker-Alpha's AppleIntelFramebufferAzul.sh.

This post really helped me get on track.

>My BIOS settings regarding IGFX are as follows.
 
```
Init Display is set to PCIe 1, 
memory allocations is set to 64MB,
DVMT is set to MAX.
```

>Here is part of the KernelAndKextPatches/KextsToPatch section of my config.plist:

```
<dict>
    <key>Name</key>
    <string>AppleIntelFramebufferAzul</string>
    <key>Find</key>
    <data>AwAiDQADAwMAAAACAAAwAQ==</data>
    <key>Replace</key>
    <data>AwAiDQADAwMAAAAEAAAgAg==</data>
    <key>Comment</key>
    <string>Increse BIOS-allocated memory to 64MB and framebuffer size to 34MB</string>
</dict>
```
```
<dict>
    <key>Name</key>
    <string>AppleIntelFramebufferAzul</string>
    <key>Find</key>
    <data>AQUJAAAEAACHAAAA</data>
    <key>Replace</key>
    <data>AQUJAAAIAAAGAAAA</data>
    <key>Comment</key>
    <string>10.10-Azul-Port_0x5-DP2HDMI</string>
</dict>
```
```
<dict>
    <key>Name</key>
    <string>AppleIntelFramebufferAzul</string>
    <key>Find</key>
    <data>AgQKAAAEAACHAAAA</data>
    <key>Replace</key>
    <data>AgQKAAACAAAGAAAA</data>
    <key>Comment</key>
    <string>10.10-Azul-Port_0x6-DP2DVI</string>
</dict>
```
```
<dict>
    <key>Name</key>
    <string>AppleIntelFramebufferAzul</string>
    <key>Find</key>
    <data>AwYIAAAEAAARAAAA</data>
    <key>Replace</key>
    <data>AwYIAAAEAAAAAAAA</data>
    <key>Comment</key>
    <string>10.10-Azul-Port_0x7-DP</string>
</dict>
```
>HDMI now also displays at 3840x2160@30Hz. This means I could drive two 4K displays from my integrated graphics, for a total of four.

>If anyone knows how to unlock the extra 30Hz to get to 60Hz, please let me know!

>I got it working with 60hz on El Capitan 10.11.4 with Clover 3389 - tested only Displayport so far. Board is a Intel DH87RL.

>My config.plist contains only this simple patch:
>This unlocks 4k at 30hz. only Displayport

```
<dict>
    <key>Comment</key>
    <string>framebuffer4K</string>
    <key>Find</key>
    <data>
    AwAiDQADAwMAAAACAAAwAQ==
    </data>
    <key>Name</key>
    <string>AppleIntelFramebufferAzul</string>
    <key>Replace</key>
    <data>
    AwAiDQADAwMAAAAEAAAAAw==
    </data>
</dict>
```


To get 60hz working I've used the [IOKit Clock patch](https://github.com/Floris497/mac-pixel-clock-patch-V2/) To apply it you have to disable SIP (System Integrity Protection). This can by archived by adding the following to the config.plist:
```
<key>RtVariables</key>
<dict>
    <key>BooterConfig</key>
    <string>0x28</string>
    <key>CsrActiveConfig</key>
    <string>0x67</string>
</dict>
```






>the third patch we not try
```
<dict>
    <key>Comment</key>
    <string>Framebuffer for 4K display</string>
    <key>Find</key>
    <data>
    AwAiDQADAwMAAAACAAAwAQAAAAAAAABgmRQ=
    </data>
    <key>Name</key>
    <string>AppleIntelFramebufferAzul</string>
    <key>Replace</key>
    <data>
    AwAiDQADAwMAAAAEAAAgAgAAAAAAAACAmRQ=
    </data>
</dict>
```


### 10.14.5 不完美的地方
1，开机log会打印 ppgtt
> 修改为下

```
<key>GraphicsEnabler</key>	<string>No</string>
```

2,开机log中出现
```
IOConsoleUsers: time(0) 0->0, lin 0, llk 1, IOConsoleUsers: gIOScreenLockState 3, hs 0, bs 0, nov 0, sm 0x0
```
> 原因是系统无法识别出你的显卡驱动，临时的解决方法是 解决为
>下面的二选一

```
1，取消勾选Inject Intel
2，或者将platform-id修改为0x12345678

```
> 然后设置正确的id

```
Clover Configurator设置正确的FakeID和ig-plaform-id
```
  


昨晚研究了一下，花了半小时定制了UBS驱动，总的来说，在目前的系统10.14.5下，还是非常简单的，由于macOS升级后可能会导致USB失效，所以，在即将到来的新版macOS，定制USB迫在眉睫

之所以要现在定制USB，是因为，目前在10.14.5下，去除USB端口数量补丁还有效，不需要复杂的操作即可轻松定制，如果后续更新，去除USB端口数量补丁失效时再定制，那就非常麻烦了

准备工作
需要这两个APP，本文不是新手教程，需要一定的黑苹果基础

Hackintool.app

Clover Configurator.app

打开去除USB端口数量补丁
>重建缓存命令
```
#!/bin/sh
sudo chmod -Rf 755 /S*/L*/E*
sudo chown -Rf 0:0 /S*/L*/E*
sudo chmod -Rf 755 /L*/E*
sudo chown -Rf 0:0 /L*/E*
sudo rm -Rf /S*/L*/PrelinkedKernels/*
sudo rm -Rf /S*/L*/Caches/com.apple.kext.caches/*
sudo touch -f /S*/L*/E*
sudo touch -f /L*/E*
sudo kextcache -Boot -U /
```

[补丁网址](https://zhuanlan.zhihu.com/p/68338188)
```
Comment: USB Port limit patch 10.14 (credits FredWst)
Name: com.apple.driver.usb.AppleUSBXHCI
Find: 83FB0F0F 83030500 00
Replace: 83FB0F90 90909090 90
MatchOS: 10.14
Comment: USB Port limit patch 10.14.1 10.14.2 Beta (credits DalianSky)
Name: com.apple.driver.usb.AppleUSBXHCI
Find: 83FB0F0F 838F0400 00
Replace: 83FB0F90 90909090 90
MatchOS: 10.14.1,10.14.2


Comment: USB port limit patch #1 10.14.x modify by DalianSky(credit ydeng)
Name: com.apple.iokit.IOUSBHostFamily
Find: 83FB0F0F
Replace: 83FB3F0F
MatchOS: 10.14.x

Comment: USB port limit patch #2 10.14.x modify by DalianSky(credit PMHeart)
Name: com.apple.iokit.IOUSBHostFamily
Find: 83E30FD3
Replace: 83E33FD3
MatchOS: 10.14.x
 
Comment: USB Port limit patch #3 10.14.x modify by DalianSky(credits PMheart)
Name: com.apple.driver.usb.AppleUSBXHCI
Find: 83FB0F0F
Replace: 83FB3F0F
MatchOS: 10.14.x
 
Comment: USB Port limit patch #4 10.14.x modify by DalianSky(credits PMheart)
Name: com.apple.driver.usb.AppleUSBXHCI
Find: 83FF0F0F
Replace: 83FF3F0F
MatchOS: 10.14.x
```
重启，USB能够正常使用后，准备开始定制USB

定制USB
打开Hackintool.app


我的是已经定制完的，没有定制的话，会有很多条，打开文本编辑器，和USB3.0优盘，2.0优盘（如果没有2.0的优盘，就拿鼠标键盘来吧）插入USB上，插入后会变成绿色，记录下来，每插一个USB口就记录一次，直到所有的USB都插入过并记录下来

注意：macOS系统的USB端口数量为15个，所以我们要保证USB端口数量要在15个及以内，USB3.0要占用两个端口，操作时，UBS3端口要拿一个USB3设备插入一次，一个2.0设备插入一次，记录下来。不过如果所有的USB端口都设置为3.0，会不够用，这时就需要取舍了，我是机箱前面板为USB3.0，后置全部2.0

这是我的，你们可以参照这个格式来记录

《使用Hackintool定制黑苹果USB驱动》
内置USB，比如蓝牙占用的USB端口，要设置为internal，typc要正反插（插完后转180度再插入，看端口是否变换，没变化就是TypeC+Sw，有变化的就是TypeC

之后删除没有没有被记录下来的端口，注意不要保留的数量不要超过15个，用不上的USB或者对速度没要求，比如鼠标键盘打印机就设置为2.0吧

《使用Hackintool定制黑苹果USB驱动》
多检查检查，确保需要的USB都被记录了，没有问题后就可以导出了

《使用Hackintool定制黑苹果USB驱动》
会生产4个文件，不过我们只需要用到其中的三个，把这三个文件放入EFI分区的EFI/CLOVER/ACPI/patched 中

《使用Hackintool定制黑苹果USB驱动》
之后禁用去除USB端口数量补丁，重启电脑

《使用Hackintool定制黑苹果USB驱动》
至此USB定制教程结束，全程不会超过20分钟吧，实操起来开始很简单的

QA
Q：为什么不用USBPorts.kext

A：USBPorts.kext很有可能在系统更新后失效，还是SSDT更加靠谱，基本是升级无忧

Q：如果我后序要更改端口类型怎么做

A：简单的改变类型，比如USB3改成internal，不需要从头做，直接更改即可

如果改的多的话，打开去除USB端口数量补丁从头再来吧

Q：定制USB有什么好处么

A：最大的好处就是可以让睡眠唤醒都正常，不用担心系统升级，如果你遇到了睡眠唤醒异常，失效，不妨定制一下USB