# 3.1 数据链路层概述

- 数据链路层在网络体系中所处的地位

  - **链路(Link)** 就是从一个结点到相邻节点的一段物理线路, 而中间没有任何其他的交换节点.
  - **数据链路(Data Link)** 是指把实现通信协议的硬件和软件加到链路上, 就构成了数据链路.
  - 数据链路层以**帧**为单位传输和处理数据

  <img src="../img/截屏2020-10-22 下午8.04.04.png"  width="800"  height = "350" />



- 数据链路层的三个重要问题 : **封装成帧,差错检测,可靠传输**

  - 封装成帧 : 给网络层交付的协议数据单元添加帧头和帧尾的操作,称为封装成帧,为了以帧为单元在链路上传数据

    <img src="../img/截屏2020-10-22 下午8.14.39.png"  width="800"  height = "250" />

  - 差错检测 : 接收方通过检错码和检错算法就可以知道这个帧有没有误码

    <img src="../img/截屏2020-10-22 下午8.20.08.png"  width="800"  height = "200" />

    

  - 可靠传输 : 接收方收到有误码的帧后是不会接收该帧的,将其**丢弃**,如果数据链路层向上层提供的是**不可靠服务**,那么**丢弃就丢弃了**,没有更多措施. 若向其上层提供**可靠服务**,那么还需要其他措施使接收可以**重新收到**这个被丢弃帧的**正确副本**	---->  **尽管误码是不能完全避免的, 但若能实现发送方发送什么,接收方就能收到什么, 就称为可靠传输**

    

- 使用**广播信道**的数据链路层(共享式局域网)

  - 发送方如何判断发送给哪个主机, 接收的主机又是如何知道接收方是他?

  ​		封装成帧的头部有两个字段是与地址相关

  <img src="../img/截屏2020-10-22 下午8.37.08.png"  width="800"  height = "300" />

  - 当**总线**上多台主机同时使用总线来传输帧时, 传输信号就会产生碰撞,(这是采用广播式信道的共享式局域网不可避免的) ,解决方法 : 使用协议 -> **共享式以太网**的媒体接控制协议 **CSMA/CD**  **载波监听多点接入/碰撞检测**

    

    <img src="../img/截屏2020-10-22 下午8.47.28.png"  width="800"  height = "150" />

    

- 交换式局域网

  随着技术的发展, **交换式局域网**在**有线(局域网)领域**已完全取代了**共享式局域网**(交换及如何转发帧的? 网桥和交换机**工作原理?**)

  <img src="../img/截屏2020-10-22 下午8.49.05.png"  width="300"  height = "150" />

- 无线局域网    (由于**无线局域网**的广播天性,使用的是**共享信道技术**)

  **802.11局域网**的媒体接入控制协议**CSMA/CA 载波监听多点接入/碰撞避免**(工作原理?)

  

> 小结 : **总线共享式**广播信道用CSMA/CD, 在**有线领域**已经被取代为交换式, **无线局域网**是天然广播特性的,要使用共享信道技术使用CSMA/CA 







#  3.1 封装成帧

- ### 封装成帧是指数据链路层给上层交付的协议数据单元添加帧头和帧尾使之成为帧

  - 帧头和帧尾中包含有重要的**控制信息**
  - 帧头和帧尾的作用之一就是**帧定界**

<img src="../img/截屏2020-10-22 下午9.16.55.png"  width="700"  height = "300" />

- ->发送方的数据链路层将上层交付下来的**协议数据单元**封装成帧后, 还要通过**物理层**将构成帧的各比特转换成电信号发送到传输媒体

  -> **接收方**的数据链路层如何从物理层交付的比特流中提取出一个个的帧?

  <img src="../img/截屏2020-10-22 下午9.29.05.png"  width="700"  height = "100" />

  

  - 1. ppp帧是通过**帧定界标志**来提取一个个的帧的 :

  <img src="../img/截屏2020-10-22 下午9.30.04.png"  width="700"  height = "300" />

  - 2. 并不是所有格式的帧都有帧定界标志: 以太网封装好MAC帧后交给物理层,**物理层添加一个前导码**, 前导码的前7个字节用于时钟同步,之后的一字节为开始定界符,表明其后面就是MAC帧.

  <img src="../img/截屏2020-10-22 下午9.32.21.png"  width="700"  height = "300" />

  ​			  另外, 以太网还规定了帧间间隔时间为96比特的发送时间, 因此MAC帧不需要帧结束定界符

  <img src="../img/截屏2020-10-22 下午9.38.18.png"  width="700"  height = "100" />

  ​				(帧间间隔还有其他作用,后面介绍)

- ### 透明传输

  - 透明传输是指数据链路层对**上层交付的传输数据**没有任何限制, 就好像数据链路层不存在一样

    - 面向**字节**的物理链路使用字节填充(或称字符填充)的方法实现透明传输

      要是传输的数据中包含**帧尾**怎么办?在发送帧之前, 对帧的数据部分进行扫描,每出现一个帧定界符,就在**前面加转义字符**

      <img src="../img/截屏2020-10-23 上午10.16.31.png"  width="700"  height = "200" />

      要是数据部分出现**帧定界符**怎么办?同样,发送前扫描,遇见帧定界符或转义字符,在前面添加转义字符

      <img src="../img/截屏2020-10-23 上午10.19.56.png"  width="700"  height = "200" />

      (转义字符是特殊的控制字符,长度为1字节,十进制值为27,并不是ESC这三个字母本身)

    - 面向**比特**的物理链路使用比特填充的方法实现透明传输

      比如,下图是某个点对点协议的帧, 帧的数据部分出现了两个帧定界标志,发送前使用0比特填充法, 对数据部分进行扫描, 每五个比特1后面就差入一个比特0

      <img src="../img/截屏2020-10-23 上午10.29.49.png"  width="700"  height = "200" />

      > [2013年 题37]] HDLC协议对01111100 01111110 组帧后对应的比特串为 
      >
      > A．01111100  00111110 10		 B．01111100 01111101 01111110 
      >
      > C．01111100 01111101 0 		   D．01111100 01111110 01111101 
      >
      > 【答案】A 
      >
      > 【解析】 本题考查HDLC协议中有关帧结构和实现“透明传输”的“零比特填充法”的相关知识。
      >
      > 

  - 为了提高帧的传输效率, 应当使帧的**数据部分的长度尽可能大些**

  - 考虑到差错控制等多种因素,每一种数据链路层协议都规定了帧的数据部分长度上限, 即最大传送单元**MTU**(Maximum Transfer Unit) 



# 3.3 差错检测

- 实际的通信链路都不是理想的, 比特在传输过程中可能会产生差错 : 1可能变成0, 而0可能变成1, 这称为**比特差错**.

- 在一段时间内, 传输错误占所传输比特总数的比特率称为误码率BER(Bit Error Rate)

- 使用**差错检测码**来来检测数据在传输过程中是否产生了比特差错, 是数据链路层所要解决的重要问题之一

  <img src="../img/截屏2020-10-23 上午11.17.09.png"  width="700"  height = "200" />

  - 奇偶校验

    在待发送的数据后面**添加1位奇偶校验位**,使**整个数据**(包括所添加的校验位在内)中“1”的个数为奇数(奇校验)或偶数(偶校验). 

    如果有奇数个位发生误码, 则奇偶性发生变化, 可以检查出误码.

    如果有偶数个位发生误码, 则奇偶性不会发生变化, 不能检查出误码(漏检)

    <img src="../img/截屏2020-10-23 上午11.25.40.png"  width="700"  height = "200" />

    

  - 循环冗余校验CRC(Cyclic Redundancy Check)

    - 收发双方约定好一个**生成多项式G(x)**
    - 发送方基于带发送的数据 和 生成多项式计算出差错检测码(**冗余项**),将其添加到带传输数据的后面一起传输
    - 接收方通过生成多项式来计算收到的数据是否产生了误码

    <img src="../img/截屏2020-10-23 上午11.36.15.png"  width="700"  height = "250" />

    

    <img src="../img/截屏2020-10-23 上午11.40.34.png"  width="700"  height = "250" />

    > [练习]
    >
    > 构造被除数
    >
    > <img src="../img/截屏2020-10-23 上午11.45.24.png"  width="850"  height = "120" />
    >
    > 构造除数
    >
    > <img src="../img/截屏2020-10-23 上午11.48.17.png"  width="850"  height = "200" />
    >
    > 计算步骤
    >
    > <img src="../img/截屏2020-10-23 上午11.52.26.png"  width="850"  height = "350" />

    

    > [练习]
    >
    > <img src="../img/截屏2020-10-23 下午12.03.28.png"  width="850"  height = "40" />
    >
    > <img src="../img/截屏2020-10-23 下午12.04.46.png"  width="250"  height = "250" />

    

    - **检错码**只能检测出帧在传输过程中出现了差错, 但并不能定位错误, 因此**无法纠正错误**
    - 要想纠正传输中的差错, 可以使用**冗余信息更多的纠错码**进行**向前纠错**, 但纠错码的开销比较大, 在计算机网络中**较少使用**
    - 循环冗余校验CRC有很好的检错能力(漏检率非常低就), 虽然计算比较复杂,但非常易于用硬件实现,因此被广泛应用于数据链路层
    - 常用**检错重传**方式来纠正传输中的差错, 或者仅仅是**丢弃**检测到差错的帧,取决于数据链路层向上层提供的是**可靠传输还是不可靠传输服务**



