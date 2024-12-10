


@[TOC](文章目录)






# 一.串口通信的基本原理

## 1.串口协议和RS-232标准 

### 串口通讯 

串口通讯 (Serial Communication)是一种设备间非常常用的串行通讯方式，电子工程师在调试设备时也经常使用该通讯方式输出调试信息。 通讯协议，我们以分层的方式来理解，最基本的是把它分为物理层和协议层。物理层规定通讯系统中具有机械、电子功能部分的特性，确保原始数据在物理媒体的传输。协议层主要规定通讯逻辑，统一收发双方的数据打包、解包标准。

### RS-232 

RS-232是现在主流的串行通信接口之一。由于RS232接口标准出现较早，难免有不足之处，主要有以下四点：  
（1）接口的信号电平值较高，易损坏接口电路的芯片。RS232接口任何一条信号线的电压均为负逻辑关系。即：逻辑“1”为-3— -15V；逻辑“0”：+3— +15V ，噪声容限为2V。即要求接收器能识别高于+3V的信号作为逻辑“0”，低于-3V的信号作为逻辑“1”，TTL电平为5V为逻辑正，0为逻辑负 。与TTL电平不兼容故需使用电平转换电路方能与TTL电路连接。  
（2）传输速率较低，在异步传输时，比特率为20Kbps；因此在51CPLD开发板中，综合程序波特率只能采用19200，也是这个原因。  
（3）接口使用一根信号线和一根信号返回线与地线构成共地的传输形式，这种共地传输容易产生共模干扰，所以抗噪声干扰性弱。  
（4）传输距离有限，最大传输距离标准值为50英尺，实际上也只能用在15米左右。  
3.RS232电平与TTL电平的区别

### （1）.TTL电平标准 

输出 L： 《0.8V ； H：》2.4V。

输入 L： 《1.2V ； H：》2.0V

TTL器件输出低电平要小于0.8V，高电平要大于2.4V。输入，低于1.2V就认为是0，高于2.0就认为是1。于是TTL电平的输入低电平的噪声容限就只有（0.8-0）/2=0.4V，高电平的噪声容限为（5-2.4）/2=1.3V。

### （2）具体区别 

1.电平的上限和下限定义不一样，CMOS具有更大的抗噪区域。 同是5伏供电的话，ttl一般是1.7V和3.5V的样子，CMOS一般是2.2V，2.9V的样子，不准确，仅供参考。

2.电流驱动能力不一样，ttl一般提供25毫安的驱动能力，而CMOS一般在10毫安左右。

3.需要的电流输入大小也不一样，一般ttl需要2.5毫安左右，CMOS几乎不需要电流输入。

4.很多器件都是兼容TTL和CMOS的，datasheet会有说明。如果不考虑速度和性能，一般器件可以互换。但是需要注意有时候负载效应可能引起电路工作不正常，因为有些ttl电路需要下一级的输入阻抗作为负载才能正常工作。

1.  TTL电路和CMOS电路的逻辑电平  
    VOH： 逻辑电平 1 的输出电压  
    VOL： 逻辑电平 0 的输出电压  
    VIH ： 逻辑电平 1 的输入电压  
    VIH ： 逻辑电平 0 的输入电压

6.TTL和CMOS的逻辑电平转换  
CMOS电平能驱动TTL电平  
TTL电平不能驱动CMOS电平，需加上拉电阻。

## 2. USB/TTL转232"模块（以CH340芯片模块为例）的工作原理。 

TXD：发送端，一般表示为自己的发送端，正常通信必须接另一个设备的RXD。

RXD：接收端，一般表示为自己的接收端，正常通信必须接另一个设备的TXD。  
正常通信时候本身的TXD永远接设备的RXD！

自收自发：正常通信时RXD接其他设备的TXD，因此如果要接收自己发送的数据顾名思义，也就是自己接收自己发送的数据，即自身的TXD直接连接到RXD，用来测试本身的发送和接收是否正常，是最快最简单的测试方法，当出现问题时首先做该测试确定是否产品故障。也称回环测试。

电平逻辑：

TTL电平：通常数据表示采用二进制，规定+5V等价于逻辑“1”，0V等价于逻辑“0”，称作TTL信号系统，是正逻辑

RS232电平：采用-12V到-3V，等价于逻辑“0”，+3V到+12V的逻辑电平，等价于逻辑“1”，是负逻辑的。

产品说明：

1、主芯片为CP2102，安装驱动后生成虚拟串口

2、USB取电，引出接口包括3.3V（《40mA），5V，GND，TX，RX，信号脚电平为3.3V，正逻辑

3、板载状态指示灯、收发指示灯，正确安装驱动后状态指示灯会常亮，收发指示灯在通信的时候会闪烁，波特率越高亮度越低

4、支持从300bps~1Mbps间的波特率

5、通信格式支持：1）5，6，7，8位数据位；2）支持1，1.5，2停止位；3）odd，even，mark，space，none校验

6、支持操作系统：windowsvista/xp/server2003/200，MacOS-X/OS-9，Linux

