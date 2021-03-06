# 2.1 物理层的基本概念

- 传输媒体(不属于任何一层,非要放到层也是在物理层之下)

  - 导引型传输媒体
    - 双绞线
    - 同轴电缆
    - 光纤
  - 非导引型传输媒体
    - 微波通信(2 ~ 40GHz)

- 物理层考虑的是怎样才能在连接各种计算机的传输媒体上***传输数据比特流***

- 物理层为数据链路层屏蔽了各种传输媒体的差异,使数据链路层只需要考虑如何完成本层的协议和服务,而不必考虑网络具体的传输媒体是什么.

  

- 物理层协议的主要任务

  - 机械特性  : 指明接口所用接线器的***形状和尺寸***,***引脚数目***和***排列、固定和锁定***装置
  - 电气特性  : 指明在接口电缆的各条线上出现的***电压范围***
  - 功能特性  : 指明某条线上出现的某一电平的***电压表示何种意义***
  - 过程特性  : 指明对于不同功能的各种可能***事件的出现顺序***





# 2.2 物理层**下面**的传输媒体

导引型传输媒体:同轴电缆 双绞线 光纤 电力线

非导引型传输媒体:无线电波 微博 红外线 可见光

- 同轴电缆: 同轴电缆价格较贵且布线不够灵活方便, 随着集线器的出现,局域网采用双绞线了
  - 基带同轴电缆(50Ω) : 数字传输,过去用于局域网
  - 宽带同轴电缆(75Ω) : 模拟传输,目前主要用于有线电视

- 双绞线 : 绞合的作用,地狱部分来自外界的电磁波干扰,减少相邻导线的电磁干扰,超5类(5E)速率不超过1Gbit/s
  - 无屏蔽双绞线UTP电缆
  - 屏蔽双绞线STP电缆

- 光纤: 

  - 纤芯直径: 多模光纤 : 50微米,62.5微米

    ​				单模光纤: 9微米

  - 包层直径125微米

  - 工作波长: 0.85微米(衰减较大) 1.30微米(衰减较小) 1.55微米(衰减较小)

  - 优点 : 通信容量大(25000~30000GHz的带宽)

    ​		  传输损耗小,远距离传输时更加经济

    ​		  抗雷电和电磁干扰性能好.在大电流脉冲干扰的环境下尤为重要

    ​		  无串音干扰,保密性好,不易被窃听

    ​		  体积小,重量轻

    缺点: 割接需要专用设备

    ​		 光电接口价格较贵

    光在纤芯中的传输方式是不断地全反射

    <img src="../img/截屏2020-10-21 下午4.58.32.png"  width="800"  height = "300" />

  - 电力线 : 老房子避免布线 

  

  

  

  - 无线电波:

    <img src="../img/截屏2020-10-21 下午5.15.06.png"  width="800"  height = "300" />

    低频和中频频段用于**地面传播**, 高频和甚高频靠**电离层的反射**

    <img src="../img/截屏2020-10-21 下午5.18.18.png"  width="800"  height = "350" />

    

  - 微波: 按直线传播,微波会穿透电离层进入宇宙空间,不能经过电离层的反射进入到很远的地方

    - 传统微波通信:
      - 地面微波接力通信 : 微波直线传播,地面是曲面,传播距离限制50Km左右,两端通信中间好要若干个放大信号基站
      - 卫星通信 : 三个卫星,通信距离远,传播时延也大
    - 低轨道卫星开始在空间部署

    <img src="../img/截屏2020-10-21 下午5.22.20.png"  width="800"  height = "330" />

  - 红外线 : 家用电器 点对点无线传输,中间不能有障碍物,速率低

  - 可见光 : 新技术, 

- 无线电频谱管理机构:

  - 中国 : 工业和信息化部无线电管理局(国家无线电办公室)
  - 美国 : 联邦通讯委员会(FCC)
  - ISM (Industrial, Scientific, Medical)频段 : 自由使用的频段  (目前的无线局域网就是2.4GHz和5.8GHz)





# 2.3 传输方式

- 1.

  - 串行传输 : 数据是一个bit 一个bit连续发送的, 只需要一条传输线路

  - 并行传输 : 一次发送n个bit, 收发双方需要n条线路(成本高)

    > 在计算机网络中数据在线路中的传输是串行传输还是并行传输?
    >
    > 答: 串行传输(计算机内部常采用并行传输方式, 如CPU与内存之间通过总线, 总线宽度8位, 16位, 32位, 64位)

  <img src="../img/截屏2020-10-21 下午5.52.04.png"  width="800"  height = "100" />

  

  <img src="../img/截屏2020-10-21 下午5.53.21.png"  width="800"  height = "230" />

