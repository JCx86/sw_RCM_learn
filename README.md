switch RCM 载入自制软件教程
===
![](https://github.com/JCx86/sw_RCM_learn/raw/master/1.jpg)

注:互联网时代资讯日新月异,请根据时间差异判断本教程的适用情况.我会尽量更新.(百度贴吧会有更加新的资讯,这里只是做个教程方便一下.)

本文完全不提倡盗版游戏的传播,这里单纯运行自制小软件,不会聊及盗版游戏.   

次世代游戏机三剑侠，理论上来说这次switch的破解才能勉强称叫做全民破解.PS4曾经闹得火火热热的刷nandflash克隆机盗版那真是叫扯dan. 
随着国外团队fail0verflow在4月24号公开switch的硬件级RCM注入漏洞后，短短几周时间，油管频道快速出现各种运行自制软件的视频、教程等。各路大神也各显神通，异常活跃。不断放出各种linux，可运行自制软件的引导ROM等。而让人值得期待的是Atmosphere自制固件那边也是说预计能在6月中放出。TX团队也放出了他们类似加密狗的方式.

简单说下这个漏洞，漏洞是基于NVIDIA Tegra X1的硬件漏洞，暂时来说不换芯片应该是没法修复的。如果我们把switch比作一台手机的话，先彻底关机,按住下面三个按钮 “音量+”、"Home"以及"电源"。就能进入类似安卓手机的(recovery 模式)，这个模式属于工厂模式，用于生产线刷入flash或者手机救砖之类。

通常，RCM模式(其实RCM的M就是模式的意思，这里不深究)只能运行授权签名的代码。但是黑客们发现了 Tegra X1有个硬件漏洞，可以绕过签名检查，于是就可以通过以下步骤进行破解(大概):
短接switch右手柄插槽P1与P10, 按下三键开机,进入RCM;
接上数据线到PC或者某些安卓手机(没错,就是安卓手机,但是能支持OTG,要带Xhci)
USB那头对switch进行攻击,跳过签名检查,然后注入bin ROM,以及一堆初始化。
机器按我们需求启动U-boot,我们通过U-boot,加载linux或者其他想要的东西。

听起来很复杂,其实很简单:
===
* 有一台switch:
  系统版本越低越好.最新5.0.x也没关系,可能以后开机会麻烦点罢了.

* TF卡:
  8G以上,基本上大家都有,没有的话上京东买个也就三四十,不过建议买个32G或更大的吧,对了还要读卡器. 
  测试阶段,建议不要一下子买个200G, 最好先试水一下,问问大家这个tf卡兼容性行不行.

* TYPE-C 数据线
   推荐淘宝紫米,10块钱包邮, 如果用手机OTG注入,还要多买一条OTG线. 

* 电脑
  可以是mac,WIN10的,linux的,都行,甚至好些安卓手机都行(支持OTG功能,并且有Xchi).
  经测试好像晓龙625以上,或者800以上应该都可以,手机其实是最方便快捷的.

* switch短路帽 
   这个是必须品,为要有短路帽才能做到按下HOME键的效果
    (注意不是switch手柄上的home键,而是连接到Tegra芯片映射的那个home引脚),   
   如果各位不想花钱买短路帽的话,可以去看看(铁丝扎带)做法,去贴吧看看教程跟着折弯一个,
   反正我自己是没试过(因为外文论坛上已经有2用回形针搞坏手柄座例子了~这个比坏手柄更麻烦啊啊啊).
   估计是回形针太粗太硬了.硬又黑~~

# 下面简单说下教程和技巧

# TF卡准备.
   如果是新卡的话,先进游戏机系统,把卡格式化一下.然后拔出来,用读卡器装到电脑上,把必备文件拉进去卡的根目录,
   以Jan4V基于5.0.X,运行自制hb manu为例,源码以及文件连接如下
   下载sdfiles.zip,解压并把里面的文件全放进TF卡根目录,根目录.
   下载payload.bin,留着等下待用(这个可以理解为引导rom)
   然后tf插回swtich内.

# RCM模式
   先彻底关机(按住10秒以上), 右边插入短路帽,确保短路帽接触良好,
   然而我发现有两种进入方式,不知道为何,
   一种是:  先按住"音量+". 再按住电源键, hold住一直按着5-10秒,不能松开同时插入typeC,如果成功进入RCM,那么switch屏幕依旧一片漆黑.
   另一种:这是我油管上看别人进入的,同样在彻底关机,插入短路帽的情况下, 同样先按住"音量+". 再按住电源键, hold住一直按着5-10秒. 这时候他就可以松手    了,慢慢插入typeC然后百分百进入RCM.
   如果处于RCM,  电脑那边在Win10设备管理器会显示APX 这样的一个设备 , (linux运行命令行lsusb 会显示NVIDIA .c )  . 
   如果失败了没进入RCM,那么因为充电的原因,switch自动开机进入任天堂系统,电脑那边会显示一个Nintendo设备.
   这时候你拔掉typeC,彻底关机然后重来.
![](https://github.com/JCx86/sw_RCM_learn/raw/master/2.jpg)
   反正我试了好多遍我自己都只能用方法一进入,不知为何,这里先打个问号,待日后总结出经验再分享分享.反正你先要判断机器是否进入RCM(设备管理器去看,或者linux用lsusb命令),确保RCM进入后再去做其他事情,退出RMC的话,长按电源及10秒以上.
 
 
# ROM注入

## 手机版

   实际上是一个很复杂的过程,这里就简单化把叫做"ROM注入"吧.  可以用手机,可以用电脑,可以用TX团队最新卖几百块那个小U盘. 这里暂时先介绍用手机比较简单的方法, 先看看多简单:
![](https://github.com/JCx86/sw_RCM_learn/raw/master/3.jpg)
   准备一台OTG功能手机,高通晓龙的或者三星的最好,MTK或者华为海思的没试过.

   上面在github下载的文件 payload.bin, 复制到你的安卓机内

   手机安装 chrome 浏览器,打开以下网站:


   选择upload playload, 点击选择文件,找到payload.bin  
   (有时候可能手机系统的文件管理器不太友好,你要安装几个文件资源管理器才能找到payload.bin)

   最后. 把你处于 RCM的 switch, 通过OTG数据线插入到手机. 

   chrome 那边点击按钮 "Do the thing!"

   正常就会弹出一个二级对话框,里面有 APX 这个设备.
   (如果没有,检查switch是不是处于RCM,然后再次插拔usb线.)
![](https://github.com/JCx86/sw_RCM_learn/raw/master/4.jpg)

   选中APX,连接, switch 闪一下,就进入安卓刷机用户很熟悉的 recover 界面了.

   音量键操作上下, power键是确认. 选launch firmware-> CFW. 确认.
![](https://github.com/JCx86/sw_RCM_learn/raw/master/5.jpg)

   switch闪几下,重启已经进入了带 自制menu的一个系统了(看上去跟官网系统无异)
![](https://github.com/JCx86/sw_RCM_learn/raw/master/6.jpg)
   点击相册,就能看到我们放在tf卡内部的自制软件啦;(类似GBA模拟器,等等,正在完善.)



## WIN10版本教程

   资料下载:

   度盘链接：pan.baidu.com/s/11xkU019g-0HSn-S1JDQi9A 密码：t31i

   Dorpbox链接:
https://www.dropbox.com/s/gkf5f3axgr0vkvi/%E6%95%B4%E7%90%86_switch%E8%BF%9B%E8%87%AA%E5%88%B6%E8%8F%9C%E5%8D%9520180520.zip?dl=0

   资料包打包了05-15日之前的文件,有更新的话请自行上github或者其他途径获取更新.

   内包含4.x,5.x两个版本系统的 homebrew-menu 进入文件.

   还有一个5.x可用的gba模拟器,还有口袋妖怪英文版rom一个,

   对了还有一个友情小广告jpg.请见谅.

   目录内三个重点文件:

   sdfiles_xxx.zip      zadig-2.3.exe      TegraRcmSmash1101

   sdfiles压缩包,解压后,把里面的文件放到你switch的TF卡根目录去,注意要根目录.

   zadig软件,负责把RCM状态下的switch,成libusbk(V3.0.7.0)状态.

   TegraRcmSmash1101  负责装载你的bin文件,注意这个bin文件分4.x 5.x,

   还有以后大气层自制,TX自制菜单出来后他们也会有属于自己的bin.

   装回TF卡, 把你的switch 进入RCM,并且插入电脑后,按照下面几个图步骤去做就行了.
![](https://github.com/JCx86/sw_RCM_learn/raw/master/7.jpg)
![](https://github.com/JCx86/sw_RCM_learn/raw/master/8.jpg)
![](https://github.com/JCx86/sw_RCM_learn/raw/master/9.jpg)
![](https://github.com/JCx86/sw_RCM_learn/raw/master/10.jpg)
![](https://github.com/JCx86/sw_RCM_learn/raw/master/11.jpg)
![](https://github.com/JCx86/sw_RCM_learn/raw/master/12.jpg)
   并不难

   如无意外,到了这里你的switch就会显示这个界面了



====================================

目前进度:

暂时不能如你所想那样玩免费游戏,如果你很需要,请等到6-7月.

这里以5.0.0以上系统为例,时间并不完全准确,

05-17: GBA模拟器放出;

05-15: TX团队放出运行免费游戏的视频,整体方案售价40美金左右;

05-10: 可以多个游戏修改存档;

05-01: 放出手机版越狱软件,以及网页端越狱软件;

04-25: switch 漏洞公开;