7、USB头为公头，可直接连接电脑USB口

8、贴片元件为SMT工艺生产，质量稳定

9、不含USB头体积为：33\*15（mm）



usb转ttl电路图（一）：USB转3线制RS232串口  
图中也是USB转3线制RS232串口，只是输出RS232信号的电平幅度略低。CH340的R232引脚为高电平，启用了辅助RS232功能，只需外加二极管、三极管、电阻和电容就可代替7.2.节中专用的电平转换电路U5，所以硬件成本更低。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0aa9c33dc96d47038bbd65a7a5ce3e0f.png)


usb转ttl电路图（二）：USB转RS232串口  
图中是USB转最基本也最常用的3线制RS232串口，U5为MAX232/ICL232/SP232等。
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/17d30af787c24f5e8e8006016fe4a043.png)


usb转ttl电路图（三）  

USB转串口设计原理

![usb转ttl电路图大全（RS232/串口/CH340T/PL2303）](https://i-blog.csdnimg.cn/direct/53a94651834f4f989380d8fc557f4945.png)


USB接口：主要由3部分组成：USB接头、USB供电、USB数据收发，其原理如如下：

usb转ttl电路图大全（RS232/串口/CH340T/PL2303）

1.USB接头：提供USB物理接口，通过USB线与USB设备进行连接。

2.USB供电：整个USB转串口线无需外接电源，直接使用USB供电即可。

3.USB数据收发：USB接口与USB转串口芯片主芯片（PL2303）的通讯。

USB转串口主芯片：USB转串口主芯片模块，USB转串口主芯片是电路的核心部分，提供USB和串口的桥转换，它主要由三个部分组成，分别是USB转串口芯片PL2303、PL2303工作晶振和PL2303外围电路。

1.USB转串口主芯片：USB转串口芯片内部功能框图如下：

usb转ttl电路图大全（RS232/串口/CH340T/PL2303）

2.PL2303工作晶振：提供PL2303工作时钟，最大支持12M频率。

3.PL2303外围电路：依据PL2303数据手册要求添加外围电路，具体各外围电路功能，见PL2303手册。

RS232接口：RS232接口部分实现串口RS232电平与TTL电平的转换。模块原理图如下，主要由2个部分组成，SP232EH芯片、串口接口。

1.SP213EH芯片：将SP2303的TTL电平的串行接口，转换成普通的RS232电平。以及将普通的RS232电平电平转换成TTL电平串行接口。
2.标准的DB9公头，可以直接设备进行数据通信。

# 二.串口通信文件互传

## 1.串口传输文件 

需要把波特率设置为一样。  
正确连接硬件，打开串口调制工具，并且打开两次SSCOM软件，选择两个不同的串口。  
硬件链接  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2c4397bc81054d4c9f65a4cbc2edfc8d.png)

  


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/50829e72523747858dfc3b15ea43b4fc.png)

进行如图连接  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/3eaf78e641314ab2b821ccb9ba47d6b9.png)



打开调试软件，开始发送一张图片  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2b96a571612e423597339d1e458001a6.png)

接收端接受文件的设置如下图所示勾选：  
![!\[在这里插入图片描述\](https://i-blog.csdnimg.cn/blog_migrate/6a82e71ab882181893827cb066211e39.pn](https://i-blog.csdnimg.cn/direct/45622f21940a4d2cacc2b608f9545603.png)
g)  
接收端如图所示：  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5a1c98df25714b94b133ee895862db38.png)

然后把收到的txt进行转换就好了

## 2.分析

1.在传输过程中，可以看出，文件的传输速率在一定程度上与波特率相关，在一定范围内，与波特率成正相关，倍数增长，传输消耗时间减少  
2.在通过串口传输文件中，通过调大波特率（我后面又试了256000等波特率）可以一定程度提高传输速率，但是设定波特率过大，并不会再度产生较明显的传输速度增长  
3.在提高传输速率进行传输时，丢包率也会上升，重传时间增加，导致传输速率虽高，但是传输准确率降低，在一定程度上延长了传输时间
4.如果只连接TX和RX，而不连接电源和地线，文件传输将无法正常工作，大多数电子设备，包括用于文件传输的设备，都需要电源来供电。电源线（VCC）提供设备运行所需的电压，而地线（GND）则提供参考电压点，确保电子元件能够正确工作。没有电源，设备内部的电子元件无法正常工作，因此无法进行数据传输，虽然信号线（TX和RX）可以传输数据，但它们需要稳定的电源来维持信号强度和稳定性。如果电源不稳定或完全缺失，可能会导致信号衰减、噪声增加，从而影响数据传输的可靠性和速度。


# 三、串口实现STM32系统和上位机通信

## 1.实验目的 

1. 了解 USART 的基本特性；

2. 掌握STM32串口通信的基本原理，了解异步通信的概念；

3. 掌握用库函数操作 USART 的方法，学习编程实现STM32的USART通信；

4. 掌握如何使用 STM32 的串口发送和接收数据。

## 2.实验内容 

