#遥控四旋翼直升机
>[原文](http://www.instructables.com/id/RC-Quadrotor-Helicopter/?ALLSTEPS) 作者 frank26080115 翻译AIrobot-Jason

![1](http://doask.qiniudn.com/openbook7-helicopter1.jpg)

![2](http://doask.qiniudn.com/openbook7-helicopter2.jpg)

![3](http://doask.qiniudn.com/openbook7-helicopter3.jpg)

![4](http://doask.qiniudn.com/openbook7-helicopter4.jpg)

![5](http://doask.qiniudn.com/openbook7-helicopter5.jpg)

![6](http://doask.qiniudn.com/openbook7-helicopter6.jpg)

![7](http://doask.qiniudn.com/openbook7-helicopter7.jpg)

![8](http://doask.qiniudn.com/openbook7-helicopter8.jpg)



这是一个遥控四旋翼直升机（四轴飞行器，四旋翼，四螺旋桨等）项目。 它是带4个螺旋桨的遥控直升机

##视频

你需要的基本技能：

>如何使用Arduino，入门级足够

>焊接，接线和基本的电子技能

>基本的手工技能

一个四旋翼飞行器 -- 一个十字形机架上有4个旋转的螺旋桨。

当一个转子比对侧的转子旋转速度快时，更快一侧将有更多的升力，因而直升机会倾斜。 当直升机倾斜时，空气稍微向侧下方吹，而不是直接向下吹，直升机将向相应的方向移动。
螺旋桨需要相同个数的正反转桨，两个顺时针和两个逆时针旋转。

这样，直升机因为旋转惯性被抵消，就不会在垂直轴上旋转。

但是当一个方向旋转的一对桨比另一对更快，直升机会绕Z轴旋转。 这是四旋翼控制方向的方式。
我们将建立一个飞行控制电路包含1个加速度计和陀螺仪传感器，使得微控制器可以检测飞机不期望变化的角度，并相应地调整每个转子的速度，以对抗变化。 微控制器将每秒进行几百次速度调整，以保持飞机在空中的稳定。

飞控是一个完全开源的电路，提供电路原理图和PCB文件。 飞行控制器完全兼容Arduino。 源代码是AeroQuad（基于Arduino的开源四轴控制码）的修改版本。 飞行特性可以使用AeroQuad的配置工具进行调整。

附件是一个示意图，显示四旋翼每个电机的自旋方向，请记住这个图！ 如果你的安装不遵循此图，将不能正常飞行。
该微控制器的输入也来自遥控无线接收机，这样你就可以在地面遥控无线发射机控制直升机。
这架直升机将采用四个无刷电机。 每台电机由一个ESC（电子调速器）来控制。 电调通过板载的微控制器来控制电机旋转。

锂聚合物电池为整个四轴供电。

下载概要：

>在所有步骤中，有100多张照片

>第9步包含飞控原理图和PCB文件

>第10步包含微控制器的引导程序和内核程序

>步骤12，13，14包含演示Arduino的工程文件

>步骤26包含了飞行控制软件

##步骤1：零件



遥控无线发射机和接收机

你至少需要4个通道的接收机，6通道更好。如果可能，最好使用2.4 GHz技术的遥控器。Turnigy有比较便宜的9通道的遥控器，它采用AVR单片机，你可以把自定义固件刷进去。我自己有一个25美元旧的无线遥控器，使用75 MHz频率，但我用一个转换套件把它转换成一个2.4 GHz无线应用。

需要四个外转子无刷电机。我用hextronics 20-22l（这个数字代表的直径和电机的线圈高度，线圈的配置也有KV值，涉及电机的速度和力量，高KV=更快的电机（多谢slick8086纠正我关于电压和KV的关系，KV值越高，相同电压下，RPM越快），800至1200是可以接受的电机KV值。他有很多配件（插塞接头，热缩管，螺旋桨适配器，螺钉，安装板，备用轴，备用c-clip，都包括在内），这样万一一炸机你可以用来替换。

需要四个无刷电机电子调速器（ESC）。额定18安培就足够了，我听说Turnigy Plush 电调的很多优点，包括高刷新频率，这意味着飞机响应速度更快，我这里有山寨的HobbyKing的Trunigy Plush电调，价格更便宜。

有些电调可以采用编程卡，这意味着你可以使用一个便宜的编程卡（6美元）来改变电调的配置，非常方便。购买与你的电调兼容的编程卡。我用turnigy电调编程卡因为他们与我的电调兼容。

很显然你需要一块电池，您可以使用一个3S1P的锂电池，额定参数至少20C（放电等级）。3S意思是3个电池串联，1P意味着两套并联平行。它会提供11.1V的电压。我建议2500毫安的电池容量（或更多）。一般的经验法则是电池的容量翻番意味着飞行时间增加50%（由于多余的重量）。
电池的更多信息参考：http://www.rcgroups.com/forums/showpost.php?P=1315199&postcount=1

请留意连接电池的连接器类型，连接器需要匹配。我的整个直升机使用香蕉头连接。电池采用4.00mm的香蕉头连接器，其它部分使用3.5毫米香蕉头连接器（马达使用3.5毫米子弹连接器）。（你可以使用别的连接器，比如xt-60连接器，务必小心接头极性，同时也留意，我所有的图片显示的香蕉头连接器连接方式）

采用3.5mm香蕉头连接器时你需要很多热收缩管作为绝缘使用。用不同的颜色，以便很好的区分。

你需要12级绞芯线。必须是12级或更粗的线以流过较大的电流。必须是绞心线保证足够柔软。用不同颜色来区分极性。最好用细铜硅胶线，但比较贵。

你需要一个好的电池充电器，它必须能够均匀给锂电池的多个单元充电。我有一个turnigy充电器，有许多设置，带有LCD，冷却风扇，非常好用。我还使用一台笔记本电脑电源给充电器供电，普通的充电适配器无法处理所需的大电流。

需要一个电池监视器以便知道电池电压是否过低。如果你把锂电池置于某一阈值以下，锂它将永久性损坏。使用电池监视器可以防止你的电池损坏。我有一个电池监视器可报告每个电子单元的状态。

四旋翼的机架是从HobbyKing购买的。15美刀一套包括配套的螺丝和螺母。相比之下，一根铝杆投递到家里会花掉我10美元，不是很合算。我建议你多买几套机架，那样你就有很多备用的机臂，如果损坏可以用来替换（螺钉和螺母也是）。

螺旋桨必须拥有相同个数的正反桨（一个“推”一个“拉”）。我用APC10x47慢桨。10表示直径英寸，4.7表示倾角。大直径意味着更多的升力进而需要更强大的电机。10英寸对我的机架来说是合适的尺寸。

你需要完整的飞控电路（意味着完整的一套BOM），我将在后面详细讲这个。同时，你需要一个USB转串口的连接线（FTDI连接线）和一个AVR编程器。

大量的伺服线用来连接不同模块，至少需要6根母头连接线用来连接遥控接收机和飞控。

？魔术贴、双面尼龙搭扣用来将需要固定的东西或者电池绑到机架上，非常轻便。

用一个气泡平衡仪（就像[这个](http://www.hobbyking.com/hobbyking/store/uh_viewItem.asp?idProduct=10614)）帮助你校准传感器。

确保你有足够的螺柱，电线，电缆，连接器，热缩管，绝缘胶带，胶水，螺丝等。

##步骤2：子弹头连接器焊接

![9](http://doask.qiniudn.com/openbook7-helicopter9.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter10.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter11.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter12.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter13.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter14.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter15.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter16.jpg)


你需要做大量的焊接，特别是这些较大的连接器。我写这部分的原因是焊接这些特殊的连接器需要特殊的焊接技术。电流通过线缆，它们必须坚固耐用并耐拉扯。并请正确焊接。

你需要一个好点的烙铁，因为连接器相当大，可以做散热。

首先，找一块木头，钻些孔用来固定住香蕉头。这是你的焊接夹。

剥线头时，暴露的金属线长度要能伸到连接器末端。如果你是4mm连接头与极化壳体焊接，确保壳体先焊接（见图片）。

把连接器放到木头的孔中，然后把你的烙铁的尖端插入连接器的一侧的小洞。用焊锡填充腔体（填充60%的容积）。当焊锡在热熔状态时，把线放入腔体，持续加热几秒钟，直到你确定线和焊锡已达到热平衡的状态，然后拿掉烙铁，让线放稳一段时间直到焊锡变冷凝固。

等到整个连接器冷却下来。用酒精清洁焊接处。

如果您使用的是塑料腔体，将线放到腔体里，直到连接器卡到腔体里。如果你不使用的这种连接器，那么用热缩管使接头绝缘。看图片。

**相关教学视频：**

>http://www.youtube.com/watch?V=b9yy9kk4bea

>http://www.youtube.com/watch?V=mnldhrtjre8

##步骤3：准备电机

![9](http://doask.qiniudn.com/openbook7-helicopter17.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter18.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter19.jpg)


如果你和我使用同样的电机（hextronics 20-22L），你会发现轴伸出电机底部。我用旋转切割工具把这部分切掉，因为我不想电机因为与地面轻微碰撞而损坏。

当你切电机轴时，一定要先用胶带包住整个电机，防止金属屑和杂物进入电机。灰尘和碎片会损坏绕组和轴承。当你完成后，用压缩空气彻底清洁马达。

组装电机时，将大型螺旋桨适配器和底部安装板，用合适的螺丝固定。

电机自带3.5mm子弹头连接器和热缩管。公头连接电机，母头连接电调(我们将稍后介绍）。

##步骤4：了解无刷电机



![9](http://doask.qiniudn.com/openbook7-helicopter20.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter21.jpg)

了解无刷电机和电调的工作原理是很重要的，这样你就不会烧毁你的电调。

**无刷电机介绍**
>http://en.wikipedia.org/wiki/brushless_dc_electric_motor

>http://electronics.howstuffworks.com/brushless-motor.htm

使电机反向旋转，切记不要将电调的电源正负极反接。

你会发现电机和电调之间有三根线。这些电机是三相电机，这意味着电机有三组线圈。线圈依次通电，使电机旋转。电调的工作则按顺序驱动线圈，但这需要准确的切换相位以使电机可以加速到正确的速度。ESC采用微控制器采样线圈的反馈信号，以判断线圈和磁铁的相对位置，通过相应的运算来决定使用FET接通或断开线圈的时间。

现在你应该了解了，改变电机的旋转方向，你可以通过交换电机和ESC三线之间的任何两个来实现。想象一下，如果原始相序如下：
123123123123123我们交换2和3，我们得到如下相序：132132132132132这样，旋转就可以反向了。

另外，电调包含一个5V的电压输出（就像一个廉价的7805），具有低电压拦截特性。我们的飞控使用这个5V电源。

拦截特性意味着当你的锂电池耗尽，它将停止转动电机以保护电池。这是因为如果电池放电低于阈值有可能永久性损坏你的电池。但是，当电机停转，电调仍然输出5V。这样，在固定翼上的伺服系统仍然可以使用5V电源功能，飞机即使没有引擎（电机）仍然可以滑翔并安全着陆。


然而，对于四旋翼情况有些不同，如果马达停转，四旋翼将在空中失去控制。这将对周围的人非常危险。因此，我们设定截止电压为“低”（电调中设置）。最大限度地提高飞行时间。通过电池监视器来防止电池过放。

>注：电调可以被破解！大家都喜欢破解，对吧？

>请参考：http://www.rcgroups.com/forums/showthread.php?t=766589.

I2C控制电调的优点是更高的刷新率，更高的精度，仅需两根线控制多达127个电机。它需要在飞控软件的编译选项做一些小的变化，因为代码已经包含这个功能了。但是在下面的步骤中我仍然使用PWM控制的电调。

##步骤5：准备电调

![9](http://doask.qiniudn.com/openbook7-helicopter22.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter23.jpg)

将3.5mm香蕉头连接器的母头连接到电调的输出线。电调的电源输入也焊上连接器。确认连接器的极性以免接反。

同时，请使用编程卡按照如下参数设置电调：

>刹车：关闭

>电池类型：锂电

>切断类型：软切换

>截止电压：低

>启动模式：正常

>定时模式：高

>锂电cell数：0（即自动检测）

>管理模式：关闭

参考这些链接：
>http://www.youtube.com/watch?V=4kg7mzgwvdw

>[turnigy BESC编程卡手册](http://www.hobbycity.com/hobbycity/forum/uploads/84/Turnigy_prog_card_Manual.pdf)

##步骤6：准备电池监视器

![9](http://doask.qiniudn.com/openbook7-helicopter24.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter25.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter26.jpg)

这一步是可选的，我还是建议你选择这种方式以防电池监视器反接。

使用4引脚JST连接器取代您现有的电池监控模块的连接器。

我还使用了一个永久性的标记显示连接的极性。

使用尼龙搭扣以便电池监视器可以很容易地安装到飞机上。这是可选的，我通常只是把它吊在电池下面。

时刻留意电池监视器。一旦有一个灯变成红色，就要尽快让飞机着陆。电池包里的任一块低于阈值都会对电池造成永久性损坏。

飞行控制器同时具有电池监测能力，但不能监视所有3个电池模块。有些电池监控器有一个蜂鸣器告警，比灯告警更好用，但这些也不能监视单独的电池包。

##步骤7：准备分电线路

![9](http://doask.qiniudn.com/openbook7-helicopter27.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter28.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter29.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter30.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter31.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter32.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter33.jpg)



 这个东西有时被称为“电源蜘蛛”。

 你需要将电池的正负极接到到四个电调的正负极。将你的12号线焊到一个蜘蛛状的物件上。你需要两个这样的蜘蛛状物件去布电源地线，一为正，一为负。我建议你用不同的颜色来表示，比如正极红色，负极黑色（或蓝色）。

其中一对导线应该更长一些，这是连接到电池正负极的导线。我把电线截断并焊到3.5毫米香蕉头连接器上所以如果我用不同电池连接器的时候，就可以更换了。

确保给这两组线做好绝缘。我把中心焊点放到瓶盖里并用热胶填充。

焊3.5毫米香蕉头连接器到分电线路的一端，以便接连接到电调，确保你没有把极性搞反。

在我的配置上，我添加了第五对线以便以后使用（也许给相机供电）。我建议你也这样做，但需要注意的是，公头连接器将暴露在外，使用备用连接器“绝缘虚拟连接”（图片中黄色的东西）并使用热缩管将暴露的连接器包住。

在整个设备里没有电源开关。供这么大电流的开关重量太重了，直接将电池拔掉来关掉飞机，无论如何，这是一个很好的习惯。

##步骤8：飞控概述


![9](http://doask.qiniudn.com/openbook7-helicopter34.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter35.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter36.jpg)


飞行控制器是利用单片机来控制四轴的系统。它需通过一个遥控无线接收机获得来自用户的输入，同时从加速度计和陀螺传感器获得飞行器姿态输入。飞控输出用来控制四个电调。

传感器采用的是BMA180 三轴加速度计，和ITG-3200 三轴陀螺。这些都可从SparkFun购买甚至是小扣板，所以你不必担心芯片焊接问题。请注意，这些都是数字传感器，这意味着他们都有内部的模数转换器（ADC），它们要比微控制器的ADC要好。这两个传感器都使用I2C总线，这意味着只需一个两线接口就可以连接两个传感器。I2C总线可以运行在400KHz频率。

遥控无线接收机有6个或更多的通道输入管脚，每个信号被连接到一个微控制器的引脚上。这些信号是PWM（脉宽调制）信号，单片机将测量从用户获取的PWM信号的宽度。

每个电调都需要一个PWM信号输入。因此每个电调都有一个输入信号连接到单片机的一个输出管脚。微控制器将给每一个电调输出脉冲来控制电调的速度。


尽管一个普通的Arduino也有足够的性能来做飞行控制，但我还是决定在我的设计里用ATmega644P，那样我会有更多的内存和引脚来做设计。我的设计仍然采用了16 MHz，使用Arduino的引导程序（bootloader），这和Arduino IDE（集成开发环境）是兼容的。


ATMEGA运行在5V电压下，因为16 MHz需要5V的电源，这个5V电源从电调获取，因为电调已经有一个内建的5V电压输出。

传感器运行在3.3V电压下，因此我们的电路将包含一个3.3V的低压差稳压器，将5V转至3.3V来给传感器供电，保证传感器的接口正常。

##步骤9：飞控深度解析


![9](http://doask.qiniudn.com/openbook7-helicopter37.jpg)

![9](http://doask.qiniudn.com/openbook7-helicopter38.jpg)


我以前设计过这个电路：[Ro-4-Copter](http://code.google.com/p/ro-4-copter/)

这是谷歌代码托管的一个项目，我是所有的维基页面，电路，PCB，和BOM的唯一作者。该代码是aeroquad和multiwiicopter的深度修改本，包含所有的原始版权声明，同时也包括我的修改。

你可以尝试着复制我所提供的所有设计文件（包含Gerber文件）和BOM都包含在里面了。我也在尝试设计一个“零售”版本，这样一旦我完成测试并最终定稿，我将出售他。

看这个维基文章来了解我是如何组装我的飞行控制器的(http://code.google.com/p/ro-4-copter/wiki/PCBAssemblyAndWalkthrough)，
这是我写的，零件列表也包含在内(http://code.google.com/p/ro-4-copter/wiki/PartsList)。

从 Eagle CAD PCB文件生成Gerber文件即可进行PCB加工，网络上有很多相关教程。这里有一个SparkFun教程(http://www.sparkfun.com/tutorials/109)。我通常把Gerber文件发到Seeed Studio进行加工（10块PCB，费用28美金，制造和快递需要5天时间）。
下面是一个简短的装配指南，获取更多的细节，看下面完整版本的指南（参考上面的链接）。

注意所有的电阻为0805的封装，虽然0603也可以。他们应该支持1 / 16W或更大的规格。注意，除非另外说明，所有的电容器，都是0805陶瓷芯片电容器（或0603，因为这个也适用）。它们可以支持16V或更大电压。
这个装配指南还包含对电路的其它方面的说明。

###微控制器

![9](http://doask.qiniudn.com/openbook7-helicopter39.png)

![9](http://doask.qiniudn.com/openbook7-helicopter40.png)


ATmega644PA显然必须安装、焊接在一个40引脚DIP插座上，这样才可以在需要的时候换掉元器件。

该ATmega644PA需要一个16 MHz的谐振器。

必须焊上0.1uF的电容C1和C2，作为ATmega644PA去耦电容。同时焊上0.1uF的去耦电容C9来作为模拟参考 。

在板的底部，有一个”AREF-SEL”标记，允许你选择5V或3.3V输入作为模拟参考电压。这里你也可以使用一个0欧姆的电阻或者磁珠。请注意，AVCC总是连接到5V电源。

###微控制器的支持领域


![9](http://doask.qiniudn.com/openbook7-helicopter41.png)

![9](http://doask.qiniudn.com/openbook7-helicopter42.png)

复位开关必须焊接，10KΩ电阻R13 是复位开关的上拉电阻。这个电阻是可选的因为ATmega644PA有一个内置的复位引脚上拉电阻。如果你不信任内置电阻，你可以焊上这个上拉电阻。

如果你使用一个非标准的Arduino Bootloader你需要焊上这个标记为“BOOT”的按钮。我个人很讨厌bootloader工作时对延时的严格要求。这个按钮连接到ATMEGA的PB6，低有效。务必使能此按钮的内部上拉电阻。

如果你需要使用AVR编程器（也许只在第一次烧写bootloader时使用），需要连接ISP接口（标记为AVRISP）。边上的长线表示“红线”（引脚1）应该对应这个位置，底部的短线表示在连接器的缺口应该在这个位置（见上图）。

###LED指示灯


![9](http://doask.qiniudn.com/openbook7-helicopter43.png)


LED1，LED2，LED3是3毫米的LED灯，选择你自己喜欢的颜色。他们可以垂直或水平（伸出板外）安装。请注意孔的形状和丝印所示的极性，正确焊接。R3,R4,R5，都是LED的1kΩ限流电阻。

###电源


![9](http://doask.qiniudn.com/openbook7-helicopter44.png)


PCB上已经标示了如何焊接sot-233-3封装的LM1117 3V3 LDO。同时焊接配套的1uF电容C3。

LED-PWR是一个3mm LED灯，用来指示3v3电源线上是否有电。你也可以垂直或水平（指出）焊接。R12是该LED的1K欧姆限流电阻。

两排3针公头焊接在“3V3-TAP”上，这些转接口提供3v3电源信号。两排3针公头焊接在“5V-TAP”上，这些转接口提供“系统”5V电源。

如果你是使用电池给电路供电，你需要在板上指示SOT-232-3的地方焊接LM1117 5V稳压器。如果你使用电调提供5V电压（一定要正确设置 SJ1），或其它情况下。

无论你是否使用5V稳压器,1uF电容C4都要焊接。

如果你需要连接电池电源作为输入,有四种方法来作为电源输入：


![9](http://doask.qiniudn.com/openbook7-helicopter45.png)


>使用圆形电源插座

>直接焊接到PCB（你可以扩大一些开孔方便焊接）

>使用螺丝端子

>使用一个3针接口

一个大功率二极管应安装在D2位置以便在电池接反的时候保护电路（如果你相信自己的能力此二极管可以省略，如果省略此二极管你必须用焊锡短接二极管的管脚）。这个二极管必须可以处理20V的反向电压，并承受得了1.5A电流。采用SMB或类似的封装。

C5和C6是5.3 X 5.3mm的铝电解电容器，必须焊接，焊接时要按照极性正确焊接。

###RC输入

![9](http://doask.qiniudn.com/openbook7-helicopter46.png)

PortC引脚PC7->PC2连接到6x3的接头，在这里焊接公头。这些接口用来从遥控接收机获取RC脉冲信号。三针引脚的中间可以使用SJ2选择连接到5V或3V3，如果你的接收机需要使用7号线供电，记得板上有一个“5V-TAP”和“3V-tTAP”。

重要的是：如果你用SJ15选择了V-RC（与接收机电压一致）同时你使用SJ3选择了3V3，那么你就不能再使用SJ17选择任何功能。这样做会导致5V电源与3V3电源短接，可能会导致设备损坏。

###传感器

![9](http://doask.qiniudn.com/openbook7-helicopter47.png)

焊上I2C总线的4K7欧姆上拉电阻R1，R2。确认您在软件里禁用内部上拉电阻（注：在我的代码里已经做了修改）。这允许你无需使用电平转换器就能正常使用3v3 I2C传感器。

焊接SparkFan的加速度计BMA180和ITG-3200陀螺仪扣板。

你也可以选择加装一个气压传感器BMP085扣板和一个hmc5842罗盘扣板。这些都是可选的，你可以配置软件使用他们来做高度保持、航向保持功能。

###输出到电机和伺服

![9](http://doask.qiniudn.com/openbook7-helicopter48.png)

四套3针公头应焊接在“ESC”的标签附近，这是标准的电调脉冲信号连接器。SJ1可以用来连接电调的电压输出以供给主系统（注意，在本设计中，请连接SJ1）

###串行端口

![9](http://doask.qiniudn.com/openbook7-helicopter49.png)

有几个可选的串口设备。该ATmega644PA有两个串口，USART00和USART1，它们在PCB上被标记为SER0和SER1。

采用水平安装公头连接器作为FTDI电缆连接头。这个连接器使用5V电压，RX和TX是+5V电平。使用SJ9和SJ10选择是否将它连接到SER0或SER1。（注意，本设计选择SER0）

###零件清单：

| 零件 | 其他属性 | 标示符 | 所需数量 | 产品实例 |
| -- | -- | -- | -- | -- |
| ATmega644PA | 40 pin plastic DIP | IC1 | 1 | Digikey: ATMEGA644PA-PU-ND |
|     | 40 pin DIP socket | IC1 | 1 |  Digikey: A411-ND |
|4K7 ohm resistor	|0805 or 0603 packaging|1/16 W minimum|	R1, R2, R9, R10	4	|Digikey: P4.7KACT-ND|
|10K ohm resistor	|0805 or 0603 packaging| 1/16 W minimum	|R8, R11, R13|	3	|Digikey: P10KACT-ND|
|15K ohm resistor	|0805 or 0603 packaging| 1/16 W minimum	|R6	|1	|Digikey: P15KACT-ND|
|7K5 ohm resistor	|0805 or 0603 packaging| 1/16 W minimum	|R7	|1	|Digikey: P7.5KACT-ND|
|1K ohm resistor	|0805 or 0603 packaging| 1/16 W minimum	|R3-5, R12|	4	|Digikey: P1.0KACT-ND|
|1 uF capacitor	ceramic| 0805 or 0603 packaging| 16V rating minimum|	C3, C4|	2|	Digikey: 311-1358-1-ND|
|0.1 uF capacitor	ceramic| 0805 or 0603 packaging| 16V rating minimum	|C1, C2, C7, C8, C9	5	Digikey: 311-1361-1-ND
33 uF capacitor	electrolytic, 5.3 mm x 5.3 mm SMD aluminum can, 16V rating minimum	C5, C6	2	Digikey: PCE3886CT-ND
1N4148 diode	MELF packaging	D1	1	Digikey: LL4148DICT-ND
Rectifier Diode	SMB packaging, 20V reverse voltage minimum, high forward current rating	D2	1	Digikey: SSB44-E3/52TGICT-ND
16 MHz resonator		Y1 (16 MHz on PCB)	1	SparkFun: COM-09420
Male headers	0.1 inch pitch	ESC, I2C, I2C-GROUP, BARO, ACCEL, IMU, GYRO, MAG, BATTERY, BATT-MON, ADC1-7, RC1-6, 5V-TAP, 3V3-TAP, SERIAL, FTDI, maybe BLUETOOTH	Buy long strips and break them off
2x3 male headers	0.1 inch pitch, no shroud allowed	AVRISP	1	Use the male headers you buy
TXB0101 bidirectional level shifter	SOT-23-6 package	IC4, IC5	2	Digikey: 296-21664-1-ND
Tactile momentary push button switch SPST	Right angle, 7.50mm x 7.10mm	RESET, BOOT	2	Digikey: P12232SCT-ND
5V regulator	SOT223, SOT-223-3 package	5V-REG	1	LM1117MPX-5.0
3V3 regulator	SOT223 package	3V3-REG	1	LM1117MPX-3.3
LED	3 mm, any colour	LED1-4	4	SparkFun: COM-00533
Barrel jack		Battery	1	SparkFun: PRT-00119
2mm female pin headers		Xbee	2	SparkFun: PRT-08272



如果你想要的只是一个飞行器而不愿不关心细节，那么只需焊接好电路然后下载代码。如果你想知道我如何设计这个电路，请继续阅读这一步。

第一步是绘制电路。

从单片机设计开始，飞控需要一个reset按钮，一个时钟源（16兆赫陶瓷谐振器），一个6针接口，可以用AVR下载工具下载程序，一个串口用来下载Arduino 引导程序，同时需要一个5V电源。

这些传感器连接到单片机的I2C引脚。I2C总线需要4.7K欧姆的上拉电阻上拉到3.3V工作。不能使用ATMEGA内部的上拉电阻，因为内部上拉电阻上拉到5V电源，会损坏传感器。


3.3V电压整流器连接到5V电源。一些电容放置到合适的位置做去耦和滤波。

无线接收机和电调的接口通过3针管脚连接到单片机，这三个管脚总是以“信号，+5V,地”的顺序连接。

以上几步可以完成基本的电路设计，然后我增加了LED指示灯，选择配置功能的跳线，xBee座等等。

画PCB也很简单。先画一个方形边框，然后四个角增加四个槽位，但是我强烈建议你使用安装孔位以便适应不同的机架。下一步，开始在上下两层布地平面。

传感器需要放置成正确的方向，如你所见，它们按照45度角布局。

单片机布置在飞控中心。芯片座放置到合适的地方。电调引脚放置在板子的四个角落，以后方便识别。遥控无线接收机的输入引脚放置在容易连接的位置。外部连接信号如FTDI串行端口放置在边缘，方便连接/断开。发光二极管也放在边缘让它们更加显眼。

记住：

>?	电源线可能需要在PCB上布得宽些

>?	避免走尖角

>?	电容尽可能近地放置到器件边上

>?	尽可能多的做丝印

>?	只是因为你的PCB制造商说他们可以做6mil厚度或间隙，并不意味适合你尝试

>?	0805小型表面贴装元件的焊盘和通孔一样大，为何节省空间和重量，我建议你使用表面贴装元件。对于ATmega644P个头比较大，我用了一个直插封装的版本，因为我想用一个芯片插座方便以后更换芯片。

当你设计完成后，打印出一个1:1比例的复印文件，确保所有的文本都是可以识别出来，所有元件的焊点都是正常的。

不断调整设计，直到你准备好制造PCB。导出Gerber文件并将它们发送给制造商。我通常用Seeed Studio的服务（费用包括：10块28美金+运费+ 5天内送达）。

把电路焊好，这应该很容易

[ro4copter_circuit_pcb_20110624.zip](http://www.instructables.com/files/orig/F4P/PQJV/GP7IF1FC/F4PPQJVGP7IF1FC.zip)

##步骤10：准备微控制器

![s1](http://www.instructables.com/files/deriv/FBR/QYBY/GOW49QYE/FBRQYBYGOW49QYE.LARGE.jpg)

如果你没有Arduino的IDE，就去下载安装(http://arduino.cc/en/Main/Software)。我用的是版本0022。

以下步骤这个维基文章都有详细说明：http://code.google.com/p/ro-4-copter/wiki/generalsoftwaresetup

本文只做一个很简短的总结，在上面讲的非常详细！

Arduino的引导程序应该先被写入控制器。这一步需要一个AVR的编程器。在引导程序写入单片机之后，你只需要一个USB转串行线（如FTDI线）从Arduino IDE下载代码。

引导程序（我已经上传）稍加修改，因为我使用的ATmega644P而不是通常的Arduino的ATmega328P。熔丝位也需要写入正确值。

AVRDUDE是我们给微控制器烧写引导程序.hex文件工具。使用如下avrdude命令来烧写程序

"avrdude -c programmer_name -p atmega644p -U flash:w:bootloader_filename.hex -U lfuse:w:0xFF:m -U hfuse:w:0xD8:m"

留意programmer_name和bootloader_filename！熔丝位用来设置ATmega使用16 MHz谐振器，设置引导程序的大小，使能引导程序，禁用JTAG并启用SPI下载。

同时复制程序文件夹”Ro4Copter_Boot”（记得要删除文件名的日期）到如下目录../ arduino-0022 /hardware/ Arduino /bootloaders/ Ro4Copter_Boot。

下载我提供的Arduino “core”（名字是Ro4Copter_Core记得要删除日期），并把它放到Arduino的“core”目录../arduino-0022/hardware/arduino/cores/Ro4Copter_Core。同时修改boards.txt文件使其包含一个条目使用core。这允许Arduino IDE编译ATmega644P代替原始的ATmega328P。一定要在Arduino IDE菜单选择正确的板子类型。更多细节，请查看我上面提供的wiki页面链接。

该board.txt条目：

```
Ro4Copter.name=Ro4Copter

Ro4Copter.upload.protocol=stk500

Ro4Copter.upload.maximum_size=57344

Ro4Copter.upload.speed=57600

Ro4Copter.bootloader.low_fuses=0xFF

Ro4Copter.bootloader.high_fuses=0xD8

Ro4Copter.bootloader.extended_fuses=0xFF

Ro4Copter.bootloader.path=Ro4Copter_Boot

Ro4Copter.bootloader.file=Ro4Copter_Boot_arduino.hex

Ro4Copter.bootloader.unlock_bits=0x3F

Ro4Copter.bootloader.lock_bits=0x0F

Ro4Copter.build.mcu=atmega644p

Ro4Copter.build.f_cpu=16000000L

Ro4Copter.build.core=Ro4Copter_Core
```

这里的截图显示你所有这些文件夹位置：

![9](http://doask.qiniudn.com/openbook7-helicopter50.png)

你需要编译的Arduino项目是在步骤26中提供的。编译并用引导程序把它下载到ATmega644P。

接下来的步骤将帮助您了解代码是如何做的。也可忽略。

[ro4copter_boot_20110624.zip](http://translate.baiducontent.com/transpage?cb=translateCallback&ie=utf8&source=url&query=http%3A%2F%2Fwww.instructables.com%2Ffiles%2Forig%2FFAX%2FALZ2%2FGPAS296V%2FFAXALZ2GPAS296V.zip&from=en&to=zh&token=&monLang=zh)

[Ro4copter_core_20110624.zip](http://www.instructables.com/files/orig/FKQ/3NUU/GP7IF9TM/FKQ3NUUGP7IF9TM.zip)

##步骤11：源代码解析

![s2](http://www.instructables.com/files/deriv/FS6/81RW/GOW49R00/FS681RWGOW49R00.LARGE.jpg)

和所有的Arduino项目一样，这段代码有一个设置流程和主循环。设置流程初始化所有的配置，主循环执行输入，计算和输出。

从遥控接收机获取的PWM信号被当做PIN脚变化中断信号。当输入引脚的电压发生变化，中断响应程序被调用，产生一个时间戳。脉冲宽度用时间戳之间的差值计算。

输出到电调的PWM信号是使用多个定时器的”比较匹配输出”来产生的，脉冲宽度设置后，定时器将在设定的周期内自动产生相应的脉冲。

传感器采用I2C总线进行通信，这是一种同步数据总线，只使用2线通信就能很好的支持多设备通信。

主环路分为多个任务，分别在不同的频率上执行。这样会给每个任务一个优先级。保持四旋翼稳定的代码具有最高的优先级。与计算机通讯的任务采用低优先级。如果给予GPS较低的优先级，实现导航功能还是有可能的。

![53]()

  之后的步骤将有一些demo，一条一条地讲解相关的技术。

##步骤12：Arduino Demo：PWM输入

![54]()

![s3](http://www.instructables.com/files/deriv/F7N/VALH/GP6NVNWH/F7NVALHGP6NVNWH.LARGE.jpg)

![s4](http://www.instructables.com/files/deriv/F06/VA43/GOW3ZJMO/F06VA43GOW3ZJMO.LARGE.jpg)

![s5](http://www.instructables.com/files/deriv/FPK/7ITK/GPA5SHNF/FPK7ITKGPA5SHNF.LARGE.jpg)

这是一个Arduino的项目，向你展示了如何从几个输入信号里使用PIN变化中断和定时器来读取脉冲宽度。每当检测到引脚的状态变化，中断向量用定时器记录一个时间戳，两个时间戳之间的差异就是是脉冲宽度。

我同时附上一个逻辑分析仪的截图来告诉你我的遥控无线电接收机信号是什么样的。每个脉冲宽度都在1000和2000毫秒之间，周期约20毫秒。

更多的信息，研究一下下面几个概念：

>?	PWM

>?	伺服信号

>?	AVR的ISR（中断服务程序）

>?	PCINT，PIN变化中断

>?	AVR 16位定时器


下面是相关的代码：

```
void setup()
{
  DDRC = 0; // pins as input

  // enable PCINT 18 to 23
  PCICR |= (1 << PCIE2);
  PCMSK2 = 0xFC;

  Serial.begin(115200);
}

typedef struct {
  byte edge;
  unsigned long riseTime;
  unsigned long fallTime;
  unsigned int  lastGoodWidth;
} tPinTimingData;
volatile static tPinTimingData pinData[6 + 1];
volatile static uint8_t PCintLast;

ISR(PCINT2_vect)
{
  uint8_t bit;
  uint8_t curr;
  uint8_t mask;
  uint32_t currentTime;
  uint32_t time;

  // get the pin states for the indicated port.
  curr = PINC & 0xFC;
  mask = curr ^ PCintLast;
  PCintLast = curr;

  currentTime = micros();

  // mask is pcint pins that have changed.
  for (uint8_t i=0; i < 6; i++) {
    bit = 0x04 << i;
    if (bit & mask) {
      // for each pin changed, record time of change
      if (bit & PCintLast) {
        time = currentTime - pinData[i].fallTime;
        pinData[i].riseTime = currentTime;
        if ((time >= 10000) && (time <= 26000))
          pinData[i].edge = 1;
        else
          pinData[i].edge = 0; // invalid rising edge detected
      }
      else {
        time = currentTime - pinData[i].riseTime;
        pinData[i].fallTime = currentTime;
        if ((time >= 800) && (time <= 2200) && (pinData[i].edge == 1)) {
          pinData[i].lastGoodWidth = time;
          pinData[i].edge = 0;
        }
      }
    }
  }
}

void loop()
{
  Serial.println();
  for (byte i = 0; i < 6; i++) {
    Serial.print("C");
    Serial.print((int)i + 1);
    Serial.print(": ");
    Serial.print(pinData[i].lastGoodWidth);
    Serial.print(", ");
  }

  delay(500);
}
```


##步骤13：Arduino Demo：PWM输出

![54]()

![55]()

附件是一个Arduino程序，它教你如何通过代码产生电调转速调节的PWM信号，来快速转动电机。

定时器的定时输出比较功能用来产生PWM信号。当定时器计数到0，它会将某个信号引脚置高，当计数器计数到某一个数值（我们指定的），它会将这个信号引脚置低。这会产生一个具有可变占空比方波。

虽然正常的伺服信号通常具有约20毫秒的周期（即约50赫兹的频率），但我们的微控制器输出的是一个较短周期（因此频率较高，约250至300赫兹）。我的Turnigy Plush电调（ 和山寨hobbyking电调）听说能够处理更高的频率，因此每秒可调整的电机转速更多，四轴飞行器会变得更加稳定。

也附上了从我的逻辑分析仪的截图，图中所示的4个信号是怎样的。

这里是该部分代码：

```
#define PWM_FREQUENCY 300 // in Hz
#define PWM_PRESCALER 8
#define PWM_COUNTER_PERIOD (F_CPU/PWM_PRESCALER/PWM_FREQUENCY)

void setup()
{
  // pins as output
  DDRD |= (1 << 4) | (1 << 5) | (1 << 6) | (1 << 7);

  // default to 1000 microsecond pulse width
  OCR1A = 1000 * 2 ;
  OCR1B = 1000 * 2 ;
  OCR2A = 1000 / 16 ;
  OCR2B = 1000 / 16 ;

  // the setup is:
  // Clear OCnA/OCnB on compare match, set OCnA/OCnB at BOTTOM (non-inverting mode)

  // setup timer 1
  TCCR1A = (1<<WGM11)|(1<<COM1A1)|(1<<COM1B1);
  TCCR1B = (1<<WGM13)|(1<<WGM12)|(1<<CS11);
  ICR1 = PWM_COUNTER_PERIOD;

  // setup timer 2
  TCCR2A = (1<<WGM20)|(1<<WGM21)|(1<<COM2A1)|(1<<COM2B1);
  TCCR2B = (1<<CS22)|(1<<CS21);
  // the period is fixed for timer 2, it's about 244 Hz

  // note that timer1 is a 16-bit timer and timer2 is a 8-bit timer
}

void loop()
{
  int pw;
  for (pw = 1000; pw <= 2000; pw += 20)
  {
    OCR1A = pw * 2 ;
    OCR1B = pw * 2 ;
    OCR2A = pw / 16 ;
    OCR2B = pw / 16 ;

    delay(10);
  }

  for (pw = 2000; pw >= 1000; pw -= 20)
  {
    OCR1A = pw * 2 ;
    OCR1B = pw * 2 ;
    OCR2A = pw / 16 ;
    OCR2B = pw / 16 ;

    delay(10);
  }
}
```

其它材料参见：
>?	atmega168a–PWM脉冲宽度调制
    (http://www.protostack.com/blog/2011/06/atmega168a-pulse-width-modulation-pwm/)

>?	步骤14：Arduino演示：传感器读数

![s6](http://www.instructables.com/files/deriv/FHB/RTWX/GP6NYKL0/FHBRTWXGP6NYKL0.LARGE.jpg)

![s7](http://www.instructables.com/files/deriv/FM9/E3B3/GP7ISC6W/FM9E3B3GP7ISC6W.LARGE.jpg)

![56]()

这一步包括一个Arduino的程序，从两个传感器读数（BMA180加速度计和ITG-3200陀螺仪）。

这两个传感器用I2C总线连接到微控制器。使这种类型的总线设计可允许多个设备采用两根线进行通信。

同时我还附上了逻辑分析仪上的I2C总线信号波形。（截图和Saleae Logic 1.1.8 session文件）

更多的信息：
	http://en.wikipedia.org/wiki/I%C2%B2C
http://www.instructables.com/id/music-playing-alarm-clock/step22

我其他关于I2C的教程

>?	I2C教程-Embedded Lab（http://embedded-lab.com/blog/?p=2583）

>?	BMA180 datasheet
（http://www.sparkfun.com/datasheets/Sensors/Accelerometer/BST-BMA180-DS000-03.pdf）

>?	ITG-3200 datasheet
 http://www.sparkfun.com/datasheets/Sensors/Gyro/PS-ITG-3200-00-01.4.pdf）

飞控软件将初始化该加速度计，先Reset，然后设置10赫兹带宽的低通滤波器并设置加速度读取范围为+/-2G，读一下数据手册，看我的演示代码详情如了解何做这件事。该芯片还有其它特性，比如中断，TAP检测等等，但我们没有使用这些功能。

飞行控制器的软件将初始化陀螺仪传感器，先Reset，然后设置它的10赫兹的低通滤波器，并设置它使用内部振荡器。读一下数据手册，看我的演示代码详情了解如何做这件事。该传感器允许你使用外部振荡器，也有温度数字输出，但我们不使用这些功能。

下面是该程序：

```
#include <Wire.h>

void setup()
{
  Serial.begin(115200);
  Wire.begin();

  Serial.println("Demo started, initializing sensors");

  AccelerometerInit();
  GyroInit();

  Serial.println("Sensors have been initialized");
}

void AccelerometerInit()
{
  Wire.beginTransmission(0x40); // address of the accelerometer
  // reset the accelerometer
  Wire.send(0x10);
  Wire.send(0xB6);
  Wire.endTransmission();
  delay(10);

  Wire.beginTransmission(0x40); // address of the accelerometer
  // low pass filter, range settings
  Wire.send(0x0D);
  Wire.send(0x10);
  Wire.endTransmission();

  Wire.beginTransmission(0x40); // address of the accelerometer
  Wire.send(0x20); // read from here
  Wire.endTransmission();
  Wire.requestFrom(0x40, 1);
  byte data = Wire.receive();
  Wire.beginTransmission(0x40); // address of the accelerometer
  Wire.send(0x20);
  Wire.send(data & 0x0F); // low pass filter to 10 Hz
  Wire.endTransmission();

  Wire.beginTransmission(0x40); // address of the accelerometer
  Wire.send(0x35); // read from here
  Wire.endTransmission();
  Wire.requestFrom(0x40, 1);
  data = Wire.receive();
  Wire.beginTransmission(0x40); // address of the accelerometer
  Wire.send(0x35);
  Wire.send((data & 0xF1) | 0x04); // range +/- 2g
  Wire.endTransmission();
}

void AccelerometerRead()
{
  Wire.beginTransmission(0x40); // address of the accelerometer
  Wire.send(0x02); // set read pointer to data
  Wire.endTransmission();
  Wire.requestFrom(0x40, 6);

  // read in the 3 axis data, each one is 16 bits
  // print the data to terminal
  Serial.print("Accelerometer: X = ");
  short data = Wire.receive();
  data += Wire.receive() << 8;
  Serial.print(data);
  Serial.print(" , Y = ");
  data = Wire.receive();
  data += Wire.receive() << 8;
  Serial.print(data);
  Serial.print(" , Z = ");
  data = Wire.receive();
  data += Wire.receive() << 8;
  Serial.print(data);
  Serial.println();
}

void GyroInit()
{
  Wire.beginTransmission(0x69); // address of the gyro
  // reset the gyro
  Wire.send(0x3E);
  Wire.send(0x80);
  Wire.endTransmission();

  Wire.beginTransmission(0x69); // address of the gyro
  // low pass filter 10 Hz
  Wire.send(0x16);
  Wire.send(0x1D);
  Wire.endTransmission();

  Wire.beginTransmission(0x69); // address of the gyro
  // use internal oscillator
  Wire.send(0x3E);
  Wire.send(0x01);
  Wire.endTransmission();
}

void GyroRead()
{
  Wire.beginTransmission(0x69); // address of the gyro
  Wire.send(0x1D); // set read pointer
  Wire.endTransmission();

  Wire.requestFrom(0x69, 6);

  // read in 3 axis of data, 16 bits each, print to terminal
  short data = Wire.receive() << 8;
  data += Wire.receive();
  Serial.print("Gyro: X = ");
  Serial.print(data);
  Serial.print(" , Y = ");
  data = Wire.receive() << 8;
  data += Wire.receive();
  Serial.print(data);
  Serial.print(" , Z = ");
  data = Wire.receive() << 8;
  data += Wire.receive();
  Serial.print(data);
  Serial.println();
}

void loop()
{
  AccelerometerRead();
  GyroRead();

  delay(500); // slow down output
}
```

[ i2c_waveform.logicdata](http://www.instructables.com/files/orig/FOH/S1IM/GP7ISC6Y/FOHS1IMGP7ISC6Y.tmp)

##步骤15：控制原理

![s8](http://www.instructables.com/files/deriv/FKE/NFMT/GOW49R6K/FKENFMTGOW49R6K.LARGE.jpg）

该代码允许四轴飞行器有两种模式：

一种模式忽略加速度计的数据，你使用操纵杆控制四轴的角速度。这被称为“特技”模式。在这种模式下，代码的工作是当您想要它反转时让四轴反转，当你不想让它停止旋转时，它保持它的状态。这种模式的优势在于可以执行特技翻转和滚动。

另一种模式是使用加速度计和陀螺仪传感器一起来计算四旋翼相对于地面的角度。你可以控制四旋翼的角度。在这种模式下，如果你放开操纵杆，四旋翼应保持在一个水平位置。如果你移动操纵杆偏离中心位置，四轴将旋转到一个角度，然后保持不动（这会导致四轴移动因为桨下的空气是以一个倾角向下吹，而不是垂直向下吹）。这种模式被称为“稳定”模式或“姿态”模式。这种模式需要加速度计工作，不允许您执行翻转和滚动（因为你的摇杆不能360度旋转）。

为了计算四轴的角，角速度，需要利用陀螺传感器读取的角速度乘以时长等于这段时间的旋转角度。在这个过程中由于漂移和陀螺传感器噪声，陀螺仪数据会有很大的误差。要修正这一误差，使用加速度计来测量重力，来与陀螺仪的数据进行融合。关于这方面的更多细节，请参阅：

>?	平衡滤波器（http://web.mit.edu/scolton/www/filter.pdf）

>?	卡尔曼滤波器（http://en.wikipedia.org/wiki/Kalman_filter）

>?	方向余弦矩阵，相关链接
http://en.wikipedia.org/wiki/Rotation_representation_%28mathematics%29

>?	AHRS（航姿系统），姿态航向参考系统
（http://en.wikipedia.org/wiki/Attitude_and_heading_reference_system）

飞控使用此传感器获取的信息连同PID控制器一起控制电动机。PID的意思是”比例，积分，微分”。看下面这些链接：

PID控制器（http://en.wikipedia.org/wiki/PID_controller）
?	http://www.engin.umich.edu/group/ctm/pid/pid.html
?	http://electronicdesign.com/article/analog-and-mixed-signal/what-s-all-this-p-i-d-stuff-anyhow-6131.aspx

为了理解“P”的含义，想象一下你正在停车。如果你离停车位非常远，你要朝停车位开快点。当你开近些的时候，你应该慢下来。如果你开过线了，你需要逆向（因此你的速度是负的）。在这种情况下速度=到该点的距离*“P”。

为了理解“I”的含义，想象你正试图停好你的车，但车是绑在绳子上的，不让你到达停车位。你一直尝试努力挣脱直到绳子断掉。发动机转速=I项乘以这段时间距离停车点的距离的积分。

为了理解“D”的含义，假如你朝停车位移动了10米，因为你知道你向目标靠近了，你应该慢一点。速度=位置变化的差值* D项。

现在想象你不是在停车场停车，而是使用PID控制器，通过控制四个马达的不同转速，控制四旋翼从一个角度转到另一个角度。通过调整PID的值，我们可以让飞控做出相应的大的调整或小的调整，以此来保证四轴在飞行中的稳定。

##步骤16：四轴组装

![s9](http://www.instructables.com/files/deriv/FCM/DZ96/GP5T0RV8/FCMDZ96GP5T0RV8.LARGE.jpg)

![s10](http://www.instructables.com/files/deriv/F1J/C099/GP7IENIY/F1JC099GP7IENIY.LARGE.jpg)

现在你的电路已经完成，你可以组装整个四旋翼。我不能让你在飞机还不能开始飞之前就开始调它。

记得在飞机有任何电路变化都要拔掉电池（任何的电气连接和断开）。

整个框架需要几个步骤来组装，这里有太多的照片，一步一个，我把他们分开。因为套件没有说明书，我会很详细的讲述整个过程。

照片可能不遵循时间顺序，这是因为我可能后续会调整一些步骤。

##步骤17：加强机臂

![s11](http://www.instructables.com/files/deriv/FSU/EUOQ/GP5SZLBO/FSUEUOQGP5SZLBO.LARGE.jpg)

![s12](http://www.instructables.com/files/deriv/FHO/2SOD/GP5T0RV9/FHO2SODGP5T0RV9.LARGE.jpg)

机架套件带有预组装的机臂。虽然关节粘在一起了，但我发现它们不太牢靠，容易开裂。也有一些关节间隙太大。

用一些Gorilla 胶，涂在关节处。当这种胶水固化，它扩大到原来体积的三倍，它会填补裂缝。

通过这些动作，框架的振动量会降低，可以让传感器读数更准。

##步骤18：安装电调到机臂上

![s13](http://www.instructables.com/files/deriv/FZP/JJQU/GP5SZLC8/FZPJJQUGP5SZLC8.LARGE.jpg)

![s14](http://www.instructables.com/files/deriv/F1B/RKSL/GP5T0RVD/F1BRKSLGP5T0RVD.LARGE.jpg)

![57]()

将电调塞到机臂里。这一步要先做，因为我发现在安装电调时，有任何的东西挡着都很难将电调装好。

我将电调的散热片一侧向上安装，希望螺旋桨的气流可以提高电调的散热能力。

确保所有的粗线朝下。细的伺服线朝上。如果你的配件和我一样，所有线的长度应该都正好。

##步骤19：顶板

![s15](http://www.instructables.com/files/deriv/FUZ/3JGG/GOW49R87/FUZ3JGGGOW49R87.LARGE.jpg)

![58]()

![s16](http://www.instructables.com/files/deriv/F30/H0BM/GP6NVO6F/F30H0BMGP6NVO6F.LARGE.jpg)

![s17](http://www.instructables.com/files/deriv/FOD/M4L1/GOW3ZJTY/FODM4L1GOW3ZJTY.LARGE.jpg)

![s18](http://www.instructables.com/files/deriv/FGZ/CG91/GP6NVO6G/FGZCG91GP6NVO6G.LARGE.jpg)

![59]()

![60]()

![61]()

![62]()

确保你有办法安装电路板。你可以在设计电路板时，按照顶板提供的装配孔进行设计，或者在顶板上钻孔。我的电路采用槽来适配任何安装方式。我也有另一个设计，按照我的顶板孔尺寸设计。

如果你在顶板钻孔，你会想到在底板上钻相同的孔位，以便你可以用支架支撑上下板。

拧紧所有的支架孔（因为我购买了套机架，因此有多余的螺丝）。

每一个机臂贴到到顶板上。应该有三个红色的机臂，一个灰色的机臂。灰色的指示“朝前”。板应该有一侧有三角形来指示朝前的方向。确保你安装的灰色“朝前”的机臂指向前面。

每一个臂都应该有一个孔与平板上的孔对应。将螺钉和螺母放在这里。你可以把尼龙螺丝截短以适应组装，同时也让机架看起来更漂亮。

##步骤20：起落架

![s19](http://www.instructables.com/files/deriv/FF0/NI54/GP5SZLFB/FF0NI54GP5SZLFB.LARGE.jpg)

![s20](http://www.instructables.com/files/deriv/FH4/DL5N/GP5SZLFH/FH4DL5NGP5SZLFH.LARGE.jpg)

![s21](http://www.instructables.com/files/deriv/F9H/KCFN/GOW3ZHSJ/F9HKCFNGOW3ZHSJ.LARGE.jpg)

![s22](http://www.instructables.com/files/deriv/FO4/SP34/GP5SZLFN/FO4SP34GP5SZLFN.LARGE.jpg)

![63]()

![s23](http://www.instructables.com/files/deriv/F4M/TKW6/GOW3ZHSG/F4MTKW6GOW3ZHSG.LARGE.jpg)

![64]()

![65]()

![66]()

![67]()

如果底板已经连接到顶板，那么就无法安装脚架。将黑色脚架放入顶板的细槽。这些插槽要安装到左机臂和右机臂。对齐孔，然后用螺丝和螺母固定。

你的套件里应该有一些黑色的细脚架支撑杆，有人用胶水固定，但我是用绝缘胶带粘的。

现在的的滑片放在脚架的底部，有槽，可以固定在那个位置，但他们仍然会很容易掉下来，我用绝缘胶带来固定。

##步骤21：底板

![s24](http://www.instructables.com/files/deriv/FBA/7C5G/GOW49R9H/FBA7C5GGOW49R9H.LARGE.jpg)

![68]()

![s25](http://www.instructables.com/files/deriv/FDT/NAT0/GP5SZLPY/FDTNAT0GP5SZLPY.LARGE.jpg)

![s26](http://www.instructables.com/files/deriv/F85/XQ18/GP5T0RYF/F85XQ18GP5T0RYF.LARGE.jpg)

![69]()

![s27](http://www.instructables.com/files/deriv/FTU/FXY2/GP7IMU8J/FTUFXY2GP7IMU8J.LARGE.jpg)

我制作了两个铝条，安装在底板上。所附的照片里有这些铝条的尺寸。

一些双面尼龙搭扣用环氧树脂粘在其中的一个铝条上。

铝条用螺母垫片固定在底板上，这样就很容易在下一步中将电池绑到底板上。

上下板固定到一起之前，将电源“蜘蛛”式分布在放到顶部和底部的板之间。将线捋顺。

用支架，螺母，螺丝将两个板子固定到一起。

##步骤22：安装电路板

![70]()

在放置顶部之前，应该先安装电路板。使用短的支撑柱和螺丝来固定。你可能需要螺丝垫圈。螺钉可能也需要裁剪到合适的尺寸以便适配支撑柱。

电路板应该有一个方向指示，确保它指向正确的方向。

##步骤23：顶部

![s28](http://www.instructables.com/files/deriv/F63/S6GO/GP5T0S52/F63S6GOGP5T0S52.LARGE.jpg)

![s29](http://www.instructables.com/files/deriv/FVP/5M3M/GOW3ZI2L/FVP5M3MGOW3ZI2L.LARGE.jpg)

![s30](http://www.instructables.com/files/deriv/FVP/5M3M/GOW3ZI2L/FVP5M3MGOW3ZI2L.LARGE.jpg)

![71]()

![s31](http://www.instructables.com/files/deriv/F3E/1BCX/GOW3ZI2M/F3E1BCXGOW3ZI2M.LARGE.jpg)

![z32](http://www.instructables.com/files/deriv/FQS/0ISW/GP5SZM6J/FQS0ISWGP5SZM6J.LARGE.jpg)

在机架套件有8个圆顶支撑件，4个跨接件，和两个小顶板。

支撑件附着在每个臂的两侧。用螺丝和螺母固定。因为机臂的宽度太窄容纳不下两个完整长度的螺钉，你需要要切下来一些。

一些支撑件是灰色的表明那些应该朝前放置，将它固定到前臂上。

 跨接件连接在两支撑件之间。两大顶板将所有支持件夹在中间。用螺丝和螺母拧紧两块顶板。请注意，因为两个板之间的间隙很容易是螺钉拧得过紧。如果拧的过紧会导致板子开裂。

我还是用了一些魔术贴来固定RC无线电接收机和圆顶，这样就可以很容易地安装和拆卸。

##步骤24：安装电机

![72]()

![s33](http://www.instructables.com/files/deriv/F5P/AX1T/GP5SZM83/F5PAX1TGP5SZM83.LARGE.jpg)

![s34](http://www.instructables.com/files/deriv/FGZ/HEUY/GP5T0S5N/FGZHEUYGP5T0S5N.LARGE.jpg)

![s35](http://www.instructables.com/files/deriv/FDO/FLL6/GP7IRXCM/FDOFLL6GP7IRXCM.LARGE.jpg)

每个无刷电机都可以挂在一条支臂上。使用1 / 2“长，# 4-40螺丝（钢质螺丝）和螺母，平垫圈，用弹簧锁紧垫圈来确保这些螺钉是很紧。

准备好之后，开始安装螺旋桨。按照我提供的旋转方向图来安装。

电机和所有硬件先准备好之后来安装螺旋桨。有一个锥形垫圈，确保它在螺母和螺旋桨之间，锥形朝向螺旋桨。锥形是为了保持螺旋桨锁死在中心轴（如果它不为中心，它会应为向心力不平衡而振动）。

##步骤25：最后布线

![s37](http://www.instructables.com/files/deriv/F2X/1MP0/GP5SZM97/F2X1MP0GP5SZM97.LARGE.jpg)

![s38](http://www.instructables.com/files/deriv/FG6/P573/GP7IEY4S/FG6P573GP7IEY4S.LARGE.jpg)


将电机线和电调相连。不需要关心的三根线的顺序。我们将在后面测试后再根据自旋方向来调整连线顺序。

将电调电源连接到配电“蜘蛛”，注意线的极性。

每个电调的伺服线连接到飞控电路上。每个ESC的位置都在PCB附近以便识别。

遥控无线接收机1到6通道必须连到飞控板的相应遥控信号输入上。确保遥控无线接收机是由飞控板的5V供电的。

请小心，不要插反，PCB上已经标识了伺服线应该插入的方向。
如果您使用的是我的电路设计，请参考下面的维基文章了解更详细信息。
（http://code.google.com/p/ro-4-copter/wiki/StartersGuide）。

##步骤26：软件安装

![s39](http://www.instructables.com/files/deriv/F8M/V5ZH/GP6NVNY3/F8MV5ZHGP6NVNY3.LARGE.jpg)

![s40](http://www.instructables.com/files/deriv/F2J/14B8/GPA5SJIO/F2J14B8GPA5SJIO.LARGE.jpg)

![s41](http://www.instructables.com/files/deriv/FCO/VV9N/GPA5SJIN/FCOVV9NGPA5SJIN.LARGE.jpg)

在“准备微控制器”的步骤中，你应该已经将引导程序安装到微控制器中，并准备好Arduino IDE来为ATmega644P编译程序。

连接FTDI转接线（或类似的USB转串口线）到电路板。

附件是一个AeroQuad v2.4.1飞控软件的改进版本。我已经进行修改以适配我的的电路板。下载程序后，打开Arduino的IDE，编译并下载到电路板。

同时请下载并安装aeroquad配置程序（我使用2.7.1版）
-http://code.google.com/p/aeroquad/downloads/list。

下面是你需要做的aeroquad配置一个小总结。更详细的配置，请遵照AeroQuad官方配置指导
（http://aeroquad.com/showwiki.php?title=AeroQuad+Configurator+Manual）。

使用配置软件与电路通信前，要确保一切工作正常。然后按照运行步骤初始化EEPROM。使用气泡水平仪对传感器进行标定。确保传感器能正确响应。确保遥控无线接收机PWM信号可以正常读入。

按照配置软件指导来进行电调校准，（基本上就是关掉电调，点击屏幕上的OK按钮，打开电调，点击另一个OK按钮）。然后就可以检查电机旋转方向是否正常（为安全起见，不要带螺旋桨检查）

[ ro4copter_aeroquad_20110706.zip](http://www.instructables.com/files/orig/F5L/9X3F/GPBDIB39/F5L9X3FGPBDIB39.zip)

##步骤27：远程控制

![s42](http://www.instructables.com/files/deriv/FTX/ONVP/GPBD3LVQ/FTXONVPGPBD3LVQ.LARGE.jpg)

![73]()

![s43](http://www.instructables.com/files/deriv/F06/VA43/GOW3ZJMO/F06VA43GOW3ZJMO.LARGE.jpg)

![s44](http://www.instructables.com/files/deriv/FIK/BPTO/GP7IMU8O/FIKBPTOGP7IMU8O.LARGE.jpg)

![s45](http://www.instructables.com/files/deriv/FAW/CHLU/GP7IRXCS/FAWCHLUGP7IRXCS.LARGE.jpg)

 遥控无线发射机的前4个通道使用两个操纵杆控制。我附上图片向你展示我的发射机的操纵杆各个方向所对应的通道（箭头指示脉冲宽度增加方向）。你的遥控可能不同，但是它们应该有标记。

通常一个操纵杆控制油门和偏航，另一个控制横滚和俯仰。

取决于你是否是左撇子，你可能希望油门和偏航使用左操纵杆或右操纵杆。如果想改，只要交换飞控电路和RC无线电接收器之间的信号线。

如果一个方向反了，遥控发射机应该有一个内置的换向的设置。如果没有，可能买到了世界上最烂的遥控，你应该买一个新的，或者，找到源代码，把信号的输入做一下反转。（或者你可以拆开遥控器自己改线路）。

操纵杆通常都使用弹簧，当你放手时它们可以回中。你可能不想油门有这功能。请参阅您的遥控器的手册，看看弹簧是否可以禁用。在我的遥控器里，我只是把它拆开，拆下弹簧（不要丢了！）。

飞控软件（aeroquad）有一个解锁和锁定状态，在锁定状态，电机被禁用，以确保使用更安全。油门向下然后向左来上锁，油门向下然后向右来解锁。记住这个，你可以在紧急情况下关掉电机。在我的电路板的设计和aeroquad修改后的版本里，LED3指示电机是否解锁。

软件中的5通道开关可以切换“特技”模式和“稳定/姿态”模式。

##步骤28：调整和校准

![s46](http://www.instructables.com/files/deriv/F4W/UOWI/GOW49RJW/F4WUOWIGOW49RJW.LARGE.jpg)

![s47](http://www.instructables.com/files/deriv/FLV/LECX/GP7IFC7R/FLVLECXGP7IFC7R.LARGE.jpg)

![s48](http://www.instructables.com/files/deriv/FJF/5G71/GP58HUAR/FJF5G71GP58HUAR.LARGE.jpg)

![s49](http://www.instructables.com/files/deriv/F2J/14B8/GPA5SJIO/F2J14B8GPA5SJIO.LARGE.jpg)

![s50](http://www.instructables.com/files/deriv/FG4/1YUI/GPBD2TE2/FG41YUIGPBD2TE2.LARGE.jpg)

 给电池充电然后绑到四旋翼上。你必须先学会锁定和解锁飞控，先熟练操作这个，因为你必须在四旋翼出状况时锁定飞控。针对aeroquad软件，油门向下然后向左为锁定，油门向下再向右解锁，对于我的电路和修改软件，LED3亮指示着四旋翼解锁。

这应该是你第一次让电机转起来。一定要抓牢机架以免被旋转的螺旋桨打伤。还要戴上护目镜！调试最好两人一起做，一人拿遥控，一人拿飞机。

首先，看这个：起飞前检查列表
（http://aeroquad.com/showwiki.php?title=Pre-Flight+Checkout+List）

带螺旋桨打开飞机。运行配置程序来校准电调（按照所提供的官方指导）。确保它们与框图中的旋转方向一致。如果某个电机需要反向，回忆一下我前面说的，交换电调和电机之间的线。
有几个参数你可以调整。

发射因子用来调整你的遥控发射机的敏感性。该值取决于你个人的遥控水平和偏好。平滑因子用来给遥控 PWM信号做低通滤波的常数，保持默认值，除非你的发射机有很大噪声。

通过使用默认值进行健全性测试。油门升高应该提高所有4个电机的速度。偏航时会将两个同向电机的转速高于另外两个电机。俯仰或滚动会引起前后或左右两对马达的旋转速度不同。手里拿着飞机，应该能感觉到力量的变化。另外用你的手给飞机施加外力，飞机应该会施加适当的反作用力。如果有哪里不对劲，可能是配置软件中的反转/负向配置错了，用配置软件找到相应的点，并在Arduino的IDE里修改程序。

你还可以调整PID参数以适应你的机架。先从默认值开始，P项先调整，在飞机启动后试着用手转动飞机，提高P值直到飞机能很好地抵抗你手上的力道，如果飞机开始振荡就降低P。所有三个轴都按该步骤做。
aeroquad的文档，不建议你在特技模式调整I项。它还说，“一个负向的D值可以帮助AeroQuad在向前飞行之后快速变换到水平位置。在“姿态”飞行模式可能要将D 设为0。根据用户的喜好来使用负D值。”

我总结的版本对PID参数整定是很短的，更多的细节，请参阅：
http://aeroquad.com/showwiki.php?title=PID+Tuning

请参看官方aeroquad配置文件了解更多调节的细节。
（http://aeroquad.com/showwiki.php?title=AeroQuad+Configurator+Manual）

##步骤29：飞行

![s51](http://www.instructables.com/files/deriv/F68/71BS/GP5T0S8F/F6871BSGP5T0S8F.LARGE.jpg)

现在，所有都调好了，你应该准备好爽飞了！既然你已经调好了机，你应该已经熟悉如何控制飞机。给电池充好电，绑上电池，开赴飞场。

离飞机远些，带上护目镜。螺旋桨真的很危险。不要试图用手“助推”飞机，以防在离手的一瞬间切到你的脖子。

尝试在室外无风条件下飞。如果你想在室内飞，找一个大房间，因为在小房间离飞机向下吹的空气会导致太多乱流。

购买足够的备用螺旋桨，螺旋桨坏得最快，而且，从HobbyKing买的胶合板机架不是很耐用，考虑到带四个机臂的机架套件才卖是15美元，你就多买几个备用就好了。

在飞行中留意你的电池监视器。当你的电池包的三块电池中的任何一个放电到安全阈值以下，你必须立即着陆，否则你会烧坏的电池。

相关阅读：

>?	开始飞行技巧
（http://aeroquad.com/showwiki.php?title=Beginning+Flight+Tips—）- AeroQuad Wiki

##步骤30：修复，重调，充电

![s52](http://www.instructables.com/files/deriv/FI1/HKY8/GP7IFKOM/FI1HKY8GP7IFKOM.LARGE.jpg)

![74]()

![s53](http://www.instructables.com/files/deriv/F0B/STKM/GPAS1TIO/F0BSTKMGPAS1TIO.LARGE.jpg)

![s54](http://www.instructables.com/files/deriv/FD8/03G7/GP7IF17X/FD803G7GP7IF17X.LARGE.jpg)

![s55](http://www.instructables.com/files/deriv/FH3/JN22/GOW3ZDFP/FH3JN22GOW3ZDFP.LARGE.jpg)

![75]()

![76]()

![77]()

如果飞行时候坠落，你可能会摔坏四轴的机臂。没什么好办法修复，所以只需拆掉并更换机臂。

坠机后，检查电池是否损坏。损坏的锂电池可能爆炸！坠机后，如果电池还没爆炸，立刻断开电源，检查一下，判断是否可以继续使用它。如果有任何的变形，膨胀，弯曲，破洞，或断开，那就扔进垃圾桶。
坏掉的螺旋桨很容易更换，但要确保轴或电机没有损坏。先拆螺旋桨，然后让电机低速运行看是否仍然发挥作用，没有任何的歪斜。如果不稳定的，那么你应该更换电机，不要冒险飞行，否则在空中出现状况就太危险了。
如果你不喜欢这些飞行特性，那就重新将飞控连接到您的计算机，并重新调整参数直到你满意。
如果你发现不管你如何努力调整配置参数你的飞机都不能保持稳定，那么可能是机体振动太大导致传感器不能有效读数。振动是由于围绕旋转电机的质量不平衡引起的。当质量的平衡，在电机的净向心力是0。当出现不平衡，向心力将大于0并且这个角度会推动它旋转。便会导致振荡，这就是振动的原因。为了消除振动，我们需要应用一个大小相等方向相反力来平等。每个电机带两个扎带。安装螺旋桨时，调整扎带的位置，直到他们消除了振动。
给电池充电其实相当简单。我的充电器很棒，一应俱全，但它不带手册（后来发现了一个在线版本）。有两个主电源线（正端和负端）从电池的主电源线（粗的）连接到充电器（两个香蕉插孔），按照正确的方向连接。然后将电池的JST平衡连接器连接到充电器的香蕉头连接器。锂电池的特点之一是，每块电池都要检测以保证与其它的电源单元保持充电平衡，这也是你的电池有2种不同连接器的原因。
将电池放到锂电袋内（这是一种充电时用来防火的包，你很容易购买到），或像我一样把它们放在一个煎锅里。在充电器上，配置锂电常规充电模式，电池选3S，并开始充电过程。这个过程应该是自动的，但在充电过程中你不应该完全不理电池充怎样了。
如果充电器用6安培的电流充电，那么就要有一个可以处理6安培的电源（我使用一台笔记本电脑电源适配器）。最普通的电源适配器？无法应付如此大的电流，不要犯这样的错误。
请按照1C或更小的充电速度充电，这意味着如果电池5000毫安，那么你就可以按照5安培的电流充电，但如果是2200毫安时，你只能用2.2安培的电流充电。最大电流除以蓄电池容量C等级。更多信息，看这份锂电池的指南
（http://www.rcgroups.com/forums/showpost.php?p=1315199&postcount=1）。

##步骤31：最后的想法

你也可以用MultiWiiCopter（http://www.multiwiicopter.com/）飞控软件
来尝试，它与AeroQuad非常相似。MultiWiiCopter支持更多的机身结构如三轴，6轴，8轴。他的图形用户界面也不同（基于processing.org（http://www.processing.org/），是用Java做的，比起基于AeroQuad的LabVIEW的配置界面，我更喜欢它）。

为了拍飞行视频，我花了5美元买了一个像这样的一个钥匙扣摄像机
（http://www.chucklohr.com/808/）。它很便宜，很轻便。不要用尼龙搭扣安装摄像头因为振动会毁了你的视频，用魔术胶带。

飞控套件带有一个摄像头配件，你可以给它连一个伺服，但我没用它。同时，很明显飞机起落架是框架很好的搭配，见下面链接。
（http://www.hobbyking.com/hobbyking/store/uh_viewItem.asp?idProduct=13138）

我的教学集中于配置+字飞行模式，但你可以轻松地将您的飞机飞行配置成X模式。它需要重新配置并重新编译源代码。电路板需要旋转45度安装。脚架需要旋转45度重新安装（机架有X配置模式的安装槽位和配件）。
你可能已经看完了我的两个飞行控制器的电路设计。我的设计允许你添加磁罗盘传感器和气压压力传感器。这些传感器可以用来让你的软件获得航向和高度的数据，它可以帮助保持直升机朝向一个方向，待在一个高度。GPS也可以加进来。这些额外的传感器的组合可以帮助提高自主飞行能力。AeroQuad代码和MultiWiiCopter代码都可以支持这些传感器。

**致谢：**
>?	aeroquad（http://aeroquad.com/）

>?	multiwiicopter（http://www.multiwii.com/）

>?	Arduino（http://arduino.cc/）

>?	维基百科（http://www.wikipedia.org/）

>?	wpclipart（http://www.wpclipart.com/working/work_supplies/shopping_cart_racing.png.html）

>?	Bird Clipart
（http://www.birdclipart.com/bird_clipart_images/bird_in_flight_outline_drawing_coloring_page_0071-0903-1511-2308.html）

>?	更多信息

>?	四轴飞行器和tricopter信息超级链接索引
（http://www.rcgroups.com/forums/showthread.php?t=1097355）

>?	rcgroups -多旋翼直升机
（ http://www.rcgroups.com/multi-rotor-helis-659/）