- 2.

  - **同步传输** : 数据块以稳定的比特流方式传输,字节之间没有间隔,接收端在比特信号**中间时刻**进行检测

    ​				 不同设备的时钟可能不同, 会出现**时钟累积误差**

    - 实现收发双方始终同步的方法 : 
      - 外同步 : 在收发双方之间添加一条**单独的时钟信号线**(接收端按照时钟的节奏接收数据)
      - 内同步 : 发送端将**时钟同步信号**, **编码到发送数据中**一起传输(如**曼彻斯特编码**)

    <img src="../img/截屏2020-10-21 下午5.55.11.png"  width="800"  height = "120" />

  - **异步传输** : 以**字节**为传输单位,字节间的时间间隔不是固定的,接收端仅对**字节内的比特**实现同步(要在每个字节前后加上起始位和结束位)

    <img src="../img/截屏2020-10-21 下午6.02.17.png"  width="800"  height = "110" />

    - **字节**之间异步(字节之间的时间间隔不固定)
    - 字节中的每个比特仍然要同步(各比特的持续时间是相同的)

- 3.

  - 单项通信(单工) -> 一条信道

    通信双方只有一个传输方向 : 无线电广播和收音机

    <img src="../img/截屏2020-10-21 下午6.12.58.png"  width="800"  height = "90" />

  - 双向交替通信(半双工) -> 两条信道

    通信双方可以交换数据,但**不能同时**进行 : 对讲机

    <img src="../img/截屏2020-10-21 下午6.13.44.png"  width="800"  height = "120" />

    

  - 双向同时通信(全双工) -> 两条信道

    通信双方可以**同时**发送和接收信息 : 电话

    <img src="../img/截屏2020-10-21 下午6.14.41.png"  width="800"  height = "120" />

    



# 2.4  编码与调制

(文字图片等)消息-----(运送消息的实体)---->数据data-----(数据的电磁表现)------->信号signal---------(信源发出的原始电信信号)----->基带信号----->**数字基带信号**(cup与内存之间) 和 **模拟基带信号**(麦克风录制)

- 信号需要在信道中进行传输

  - 对数字基带信号**波形**的改变称为**编码**,编码之后仍为数字信号

    把数字信号频率范围搬移到**较高频率范围**转换成模拟信号称为**调制**

  - 把模拟信号也可以**编码**转换为数字信号

    模拟信号调制的例子是加载到载波信号中传输

<img src="../img/截屏2020-10-21 下午8.07.26.png"  width="800"  height = "400" />



-  码元

  在使用时间域的波形表示数字信号时, 代表不同离散数值的基本波形

  <img src="../img/截屏2020-10-21 下午8.16.32.png"  width="400"  height = "100" />

- 传输媒体 != 信道

  对于**单工传输**,传输媒体中只包含一个信道, 要么是发送信道,要么是接受信道. 对于**半双工**和**全双工**,传输媒体中要包含两个信道: 一个发送信道,一个接受信道. 如果使用信道复用技术,一个媒体中可以包含多个信道.

  一般我们说,在计算机网络中,将数字基带信号通过编码在相应的信道上传输

  <img src="../img/截屏2020-10-21 下午8.22.05.png"  width="400"  height = "150" />

- 常用编码

  - 不归零编码(存在同步问题)

    需要额外一根传输线来传输时钟信号,使发送方和接收方同步.

    宁愿利用这根传输线传输**数据信号**,而不是时钟信号

    <img src="../img/截屏2020-10-21 下午8.28.00.png"  width="450"  height = "150" />

  - 归零编码

    每个码元传输结束后信号都要归零,所以接收方只要在信号归零后进行采样即可,不需要单独的时钟信号

    实际上,归零信号相当于把时钟信号用归零方式编码在了数据之内,成为**自同步**信号

    但是,归零编码中大部分的数据带宽,都用来**传输归零而浪费掉了**

    <img src="../img/截屏2020-10-21 下午8.30.44.png"  width="455"  height = "130" />

  - 曼彻斯特编码

    码元**中间时刻**的调变即表示时钟,有表示数据

    <img src="../img/截屏2020-10-21 下午8.37.11.png"  width="455"  height = "130" />

    

  - 差分曼彻斯特编码(比曼彻斯特编码变化少,适合较高传输速率)

    跳变仅表示时钟

    码元开始处电平是否发生变化表示数据

    <img src="../img/截屏2020-10-21 下午8.40.32.png"  width="500"  height = "130" />

> [2013年 题34]若下图为10 BaseT 网卡接收到的信号波形，则该网卡收到的比特串是 
>
> <img src="../img/截屏2020-10-21 下午8.45.25.png"  width="500"  height = "130" />
>
> 
>
> A．0011 0110 B．1010 1101 C．0101 0010 D．1100 0101 
>
> 【答案】A
>
> 【解析】 10BaseT代表的是传统以太网，而以太网使用的是曼彻斯特编码。
>
> <img src="../img/截屏2020-10-21 下午8.49.32.png"  width="500"  height = "130" />



