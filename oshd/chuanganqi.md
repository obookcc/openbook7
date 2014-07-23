# 廉价颗粒物检测传感器的“是是非非”
>作者： davidce

```
随着生活水平的提高,人们对空气污染的重视程度也越来越高。大气颗粒物是 空气污染中的重要组成部分,其中 PM2.5(粒径小于等于 2.5 微米的大气颗粒物) 由于对人体健康的危害巨大受到特别关注。对大气颗粒物污染状况监测成为全国各 环境监测中心的重要监测项目,同时也极大的激发了广大 DIY 爱好者的动手兴趣。 由于受成本因素限制,一般民用较多的是低于 100 元人民币的廉价颗粒物传感器, 本文针对 DSM501A、PPD42NS 和 GP2Y1010AU0F 三种廉价传感器,通过收集国 内外相关应用资料,阐述使用方法、对比分析测量精度等性能指标为大家介绍廉价 颗粒物检测传感器使用中需要注意的地方,供在使用时参考。

```

##1.	传感器介绍

当前市场上容易获得的廉价颗粒物传感器主要有SYHITECH DSM501A、SHINYEI PPD42NS和SHAPE GP2Y1010AU0F，三种传感器均采用光散射法进行颗粒物检测，光散射法的原理是当光照射在空气中悬浮的颗粒物上时产生散射光，在颗粒物性质一定的条件下，颗粒物的散射光强度与其浓度成正比。传感器原理图如图1所示。