1. 用STM32设计一个与计算机进行串口通信的实验，实现查询方式串口的收发功能，接收来自串口的字符并将接收到的字符发送到超级终端，用串口调试助手显示出来，查询方式完成。使用STM32单片机完成基本的IO口控制，了解嵌入式系统中的中断处理方法、定时器、串口通讯等内容。

	`串口通信是指外设和计算机间，通过数据信号线 、地线、控制线等，按位进行传输数据的一种通讯方式，如SPI通信、USART通信、EEPROM通信等。简单讲，串口通信实现了上位机（PC）与下位机（如STM32）之间的信息交互。`

上位机（PC）通过串口调试助手等实现数据的接收和发送；

下位机（STM32）通过printf()、getchar()等函数实现字符或字符串的接收和发送。

2. STM32串口与电脑USB口通信示意图

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/eaeff6d73f10446589ff25f23791d38d.png)


## 3.实验器材（设备、元器件） 



UART：通用异步收发器  
USART：通用同步/异步收发器（两种模式可切换）  
STM32F103系列提供5路串口，包含3个 USART 和2个 UART 。

## 4.实验步骤 

1. 串行接口 (Serial Interface)简称串口，也称串行通信接口或串行通讯接口（通常指COM接口），是采用串行通信方式的扩展接口。串口通信指串口按位（bit）发送和接收字节。尽管比特字节（byte）的串行通信慢，但是串口可以在使用一根线发送数据的同时用另一根线接收数据。串口通信协议是指规定了数据包的内容，内容包含了起始位、主体数据、校验位及停止位，双方需要约定一致的数据包格式才能正常收发数据的有关规范。在串口通信中，常用的协议包括RS-232、RS-422和RS-485。

（1）数据通信按数据传输方向分类：单工通信、半双工通信、全双工通信

单工通信：数据只能沿一个方向传输；

半双工通信：数据可以沿两个方向传输，但需要分时进行；

全双工通信：数据可以同时进行双向传输；

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/3a0d1f6a682b478ebaf46bddb5089335.png)


（2）按照通信方式，分为：同步通信，异步通信

同步通信：带时钟同步信号传输。比如：SPI，IIC通信接口；

异步通信：不带时钟同步信号。比如：UART(通用异步收发器)，单总线。通信双方约定相同波特率来保证数据传输和接收的有效性；常见的异步串行通信方式有Uart、Usart。

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d069d2cbcd1e403bb59e53177ba40616.png)
（3）数据通信按数据通信方式分类：串行通信、并行通信

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/822eeda6bc1f49d8bd34462fa79aa31e.png)


（4）常见的串行通信接口

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/21b5fad4205d4fe08a0e50e0c3037353.png)





(5)数据传输的格式/通信协议 

串行通信一定要有适合的通信协议。

通信协议指通信双方之间为完成信息交互所必须遵守的一种规则和约定。比如两个人约定在何时交流、用中文还是英文交流、交流什么内容。

1.起始位

当未有数据发送时，数据线处于逻辑“1”状态；先发出一个逻辑“0”信号，表示开始传输字符。

2.数据位

紧随起始位之后，数据位表示真正要发送或接收的信息，位数一般有8位或9位

3.奇偶校验位

数据位末尾可以选择是否添加奇偶校验位，用于检测数据传输是否正确

4.停止位

代表信息传输结束的标志位，可以是1位，1.5位或2位。停止位的位数越多，数据传输的速率也越慢。

5.波特率设置

波特率表示每秒钟传输码元的个数，是衡量数据传输速率的指标，单位Baud。另外有个名词叫比特率，比特率表示每秒钟传输二进制位bit的个数，单位 bit/s。

比特(bit)就是指一位信息，当用二进制表示数据时，0是一位，1也是一位信息，它是固定不变的，一个比特就代表二进制下的一位。

通常描述码元，我们会说M进制的码元。比如八进制，我们知道八进制包含0~7共八种数据，而计算机是只识别0,1两种的，我们若是想将这八种数据发送给计算机，可以用3个比特为一组的形式来表示，即000,001,…,111共八组，因而一个八进制的码元就表示携带了3个比特，这时的比特率也就是波特率的3倍。那么，一个M进制的码元，就携带log2 M个比特。

(6)USART的使用步骤 

串口设置的一般步骤可以总结为如下几个步骤：

1）串口时钟使能 GPIO 时钟使能

2）串口复位

3）GPIO 端口模式设置

4）串口参数初始化

5）开启中断 并且初始化 NVIC（如果需要开启中断才需要这个步骤)

6）使能串口

7）编写中断处理函数


## 5.通过STM32CubeMX配置项目 

（1）打开STM32CubeMX，在主界面点击：ACCESS TO MCU SELECTOR:  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/3c4f1af8a4024e039e60aa2bba78cb41.png)


（2）选择的单片机型号STM32F103C8T6以及点击开始工程项目：  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/afdcdd85ec8541e2bc70cc1702335ced.png)


(3)在RCC下开启高速时钟(HSE)选择外部晶振：  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/29f0af6ec27b4ec4ac85099e9c44c339.png)