- 基本调制方法: 数字信号转换为模拟信号

  <img src="../img/截屏2020-10-21 下午8.51.58.png"  width="700"  height = "300" />

  使用基本调制方法, 1个码元只能包含1个比特信息. 如何能使1个码元包含更多的比特呢?

  - 因为频率和相位是相关的,即频率是相位随时间的变化率,所以一次只能调制频率和相位两个中的一个

  - 通常情况下,相位和振幅可以结合起来一起调制,称为**正交振幅调制QAM**

    - QAM-16

      <img src="../img/截屏2020-10-21 下午9.00.16.png"  width="400"  height = "350" />

      - 12种相位

      - 每种相位有1或2种振幅可选

      - 可以调制出16种码元(波形), 每种码元可以对应表示4个比特

      - 码元与4个比特的对应关系采用格雷码

        任意两个相邻码元只有1个比特不同(减少错位)、

      - 每个码元可以携带4bit的信息量

        

        <img src="../img/截屏2020-10-21 下午9.03.04.png"  width="400"  height = "350" />









# 2.5 信道的极限容量

- 输入信号波形---------通信质量较好的信道----------> 输出信号波形失真不严重**可以识别**

- 输入信号波形---------通信质量很差的信道---------->输出信号波形失真**严重**无法识别

  失真因素: 码元传输速率, 信号传输距离, 噪声干扰, 传输媒体质量

### 奈氏准则 : 在假定的理想条件下, 为了避免码间串扰, 码元传输速率是有上限的

理想**低通信道**的最高码元传输速率 = 2W Baud = 2W码元/秒

理想**带通信道**的最高码元传输速率 = W Baud = W码元/秒

​		W : 信道带宽(单位为Hz)

​		Baud : 波特, 即码元/秒

- 码元传输速率又称为波特率、调制速率、波形速率或符号速率.(与比特率有一定关系)
  - 当1个码元只携带1比特的信息量时,则波特率(码元/秒)与比特率(比特/秒)在数值上是相等的
  - 当1个码元携带n比特的信息量时,则波特率转换成比特率时, 数值要乘以n
- 要提高信息传输速率(比特率), 就必须设法使每一个码元能携带更多个比特信息量. 这需要采用多元制
- 实际的信道所能传输的最高码元速率, 要明显低于奈氏准则给出的这个上限数值



-> 只要采用更好的调制方法, 让码元可以携带更多的比特, 岂不是可以无限制地提高信息的传输速率?

答案是否定的, 因为信道的极限信息传输速率好要受限于实际的信号在信道中传输的**信噪比**



### 香浓公式 : 带宽受限 且有 高斯白噪声干扰的 信道 的极限信息传输速率,

### $c = W \times log_2(1 + \frac{S}{N})$ (重要)

c : 信道的极限信息传输速率 (单位 b/s)

W : 信道带宽(单位: Hz)

S : 信道内所传**信号**的平均功率

N : 信道内的高斯**噪声**功率

S/N : 信噪比, 使用分贝(dB)作为度量单位时, 

### 信噪比(dB) = $10 \times log_{10}(\frac{S}{N})(dB)$ (重要)

- 信道带宽或信道中**信噪比越大**, 信息的极限**传输速率越高**
- 实际速率比极限低,实际中有干扰,如脉冲干扰、衰减和失真等,在香浓公式中并未考虑



-> 在信道带宽一定的情况下, 根据奈氏准则和香浓公式, 要想提高信息的传输熟虑就必须采用**多元制**(更好的调制方法)和努力**提高信道中的信噪比**

自从香浓公式发表后, 各种新的信号处理和调制方法就不断出现, 其目的都是为了尽可能地接近香浓公式给出的传输速率极限

> [2014年 题35]] 下列因素中，不会影响信道数据传输速率的是 
>
> A．信噪比 B．频率宽带 C．调制速度 D．信号传播速度 
>
> 【答案】D 
>
> 【解析】 本题考查以下三个知识点： 
>
> （1）奈氏准则：理想低通信道的最高码元传输速率=2W Baud，其中W为信道带宽，单位为Hz；Baud为波特，是码元传输速率的单位，1波特为每秒传送1个码元。 
>
> （2）香农公式：信道的极限信息传输速率C=W log2(1 + S/N) (bit/s)，其中W为信道的带宽，单位为Hz；S为信道内所传信号的平均功率；N为信道内部的高斯噪声功率;S/N为信噪比。
>
>  综合（1）和（2），对于频带宽度已确定的信道，如果信噪比也不能再提高了，并且码元传输速率也达到了上限值，还可以通过新的信号处理和调制方法，使码元可以携带更多比特的信息量，其目的都是为了尽可能地接近香农公式给出的传输速率极限。
>
> 