# 3.4.1 可靠传输的基本概念

- 使用差错检测技术(如循环冗余检验CRC), 接收方的数据链路层就可以检测出帧在传输过程中是否产生了误码
- 数据链路层向上层提供的服务类型
  - **不可靠传输**服务: 仅仅丢弃有误码的帧, 其他什么也不做
  - **可靠传输**服务 : 想办法实现发送端发送什么, 接收端就收到什么

- 一般情况下, **有线链路**的误码率比较低, 为了减小开销, 并**不要求数据链路层**向上层提供可靠传输服务, 即使出现了误码, 可靠传输的问题由其上层处理

- **无线链路**易受干扰, 误码率比较高,因此**要求数据链路层**必须向上层提供**可靠**传输服务

  <img src="../img/截屏2020-10-23 下午12.31.48.png"  width="750"  height = "150" />

- 比特差错只是传输差错中的一种

- 从整个计算机网络体系结构来看, 传输差错还包括**分组丢失**、分组失序以及**分组重复**

- **分组丢失**、分组失序以及**分组重复**这些传输差错, 一般不会出现在数据链路层, 而会出现在其上层

- 可靠传输服务并不仅局限于数据链路层, 其他**各层**均可选择实现可靠传输

  <img src="../img/截屏2020-10-23 下午12.36.42.png"  width="750"  height = "150" />

- 可靠传输的实现比较复杂, 开销也比较大, 是否使用可靠传输取决于应用需求

# 三种可靠传输的实现机制

这三种可靠传输实现机制的基本原理并不仅限于数据链路层, 可以应用到计算机网络体系结构的各层协议中

要放眼于整个网络体系结构

- **停止等待协议SW** 

- **回退N帧协议GBN**

- **选择重传协议SR**

## 3.4.2 可靠传输的实现机制 - 停止等待协议SW(Stop-and-Wait)

收发双方基于互联网进行通信, 而不是局限在一条点对点的数据链路

- 情况1 : ( 理想情况 ) 接收方发送确认分组 或 否认分组,

  <img src="../img/截屏2020-10-23 下午4.52.57.png"  width="200"  height = "300" />

- 情况2 : 发送方丢失发送的分组

  <img src="../img/截屏2020-10-24 上午9.14.53.png"  width="550"  height = "300" />

- 情况3 : 接收方丢失确认分组

  <img src="../img/截屏2020-10-24 上午9.21.59副本.png"  width="350"  height = "250" />

- 情况4 : 

  <img src="../img/截屏2020-10-24 上午9.33.51.png"  width="220"  height = "300" />

  

   	数据链路层点对点的往返时间比较确定, 重传时间比较好设定

  ​	 然而在运输层, 由于端到端往返时间非常不确定, 设置适合的重传时间有时并不容易

- 停止-等待协议的信道利用率

  - 当往返时延RTT远大于数据帧发送时延$T_d$时(例如使用卫星链路), 信道利用率非常低
  - 若出现重传, 则对于传送有用的数据信息来说, 信道利用率还要降低
  - 为了克服停止-等待协议信道利用率很低的缺点, 就产生了另外两种协议, 即后退N帧协议GBN 和 选择重传协议SR

  <img src="../img/截屏2020-10-24 上午9.42.26.png"  width="750"  height = "250" />



> [2018年 题 36]]  主机甲采用停-等协议向主机乙发送数据，数据传输速率是3kbps，单向传播延时是200ms，忽略确认帧的传输延时。当信道利用率等于40%时，数据帧的长度为 
>
> A．240比特 B．400比特 C．480比特 D．800比特 
>
> 【答案】D 
>
> 【解析】 根据题意，可画出下图， 
>
> <img src="../img/截屏2020-10-24 上午9.50.37.png"  width="750"  height = "200" />
>
> 信道利用率 = a ÷ (a + b + c) = 40% 设数据帧长度为x比特，$则信道利用率 = \frac{(xb ÷ 3kb/s) }{ (xb ÷ 3kb/s) + 200ms + 200ms)} = 40%$，解得 x = 800比特，因此选项D正确。





## 3.4.3 可靠传输的实现机制 - 回退N帧协议GBN

由于停止等待协议信道利用率低, 所以采用流水线方式可以提高, 回退N帧GBN是一种改进型的流水线协议

<img src="../img/截屏2020-10-24 上午10.06.51.png"  width="750"  height = "250" />

- 定义 : 

<img src="../img/截屏2020-10-24 上午10.09.16.png"  width="750"  height = "300" />

- **情况1** : 无差错情况

  ​		发送方发送发送窗口中的数据

  <img src="../img/截屏2020-10-24 上午10.11.45.png"  width="750"  height = "200" />

  

  ​			接收方每收到一个分组,就收窗口向前滑动,

  <img src="../img/截屏2020-10-24 上午10.33.43.png"  width="750"  height = "200" />

  

  ​			发送方每收到一个确认分组, 窗口就向前滑动一个

  <img src="../img/截屏2020-10-24 上午10.36.17.png"  width="750"  height = "150" />

  

  - 累积确认 : 接收方不一定要对收到书数据分组**逐个发送确认**, 可以在收到集合数据分组后, 对按序到达的最后一个数据分组发送确认. ACKn表示**序号为n以及以前的所有数据分组**都已正确接收

    <img src="../img/截屏2020-10-24 上午10.47.15.png"  width="250"  height = "150" />

    <img src="../img/截屏2020-10-24 上午10.47.23.png"  width="250"  height = "150" />

     假设ACK1在回传过程中丢失了, ACK4到达接收方也可以确认前5个分组都到达了.

<img src="../img/截屏2020-10-24 上午10.51.13.png"  width="450"  height = "150" />

​		优点 : 即使确认分组丢失, 发送方也可能不必重传, 也可以减小接收方的开销, 减少对网络资源的占用

​		缺点 : 不能向发送方及时反映出, 接收方已经正确接收的数据分组信息



- **情况2** : 有差错情况

  ​				   发送方发送发送窗口中的数据, 但在传输过程中出现了误码

  <img src="../img/截屏2020-10-24 上午11.06.10.png"  width="650"  height = "150" />

  

  ​					接收方通过校验码知道了错误, 丢弃该分组, 后续到达的四个分组与接受窗口序号不匹配, 同样不接受并丢					弃, 然后对**之前**按序接收的**最后一个分组**进行确认(ACK4) , 每丢弃一个分组, 就发送一个ACK4

  <img src="../img/截屏2020-10-24 上午11.08.07.png"  width="650"  height = "150" />

  <img src="../img/截屏2020-10-24 上午11.10.56.png"  width="650"  height = "150" />

  <img src="../img/截屏2020-10-24 上午11.17.00.png"  width="650"  height = "150" />

  ​					接收方收到过ACK4, 再收到重复的ACK4就知道传输产生了错误要重传

  <img src="../img/截屏2020-10-24 上午11.18.49.png"  width="650"  height = "150" />

  ​					其他分组被误码的组牵连, 要全部重传 -> 回退N帧

  <img src="../img/截屏2020-10-24 上午11.20.25.png"  width="650"  height = "150" />

  ​			当通信线路质量不好时, 回退N帧协议的信道利用率并比不上停止-等待协议高. 

  - 情况3:

    <img src="../img/截屏2020-10-24 上午11.24.50.png"  width="650"  height = "250" />

    > [2009年 题35] 数据链路层采用后退N 帧（GBN）协议，发送方已经发送了编号为0~7的帧。当计时器超时时，若发送方只收到0、2、3号帧的确认，则发送方需要重发的帧数是
    >
    >  A．2		 B．3		 C．4		 D．5 
    >
    > 【答案】C 
    >
    > 【解析】 根据GBN协议的特点，接收方只对按序到达且正确（无误码）的帧发送确认帧。题中所述发送方已经发送了编号为0~7的帧，但在重传计时器超时时只收到接收方针对0、2、3号帧的确认，这表明接收方已经收到按序到达且正确的编号为0~3的帧，并针对0~3号帧给发送方依次发回了确认。发送方只收到0、2、3号帧的确认，只能表明针对1号帧的确认丢失了，但这并不会引起发送方对1号帧的超时重传，因为收到3号帧的确认就足以说明接收方正确接收了0~3号帧；但编号为4的帧可能未按序到达接收方，也可能丢失，还可能出现误码，因此接收方不会针对4号帧给发送方发回确认，这将导致发送方对4号帧的超时重传，而4号帧后续的已发送的5~7号帧不管是否到达接收方，都会被发送方重传，也就是发送方会**重传4~7号帧**，选项C正确。
    >
    > <img src="../img/截屏2020-10-24 上午11.32.11.png"  width="550"  height = "100" />
    >
    > 

  ​			

  ## 3.4.4 可靠传输的实现机制 - 选择重传协议 

  - 回退N帧协议的接收窗口尺寸**$W_r$只能等于1**,  因此接收方只能按序接收正确到达的数据分组.

  - 一个数据分组误码会牵连其他分组重传,资源浪费

    

  - 设法只重传出现误码的数据分组 : **$W_r$尺寸应该大于1**, 接收方先收下失序到达(无误码)且落在接受窗口内的数据分组, 等缺失分组收齐后再一并交给山层, 这就是**选择重传协议**

    [注意] 选择重传协议为了让发送方仅重传出现差错的分组, **接收方不能采用累积确认**, 而需要对每个正确接收到的数据分组进行逐一确认