（4）在SYS下选择串口线  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e834d471d18c45ab8369fe4ad0b1f8c4.png)


（5）选择串口USART1，设置MODE为异步通信(Asynchronous)  
基础参数：波特率为115200 Bits/s。传输数据长度为8 Bit。奇偶检验无，停止位1 接收和发送都使能。  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1736669010954814add3a377d050e2a1.png)


（6）设置NVIC使能接收中断  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8498ba0e41794c29bcbf14394257347d.png)


（7）设置项目名称，存储路径以及选择所用IDE，最后创建项目工程文件

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d041ea07776e446fb37bd64db9817ed2.png)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/cc8a90701583441ea64190c67c3e2762.png)


## 6.HAL库USART函数库介绍 

### 1.串口发送/接收函数 

HAL\_UART\_Transmit()：串口发送数据，使用超时管理机制;  
HAL\_UART\_Receive()：串口接收数据，使用超时管理机制;  
HAL\_UART\_Transmit\_IT()：串口中断模式发送;  
HAL\_UART\_Receive\_IT()：串口中断模式接收;  
HAL\_UART\_Transmit\_DMA()：串口DMA模式发送;  
HAL\_UART\_Transmit\_DMA()：串口DMA模式接收;

这几个函数的参数基本都是一样的，我们选择两个在代码中用到的详细说明一下：

#### 1.1.串口发送数据函数 

函数说明：  
HAL\_UART\_Transmit(UART\_HandleTypeDef \*huart, uint8\_t \*pData, uint16\_t Size, uint32\_t Timeout);

功能：

> 串口发送指定长度的数据。如果超时没发送完成，则不再发送，返回超时标志（HAL\_TIMEOUT）。

举例：  
HAL\_UART\_Transmit(&huart1, (uint8\_t \*)ZZX, 3, 0xffff);  
上述函数的意思是串口USART1发送三个字节数据，最大传输时间是0xffff

#### 1.2.中断接收数据函数 

中断接收数据：  
HAL\_UART\_Receive\_IT(UART\_HandleTypeDef \*huart, uint8\_t \*pData, uint16\_t Size);

功能：

> 串口中断接收，以中断方式接收指定长度数据。大致过程是，设置数据存放位置，接收数据长度，然后使能串口接收中断。接收到数据时，会触发串口中断。再然后，串口中断函数处理，直到接收到指定长度数据，而后关闭中断，进入中断接收回调函数，不再触发接收中断。(只触发一次中断)

举例：  
HAL\_UART\_Receive\_IT(&huart1,(uint8\_t \*)&value,1);  
上述函数的意思是触发中断接收一个字符，存储到value中

### 2.串口中断函数 

HAL\_UART\_IRQHandler(UART\_HandleTypeDef \*huart)： 串口中断处理函数  
HAL\_UART\_TxCpltCallback(UART\_HandleTypeDef \*huart)： 串口发送中断回调函数  
HAL\_UART\_TxHalfCpltCallback(UART\_HandleTypeDef \*huart)： 串口发送一半中断回调函数（用的较少）  
HAL\_UART\_RxCpltCallback(UART\_HandleTypeDef \*huart)： 串口接收中断回调函数  
HAL\_UART\_RxHalfCpltCallback(UART\_HandleTypeDef \*huart)：串口接收一半回调函数（用的较少）  
HAL\_UART\_ErrorCallback()：串口接收错误函数

选择两个在代码中用到的详细说明一下：

#### 2.1 串口接收中断回调函数 

串口接收中断回调:  
HAL\_UART\_RxCpltCallback(UART\_HandleTypeDef \*huart);

功能：

> HAL库的中断进行完之后，并不会直接退出，而是会进入中断回调函数中，用户可以在其中设置代码, 串口中断接收完成之后，会进入该函数，该函数为空函数，用户需自行修改

举例：  
HAL\_UART\_RxCpltCallback(&huart1)\{ //用户自定义的代码 \}

#### 2.2 串口中断处理函数 

串口中断处理：  
HAL\_UART\_IRQHandler(UART\_HandleTypeDef \*huart);

功能：  
对接收到的数据进行判断和处理 判断是发送中断还是接收中断，然后进行数据的发送和接收，在中断服务函数中使用。如果接收数据，则会进行接收中断处理函数；如果发送数据，则会进行发送中断处理函数。

### 3.串口查询函数 

HAL\_UART\_GetState()： 判断UART的接收是否结束，或者发送数据是否忙碌

举例：  
while(HAL\_UART\_GetState(&huart4) == HAL\_UART\_STATE\_BUSY\_TX) //检测UART发送结束

### 4.重定义printf函数（以串口UART1为例） 

我们以USART接收与发送为例进行简单说明，首先我们需要重定向printf函数，在c语言中我们只要包含<stdio.h>的库就可以直接调用printf打印函数，但是对于单片机而言，即使包含了这个库，我们也不知道打印的输出方是谁，这时候我们就需要告诉单片机向电脑PC端打印数据，因此需要重写printf函数,将fputc和fgetc函数重写即可：

```java
//重写fget和fput函数
#include <stdio.h>
/**
  * 函数功能: 重定向c库函数printf到DEBUG_USARTx
  * 输入参数: 无
  * 返 回 值: 无
  * 说    明：无
  */
int fputc(int ch, FILE *f)
{
            
   
     
     
  HAL_UART_Transmit(&huart1, (uint8_t *)&ch, 1, 0xffff);
  return ch;
}
/**
  * 函数功能: 重定向c库函数getchar,scanf到DEBUG_USARTx
  * 输入参数: 无
  * 返 回 值: 无
  * 说    明：无
  */
int fgetc(FILE *f)
{
            
   
     
     
  uint8_t ch = 0;
  HAL_UART_Receive(&huart1, &ch, 1, 0xffff);
  return ch;
}
```

## 7.在keil5配置代码 

### 1.打开通过CubeMX创建的项目 

### 2.测试重定向的printf函数 

在mian函数中写入重定向的printf函数：  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/01ea6eb93eb5472191f1c761cfb4042f.png)


