---
title: Mac mini(Late 2012) 输出2K视频 升级系统到10.13（macOS High Sierra）
date: 2018-7-24
tags: [Mac mini,2K分辨率,系统升级]
categories: macOS
---
<!--文章摘要-->
升级系统，正常输出2K视频
<!-- more -->
# 升级系统
本来不打算升级系统的，毕竟搭建了那么多开发环境，怕给整的不能用了。但是，LG25UM58的2K的宽屏显示器分辨率一直无法调好，试了很多办法都无果，本想着扁就扁吧（图标什么的都被拉长了，看着难受是肯定的），可看到同事都升级了最新系统，就想着升级一下，也许能解决分辨率问题，虽然知道这是主要是显卡和接口的问题。这款LG显示器接口只有两个HDMI，没有VGA和DP，也是够坑的。
## 环境
硬件：Mac mini(Late 2012) 
系统：从10.12 (macOS Sierra) 升级到 10.13（macOS High Sierra）
## 方法
在Appstore里进行免费升级
安装器下载完成后开始升级,但出现找不到资源的提示，重启还是一样。

![安装器](/images/blog/安装器.jpg)
图1 安装器

## 解决问题
在重启多次无效后，只好恢复到之前的系统：
方法1：按alt重启，选择升级前的启动盘（旧系统）而非升级盘（新系统）
方法2：按cmd +R重启

![系统工具](/images/blog/系统工具.png)
图2 系统工具

其实，在执行方法1的时候，再升级（直接选择升级盘或进入旧系统后再次打开安装器）一次或多次，往往会升级成功！
我的操作则一波三折,先用方法2尝试，结果发现Time Machine没有备份，第二项好像需要下载副本，第三项也是扯淡，想着就用第四项（Disk Utility）恢复磁盘吧。结果发现旧系统盘是灰色的，无法修改。现在想来，应该是格式转换（NTFS-> APFS）还没有完成。于是乎选用方法1，发现找不到旧启动盘，只有升级盘，有点绝望，觉得没救了，打算备份数据，然后借助另一台Mac来恢复或用U盘重装。折腾的太晚了，就先洗洗睡了。
第二天从实验室借了另一台Mac mini(Mid 2014) ，顺便接入我的2K显示器试了一下，显示正常。由此可见，应该是我的Mac mini版本太旧，默认分辨率不支持2K。用雷电线（Thunderbolt cable）将我的 Mac mini（按T重启）连接到另一台Mac mini上，我旧启动盘没有直接显示在桌面上（黄色盘标），进入磁盘工具发现旧启动盘依然是灰色，感觉好无奈。

![Thunderbolt](/images/blog/Thunderbolt.png) 
图3 Thunderbolt cable（mini DP接口）

只好再试方法2来恢复磁盘，却惊喜地发现旧启动盘已不是灰色了（应该是格式转换完成了），就又连到借来的那台Macmini上备份了数据并准备恢复系统，可转念一想，既然启动盘正常了，那应该在用方法1时可以找到旧启动盘了。果然，再次使用方法1时旧启动盘（10.12）出现了，蛮激动的，不过还是想选择升级盘再升级一次。结果还成了，只是分辨率问题并没得到解决。
 
![升级系统](/images/blog/升级系统.png) 
图4 升级系统

# 解决分辨率问题
## 下载补丁（脚本）
### 相关补丁
[mac-pixel-clock-patch-V2](https://github.com/Floris497/mac-pixel-clock-patch-V2)
选择合适的版本，如：[CoreDisplay](https://github.com/Floris497/mac-pixel-clock-patch-V2/blob/master/CoreDisplay-patcher.command)
### 获取脚本
不熟悉GitHub的可以用以下方式获取相应脚本。
方法1：
点击Clone or download -> Download ZIP

![补丁仓库](/images/blog/补丁仓库.png) 
图5 补丁仓库

方法2：
点击Raw 

![补丁](/images/blog/补丁.png)
图6 补丁（脚本）

查看源码->在页面右击->将页面存储为

![存储页面](/images/blog/存储页面.png) 
图7存储页面

选择位置、格式（页面源码）-> 存储

![存储路径和格式](/images/blog/存储路径和格式.png)     
图8 存储路径和格式

选择“不追加”

![存储提示](/images/blog/存储提示.png) 
图9 存储提示

下载完成

![下载完成](/images/blog/下载完成.png) 
图10 下载完成的补丁（脚本）

>注：方法2有点作死，可以忽略。

## 执行该脚本（在终端里进行）
1.当系统版本≧10.11时，需要确认系统保护处于关闭（disable）状态
``` bash
$ csrutil status
```
2.修改脚本权限
``` bash
$ chmod +x  /Users/Wade/Documents/CoreDisplay-patcher.command
```
3.执行
``` bash
$ /Users/Wade/Documents/CoreDisplay-patcher.command
``` 
4.设置->显示器->按alt+点击“缩放”，查看是否有自己需要的分辨率（如2560*1080）
5.如果通过4还是查不到所需分辨率，则安装SwitchResX来设置分辨率
[SwitchResX](https://pan.baidu.com/s/12jdM-EMUJIM8aswNe2VEcw)，Password：qqdw
查看当前可用分辨率，有所需分辨率则直接应用

![SwitchResX-1](/images/blog/SwitchResX-1.png)     
图11 SwitchResX-1

没有则添加（+）

![SwitchResX-2](/images/blog/SwitchResX-2.png)     
图12 SwitchResX-2

应用（Aplay）

![SwitchResX-3](/images/blog/SwitchResX-3.png)     
图13 SwitchResX-3

6.额外功能（具体问题可以在issues中查看）
如：
``` bash
$ /Users/Wade/Documents/CoreDisplay-patcher.command patch v4
``` 



# 参考文献
1.HONG, 2017. [FIGHT Q34--记升级34寸曲面显示器及MAC MINI MID 2011(MC815)折腾](http://oohong.com/post/fight-q34)
2.ryan2000, 2017. [[求助－已解决] MacOS 10.13升级失败！](https://bbs.feng.com/forum.php?mod=viewthread&tid=11455162&extra=&page=1)
3.artoostark, mcluyu, 2018. [升级 macOS High Sierra 出错，然后我救回来了](http://jp.v2ex.com/t/446463)
4.方水泉, 2017. [macOS High Sierra升级注意事项和一些故障的解决办法](http://baijiahao.baidu.com/s?id=1580123441382649040&wfr=spider&for=pc)
5.竹山流水, 2017. [Mac SIP系统完整性保护如何关闭启动启用教程](https://jingyan.baidu.com/article/9c69d48ff88b3813c9024e9d.html)