定义 : 

<img src="../img/截屏2020-10-24 下午2.02.32.png"  width="550"  height = "200" />

- **情况**1 :  

  ​	 发送方发送,2号分组丢失

<img src="../img/截屏2020-10-24 下午2.11.15.png"  width="750"  height = "150" />

​			接收方接收落入接受窗口内且无误码的数据分组, 接收方都会接收,

​			接收0和1,发回确认分组,窗口向前滑动

<img src="../img/截屏2020-10-24 下午2.14.20.png"  width="750"  height = "150" />

​			接收3,发回确认分组, 但窗口**不能向前滑动**

<img src="../img/截屏2020-10-24 下午2.17.37.png"  width="750"  height = "150" />

​			发送方接收确认, 标记3号已经传过了, 之后45同理, 然后标记

<img src="../img/截屏2020-10-24 下午2.22.38.png"  width="750"  height = "150" />

​		  发送方 窗口不能滑动, 重新发送2号分组, 返回2号确认分组

<img src="../img/截屏2020-10-24 下午2.21.25.png"  width="750"  height = "150" />



- **情况**1 :    

  <img src="../img/截屏2020-10-24 下午2.30.00.png"  width="750"  height = "300" />

  

  > [2011年 题35] 数据链路层采用选择重传协议（SR）传输数据，发送方已发送了0～3号数据帧，现已收到1号帧的确认，而0、2号帧依次超时，则此时需要重传的帧数是
  >
  >  A．1 		B．2 		C．3 		D．4 
  >
  > 【答案】B 
  >
  > 【解析】本题考查选择重传协议（SR）的基本工作原理。与后退N帧（GBN）协议不同，选择重传协议不支持累积确认。累积确认是指接收方不必每正确收到一个帧就给发送方发回确认帧，而是可以在连续正确收到几个帧后给发送方发回一个确认帧来表明已经正确接收到了这些帧。例如，接收方收到0~5号帧，可以给发送方发送一个确认帧ACK6，表明已经收到编号0~5的帧，现在想接收编号为6的帧。 综上所述，本题给出发送方只收到1号帧的确认，**0、2号帧超时**，由于采用的是选择重传协议（SR）传输数据，不支持累积确认，因此需要重传0、2号帧。题目并没指明3号帧是否正确接收，因此无须考虑3号帧的状态。故选项B正确。 
  >
  > <img src="../img/截屏2020-10-24 下午2.37.06.png"  width="450"  height = "90" />
  >
  > 【注意】 
  >
  > （1）如果本题采用后退N帧（GBN）协议传输数据，则具有累积确认的功能，由于收到1号帧的确认，也就表明0号帧也被正确接收了，因此只需重传1号帧的后续所有帧，也就是需要重传2、3号帧。 
  >
  > （2）请注意本题与【2009年 题35】的对比。
  >
  > 

  

  

# 3.5 点对点协议PPP

- 点对点协议PPP(Point-to-Point Protocol) 是目前使用最广泛的点对点数据链路层协议

  1999年运行的在**以太网上运行的PPP协议**(PPP over Ethernet) 使得ISP可以通过DSL、电路调制解调器、以太网等宽带接入技术以以太网接口的形式为用户提供接入服务

  PPP也广泛应用于**广域网路由器**之间的专用线路

  <img src="../img/截屏2020-10-24 下午2.41.57.png"  width="450"  height = "120" />

- PPP协议是因特网工程任务组IETF在1992年制定的, 经过1993年和1994年修订, 现在已成为因特网的正式标准[RFC1661, RFC1662]



- PPP协议在为点对点链路传输各种协议数据报提供了一个标准方法, 主要由以下三部分构成 : 

  <img src="../img/截屏2020-10-24 下午2.51.10.png"  width="550"  height = "250" />

- 帧格式

  <img src="../img/截屏2020-10-24 下午2.55.43.png"  width="550"  height = "250" />

- 透明传输

  <img src="../img/截屏2020-10-24 下午3.01.08.png"  width="550"  height = "250" />

  - 面向字节的异步链路

    <img src="../img/截屏2020-10-24 下午3.06.41.png"  width="550"  height = "250" />

  - 面向比特的同步链路

    <img src="../img/截屏2020-10-24 下午3.15.41.png"  width="550"  height = "250" />

- 差错检测

  <img src="../img/截屏2020-10-24 下午3.18.24.png"  width="550"  height = "250" />

  

- 工作状态

  <img src="../img/截屏2020-10-24 下午3.21.01.png"  width="650"  height = "300" />

  

  
  
  
  
  
  
  
  
    # 3.6.1 媒体接入控制的基本概念

<img src="../img/截屏2020-10-24 下午3.26.25.png"  width="750"  height = "350" />



# 3.6.2 媒体接入控制 - 静态划分信道

- 复用(Multiplexing) 是通信技术中的一个重要概念家. 复用就是通过一条物理线路同时传输多路用户的信号
- 当**网络中传输数字媒体的传输容量**大于**多条单一信道传输的总通信量**时, 可利用复用技术在一条物理线路上建立多条通信信道来充分利用传输媒体的带宽

<img src="../img/截屏2020-10-24 下午3.32.03.png"  width="750"  height = "200" />

- 频分复用FDM    

  <img src="../img/截屏2020-10-24 下午3.38.20.png"  width="550"  height = "200" />

- 时分复用TDM     

  <img src="../img/截屏2020-10-24 下午3.39.58.png"  width="550"  height = "200" />

- 波分复用WDM : 光的频分复用, 光传输会衰减,要在中间加放大器

  <img src="../img/截屏2020-10-24 下午3.43.52.png"  width="550"  height = "200" />

- **码分复用CDM** : 

  - 码分复用**CDM**是另一种共享信道的方法. 实际上由于该技术主要用于**多址接入**, 常用的名词是码分多址**CDMA(Code Division Multiple Access)**

  - 同理, 频分复用FDM和时分复用TDM同样可用于多址接入, 相应的名词是**频分多址FDMA(Frequency Division Multiple Access)** 和**时分多址TDMA(Time Division Division Multiple Access)**

  - 本课程中, 不严格区分复用与多址的概念, 可简单理解如下

    - 复用是将单一媒体的频带资源划分成很多子信道, 这些子信道之间相互独立, 互不干扰 ,每个子信道只占用资源一部分
    - 多址(确切为多点接入) 处理的事动态分配信道给用户. 移动通信是这样, 永久信道多址不需要(对于无线广播或电视广播站就是这样)
    - 某种程度上, FDMA、TDMA、 CDMA可以分别看成是FDM、TDM、CDM的应用

  - 与FDA与TDM不同, CDM的每个用户可以在同样的时间使用**同样的频带进行通信**

  - CDM各个用户使用**经过特殊挑选的不同码型**, 因此各个用户之间**不会造成干扰**

  - CDM早期用于军事通信

    直接序列扩频DSSS : 

  - 在CDMA中, 每一个比特时间再画分为m个短的间隔, 称为**码片(Chip)**, 通常m的值是128或64, 为了简单起见,后面举例假设m为8

  - 使用CDMA的每一个站被指派一个**唯一的m bit码片序列**(Chip Sequence).

    - 一个站如果要发送比特1, 则发送他**自己的m bit码片序列**

    - 一个站如果要发送比特0, 则发送他**自己的m bit码片序列的二进制反码**

      > [举例] 指派给CDMA系统中某个站点的码片序列为00011011
  >
      > ​			发送比特1 : **发送**自己的码片序列00011011
  >
      > ​			发送比特0 : 发送自己的码片序列 的二进制反码11100100
      >
      > 
      >
      > 为了方便, 按照惯例将码片序列中的0写为"-1", 将1 写为 "+1"
      >
      > 该站点的码片序列是(-1, -1, -1, +1, +1, -1, +1, +1)
  
    - 码片序列的挑选原则如下 : 
    
      1. 分配给每个站的码片序列必须各不相同,  实际常采用伪随机码序列

      2. 分配给每个站的码片序列必须**相互正交**(规格化内积为0)

         的规格化内积令向量S表示站S的码片序列, 令向量T表示其他任何站的码片序列. 
  
         两个不同站S和T的码片序列正交, 就是向量S和T的规格化内积为0;
  
         $S·T \equiv \frac{1}{m} \sum_{i=1}^nS_iT_i = 0$
      
         
      
         $S·S \equiv \frac{1}{m} \sum_{i=1}^nS_iS_i =  \frac{1}{m} \sum_{i=1}^n {(\pm1)}^2 = 1$
      
         $S·\bar{T} \equiv 0$
      
         $S·\bar{S} \equiv -1$
      
         > 假设给站S分配的码片序列为01011101, 给站T分配的码片序列为10111000,这样的分配正确吗?
         >
         > 检查码片序列是否各不相同 : 满足
         >
         > 检查码片序列是否相互正交 : 不满足
         >
         > 用向量S表示站S的码片序列(-1, +1, -1, +1, +1, +1, -1, +1), 用向量T表示站T的码片序列(+1, -1, +1, +1, +1, -1, -1, -1)
         >
         > $S·T  = \frac{(-1)(+1) + (+1)(-1) +(-1)(+1)+(+1)(+1) + (+1)(+1) + (+1)(-1) + (-1)(-1) +(+1)(-1)}{8} ≠ 0$
      
         
      
         > 
         >
         > <img src="../img/截屏2020-10-24 下午5.35.26.png"  width="650"  height = "300" />
      
         
      
         > <img src="../img/截屏2020-10-24 下午5.37.49.png"  width="650"  height = "300" />
      
         
      
         > [2014年 题37] 站点A、B、C通过CDMA共享链路，A、B、C的码片序列(chipping sequence)分别是(1,1,1,1)、(1,-1,1,-1)和(1,1,-1,-1)。若C从链路上收到的序列是(2,0,2,0,0,-2,0,-2,0,2,0,2)，则C收到A发送的数据是 
         >
         > A．000		 B．101 		C．110 		D．111 
         >
         > 【答案】B 
         >
         > 【解析】 将站点C收到的序列分成三部分：(2, 0, 2, 0)，(0, -2, 0, -2)，(0, 2, 0, 2)；题目要求回答站点A发送的数据是什么，因此把站点A的码片序列(1, 1, 1, 1)分别与这三部分做内积运算：
         >
         >  (2, 0, 2, 0)·(1, 1, 1, 1)÷4 = (2×1 + 0×1 + 2×1 + 0×1) ÷ 4 = 1，表明A发送的是比特1；
         >
         >  (0, -2, 0, -2)·(1, 1, 1, 1)÷4 = (0×1 + -2×1 + 0×1 + -2×1) ÷ 4 = -1，表明A发送的是比特0；
         >
         >  (0, 2, 0, 2)·(1, 1, 1, 1)÷4 = (0×1 + 2×1 + 0×1 + 2×1) ÷ 4 = 1，表明A发送的是比特1；
         >
         >  因此选项B正确。
         >
         > 
      
      
      
  
  
  
  
  
  
  
  
  
  
  
  
  
  # 3.6.3 动态接入控制 - 随机接入CSMA/CD协议