> [考研2009 题34]] 在无噪声情况下，若某通信链路的带宽为3kHz，采用4个相位，每个相位具有4种振幅的QAM调制技术，则该通信链路的最大数据传输速率是
>
>  A．12kbps B．24 kbps C．48 kbps D．96 kbps 
>
> 【答案】B 
>
> 【解析】 
>
> （1）奈氏准则给出了理想低通信道的**最高码元传输速率** = 2W Baud，其中W为信道带宽，单位为Hz；Baud为波特，是码元传输速率的单位，1波特为每秒传送1个码元。因此，该通信链路的最高码元传输速率 = 2 × 3k = 6k(Baud) = 6k(码元/秒)。
>
>  （2）采用4个相位，每个相位具有4种振幅的QAM调制技术，可以表示出4 × 4 = 16种状态，采用**二进制**对这**16种状态**进行编码，需要使用4个二进制位（$log_216=4$），也就是4bit，换句话说，**每个码元可以携带的信息量为4bit**。
>
> **数据传输速率 = 波特率（码元传输速率） × 每个码元所携带的信息量**
>
>  综合（1）和（2）可知，该通信链路的最大数据传输速率 = 6k(码元/秒) × 4(bit/码元) = 24 kbit/s = 24 kbps。



> [2011年 题34] 若某通信链路的数据传输速率为2400bps，采用4相位调制，则该链路的波特率是 
>
> A．600波特 B．1200波特 C．4800波特 D．9600波特 
>
> 【答案】B 
>
> 【解析】 
>
> （1）采用4相位调制，可以表示出4种状态，采用**二进制**对这**4种状态**进行编码，需要使用2个二进制位（$log_24=2$），也就是2bit，换句话说，**每个码元可以携带的信息量为2bit**。 
>
> （2）**数据传输速率 = 波特率（码元传输速率） × 每个码元所携带的信息量**
>
> ​    2400bps = 波特率 × 2bit      因此，波特率 = 1200波特 【注意】本题与2009年第34题类似。
>
> 



> [2016年 题34] 若连接R2和R3链路的频率带宽为8 kHz，信噪比为30 dB，该链路实际数据传输速率约为理论最大数据传输速率的50％，则该链路的实际数据传输速率约是
>
>  A．8kbps B．20kbps C．40kbps D．80kbps 
>
> 【答案】C 
>
> 【解析】 香农公式给出了带宽受限且有高斯白噪声干扰的信道的极限数据传输速率为 $W \times log_2(1 + \frac{S}{N})$，单位为b/s。其中，W为信道带宽，单位为Hz；**S/N为信噪比，即信号的平均功率和噪声的平均功率之比，并用分贝(dB)作为度量单位**，即：**$信噪比(dB) = 10 \times log_{10}(\frac{S}{N}) (dB)$**。 本题所给**信噪比为30dB**，因此**S/N=1000**；根据香农公式可计算出理论最大数据传输速率为 $8k × log_2(1 + 1000) ≈ 80kbps$，本题所给定实际数据传输速率为理论最大数据传输速率的50%，因此该链路的实际数据传输速率 ≈ 40kbps。 
>
> 【注意】本题与2009年第34题、2011年第34题以及2014年第35题类似。
>
> 



> [2017 题34] 若信道在无噪声情况下的极限数据传输速率不小于信噪比为30dB条件下的极限数据传输速率，则信号状态数至少是
>
>  A．4 B．8 C．16 D．32 
>
> 【答案】D 
>
> 【解析】 
>
> 设信号状态数为X, 以二进制编码,每个码元可携带bit数为$log_2X$
>
> （1）使用奈奎斯特采样定理计算无噪声情况下的**极限数据传输速率** = $2Wlog_2X$，其中W是信道带宽，单位是Hz；X是信号的状态数； 
>
> （2）使用香农公式计算带宽受限且有高斯白噪声干扰的极限数据传输速率 = Wlog2（1 + S/N），其中W是信道带宽，单位是Hz；S/N为信噪比，即信号的平均功率和噪声的平均功率之比，并用分贝(dB)作为度量单位，即：信噪比(dB) = 10log10(S/N) (dB)。 
>
> 根据题意可以列出不等式：2Wlog2X ≥ Wlog2（1 + S/N），题中给出信噪比为30dB，因此S/N=1000，将其代入不等式可以解出X≥32，选项D正确。 
>
> 【注意】本题与2009年第34题、2011年第34题、2014年第35题以及2016年第34题类似。
>
> 

