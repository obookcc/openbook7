# 铁甲钢拳2

![](http://doask.qiniudn.com/openbook7-realsteel1.JPG)

在上一期的文章中我们已经完成了格斗机器人的基本功能，但它的造型还有点简陋。如果大家留意杂志上的人形机器人套件，会发现套件中的机器人多了图1中的几个部分——胸甲、背甲和手。手的小结构件可以替代之前用PVC材料制作的手，胸甲和背甲可以将控制板包裹起来，保护控制板的同时也让人形机器人更加的好看。

![](http://doask.qiniudn.com/openbook7-realsteel2.JPG)
>图1 胸甲、背甲和手

除了把机器人的外观做的更加好看之外，我们还想让它在动作的时候发出声音，就像游戏拳皇或者街霸一样。这就需要对之前的制作进行一些修改,上一期完成后的机器人结构示意图如图2所示。

![](http://doask.qiniudn.com/openbook7-realsteel3.jpg)
>图2 控制示意图

为了实现上面提到的功能，我们就不能让手柄的控制信号通过无线模块直接发给舵机控制板了，而是需要一个模块在收到手柄的信号之后，一方面可以发送指令给舵机控制板，另一方面可以实现语音播放。改造后的机器人端连接示意图如下。

![](http://doask.qiniudn.com/openbook7-realsteel4.jpg)
>图3 改造后的机器人端连接示意图

因为机器人背甲和胸甲内的空间有限，所以我们选用了小巧的Flyduino来作为中间的信号处理板，如图4所示。Flyduino是一款基于A
rduino的微型控制器，大小只有40mmX24mm，集成了12个数字口，8个模拟口，1个XBee无线数传接口可直接连接我们的无线模块。


![](http://doask.qiniudn.com/openbook7-realsteel5.jpg)
>图4 Flyduino

另外，让机器人发声的语音模块我们选择的是DFRduino Player V2.0语音播放模块，如图5所示。该模块支持TTL电平串口，只需要将语音文件放置在SD卡中，就可以通过串口发送文件名信息播放相应的语音文件。同时模块还提供一个播放完毕提示端口，当播放完一首歌曲后，会输出一个高电平。

![](http://doask.qiniudn.com/openbook7-realsteel6.jpg)
>图5 DFRduino Player V2.0语音播放模块


器件选定以后，就可以开始我们的改造工作了。

##第一步，安装语音播放模块。
安装模块之前我们首先要将声音文件保存在存储卡中，根据机器人的动作我们找了以下几种动作的声音，并按照下表给各个文件命名。大家也可以根据自己的喜好选择拳皇、街霸之类游戏里的声音，但文件名一定要对上。


|序号|	动作说明|	声音文件名|	备注|
| -- | -- | -- | -- |
|1	|机器人移动|	yidong.mp3|包括前进、后退、左转、右转、滑步、蹲下起身|
|2	|出拳|	quan.mp3|	包括左右手|
|3	|摆臂|	bai.mp3|	包括左右手|
|4	|跌倒后起身|dao.mp3|	包括向前和向后|

声音文件拷贝完成后，我们将模块和扬声器都放在胸甲内（图1右上角），固定语音模块的螺丝刚好将扬声器卡住，然后用排线将模块的控制引脚和电源引出，如图6所示。这里要说明一点，本身套件中的胸甲是没有发声孔和模块安装孔的，这些需要用电钻自己钻。

![](http://doask.qiniudn.com/openbook7-realsteel7.JPG)
>图6 安装语音模块

连接语音模块的线缆需要从机器人的胸腔中穿过，以连接后面的Flyduino。

![](http://doask.qiniudn.com/openbook7-realsteel8.JPG)
>图7 控制线缆穿过胸腔

最后将胸甲固定在机器人身上，如图8所示。

![](http://doask.qiniudn.com/openbook7-realsteel9.JPG)
>图8 固定机器人的胸甲

##第二步，安装Flyduino。
由于Flyduino没有安装孔，所以我们利用它背面的XBee无线数传接口将其固定在背甲上。我们先在背甲上开两个槽，其大小与无线数传接口一致。如图9所示。

![](http://doask.qiniudn.com/openbook7-realsteel10.JPG)
>图9 背甲上的安装槽

然后将Flyduino固定在背甲上，最后插上之前用到的无线模块。如图10、图11所示。

![](http://doask.qiniudn.com/openbook7-realsteel11.JPG)
>图10 固定好Flyduino

![](http://doask.qiniudn.com/openbook7-realsteel12.JPG)
>图11 插上无线模块

##第三步，将Flyduino、语音播放模块和舵机控制板3部分连接起来。

由于模块之间都是通过TTL串口通信，而Flyduino只有一个硬件的TTL串口，且用在了与无线模块的通信上，所以需要用软件在Flyduino端模拟两个串口。在Arduino的库中有一个SoftwareSerial库，定义了一个SoftwareSerial 的类，可以实现模拟串口，我们在代码中定义两个SoftwareSerial 的对象mySerial1和mySerial2，mySerial1占用引脚2、3，用来给语音播放模块发送指令；mySerial2占用引脚4、5，用来给舵机控制板发送指令。定义对象的代码如下。

```
SoftwareSerial mySerial1(2, 3);	//模拟串口1,用来发送音频指令
SoftwareSerial mySerial2(4, 5);	//模拟串口2,用来发送动作指令
```

对照上一期中的动作表以及这次的声音表，我们能够得到一张声音动作对照表。

|序号|	动作说明|	触发指令|	声音文件|
|--|--|--|--|
|1|	机器人前进一步	|1|	yidong.mp3|
|2|	机器人后退一步	|2|	yidong.mp3|
|3|	机器人左转|	15|	yidong.mp3|
|4|	机器人右转|	14|	yidong.mp3|
|5|	机器人向左滑动一步|	8|	yidong.mp3|
|6|	机器人向右滑动一步|	7|	yidong.mp3|
|7|	机器人下蹲|	12|	yidong.mp3|
|8|	机器人起身|	13|	yidong.mp3|
|9|	左拳|	6|	quan.mp3|
|10|右拳|	5|	quan.mp3|
|11|左臂摆动|	4|	bai.mp3|
|12|右臂摆动|	3|	bai.mp3|
|13|向前倒下后起身|	9|	dao.mp3|
|14|向后倒下后起身|	16|	dao.mp3|


根据这张表能够很方便的完成Flyduino中的程序，源代码如下：

```
#define MusicEnd 6		//用引脚6来检测声音文件是否播放完毕

SoftwareSerial mySerial1(2, 3);	//模拟串口1，用来发送音频指令
SoftwareSerial mySerial2(4, 5);	//模拟串口2，用来发送动作指令

char w;						//接收字符
int flag=1;					//音频结束标志，初始为1

void setup()
{
  Serial.begin(57600);			//串口波特率，57600
  mySerial1.begin(19200);		//模拟串口1波特率19200
  mySerial2.begin(9600);		//模拟串口2波特率9600
  pinMode(MusicEnd, INPUT);	//第6脚接收音频结束标志
}

void loop()
{
  if(Serial.available()>0)			//判断串口是否收到数据
  {
    w = Serial.read();			//读取串口数据
    if(flag==1)				//当音频结束标志位为1的时候，播放音频
    {
    	if(w==1||w==2||w==7||w==8||w==12||w==13||w==14||w==15)
{
      		//播放“yidong”这个音频文件，\r\n表示换行回车
mySerial1.print("\\yidong\r\n");
}
    	else if(w==3||w==4)
      	mySerial1.print("\\bai\r\n");		//摆臂
    	else if(w==5||w==6)
      	mySerial1.print("\\quan\r\n");	//出拳
        else if(w==9||w==16)
      	mySerial1.print("\\dao\r\n");		//跌倒起身
    }
    if(w!=0)
    {
  		mySerial2.write(w);			//动作指令通过TTL电平串口直接发给机器人
    	flag=0;					//音频结束标志置0
  	}
  	if(digitalRead(MusicEnd) == LOW)
flag=1;						//检测到语音播放完毕后将标志位至1
}

```
![](http://doask.qiniudn.com/openbook7-realsteel13.JPG)
>图12 连接各个部分

代码完成并下载到Flyduino中以后，按照图12完成这几部分的连接，Flyduino和语音播放模块的电源均取自舵机控制板，语音播放模块的排线连接到Flyduino的引脚2、3、6，舵机控制板的TTL串口连接到Flyduino的4、5。

最后一步，将背甲安装在机器人身上，换上套件中手的小结构件。如图13所示。

![](http://doask.qiniudn.com/openbook7-realsteel14.JPG)
>图13 安装上背甲

至此，我们的格斗机器人就算完成了。开始战斗吧！！


大家可以在这里看到一段两个机器人格斗的视频，很有意思哦。

<iframe height=498 width=510 src="http://player.youku.com/embed/XNDEyNTcxNTc2" frameborder=0 allowfullscreen></iframe>