<img src="../img/截屏2020-10-25 下午12.54.59.png"  width="700"  height = "350" />

多址接入 : 多个主机连接到一根总线上, 各个主机随机发送帧

载波监听 : 主机C要发送帧, 他首先要载波监听,检测到总线空闲96比特时间后就可以发送帧了

​				假设C发送帧的过程中, B也要发送帧,B就要载波监听, 发现总线忙,于是持续检测总线,发现空闲96比特时间,立即				发送帧

碰撞检测 : B发送帧还要边检测碰撞, 只要没检测到碰撞就可以继续发送帧的剩余部分

<img src="../img/截屏2020-10-25 下午1.05.48.png"  width="700"  height = "300" />

- 争用期(碰撞窗口)

  <img src="../img/截屏2020-10-25 下午1.26.35.png"  width="700"  height = "250" />

  - 主机最多经过2$\tau$(即$\delta$→0) 的时长就可检测到本次发送是否遭受了碰撞

  - 因此, 以太网的端到端往返传播时延$2\tau$称为**争用期**或**碰撞窗口**
  
  - 经过争用期这段时间**还没有检测到碰撞,** 才能肯定这次发送不会发生碰撞
  
  - 每个主机在自己发送帧之后的一小段时间内, 存在着遭遇碰撞的可能性, 这一小段时间取决于俩主机间距离, 但不会超过总线的端到端往返传播时延(一个争用期时间)

  - 显然, 在以太网中发送帧的主机越多, 端到端往返传播时延越大, 发生碰撞的概率就越大, 因此共享式以太网主机, 使用的总线也不能太长,
  
  - 10Mb/s以太网把争用期定为512比特发送给时间, 即51.2µs, 因此其总线长度不能超过5120m, 但考虑到其他一些因素, 如信号衰减等, 以太网规定总线长度不能超过2500m
  
    

- 最短帧长

    - 以太网规定最小帧长为64字节, 即512比特(512比特时间即为争用期);

      - 如果要发送的数据非常少, 那么必须加入一些填充字节, 使帧长不小于64字节

    - 以太网的最小帧长确保了主机可在帧发送完成之前就检测到该帧的发送过程中是否遭遇了碰撞;
    
      - 如果在争用期(共发送64字节)没有检测到碰撞, 那么后续发送的数据就一定不会发生碰撞
    
  - 如果在争用期内检测到碰撞, 就立即终止发送, 这时已经发送出去的数据一定小于64字节, 因此凡事长度小于64字节的帧都是由于碰撞而异常中止的无效帧.
    

<img src="../img/截屏2020-10-25 下午2.16.30.png"  width="700"  height = "250" />

- 最大帧长

  <img src="../img/截屏2020-10-25 下午2.30.32.png"  width="700"  height = "350" />

- 截断二进制指数退避算法

  <img src="../img/截屏2020-10-25 下午2.34.44.png"  width="700"  height = "200" />

  - 若连续多次发生碰撞, 就表明可能有较多的主机参与竞争信道. 但使用上述退避算法可**使重传需要推迟的平均时间**随**重传次数而增大**(这也称为动态退避), 因而减小发生碰撞的概率, 有利于整个系统的稳定.
  - 当重**传达16次**仍不能成功时, 表明同时打算发送帧的主机太多, 以至于连续发生碰撞, 则丢弃该帧, 并向高层报告

- 信道利用率

  <img src="../img/截屏2020-10-25 下午2.43.43.png"  width="700"  height = "300" />

  <img src="../img/截屏2020-10-25 下午2.45.26.png"  width="700"  height = "150" />

- 帧接收流程图

  <img src="../img/截屏2020-10-25 下午2.46.24.png"  width="700"  height = "300" />
  
  <img src="../img/截屏2020-10-25 下午2.50.07.png"  width="700"  height = "350" />

> [2015年 题36]] 下列关于CSMA/CD协议的叙述中，错误的是 
>
> A．边发送数据帧，边检测是否发生冲突 
>
> B．适用于无线网络，以实现无线链路共享 
>
> C．需要根据网络跨距和数据传输速率限定最小帧长 
>
> D．当信号传播延迟趋近0时，信道利用率趋近100％ 
>
> 【答案】B 
>
> 【解析】 CSMA/CD适用于有线网络，而CSMA/CA则广泛应用于无线网络。而其他选项的表述都正确。
>
> 

> [2009年 题37]] 在一个采用CSMA/CD协议的网络中，传输介质是一根完整的电缆，传输速率为1Gbps，电缆中的信号传播速度是200 000km/s。若最小数据帧长度减少800比特，则最远的两个站点之间的距离至少需要
>
>  A．增加160m 		B．增加80m 		C．减少160m		 D．减少80m 
>
> 【答案】D 
>
> 【解析】 本题考查采用CSMA/CD协议的以太网的最小帧长的相关概念。
>
>  解法一： 设最远两个站点之间的距离为d，单位为m；以太网的最小帧长为Lmin，单位为bit； **Lmin = 数据传输速率 × 争用期**（即往返时延），将题目所给的相关量的单位化为标准单位后代入该式为$ Lmin = 10^9 × (\frac{d}{200000×10^3} ) × 2$，化简为$d = \frac{Lmin}{10}$，很显然，若最小帧长Lmin减少800比特，最远的两个站点之间的距离d至少会减少80m。 
>
> 解法二： 若最小数据帧长度减少800比特，则可以节省发送时间 800b / 10^9b/s = 8×10^(-7)s，那么最远的两个站点之间的往返时延可以允许减少8×10^(-7)s，则最大端到端单程时延可以减少4×10^(-7)s。要使得单程时延减少，且信号的传播速度不变，只有将最远两个站点之间的距离减少才能满足要求，需要减少的量为 4×10^(-7)s × 2×10^8m/s = 80m。

> [2010 年 47.（9 分）]某局域网采用CSMA/CD协议实现介质访问控制，数据传输速率为10Mbps，主机甲和主机乙之间的距离为2 km，信号传播速度是200 000 km/s。请回答下列问题，要求说明理由或写出计算过程。
> (1)若主机甲和主机乙发送数据时发生冲突，则从开始发送数据时刻起，到两台主机均检测到冲突时刻止，最短需经过多长时间？最长需经过多长时间（假设主机甲和主机乙发送数据过程中，其他主机不发送数据）？
>
> 【解析】只有主机甲和主机乙**同时**开始发送数据，才能使得它们从开始发送数据时刻起，到它们都检测到冲突时刻止，所经过的时间最短。
>
> 2km ÷ 200 000km/s = 0.01ms 。
>
> 当主机甲发送的数据信号传播到**无限接近主机乙**的某个时刻，主机乙也要发送数据，它检测到信道空闲（但信道此时并不空闲），就立刻开始发送数据
>
> 最长需经过的时间为 (2km ÷ 200 000km/s) × 2 = 0.02ms