然后在while循环中测试：  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/310d41ee137e41628bb133e7653b936b.png)


测试结果如图：

USAT1串口每隔1s打印输出一次“hello windows！”

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/feaa86e0e6e742c3980376b5090dbc02.png)


### 3.测试串口中断(发送"\*“打印，发送”\#"停止) 

#### 3.1 要求： 

当上位机给stm32发送一个字符“\#”后，stm32暂停发送“hello windows！”；发送一个字符“\*”后，stm32继续发送；

#### 3.2 思路： 

首先，我们给STM32设置一个接收中断函数 HAL\_UART\_Receive\_IT，该函数是为了接收上位机发送的指令 order，当上位机发送指令时，触发中断，STM32以中断方式接收到命令后，不会退出中断，而是会触发中断回滚函数HAL\_UART\_RxCpltCallback，而中断回滚函数函数体内部是由我们用户自定义的代码，因此我们可以在此根据指令的不同进行数据发送或者停止发送。

最终main函数内容如下：

```java
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2023 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"
#include "usart.h"
#include "gpio.h"
#include <String.h>



//重写fget和fput函数！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！
#include <stdio.h>
/**
  * 函数功能: 重定向c库函数printf到DEBUG_USARTx
  * 输入参数: 无
  * 返 回 值: 无
  * 说    明：无
  */
int fputc(int ch, FILE *f)
{
            
   
     
     
  HAL_UART_Transmit(&huart1, (uint8_t *)&ch, 1, 0xffff);
  return ch;
}
/**
  * 函数功能: 重定向c库函数getchar,scanf到DEBUG_USARTx
  * 输入参数: 无
  * 返 回 值: 无
  * 说    明：无
  */
int fgetc(FILE *f)
{
            
   
     
     
  uint8_t ch = 0;
  HAL_UART_Receive(&huart1, &ch, 1, 0xffff);
  return ch;
}



char order;//指令 0:停止  1:开始
char message[]="hello windows！\n";//输出信息
char tips[]="CommandError\n";//提示1
char tips1[]="Start.....\n";//提示2
char tips2[]="Stop......\n";//提示3
int flag=0;//标志 0:停止发送 1.开始发送



void SystemClock_Config(void);


void test()
{
            
   
     
     
	printf("helloworld");
	HAL_Delay(1000);
}

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{
            
   
     
     

  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();
  MX_USART1_UART_Init();
	
	
	//设置接受中断
//	串口中断接收，以中断方式接收指定长度数据。大致过程是，设置数据存放位置，接收数据长度，
//	然后使能串口接收中断。接收到数据时，会触发串口中断。再然后，串口中断函数处理，直到接收到指定长度数据，
//	而后关闭中断，进入中断接收回调函数，不再触发接收中断。(只触发一次中断)
	HAL_UART_Receive_IT(&huart1, (uint8_t *)&order, 1);

	//当flag为1时,每秒发送一次信息
	//当flag为0时,停止
  while (1)
  {
            
   
     
     
	if(flag==1)
	{
            
   
     
     
	//发送信息
	HAL_UART_Transmit(&huart1, (uint8_t *)&message,，strlen(message),0xFFFF); 
	//延时
	HAL_Delay(1000);
	}
			
//	test();
  }
}


//HAL库的接收中断进行完之后，并不会直接退出，而是会进入中断回调函数中，用户可以在其中设置代码, 
//串口中断接收完成之后，会进入该函数，该函数为空函数，用户需自行修改
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
{
            
   
     
     
	//当输入的指令为0时,发送提示并改变flag
	if(order=='#'){
            
   
     
     
		flag=0;
		HAL_UART_Transmit(&huart1, (uint8_t *)&tips2, strlen(tips2),0xFFFF); 
	}
	
	//当输入的指令为1时,发送提示并改变flag
	else if(order=='*'){
            
   
     
     
		flag=1;
		HAL_UART_Transmit(&huart1, (uint8_t *)&tips1, strlen(tips1),0xFFFF); 
	}
	
	//当输入不存在指令时,发送提示并改变flag
	else {
            
   
     
     
		flag=0;
		HAL_UART_Transmit(&huart1, (uint8_t *)&tips, strlen(tips),0xFFFF); 
	}

	//重新设置中断
		HAL_UART_Receive_IT(&huart1, (uint8_t *)&order, 1); 
}



/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
            
   
     
     
  RCC_OscInitTypeDef RCC_OscInitStruct = {
            
   
     
     0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {
            
   
     
     0};

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSI;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.HSICalibrationValue = RCC_HSICALIBRATION_DEFAULT;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
            
   
     
     
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSI;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
            
   
     
     
    Error_Handler();
  }
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
            
   
     
     
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
            
   
     
     
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
            
   
     
     
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```