![](http://doask.qiniudn.com/openbook7-dust1.png)
图1. 光散射法测量仪器原理图[1]

三种传感器主要参数对比如表1所示，从中可以看出DSM501A和PPD42NS的特性相近，与GP2Y1010AU0F存在较大差异。PPD42NS的精度高于DSM501A但后者量程较大，由于两者驱动方式类似，以下就从GP2Y1010AU0F和PPD42NS两种传感器入手介绍其使用方法。

>表1. 传感器主要参数对比


| 主要参数 | DSM501A | PPD42NS | GP2Y1010AU0F |
| -- | -- | -- | -- |
| 生产商 | SYHITECH | SHINYEI | SHAPE |
| 输出方式 | PWM | PWM | 脉冲电压 |
| 检测粒径 | >1微米、>2.5微米 | >1微米、>2.5微米 | 未指定 |
| 空隙流动方式 | 主动(电阻加热) | 主动(电阻加热) | 被动(风扇)* |
| 灵敏度 | 未指定 | 未指定 | 0.65V/(0.1mg/m3)(MAX) |
| 量程 | 0-15000/283mL | 0-8000pcs/283mL | 0-0.8mg/m3 |
| 电压(V) | 5±0.5 | 5±0.5 | 5±0.5 |
| 电流(mA) | 90 | 90 | 20 |
| 参考价格(RMB) | 35 | 85 | 30 |
*也可参考

**使用GP2Y1010AU0F**

GP2Y1010AU0F需要额外外围电路才能正常工作，但由于输出采用脉冲电压方式使得数据获取非常容易，易于使用。电路连接方法请参考[1]和[2]，驱动程序请参阅[2]。GP2Y1010AU0F检测数据与PM10有很强的相关性如图2所示，通过最小二乘法拟合R2达到0.99[3]，相关arduino程序可在PACMAN网站上下载[4]。由于受温度影响GP2Y1010AU0F输出数据需要进行温度补偿。

![](http://doask.qiniudn.com/openbook7-dust2.png)

图2 GP2Y1010AU0F与PM10检测数据[3]

**使用PPD42NS**

PPD42NS采用PWM输出方式，有大于1微米和2.5微米粒径颗粒数输出，电路连接方法和驱动程序请参考[5]。在使用过程中为了避免干扰需要使用遮光盖遮住PPD42NS的镜片窗，传感器也需要垂直放置。

使用Arduino读取PWM输出数据通常使用pluseIn() 函数，由于调用时会暂停程序执行并存在一定计时误差，pluseIn() 函数不适合在要求高实时性的程序中使用。有一种代替方法是通过定时器实现[6]，在获取数据的同时不会暂停程序执行。

![](http://doask.qiniudn.com/openbook7-dust3.png)
图3 PPD42NS数据对比[7]

Dylos DC1700系列大气颗粒物检测仪的原理同样是基于光散射法，能够检测大于0.5微米和大于1微米两种粒径范围的颗粒物。从图3中可知，Dylos DC1700系列具有与专业设备最接近的检测性能，民用价格适中，是评价廉价颗粒物检测传感器性能的理想设备。PPD42NS与Dylos DC1700系列检测设备结果也有很高的相关性，如图4所示。

![](http://doask.qiniudn.com/openbook7-dust4.png)
图4 经过滤波的数据(A),经过校正的曲线(B)[8]

DSM501A使用方法与PPD42NS类似，就不再赘述。DSM501A量程较大，但精度稍差。图5是DSM501A与DataRAM检测数据对比，两者拟合公式为y = 0.0984x + 94.656x (R² = 0.61643)[9]。AirGo使用DSM501A作为颗粒物传感器。

![](http://doask.qiniudn.com/openbook7-dust5.png)
图5 DSM501A数据对比[9]

SM-PWM-01A是另一种基于光散射法的廉价颗粒物传感器[10]，由于商家无货拿不到产品还无法得知其性能。

##2.质量浓度和数浓度的转化
质量浓度和数浓度是颗粒物浓度的两种计量单位，环境监测部门公布的是质量浓度（如：微克/立方米），医学人体健康领域多以数浓度为主（如：个/毫升）。由于无法准确量化每个颗粒粒子的属性，数浓度到质量浓度只能近似的进行转换。通过假设所有粒子为球体且具有相同的密度和半径建立而转换模型和实现算法[11]，同时也考虑了降雨对检测结果的影响。

本文通过对国内外廉价颗粒物传感器相关应用资料收集为读者提供使用参考，GP2Y1010AU0F适合检测PM10浓度，PPD42NS检测精度较好而DSM501A具有较大的检测量程。廉价颗粒物传感器的精度和准确性虽然低于专业设备，但低廉的价格和可接受的精度也能够在业余空气质量监测中有用武之地。

```
**参考文献：**

[1] [The data sheet of GP2Y1010AU0F]( https://www.sparkfun.com/datasheets/Sensors/gp2y1010au_e.pdf)

[2] [Chris Nafis, 2012. Air Quality Monitoring - Automatically measuring and graphing Air Quality with an inexpensive device (Sharp GP2Y1010AU0F Optical Dust Sensor)]( http://www.howmuchsnow.com/arduino/airquality/)

[3] [Olivares, Gustavo; Longley, Ian; Coulson, guy (2013): Development of a low-cost device for observing indoor particle levels associated with source activities in the home. figshare.](http://dx.doi.org/10.6084/m9.figshare.646186)

[4][Guolivar PACMAN wiki](https://bitbucket.org/guolivar/pacman/wiki/Home)

[5][Chris Nafis, 2012. Air Quality Monitoring - Automatically measuring and graphing Air Quality with an inexpensive device (Shinyei Model PPD42NS Dust Sensor)](http://www.howmuchsnow.com/arduino/airquality/grovedust/)

[6][AirCasting Particle Monitor](http://takingspace.org/wp-content/uploads/AirCastingParticleMonitorInstructions.pdf)

[7][Make Your Own AirCasting Particle Monitor](http://www.takingspace.org/make-your-own-aircasting-particle-monitor/)

[8] Xiaofan Jiang, Ji Jia, Gansha Wu, and Jesse Fang. Demo: Low-Cost Personal Air-Quality Monitor. MobiSys’13, June 25–28, 2013, Taipei, Taiwan. ACM 978-1-4503-1672-9/13/06

[9] [Michael Heimbinder, Dr. George Thurston, Dr. Carlos Restrepo, Michael Taylor, Joshua Schapiro, Andrzej Grzesik.My Air, My Health Challenge Solution.]( https://github.com/JSchapiro/AirGo)

[10] [The data sheet of SM-PWM-01A](http://www.tinyie.com/PDF/SM-PWM-01A.pdf)

[11][Preliminary Screening System for Ambient Air Quality in Southeast Philadelphia, 2009.](http://www.cleanair.org/sites/default/files/Drexel%20Air%20Monitoring_-_Final_Report_-_Team_19_0.pdf)

```