# 3.6.4 动态接入控制 - 随机接入CSMA/CA协议

- 载波监听多址接入/碰撞避免CSMA/CA ( Carrier Sense Multiple Access / Collision Avoidance )

  既然CSMA/CD协议已经成功的应用于使用广播信道的**有线局域网**, 那么同样使用广播信道的无线局域网能不能也使用CSMA/CD协议呢?

  - 在无线局域网中, 仍然可以是使用**载波监听多址接入CSMA**, 即在发送帧之前先对传输媒体进行**载波监听**, 若**发现有其他站在发送帧**, 就推迟发送以免发生碰撞

  - 在无线局域网中, 不能使用碰撞检测CD , 

    - 无线网卡上收到信号前度远小于发送信号, 对硬件要求非常高

    - 即使硬件上可以实现,无线电波的特殊性, 存在**隐蔽站问题**, 进行碰撞检测的意义也不大

      <img src="../img/截屏2020-10-25 下午3.40.04.png"  width="750"  height = "150" />

    - 802.11无线局域网使用CSMA/CA协议, 在CSMA的基础上增加了一个**碰撞避免CA**功能, 而不再实现碰撞检测功能
  
- 由于不可能避免所有的碰撞, 并且无线信道的误码率较高 802.11标准还使用了**数据链路<u>层确认机制(停止-等待协议)</u>** 来保证数据被正确接收
  - 802.11的MAC层标准定义了两种不同的媒体接入控制方式 :
  - **分布式协调功能DCF**(Distributed Coordination Function) 在DCF方式下, **没有中心控制站点**, 每个站点使用CSMA/CA协议通过**争用信道**来获取发送权, 这是802.11定义的默认方式
    - 点协调功能PCF(Point Coordination Function) PCF方式使用**集中控制**的接入算法(一般在接入点AP实现集中控制), 是802.11定义的可选方式, 在实际中较少使用
      
    #### 帧间间隔IFS(InterFrame Space)

    - 802.11标准规定, 所有的站点必须在持续**检测到信道空闲一段指定时间**后才能发送帧, 这段时间称为帧间间隔IFS

    - 帧间间隔的长短取决于该站点要发送的帧的类型

      - **高优先级帧**需要等待的时间较短, 因此可优先获得发送权
      - **低优先级帧**需要等待的时间较长. 若某个站的**低优先级帧**还没来得及发送, 而其他站的高优先级帧已经发送到信道上, 则**信道变为忙态**, 因而低优先级只能推迟,就减少了发生碰撞的机会.

    - 常用的两种帧间间隔如下

      - **短帧时间间隔SIFS**(28µs), 是最短的帧间间隔, 用来**分隔开属于<u>一次</u>对话的各帧**, 一个站点应当能够在这段时间内**从发送方式切换到接收方式.**

        使用SIFS的帧类型有ACK帧, CTS帧, 由过长的MAC帧分片后的数据帧, 以及*所有回答AP探寻的帧*和**在**PCF方式中接入点AP发送出的任何帧*.

      - **DCF帧间间隔DIFS**(128µs), 他比短帧间间隔SIFS要长的多, 在DCF方式中采用发送**数据帧和管理帧**

        

    #### CSMA/CA的工作原理

    <img src="../img/截屏2020-10-25 下午4.22.44.png"  width="750"  height = "250" />

    **CSMA/CA退避算法**

    - 当站点检测到信道是空闲的, 并且所发送的数据帧**不是成功发送完上一个数据帧之后**立即连续发送的数据帧, 则不使用退避算法

    - 以下情况必须使用退避算法

      - 在发送数据帧之前检测到信道处于忙状态
      - 在每一次重传一个数据帧时
      - 在每一次成功发送后要连续发送下一个帧时(这是为了避免一个站长时间占用信道)
      
    - 在执行退避算法时 ,站点为退避计时器设置一个随机的退避时间

      - 当退避计时器的时间减小到0时, 就开始发送数据
      - 当退避计时器的时间还未减小到0时而信道又转变为忙状态, 这时就冻结退避计时器的数值, 重新等待信道变为空闲, 再经过时间DIFS后, 继续启动退避计时器

    - 在进行第i次退避时, 退避时间在时隙编号${0,1,...,2^{2+1}  - 1}$中随机选择一个, 然后乘以基本退避时间(就是一个时隙的长度) 就可以得到随机退避时间. 这样做是为了使不同站点选择相同退避时间的概率减少. 当时隙编号达到255时(对应于第六次退避)就不再增加了

      <img src="../img/截屏2020-10-25 下午4.44.50.png"  width="750"  height = "250" />


​    

  **CSMA/CA协议的信道预约和虚拟载波监听**

  - 为了尽可能减少碰撞的概率和降低碰撞的影响, 802.11标准允许要发送数据的站点对信道进行预约

    <img src="../img/截屏2020-10-25 下午5.00.12.png"  width="750"  height = "150" />

    (1) 源站在发送数据帧之前先发送一个短的控制帧, 称为**请求发送RTS**(Request To Send), 它包括**源地址、目的地址以及这次通信**(包括相应的确认帧)**所需的持续时间**

    (2) 若目的站正确收到源站发来的RTS帧, 且媒体空间, 就发送一个相应控制帧, 称为允许发送CTS(Clear To Send), 他也包括这次通信所需持续时间(从RTS帧中将此持续时间复制到CTS帧中)

    (3)源站收到CTS帧后, 再等待一段时间SIFS后, 就可发送其数据帧

    (4)若目的站正确收到了源站发来的数据帧, 在等待时间SIFS后, 就向源站发送确认帧ACK

- 除源站和目的站以外的其他各站, 在收到CTS帧(或数据帧)后就**推迟接入**到无线局域网中, 这样就保证了源站和目的站之间的通信不会受到其他站的干扰

- 如果RTS帧发生碰撞, 源站就收不到CTS帧, 需要执行退避算法重传RTS帧

- 由于RTS帧和CTS帧很短, 发送碰撞的概率、碰撞产生的开销及本身的开销都很小, 而对于一般的数据帧, 其发送时延往往大于传播时延(因为是局域网), 碰撞的概率很大, 且一旦发生碰撞而导致数据帧重发,则浪费就很多忙因此用很小的代价对信道进行预约往往是值得的, 802.1.1标准规定了3种情况供用户选择

  - 使用RTS帧和CTS帧
  - 不实用RTS帧和CTS帧
  - 只有当数据帧的长度超过某一数值时才使用TRS和CTS帧

  

  

- 除RTS帧和CTS帧会携带通信需要持续的时间, 数据帧也能携带通信需要持续的时间, 这称为802.11的**虚拟载波监听**机制