#### 3.3 运行效果 

电脑发送“\*”，串口打印“hello windows！”

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1d361b43cdda429b94c0935b8ed9c636.png)


电脑发送“\#”，串口停止发送“hello windows！”  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d71b68e6aacf444cbec4aa2f3559639d.png)


#### 3.4使用Keil的软件仿真逻辑分析仪功能观察串口输出波形 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/19a8988911cd45feaf460687e0721265.png)


波形图，看出发送数据周期是1s左右：  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/65b9407fe86a45e48029300d49cb5d6b.png)

虚拟示波器看串口通信波形时，因为波特率很高，所以只能看到一个略粗的线条，要选中它，Ctrl+鼠标滚轮，zoom 不断放大，才能看见串口帧数据波形，这样才能测量出时间、波特率。  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/215f66565e8c4f14a2532ecbf45c87aa.png)

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/9b36e62c15ec484bb6359ebea53cb26a.png)


## 总结 

RS232串口标准和TTL电平区别

RS232是一种异步串行通信标准,定义了通信设备之间信号的电平和格式。它使用了±12V的电平来表示二进制1和0,1表示-12V至-3V,0表示+3V~+12V。

而TTL电平标准采用0-5V电平范围,0V表示二进制0,3-5V表示二进制1。

由于微控制器芯片的IO口大多采用低电平为0,高电平为1的TTL标准,无法直接与RS232标准连接。因此需要通过"USB/TTL转232"模块进行电平转换。

CH340模块内部集成了CH340G芯片,可以完成USB与UART之间的转换。它将USB标准的±5V电平转换为RS232标准的±12V电平,或直接输出3.3V TTL标准电平,实现了USB与UART的连接。

STM32串口通信程序设计

1.安装STM32CubeMX软件,选择USART1作为串口通道,设置波特率为115200bps,停止位1位,无校验。生成初始化代码。

2.在Keil中打开stm32项目,添加HAL串口驱动头文件,实现串口发送和接收函数。

3.编写主函数,初始化串口,然后进入发送循环,每次发送"hello windows!"字符串。

4.同时开启串口接收中断,在中断服务程序中判断接收字符,如果为’\#‘则暂停发送,如果为’\*'则恢复发送。

5.编译下载到开发板,使用串口助手软件连接PC与开发板,测试串口通信功能是否正常。

总之,通过STM32CubeMX生成串口初始化代码,利用HAL库函数实现串口通信,并通过接收中断控制发送过程,实现了基于条件的串口数据发送。

# 四 、串口中断

## 一、通过CubeMX配置项目 

### 1.设置RCC 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6837ca30ad0e46e6b6ca2877f5217983.png)


### 2.设置SYS 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/1ac62e346f1840da855d37d5ca847293.png)


### 3.设置USART 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/d24c5e6e436f4e9cb9a96829168df702.png)


### 4.设置NVIC 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/70e76e9e00f742be87947b0fbd0b8f98.png)


### 5.创建项目 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8606942bf73d49ffb2dbd06ae4f725e0.png)


## 二、在keil配置代码 

### 1.打开通过CubeMX生成的项目 

### 2.在main函数前定义全局变量 

```java
char c;//指令 0:停止  1:开始
char message[]="hello Windows\n";//输出信息
char tips[]="CommandError\n";//提示1
char tips1[]="Start.....\n";//提示2
char tips2[]="Stop......\n";//提示3
int flag=0;//标志 0:停止发送 1.开始发送
```

### 3.在main函数中设置接收中断 

函数说明：

 * 函数原型

   ```java
   HAL_UART_Receive_IT(UART_HandleTypeDef *huart, uint8_t *pData, uint16_t Size)
   ```

 * 功能

   ```java
   功能：串口中断接收，以中断方式接收指定长度数据。
    大致过程是，设置数据存放位置，接收数据长度，然后使能串口接收中断。
    接收到数据时，会触发串口中断。
    再然后，串口中断函数处理，直到接收到指定长度数据
    而后关闭中断，进入中断接收回调函数，不再触发接收中断。(只触发一次中断)
   ```

 * 参数

   ```java
   UART_HandleTypeDef *huart      UATR的别名    
    huart1  *pData      			接收到的数据存放地址
    Size                      		接收的字节数
   ```

```java
HAL_UART_Receive_IT(&huart1, (uint8_t *)&c, 1);
```

### 4.main函数中的while循环里面添加传输代码 

```java
if(flag==1){
            
   
     
     
			//发送信息
			HAL_UART_Transmit(&huart1, (uint8_t *)&message, strlen(message),0xFFFF); 
			
			//延时
			HAL_Delay(1000);
		}
```

### 5.在main函数下面重写中断处理函数 

```java
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
{
            
   
     
     
	
	//当输入的指令为0时,发送提示并改变flag
	if(c=='0'){
            
   
     
     
		flag=0;
		HAL_UART_Transmit(&huart1, (uint8_t *)&tips2, strlen(tips2),0xFFFF); 
	}
	
	//当输入的指令为1时,发送提示并改变flag
	else if(c=='1'){
            
   
     
     
		flag=1;
		HAL_UART_Transmit(&huart1, (uint8_t *)&tips1, strlen(tips1),0xFFFF); 
	}
	
	//当输入不存在指令时,发送提示并改变flag
	else {
            
   
     
     
		flag=0;
		HAL_UART_Transmit(&huart1, (uint8_t *)&tips, strlen(tips),0xFFFF); 
	}

	//重新设置中断
		HAL_UART_Receive_IT(&huart1, (uint8_t *)&c, 1);  
}
```

### 6.main函数全部代码 

```java
#include "main.h"
#include "usart.h"
#include "gpio.h"
#include <string.h>

void SystemClock_Config(void);

char c;//指令 0:停止  1:开始
char message[]="hello Windows\n";//输出信息
char tips[]="CommandError\n";//提示1
char tips1[]="Start.....\n";//提示2
char tips2[]="Stop......\n";//提示3
int flag=0;//标志 0:停止发送 1.开始发送


int main(void)
{
            
   
     
     
	HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();
  MX_USART1_UART_Init();
	
	//设置接受中断
	HAL_UART_Receive_IT(&huart1, (uint8_t *)&c, 1);

	
	//当flag为1时,每秒发送一次信息
	//当flag为0时,停止
  while (1)
  {
            
   
     
     
		if(flag==1){
            
   
     
     
			//发送信息
			HAL_UART_Transmit(&huart1, (uint8_t *)&message, strlen(message),0xFFFF); 
			
			//延时
			HAL_Delay(1000);
		}
  }
}

void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
{
            
   
     
     
	
	//当输入的指令为0时,发送提示并改变flag
	if(c=='0'){
            
   
     
     
		flag=0;
		HAL_UART_Transmit(&huart1, (uint8_t *)&tips2, strlen(tips2),0xFFFF); 
	}
	
	//当输入的指令为1时,发送提示并改变flag
	else if(c=='1'){
            
   
     
     
		flag=1;
		HAL_UART_Transmit(&huart1, (uint8_t *)&tips1, strlen(tips1),0xFFFF); 
	}
	
	//当输入不存在指令时,发送提示并改变flag
	else {
            
   
     
     
		flag=0;
		HAL_UART_Transmit(&huart1, (uint8_t *)&tips, strlen(tips),0xFFFF); 
	}

	//重新设置中断
		HAL_UART_Receive_IT(&huart1, (uint8_t *)&c, 1);  
}
/* USER CODE END 4 */
/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
            
   
     
     
  RCC_OscInitTypeDef RCC_OscInitStruct = {
            
   
     
     0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {
            
   
     
     0};

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSI;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.HSICalibrationValue = RCC_HSICALIBRATION_DEFAULT;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_NONE;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
            
   
     
     
    Error_Handler();
  }
  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_HSI;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_0) != HAL_OK)
  {
            
   
     
     
    Error_Handler();
  }
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
            
   
     
     
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
            
   
     
     
  }
  /* USER CODE END Error_Handler_Debug */
}
```

### 7.编译并烧录 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/5ecbe194a6e740bcb923afdaffa4f7b7.png)


## 三、效果 

1.当发送1后可以看到不断输出“hello Windows”  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/69adc3bdc8094d6fba74c64325dc2842.png)

2.当输入0后端口停止输出  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/3348f36f44b24a1d98c6f5488044c74e.png)

# 五、串口DMA

## 一、DMA的基本介绍 

### （1）DMA的定义 

DMA，全称Direct Memory Access，即直接存储器访问。

DMA传输将数据从一个地址空间复制到另一个地址空间，提供在外设和存储器之间或者存储器和存储器之间的高速数据传输。

我们知道系统的运作核心是CPU，CPU无时无刻都在完成计算，控制，转存数据等大量事务，如果处理事务过多就会导致CPU运转不过来，因此有些程序就会出现卡顿；仔细想想如果将一些不重要的转移，存储数据交给其它外设来做，就可以减轻CPU负荷，让CPU转手去做其它更加复杂的事。

如何实现呢？我们只需要在两个外设之间建立一个数据通道让他们数据传输不经过CPU，就可以直接进行数据传输拷贝，如下图：  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0cac33fd9ab74dfa96be79e2392fdc4f.png)

所以这样就可以引出DMA的定义了：