- 由于利用虚拟载波监听机制, 站点只要**监听到RTS帧, CTS帧或数据帧中间的任何一个**,就能知道信道被占用的持续时间, 而不需要真正监听到信道上的信号 因此虚拟载波监听机制能**减少隐蔽站带来的碰撞问题**

  <img src="../img/截屏2020-10-25 下午5.15.56.png"  width="350"  height = "200" />

    

    

  > [2011年 题36]] 下列选项中，对正确接收到的数据帧进行确认的MAC协议是
  >
  >  A．CSMA		 B．CDMA 		C．CSMA/CD 		D．CSMA/CA 
  >
  > 【答案】D 
  >
  > 【解析】 CSMA是指载波监听多点接入。
  >
  >  CSMA/CD是对CSMA的改进，指载波监听多点接入/碰撞检测，是早期以太网使用的有线信道访问控制协议。
  >
  >  CSMA/CA是指载波监听多点接入/碰撞避免，是802.11局域网采用的无线信道访问控制协议。802.11局域网在使用CSMA/CA的同时，还使用停止等待协议。这是因为无线信道的通信质量远不如有线信道，因此无线站点每通过无线局域网发送完一帧后，要等到收到对方的确认帧后才能继续发送下一帧，也就是所谓的链路层确认。
  >
  >  CDMA是指码分多址，是一种信道复用技术，不属于MAC协议。 综上所述，选项D正确。 
  >
  > 【注意】 即便CSMA/CA是考生的知识盲点，也不影响考生对本题的正确解答。因为CSMA/CD是考生应该非常熟悉的链路层协议，而CDMA也是考生应掌握的一种信道复用技术（2014年 题37就考查了CDMA的相关知识），它不属于MAC协议，因此通过排除法可以选出正确答案为D。
  
  
  
  > [2013年 题36]  下列介质访问控制方法中，可能发生冲突的是 A．CDMA B．CSMA C．TDMA D．FDMA 
  >
  > 【答案】B 
  >
  > 【解析】
  >
  > CDMA(Code Division Multiplex Access)是指码分多址 
  >
  > CSMA(Carrier Sense Multiple Access)是指载波监听多点接入
  >
  > TDMA(Time Division Multiple Access)是指时分多址
  >
  > FDMA(Frequency Division Multiple Access)是指频分多址 
  >
  > TDMA，FDMA，CDMA是常见的信道复用技术，属于静态划分信道，用于多用户共享信道，不会发生冲突。
  >
  >  CSMA属于争用型的介质访问控制协议，连接在同一介质上的多个主机使用该协议以竞争方式发送数据帧，可能出现冲突（也称为碰撞）。 综上所述，选项B正确。
  
  > [2018年 题35]] IEEE 802.11无线局域网的MAC协议CSMA/CA进行信道预约的方法是 
  >
  > A．发送确认帧 					B．采用二进制指数退避
  >
  >  C．使用多个MAC地址 		D．交换RTS与CTS帧 
  >
  > 【答案】D 
  >
  > 【解析】 为了更好地解决隐蔽站带来的碰撞问题，802.11允许要发送数据的站对信道进行预约。具体的做法是这样的。A在向B发送数据帧之前，先发送一个短的控制帧，叫做请求发送RTS(Request To Send)，它包括源地址、目的地址和这次通信（包括相应的确认帧）所需的持续时间。当然，A在发送RTS帧之前，必须先监听信道。若信道空闲，则等待一段时间DIFS后，才能够发送RTS帧。若B正确收到A发来的RTS帧，且媒体空闲，则等待一段时间SIFS后，就向A发送一个叫做允许发送CTS(Clear To Send)的控制帧，它也包括这次通信所需的持续时间。A收到CTS帧后，再等待一段时间SIFS后，就可发送数据帧。若B正确收到了A发来的数据帧，在等待时间SIFS后，就向A发送确认帧ACK。 因此，选项D正确
  >
  > CSMA/CA协议使用RTS和CTS帧来预约信道, 他们都携带有通信需要持续的时间.另外, 除TRS和CTS帧外, 数据帧也能携带通信需要持续的时间,这就是802.11的虚拟载波监听











# 3.7 MAC地址、 IP地址、 ARP协议

<img src="../img/截屏2020-10-25 下午5.31.09.png"  width="500"  height = "300" />

## - MAC地址

<img src="../img/截屏2020-10-25 下午7.45.03.png"  width="800"  height = "100" />

- 当多个主机连接在同一个广播信道上, 要想实现两个主机之间的通信, 则每个主机都必须有一个唯一的标志, 即一个数据链路层地址

- 在每个主机发送的**帧中**必须携带**标识发送主机和接收主机地址**, 由于这类地址是用于媒体接入控制MAC(Media Access Control), 因此这类地址被称为**MAC地址**

  <img src="../img/截屏2020-10-25 下午7.46.47.png"  width="800"  height = "100" />

  - MAC地址一般被固化在网卡(网络适配器)的电可擦可编程只读存储器EEPROM, 因此MAC地址也被称为**硬件地址**

  - MAC地址有时也被称为物理地址, **请注意这并不意味着MAC地址属于网络体系结构中的物理层**

    <img src="../img/截屏2020-10-25 下午7.59.45.png"  width="800"  height = "200" />

    > [2018年 题34] 下列选项中，不属于物理层接口规范定义范畴的是 
    >
    > A．接口形状 B．引脚功能 C．物理地址 D．信号电平 
    >
    > 【答案】C 
    >
    > 【解析】 物理层包含以下四种接口特性：    
    >
    > （1）机械特性：指明接口所用接线器的形状和尺寸、引脚数目和排列、固定和锁定装置等。平时常见的各种规格的接插件都有严格的标准化的规定。    
    >
    > （2）电气特性：指明在接口电缆的各条线上出现的电压的范围。 
    >
    > （3）功能特性：指明某条线上出现的某一电平的电压的意义。    
    >
    > （4）过程特性（规程特性）：指明对于不同功能的各种可能事件的出现顺序。 物理地址又称硬件地址或MAC地址，属于数据链路层，不要被其名称中的“物理”二字误导认为物理地址属于物理层。 
    >
    > 【注意】本题与2012年第34题类似。

    一般情况下, 用户主机会包含两个网络适配器: 有线局域网适配器(又想网卡) 和 无线局域网适配器(无线网卡). 每个**网络适配器都有一个全球唯一的MAC地址**. 而交换机和路由器往往拥有更多的网络接口, 所以会拥有更多的MAC地址.综上, 严格来说, MAC地址是对网络上各个接口的唯一标识, 而不是对网络上各设备的唯一标识.

  - IEEE 802局域网的MAC地址格式

    <img src="../img/截屏2020-10-25 下午8.28.54.png"  width="600"  height = "200" />

    这个MAC被厂商小米拿走了

    <img src="../img/截屏2020-10-25 下午8.30.48.png"  width="600"  height = "150" />

    IEEE 802局域网的MAC地址格式:

    <img src="../img/截屏2020-10-25 下午8.32.57.png"  width="900"  height = "500" />

    MAC地址的发送顺序, 按字节顺序发送,字节内部的bit**从后往前发送**

    <img src="../img/截屏2020-10-25 下午8.35.09.png"  width="500"  height = "250" />

    解释单播地址多播地址 : 

    <img src="../img/截屏2020-10-25 下午8.46.09.png"  width="500"  height = "250" />

    <img src="../img/截屏2020-10-25 下午8.49.04.png"  width="500"  height = "250" />

    <img src="../img/截屏2020-10-25 下午8.51.01.png"  width="500"  height = "250" />

    给主机配置多播组列表进行私有应用时, 不得使用公有的标准多播地址, 具体可以在以下网址查询 : 

    http://standards.ieee.org/develop/regauth/grpmac/public.html

    

    

    ## - IP地址

    - IP地址是因特网(Internet)上的主机和路由器所使用的地址, 用于标识两部分信息 : 

      - 网络编号 : 标识因特网上数以百万计的**网络**
      - 主机编号 : 标识同一网络上的不同**主机**(或路由器各接口)

    - 很显然, 之前介绍的**MAC地址不具备区分不同网络**的功能

      - 如果只是一个单独的网络, 不接入因特网, 可以只使用MAC地址(这不是一般用户的应用方式).

      - 如果主机所在的网络要接入因特网, 则IP地址和MAC地址都需要使用

        <img src="../img/截屏2020-10-25 下午9.43.18.png"  width="500"  height = "300" />

     - 从网络体系结构看IP地址与MAC地址

        <img src="../img/截屏2020-10-25 下午9.46.47.png"  width="550"  height = "300" />

        - 数据报转发过程中IP地址与MAC地址的变化情况

        <img src="../img/截屏2020-10-25 下午9.49.34.png"  width="750"  height = "450" />

        <img src="../img/截屏2020-10-25 下午10.14.22.png"  width="750"  height = "450" />

        如何把IP地址解析为MAC地址,要用到**ARP协议**

        <img src="../img/截屏2020-10-25 下午10.15.34.png"  width="750"  height = "350" />

        > [2018年 题37]路由器R通过以太网交换机S1和S2连接两个网络，R的接口、主机H1和H2的IP地址与MAC地址如下图所示。若H1向H2发送一个IP分组P，则H1发出的封装P的以太网帧的目的MAC地址、H2收到的封装P的以太网帧的源MAC地址分别是
        >
        >  A．00-a1-b2-c3-d4-62、00-1a-2b-3c-4d-52 	 B．00-a1-b2-c3-d4-62、00-1a-2b-3c-4d-61 
        >
        > C．00-1a-2b-3c-4d-51、00-1a-2b-3c-4d-52 	  D．00-1a-2b-3c-4d-51、00-a1-b2-c3-d4-61 
        >
        > 【答案】D
        >
        > <img src="../img/截屏2020-10-25 下午10.20.15.png"  width="750"  height = "350" />
        >
        > 

        

        

        

        ## ARP协议

        <img src="../img/截屏2020-10-25 下午10.30.22.png"  width="750"  height = "350" />

        ​			 主机B先在自己的ARP高速缓存查找IP对应的MAC地址,

        <img src="../img/截屏2020-10-25 下午10.31.44.png"  width="750"  height = "350" />

        ​				如果没有找到, B就广播ARP请求报文, 

        <img src="../img/截屏2020-10-25 下午10.34.45.png"  width="750"  height = "350" />

        ​			  C主机 单播发送ARP响应报文,B接收,将该IP与MAC的记录存到自己的ARP缓存表中.

        <img src="../img/截屏2020-10-25 下午10.36.00.png"  width="750"  height = "350" />

        ​			然后B就可以与主机C沟通了

        <img src="../img/截屏2020-10-25 下午10.39.09.png"  width="750"  height = "350" />

        ​			[思考] 从主机H1到主机H2能否使用ARP协议?

        ​			ARP是网络层协议, 在逐段链路上解析IP和MAC的对应

        <img src="../img/截屏2020-10-25 下午10.43.19.png"  width="750"  height = "350" />

        ​			 

        

# 3.8 集线器与交换机的区别