DMA用来提供在外设和存储器之间或者存储器和存储器之间的高速数据传输。无须CPU的干预，通过DMA数据可以快速地移动。这就节省了CPU的资源来做其他操作。

### （2）DMA传输方式 

DMA没有采用传统的CPU参与数据存储方式，DMA传输主要涉及几种数据传输方式：

 *  外设到内存
 *  内存到外设
 *  内存到内存
 *  外设到外设

### （3）DMA传输参数 

数据传输的基本参数有：

1.传输数据的起始地址  
2.传输数据的目标位置  
3.传输数据的大小  
4.传输数据的传输模式

用户配置好这些传输参数后就可以进行DMA传输了，此时DMA控制器将会启动数据传输，当数据传输量为0时标志着DMA数据传输结束

### （4）DMA主要特征 

每个通道都直接连接专用的硬件DMA请求，每个通道都同样支持软件触发。这些功能通过软件来配置；

 *  在同一个DMA模块上，多个请求间的优先权可以通过软件编程设置（共有四级：很高、高、中等和低），优先权设置相等时由硬件决定（请求0优先于请求1，依此类推）；
 *  独立数据源和目标数据区的传输宽度（字节、半字、全字），模拟打包和拆包的过程。源和目标地址必须按数据传输宽度对齐；
 *  支持循环的缓冲器管理；
 *  每个通道都有3个事件标志（DMA半传输、DMA传输完成和DMA传输出错），这3个事件标志逻辑或成为一个单独的中断请求；
 *  存储器和存储器间的传输、外设和存储器、存储器和外设之间的传输；
 *  闪存、SRAM、外设的SRAM、APB1、APB2和AHB外设均可作为访问的源和目标；
 *  可编程的数据传输数目：最大为65535。

### （5）DMA工作系统框图 

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b37de9e071f0405aacc52e5b047f9981.png)

通过上面的系统框图可以看到DMA与外设有一个DMA Request 通道，通过这个通道可以独立在DMA和外设之间进行数据传输

## 二、串口DMA通信程序设计 

1.设置RCC，设置高速外部时钟HSE 选择外部时钟源![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/afc162bcf0874a1386b78a1374a25123.png)


2.设置串口  
在这里插入图片描述  
使能中断：  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/c0c883b943774f3ea87f056f679b6108.png)


3.DMA设置

1.点击DMASettings 点击 Add 添加通道  
2.选择USART\_RX USART\_TX 传输速率设置为中速  
3.DMA传输模式为正常模式  
4.DMA内存地址自增，每次增加一个Byte(字节)  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ff2cee93541048138311a8736067b443.png)


4.时钟源设置

1.选择外部时钟HSE 8MHz  
2.PLL锁相环倍频9倍  
3.系统时钟来源选择为PLL  
4.设置APB1分频器为 /2  
5.使能CSS监视时钟  
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/34538ea259c8402ea32af2231501746f.png)


接下来就是配置项目文件了，只需要按照常规配法就行了，这里就不详细讲了

## 三、使用KEIL5完成简单DMA数据发送 

1.打开上述创建的HAL库项目，定义数据缓存区以及数据长度宏定义

```java
#define LENGTH 20
uint8_t message[LENGTH] = "hello windows\n";
```

2.在对应main函数while(1)中输入以下代码：

```java
HAL_UART_Transmit_DMA(&huart1, (uint8_t *)message, LENGTH);//使用DMA发送数据
HAL_Delay(1000);
```

3.烧录到芯片中，查看运行结果：

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/7eb89a7584b0dd478759385321919d82.gif#pic_center)

# 六、总结

本文全面深入地阐述了串口通信相关知识，包括基本原理、文件互传、STM32 系统与上位机通信、串口中断以及串口 DMA 等内容。串口通信的基本原理涵盖串口协议与 RS - 232 标准，介绍了其分层结构、电平特点、传输速率、距离限制及与 TTL 电平的差异，同时阐述了 USB/TTL 转 232 模块工作原理。在文件互传方面，说明了硬件连接、波特率设置及传输特性。STM32 串口通信则从实验目的、内容、器材、步骤展开，详细介绍了通过 STM32CubeMX 配置项目、HAL 库函数使用及代码编写与测试，实现了串口数据的收发控制。串口中断部分讲解了 CubeMX 配置与 Keil 代码编写以达成基于中断的串口数据发送控制。串口 DMA 介绍了其定义、传输方式、参数、特征、系统框图，以及串口 DMA 通信程序设计与 KEIL5 中的简单数据发送实现。总之，这些内容系统地展现了串口通信在不同场景下的应用与技术要点，为电子工程师及相关技术人员深入理解和应用串口通信技术提供了全面的参考与指导，有助于提升在嵌入式系统开发、设备调试等领域的实践能力。

# 参考

https://blog.csdn.net/qq_47281915/article/details/121024427

https://blog.csdn.net/qq_47281915/article/details/121053903

https://www.cnblogs.com/breezy-ye/articles/12157442.html

https://blog.csdn.net/as480133937/article/details/104827639/

https://www.pianshen.com/article/8285571527/

https://blog.csdn.net/cleveryoga/article/details/121344003