- 早期的总线型以太网

  细同轴电缆

- 使用双绞线和集线器HUB的星型以太网

  <img src="../img/截屏2020-10-28 下午3.40.10.png"  width="250"  height = "250" />

  - 使用集线器的以太网在逻辑上仍是一个**总线网**, 各站共享总线资源, 使用的还是CSMA/CD协议
  - 集线器只工作在**物理层**, 他的每个接口仅简单的转发比特, 不进行碰撞检测(由各站的网卡检测)
  - 集线器一般都有**少量的容错能力**和网络管理功能, 例如,某网卡故障, 集线器可以检测问题断开链接, 使整个以太网仍能正常工作.
  - 使用集线器HUB在物理层扩展以太网

  <img src="../img/截屏2020-10-28 下午3.52.06.png"  width="450"  height = "250" />

- 以太网交换机 ( 1.忽略ARP过程, 2.假设交换机帧交换表已经学习好了)

  <img src="../img/截屏2020-10-28 下午3.58.17.png"  width="350"  height = "150" />

  <img src="../img/截屏2020-10-28 下午4.04.12.png"  width="350"  height = "250" />

  - 以太网交换机通常都有多个借口, 每个接口都可以直接与一台主机或另一个以太网交换机相连. 一般都工作在全双工方式

  - 以太网交换机具有并行性, 能**同时连通多对接口**, 使多对主机能同时通信, **无碰撞(不使用CSMA/CD协议)**

  - 以太网交换机一般都具有多种速率的接口, 10Mb/s, 100Mb/s, 1Gb/s, 10Gb/s接口的多种组合

  - 以太网交换机工作在**数据链路层(也包括物理层)**, 他收到帧后,在帧交换表中查找**帧的目的MAC地址所对应的接口号**, 然后通过该接口转发帧

  - 以太网交换机是一种即插即用设备, 其内部的**帧交换表**是通过**自学习算法**自动地逐渐建立起来的

  - 帧的两种转发方式

    - 1.存储转发

    - 2.直通交换 : 采用基于硬件的交叉矩阵(交换时延非常小, 单不检查帧是否有差错)

- 对比集线器和交换机

  <img src="../img/截屏2020-10-28 下午4.22.16.png"  width="650"  height = "350" />

  

  # 3.9以太网交换机自学习和转发帧的流程

  <img src="../img/截屏2020-10-28 下午4.45.29.png"  width="650"  height = "250" />

  交换机从头开始学习登记转发过程

  ### Step 1 :

  主机A要给主机B发送帧

  帧从端口1发出, 先**登记**将发送的主机号记录到表中, 将端口号记录到表中. 

  <img src="../img/截屏2020-10-28 下午4.48.16.png"  width="650"  height = "250" />

  然后**交换机1**对帧进行转发,找B -> 找不到B -> **盲目转发(泛洪)**

  盲转到3: B是一看是发给自己的, 接收,

  盲转到2: C一看不是自己的,抛弃

  <img src="../img/截屏2020-10-28 下午4.51.11.png"  width="650"  height = "250" />

  盲转到4: 该帧从**交换机2的接口2**进入, 首先进行登记的工作

  将原MAC地址A记录到表中, 将进入的接口号2记录到表中

  <img src="../img/截屏2020-10-28 下午4.55.23.png"  width="650"  height = "250" />

  找目的主机B, 在帧交换表中找不到B	-> 进行盲目转发 -> 主机D、E、F都不是, 直接丢弃

  <img src="../img/截屏2020-10-28 下午5.03.36.png"  width="650"  height = "250" />

  ### Step 2:

  主机B要给主机A发送帧

  帧从端口3发出, 先**登记**,将发送主机的MAC地址记录到表中, 进入的接口号3也记录到表中

  <img src="../img/截屏2020-10-28 下午5.09.55.png"  width="650"  height = "250" />

  转发该帧, 查找目的地址 -> 找到A -> 按记录的端口号**转发(明确转发)**

  同时, 交换机2不会收到该帧

  

  ### Step3:

  主机E发送到A的帧: 

  首先进行**登记**, 将发送主机的MAC地址和, 发送出的端口号进行登记

  <img src="../img/截屏2020-10-28 下午5.14.25.png"  width="650"  height = "250" />

  **转发** -> 表中查找目的主机主机A -> 找到 -> 从接口号2转发(明确转发)

  帧从接口号4进入交换机1, 交换机1登记源MAC地址和进入的端口号

  <img src="../img/截屏2020-10-28 下午5.17.31.png"  width="650"  height = "250" />

  转发(明确)-> 查找要发送的MAC地址 -> 找到了A -> 发送帧到表中对应的端口

  ### Step4 : 

  G发送帧给A

  A直接接受了, 1号端口也接受了-> **登记**「G和1」,**转发**找路由表的A找到了,进行转发接收过了丢弃
  
  <img src="../img/截屏2020-11-03 下午1.01.40.png"  width="650"  height = "250" />
  
  ### Step5:
  
  每个帧都学习完了,表也填写好了: **注意到期会自动删除**
  
  <img src="../img/截屏2020-11-03 下午1.08.05.png"  width="650"  height = "250" />
  
  > [习题]
  >
  > <img src="../img/截屏2020-11-03 下午1.12.56.png"  width="650"  height = "250" />
  
  
  
  > [考研2009 题36]以太网交换机进行转发决策时使用的PDU地址是 
  >
  > A．目的物理地址 	B．目的IP地址 
  >
  > C．源物理地址 		D．源IP地址 
  >
  > 【答案】A 
  >
  > 【解析】 （1） **PDU(Protocol Data Unit)的意思是协议数据单元，**它是计算机网络体系结构中对等实体间逻辑通信的对象。物理层对等实体间逻辑通信的对象为比特流，数据链路层对等实体间逻辑通信的对象为帧，网络层对等实体间逻辑通信的对象为分组，传输层对等实体间逻辑通信的对象为报文段，应用层对等实体间逻辑通信的对象为应用报文。 
  >
  > （2）以太网交换机收到帧后，提取帧首部中的源MAC地址和目的MAC地址字段的值，将源MAC地址与该帧进入交换机的端口的端口号作为一条记录写入自己的帧交换表（或称帧转发表），然后在帧交换表中查找目的MAC地址，如果找到了匹配的记录，则从该记录中的端口项所指定的端口“明确”转发该帧；如果找不到匹配的记录，则采用泛洪法转发该帧，也就是通过除该帧进入交换机端口外的其他所有端口转发该帧。 MAC地址又称为物理地址或硬件地址。由（2）可知，以太网交换机根据**帧的目的MAC地址**对帧进行转发，目的MAC地址又称为目的物理地址或目的硬件地址，因此选项A正确。 
  >
  > 【注意】不要被物理地址中的“物理”二字误导，错误地认为物理地址属于计算机网络体系结构中的物理层；物理地址应该属于体系结构中的**数据链路层**。
  
  
  
  > [考研2014年 题34] 某以太网拓扑及交换机当前转发表如下图所示，主机00-e1-d5-00-23-a1向主机00-e1-d5-00-23-c1发送1个数据帧，主机00-e1-d5-00-23-c1收到该帧后，向主机00-e1-d5-00-23-a1发送1个确认帧，交换机对这两个帧的转发端口分别是
  >
  >  A．{3}和{1} 		B．{2,3}和{1} 		C．{2,3}和{1,2} 		D．{1,2,3}和{1} 
  >
  > <img src="../img/截屏2020-11-03 下午1.30.19.png"  width="650"  height = "150" />
  >
  > 【答案】B 
  >
  > 【解析】 以太网交换机收到一个帧时的自学习和转发过程如下： 
  >
  > （1）登记：提取帧的源MAC地址，然后将其与帧进入交换机端口的端口号作为一条记录存储到帧交换表（或称转发表）中； 
  >
  > （2）转发：提取帧的目的MAC地址，若该MAC地址是一个广播地址，则将该帧从除进入交换机的端口外的其他所有端口转发；若该MAC地址是一个单播地址，则在帧交换表中查找该MAC地址，按以下情况处理： 1)   若找不到该MAC地址的记录，则进行“盲目地转发”，也就是除帧进入交换机的端口外的其他所有端口转发该帧，也称为泛洪； 2)   若找到该MAC地址的记录，取出记录中的端口号，若该端口号与帧进入交换机的端口的端口号不同，则按该端口号进行“明确地转发”；若该端口号与帧进入交换机的端口的端口号相同，则丢弃该帧。 在本题中，主机00-e1-d5-00-23-a1给主机00-e1-d5-00-23-c1发送1个数据帧，该数据帧从交换机的端口1进入交换机，交换机取出帧的源MAC地址00-e1-d5-00-23-a1，将其与帧进入交换机的端口的端口号1作为一条记录存储到帧交换表中
  
  
  
  
  
  
  
  
  
  # 3.10 以太网交换机的生成树协议STP
  
  - 如何提高以太网的可靠性?
  
  - 添加**冗余链路**可以提高以太网的可靠性
  
    <img src="../img/截屏2020-11-03 下午2.39.39.png"  width="350"  height = "250" />
  
  - 但是,冗余链路也会带来负面效应- 形成**网络环路**
  
  <img src="../img/截屏2020-11-03 下午2.36.07.png"  width="350"  height = "250" />
  
  - 网络环路会带来以下问题 : 
  
    - 广播风暴 : 大量消耗**网络资源**, 使得网络无法正常转发其他数据帧
  
    - 主机收到重复的广播帧 : 大量消耗**主机资源**
  
    - 交换机的帧交换表震荡(漂移)
  
      <img src="../img/截屏2020-11-03 下午2.43.12.png"  width="150"  height = "100" />
  
      
  
  
  
  
  
  ### 以太网交换机的生成树协议STP
  
- 以太网交换机使用生成树协议**STP(Spanning Tree Protocol)**, 可以在增加冗余链路来提高网络可靠性的同时又**避免网络环路带来的各种问题**

  - 不论交换机之间采用怎样的物理连接, 交换机都能够自动计算并构建一个**逻辑上没有环路**的网络, 其逻辑拓扑结构必须是**树形的(无逻辑环路)**

  - 最终生成的树型逻辑拓扑要**确保连通整个网络**

  - 当**首次连接交换机**或**物理拓扑发生变化时**(人为改变或故障), 交换机都将进行**生成树的重新计算**

    <img src="../img/截屏2020-11-03 下午2.58.08.png"  width="250"  height = "350" />

  

  

  # 13.11.1 虚拟局域网VLAN概述

  - 以太网交换机工作在数据链路层(也包括物理层)

  - 使用一个或多个以太网交换机互联起来的交换式以太网, 其所有站点都属于同一个广播域

  - 随着交换式以太网规模扩大, 广播域相应扩大.

  - 巨大的广播域会带来很多弊端

    - 广播风暴

    - 难以管理和维护

    - 潜在的安全问题

      <img src="../img/截屏2020-11-03 下午3.45.39.png"  width="350"  height = "350" />

  

  -  网络中会频繁出现广播信息
    - TCP/IP协议栈中的很多协议都会使用广播:
      - 地址解析协议ARP(已知IP地址, 找出其相应的MAC地址)
      - 路由信息协议RIP(一种小型的内部路由协议)
      - 动态主机配置协议DHCP(用于自动配置IP地址)
    - NetBEUI : Windows下使用的广播型协议
    - IPX/SPX: NoveII网络协议栈
    - Apple Talk : Apple公司的网络协议栈

  

  - 分割广播域的方法

    - 使用**路由器**可以隔离广播域

      然而路由器**成本较高**

      

      <img src="../img/截屏2020-11-03 下午3.55.11.png"  width="450"  height = "250" />

      <img src="../img/截屏2020-11-03 下午3.55.17.png"  width="550"  height = "250" />

    - 虚拟局域网VLAN技术应运而生

      - **虚拟局域网VLAN(Virtual Local Area Network)**是一种将局域网内的设备划分成**与物理位置无关的逻辑组的技术**, 这些逻辑组具有某些共同的需求

        <img src="../img/截屏2020-11-03 下午4.06.02.png"  width="550"  height = "300" />

        根据应用需求, 我们将该局域网划分成两个vlan

        <img src="../img/截屏2020-11-03 下午4.07.39.png"  width="550"  height = "300" />

  ​      

  ​      

  

  # 13.11.2 虚拟局域网VLAN的实现机制

  虚拟局域网VLAN技术是在交换机上实现的, 需要交换机实现**以下两个功能**

  ### 能够处理带有VLAN标记的帧 ( IEEE 802.1Q帧 )

  - IEEE 802.1Q帧 (也称Dot One Q帧) 对以太网的MAC帧格式**进行了扩展**, 插入了4字节的VLAN标记

    <img src="../img/截屏2020-11-03 下午5.23.32.png"  width="750"  height = "100" />

  - VLAN标记的**最后12比特**称为VLAN标识符**VID**, 它唯一地标志了以太网帧属于哪一个VLAN
    - VID的取值范围是0~4095 (  $0 $ ~ $2^{12} - 1$ )
    - 0和4095都不用来表示VLAN, 因此用于表示VLAN的VID的有效取值范围是1 ~ 4094
  - 802.1Q帧是由**交换机**来处理的, 而不是**用户主机**来处理的
    - 当交换机**收到**普通的以太网帧时, 会将其插入4字节的VLAN标记转变为802.1Q帧, **简称“打标签”**
    - 当交换机**转发**802.1Q帧时, 可能会删除其4字节VLAN标记转变为普通以太网帧, **简称“去标签”**

    

  ### 交换机的端口类型 ( 可以支持不同的端口类型 )

  - 交换机的端口类型有以下三种:
    - Access
    - Trunk
    - Hybrid
  - 交换机各端口的缺省VLAN ID
    - 在思科交换机上称为Native VLAN, 即本征VLAN,默认为VLAN1
    - 在华为交换机上称为Port VLAN ID, 即端口VLAN ID, 简记为PVID

  

  #### Access端口 : 

  - Access端口一般用于连接用户计算机

  - Access端口只能属于一个VLAN

  - Access端口的PVID值与端口所属VLAN的ID相同(默认为1)

  - Access端口接收处理方法:

    一般只**接收“未打标签”的普通以太网MAC帧**, **根据**接收帧的端口的**PVID给帧“打标签”**,即插入4字节VLAN标记字段, 字段中的VID取值与端口的PVID取值相等

    <img src="../img/截屏2020-11-03 下午6.09.06.png"  width="450"  height = "250" />

  - Access端口发送处理方法:

    若帧中的VID与端口的PVID相等, **则“去标签”并转发**该帧; 否则不转发

    <img src="../img/截屏2020-11-03 下午6.13.20.png"  width="450"  height = "250" />

    

    

    [例] 一个VLAN的划分需求是这样的, 需要手动在交换机上创建VLAN2和VLAN3, 然后将交互机的端口**1和2**划归到VLAN2, 将交换机的端口**3和4**划分到VLAN3

  <img src="../img/截屏2020-11-03 下午6.16.28.png"  width="450"  height = "250" />

  ​    主机A发送广播帧的情况

  <img src="../img/截屏2020-11-03 下午8.13.18.png"  width="450"  height = "250" />

  主机C发送广播帧的情况

  <img src="../img/截屏2020-11-03 下午8.14.08.png"  width="450"  height = "250" />

​    

    #### Trunk端口:

- Trunk端口一般用于交换机之间或交换机与路由器

- Trunk端口可以属于多个VLAN

- 用户可以设置Trunk端口的PVID值, 默认情况下, Trunk端口的PVID值为1

- Trunk端口发送处理方法:

  - 对VID**等于**PVID的帧, **“去标签”再转发**;

- Trunk端口接收处理方法:

  - 接收“未打标签”的帧, 根据接收帧的端口的PVID给帧**“打标签”**, 即插入4字节VLAN标记字段, 字段中的VID取值与端口的PVID取值相等

    <img src="../img/截屏2020-11-03 下午8.22.01.png"  width="450"  height = "350" />



- Trunk端口发送处理方法:

  - 对VID**不等于**PVID的帧, **直接转发**;

- Trunk端口接收处理方法:

  - 接收“已打标签”的帧.

    <img src="../img/截屏2020-11-03 下午8.42.20.png"  width="450"  height = "350" />

​    [习题]下图给出了用于交换机互联的Trunk端口的PVID值的组合, 试回答以下问题:

(1)主机A发送广播帧, 则帧的传递过程是什么?

(2)主机C发送广播帧, 则帧的传递过程是什么?

(3)从上述过程可以得出什么结论?

<img src="../img/截屏2020-11-03 下午8.58.47.png"  width="950"  height = "450" />

Case1: A广播帧->打标签VID=1-> 标签=PVID, 去标签转发   ->   接收无标签帧,打标签VID=2 -> 转发到G、H(**分组错误**)

​			C广播帧->打标签VID=2-> 标签!=PVID, 直接转发   ->   直接接收“已打标签帧” -> 转发到G、H(分组正确)

Case2: A广播帧->打标签VID=1 -> 标签!=PVID, 直接转发   ->   直接接收“已打标签的帧”->转发到E、F(分组正确)

​			C广播帧->打标签VID=2 -> 标签=PVID, 去标签转发   ->   接收无标签帧, 打标签VID=1 -> 转发到 E、F(**分组错误**)

Case3: 略

Case4: A广播帧 -> 打标签VID=1 -> 标签!=PVID,直接转发  -> 直接接收“已打标签的帧” -> VID=1转发到E、F(分组正确)

​			C广播帧 -> 打标签VID=2 -> 标签!=PVID,直接转发  -> 直接接收“已打标签的帧” -> VID=2转发到G、H(分组正确)



#### Hybrid简介 :

​    <img src="../img/截屏2020-11-03 下午9.27.36.png"  width="950"  height = "350" />

去标签转发, 只能接受去标签后的帧,不再去标签列表里直接转发就不能识别

<img src="../img/截屏2020-11-03 下午9.31.59.png"  width="350"  height = "250" />







  





