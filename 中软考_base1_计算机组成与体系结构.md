# 中级软件设计

## 计算机组成与体系结构

### 1. 数据的表示

#### 1.1 R进制

##### 1.1.1 概述

​		R进制转十进制使用按权展开法，其具体操作方式为：将R进制数的一位数值用R的k次方形式表示，即幕的底数是R，指数为k，k与该位和小数点之间的距离有关。当该位位于小数点左边，k值是该位和小数点之间数码的个数，而当该位位于小数点右边，k值是负值，其绝对值是该位和小数点之间数码的个数加1

##### 1.1.2 短除法

​		十进制转R进制使用短除法。例如将94转换为二进制数。

![1571071103501](.\image\1571071103501.png)

##### 1.1.2 二次方进制

二进制转八进制与十六进制数。

![1571071134631](.\image\1571071134631.png)



#### 1.2 原码、反码、补码、移码

![1571071246639](.\image\1571071246639.png)

1. 正数的原码、反码与补码是一样的
2. 反码：负数，除了符号位，其他位数的相反值。
3. 补码：负数，在反码的基础上+1
4. 移码：把补码取反（负数位取反）
5. 计算机中加减数计算，使用的是补码！！浮点数计算使用移码。

~~~
(2016年上半年试题3)
如果“2X”的补码是“90H”，那么X的真值是（  ）。
（3）A．72 
B.-56 
C.56 
D.111
试题分析
90H 即为二进制的：10010000。说明此数为负数，其反码为：10001111，其原码为：11110000，即-112，2X=-112，所以X=-56。
试题答案
（3）B

~~~





### 2. 数值表示范围

![1571071369251](.\image\1571071369251.png)

补码是反码基础上+1，移码是在补码的的基础上取符号数反，所以都会出现-0的情况



### 3. 浮点数运算

#### 3.1 结构与运算

浮点数表示：

~~~
N=尾数*基数细数
浮点数经常被写成如下的形式：X　=　Mx * 2Ex
~~~

![1572170461042](.\image\1572170461042.png)

​		其中Mx为该浮点数的尾数,一般为绝对值小于1的规格化的二进制小数,机器中多用原码（或补码）形式表示。Ex为该浮点数的阶码,一般为二进制整数,机器中多用移码（或补码）表示，给出的是一个指数的幂 

运算过程：

~~~
对阶>尾数计算>结果格式化
~~~

存储格式：

~~~
阶符+阶码+数符+尾数
~~~

~~~
1. 浮点数的表示分为阶和尾数两部分。两个浮点数相加时，需要先对阶，即( ) (n为阶差的绝对值)。
   A.将大阶向小阶对齐，同时将尾数左移n位
   B.将大阶向小阶对齐，同时将尾数右移n位
   C.将小阶向大阶对齐，同时将尾数左移n位
   D.将小阶向大阶对齐，同时将尾数右移n位

1、D
单击此链接查看真题视频解析http://edu.51cto.com/course/5827.html
解析:
所谓对阶是指将两个进行运算的浮点数的阶码对齐的操作。对阶的目的是为使两个浮点数的尾数能够进行加减运算。因为，当进行Mx●2Ex与My.2Ey加减运算时，只有使两浮点数的指数值部分相同，才能将相同的指数值作为公因数提出来，然后进行尾数的加减运算。
对阶的具体方法是:首先求出两浮点数阶码的差，即4E=Ex-Ey，将小阶码加上4E，使之与大阶码相等，同时将小阶码对应的浮点数的尾数右移相应位数，以保证该浮点数的值不变。
对阶的原则是小阶对大阶，之所以这样做是因为若大阶对小阶，则尾数的数值部分的高位需移出，而小阶对大阶移出的是尾数的数值部分的低位，这样损失的精度更小。

~~~



#### 3.2 特点

1. 一般尾数用补码，阶码用移码
2. 阶码的位数决定数的表示范围，位数越多范围越大
3. 尾数的位数决定数的有效精度，位数越多精度越高
4. 对阶时，小数向大数看齐
5. 对阶是通过较小数的尾数右移实现的

~~~
(2015年下半年试题3)
浮点数能够表示的数的范围是由其（  ）的位数决定的。
（3）A．尾数
B.阶码 
C.数符
D.阶符
试题分析
浮点数能表示的数的范围由阶码的位数决定，精度由尾数的位数决定。
试题答案
（3）B

~~~





### 4. 计算机结构

![1571071666937](.\image\1571071666937.png)

#### 4.1 运算器

运算器（算术运算+逻辑运算的部分）

- 算术逻辑单元ALU：数据的算术运算和逻辑运算
- 累加寄存器AC：通用寄存器，为ALU提供一个工作区，用在暂存数据
- 数据缓冲寄存器DR：写内存时，暂存指令或数据
- 状态条件寄存器PSW：存状态标志与控制标志（争议：也有将其归为控制器的）

~~~
(2017年上半年试题1)
CPU执行算术运算或者逻辑运算时，常将源操作数和结果暂存在（  ）中。

（1）A． 程序计数器 (PC)
B. 累加器 (AC)
C. 指令寄存器 (IR)
D. 地址寄存器 (AR)
试题分析
本题考查计算机组成原理中的CPU构成。
答案应该是累加寄存器，用来暂时存放算术逻辑运算部件ALU运算的结果信息。程序计数器（PC）是存放执行指令的地方，计算之前就要用到。指令寄存器（IR）保存当前正在执行的一条指令。地址寄存器（AR）用来保存当前CPU所要访问的内存单元的地址。
试题答案
（1）B
~~~

~~~
(2015年上半年试题2)
计算机中CPU对其访问速度最快的是（  ）。
（2）A．内存 
B.Cache 
C.通用寄存器 
D.硬盘
试题分析
题目中的存储设备按访问速度排序为：通用寄存器> Cache>内存>硬盘。
试题答案
（2）C

~~~

~~~
(2014年下半年试题3)
属于CPU中算术逻辑单元的部件是（  ）。
（3）A．程序计数器 
B.加法器 
C.指令寄存器 
D.指令译码器
试题分析
运算器：
① 算术逻辑单元ALU
② 累加寄存器
③ 数据缓冲寄存器
④ 状态条件寄存器
控制器：
① 程序计数器PC
② 指令寄存器IR
③ 指令译码器
④ 时序部件
试题答案
（3）B

~~~





#### 4.2 控制器

- 程序计数器PC：存储下一条要执行指令的地址
- 指令寄存器IR：存储即将执行的指令
- 指令译码器ID：对指令中的操作码字段进行分析解释
- 时序部件：提供时序控制信号
- 地址寄存器（当前访问的指令地址）



### 5. 计算机系统结构分类

![1571071997139](.\image\1571071997139.png)

~~~
(2017年上半年试题3)
计算机系统中常用的输入/输出控制方式有无条件传送、中断、程序查询和 DMA方式等。当采用（  ）方式时，不需要 CPU 执行程序指令来传送数据。

（3）A．中断
B.程序查询
C.无条件传送
D.DMA
试题分析
本题考查DMA方式的特点。在计算机中，实现计算机与外部设备之间数据交换经常使用的方式有无条件传送、程序查询、中断和直接存储器存取(DMA)。其中前三种都是通过CPU执行某一段程序，实现计算机内存与外设间的数据交换。
只有DMA方式下，CPU交出计算机系统总线的控制权，不参与内存与外设间的数据交换。而DMA方式工作时，是在DMA控制硬件的控制下，实现内存与外设间数据的直接传送，并不需要CPU参与工作。由于DMA方式是在DMA控制器硬件的控制下实现数据的传送，不需要CPU执行程序，故这种方式传送的速度最快。
试题答案
（3）D
~~~

~~~
试题21(2016年上半年试题1)
VLIW是（  ）的简称。
（1）A．复杂指令系统计算机
B.超大规模集成电路
C.单指令流多数据流
D.超长指令字
试题分析
VLIW：（Very Long Instruction Word，超长指令字）一种非常长的指令组合，它把许多条指令连在一起，增加了运算的速度。
试题答案
（1）D

~~~

~~~
(2015年上半年试题4)
计算机中CPU的中断响应时间指的是（  ）的时间。
（4）A．从发出中斯请求到中断处理结束
B.从中断处理开始到中断处理结束
C.CPU分析判断中断请求
D.从发出中断请求到开始进入中断处理程序
试题分析
本题考查计算机系统的基础知识。
中断系统是计算机实现中断功能的软硬件总称。一般在CPU中设置中断机构，在外设接口中设置中断控制器，在软件上设置相应的中断服务程序。中断源在需要得到CPU服务时，请求CPU暂停现行工作转向为中断源服务，服务完成后，再让CPU回到原工作状态继续完成被打断的工作。中断的发生起始于中断源发出中断请求，中断处理过程中，中断系统需要解决一系列问题，包括中断响应的条件和时机，断点信息的保护与恢复，中断服务程序入口、中断处理等。中断响应时间，是指从发出中断请求到开始进入中断服务程序所需的时间。
试题答案
（4）D

~~~

~~~
(2017年下半年试题6)
计算机运行过程中，CPU需要与外设进行数据交换。采用（  ）控制技术时，CPU与外设可并行工作。
（6）A．程序查询方式和中断方式
B.中断方式和DMA方式
C.程序查询方式和DMA方式
D.程序查询方式、中断方式和DMA方式
试题分析
程序查询方式是通过CPU执行程序来查询状态的。
试题答案
（6）B

~~~





### 6.指令的基本概念

一条指令就是机器语言的一个语句，它是一组有意义的二进制代码，指令的基本格式如下：

~~~
操作码字段 | 地址码字段
~~~

- 操作码部分指出了计算机要执行什么性质的操作，如加法、减法、取数、存数等。
- 地址码字段需要包含各操作数的地址及操作结果的存放地址等，从其地址结构的角度可以分为三地址指令、二地址指令、一地址指令和零地址指令。

![1571072164445](.\image\1571072164445.png)

~~~
(2014年下半年试题6)
Flynn分类法基于倍息流特征将计算机分成4类，其中（  ）只有理论意义而无实例。
（6）A．SISD 
B.MISD 
C.SIMD 
D.MIMD
试题分析
Flynn于1972年提出了计算平台的Flynn分类法，主要根据指令流和数据流来分类，共分为四种类型的计算平台：
单指令流单数据流机器（SISD）
SISD机器是一种传统的串行计算机，它的硬件不支持任何形式的并行计算，所有的指令都是串行执行。并且在某个时钟周期内，CPU只能处理一个数据流。因此这种机器被称作单指令流单数据流机器。早期的计算机都是SISD机器，如冯诺.依曼架构，如IBM PC机，早期的巨型机和许多8位的家用机等。
单指令流多数据流机器（SIMD）
SIMD是采用一个指令流处理多个数据流。这类机器在数字信号处理、图像处理、以及多媒体信息处理等领域非常有效。
Intel处理器实现的MMXTM、SSE（Streaming SIMD Extensions）、SSE2及SSE3扩展指令集，都能在单个时钟周期内处理多个数据单元。也就是说我们现在用的单核计算机基本上都属于SIMD机器。
多指令流单数据流机器（MISD）
MISD是采用多个指令流来处理单个数据流。由于实际情况中，采用多指令流处理多数据流才是更有效的方法，因此MISD只是作为理论模型出现，没有投入到实际应用之中。
多指令流多数据流机器（MIMD）
MIMD机器可以同时执行多个指令流，这些指令流分别对不同数据流进行操作。最新的多核计算平台就属于MIMD的范畴，例如Intel和AMD的双核处理器等都属于MIMD。
试题答案
（6）B

~~~





### 7.寻址方式

![1571072201806](.\image\1571072201806.png)

~~~
(2016年下半年试题1)
在程序运行过程中，CPU需要将指令从内存中取出并加以分析和执行。CPU依据（  ）来区分在内存中以二进制编码形式存放的指令和数据。
（1）A．指令周期的不同阶段
B.指令和数据的寻址方式
C.指令操作码的译码结果
D.指令和数据所在的存储单元
试题分析
指令和数据均存放在内存中，通常由PC（程序计数器）提供存储单元地址取出的是指令，由指令地址码部分提供存储单元地址取出的是数据。因此通过不同的寻址方式来区别指令和数据。
试题答案
（1）B

~~~

~~~
(2016年下半年试题2)
计算机在一个指令周期的过程中，为从内存读取指令操作码，首先要将（  ）的内容送到地址总线上。
（2）A．指令寄存器（IR）
B.通用寄存器（GR）
C.程序计数器（PC）
D.状态寄存器（PSW）
试题分析
PC（程序计数器）是用于存放下一条指令所在单元的地址。当执行一条指令时，处理器首先需要从PC中取出指令在内存中的地址，通过地址总线寻址获取。
试题答案
（2）C

~~~

~~~
(2015年下半年试题4)
在机器指令的地址字段中，直接指出操作数本身的寻址方式称为（  ）。
（4）A．隐含寻址 
B.寄存器寻址 
C.立即寻址 
D.直接寻址
试题分析
立即寻址是一种特殊的寻址方式，指令中在操作码字段后面的部分不是通常意义上的操作数地址，而是操作数本身，也就是说数据就包含在指令中，只要取出指令，也就取出了可以立即使用的操作数。
在直接寻址中，指令中地址码字段给出的地址A就是操作数的有效地址，即形式地址等于有效地址。
间接寻址意味着指令中给出的地址A不是操作数的地址，而是存放操作数地址的主存单元的地址，简称操作数地址的地址。
寄存器寻址指令的地址码部分给出了某一个通用寄存器的编号Ri，这个指定的寄存器中存放着操作数。
试题答案
（4）C

~~~





### 8.CISC与RISC

![1571072228565](.\image\1571072228565.png)

指令数量、指令使用频率，寻址方式，寄存器，流水线支持，高级语言支持

- CISC：复杂，指令数量多，频率差别大，多寻址
- RISC：精简，指令数量少，操作寄存器，单周期，少寻址，多通用寄存器，流水线

~~~
(2014年下半年试题5)
以下关于RISC和CISC的叙述中，不正确的是（  ）。
（5）A．RISC通常比CISC的指令系统更复杂 
B.RISC通常会比CISC配置更多的寄存器 
C.RISC编译器的子程序库通常要比CISC编译器的子程序库大得多 
D.RISC比CISC更加适合VLSI工艺的规整性要求
试题分析
本题考查计算机复杂指令集。
CISC计算机指复杂指令集计算机，是20世纪六、七十年代发展起来的系列计算机。这种计算机所支持的指令系统趋于多用途、强功能化。指令系统围绕着缩小与高级语言的语义差距以及有利于操作系统的优化而设计。指令系统的复杂化使得设计周期变长，正确性难于保证，不易维护。而且在复杂的指令系统中，只有少数基本指令是经常使用的，需要大量硬件支持的复杂指令利用率却很低。所以在70年代末，随着VLSI技术的发展产生了RISC计算机。
RISC计算机指精简指令集计算机，这种计算机有下列特点。
(1)指令系统中只包含使用频率较高但不复杂的指令。
(2)指令长度固定，指令格式少，寻址方式少。
(3)只有存取数指令访问主存，其他指令都在寄存器之间运算。
(4)大部分指令在一个机器周期内完成，采用流水技术。
(5)CPU中增加了通用寄存器的数量。
(6)硬联逻辑控制，不用微程序控制技术。
(7)采用优化的编译，以有效地支持高级语言。
试题答案
（5）A

~~~





### 9.流水线

#### 9.0 真题

1. （2018-上）流水线的吞吐率是指单位时间流水线处理的任务数，如果各段流水的操作时间不同，则流水线的吞吐率是（   ）的倒数。 
   A.最短流水段操作时间     B.各段流水的操作时间总和 
   C.最长流水段操作时间     D.流水段数乘以最长流水段操作时

   ~~~
   流水线处理机在执行指令时，把执行过程分为若干个流水级，若各流水级需要的时间不同，则流水线必须选择各级中时间最大者为流水级的处理时间。
   理想情况下，当流水线充满时，每一个流水级时间流水线输出-个结果。流水线的吞吐率是指单位时间流水线处理机输出的结果的数目,因此流水线的吞吐率为-一个流水级时间的倒数，即最长流水级时间的倒数。
   ~~~

   ~~~
2017年下半年试题2
   某四级指令流水线分别完成取指、取数、运算、保存结果四步操作。若完成上述操作的时间依次为8ns、9ns、 4ns、8ns，则该流水线的操作周期应至少为（  ）ns 。
   （2）A．4
   B.8
   C.9
   D.33
   试题分析
   流水周期为9ns。
   试题答案
   （2）C
   ~~~
   
   ~~~
   (2015年上半年试题6)
   以下关于指令流水线性能度量的叙述中，错误的是（  ）。
   （6）A．最大吞吐率取决于流水线中最慢一段所需的时间 
   B.如果流水线出现断流，加速比会明显下降 
   C.要使加速比和效率最大化应该对流水线各级采用相同的运行时间 
   D.流水线采用异步控制会明显提高其性能
   试题分析
   采用异步控制方式在给流水线提速的同时，会明显增加流水线阻塞的概率，所以不会明显提高整体性能。
   试题答案
   （6）D
   
   ~~~
   
   

#### 9.1 概念

- 相差相关参数计算：流水线执行时间计算、流水线吞吐率、流水线加速比、流水线效率
- 流水线是指在程序执行时多条指令重叠进行操作的一种准并行处理实现技术。各种部件同时处理是针对不同指令而言的，它们可同时为多条指令的不同部分进行工作，以提高各部件的利用率和指令的平均执行速度

![1571072336572](.\image\1571072336572.png)

#### 9.2 计算

- 流水线周期为执行时间最长的一段

- 流水线计算公式为：

  ~~~
  1条指令执行时间+(指令条数-1)*流水线周期
  ①理论公式：(t1+t2+...+tk)+(n-1)*△t
  ②实践公式：k*△t+(n-1)*△t
  ~~~

![1571072364215](.\image\1571072364215.png)

​		一条指令的执行过程可以分解为取指、分析和执行三步，在取指时间t取指=3△t、分析时间t份析=2△t、执行时间t执行=4△t的情况下，若按串行方式执行，则10条指令全部执行完需要（90）△t；若按流水线的方式执行，流水线周期为（4）△t，则10条指令全部执行完需要（45）△t

~~~~
理论公式：(t1+t2+...+tk)+(n-1)*△t
~~~~

![1571072581921](.\image\1571072581921.png)

~~~
实践公式：k*△t+(n-1)*△t
~~~

![1571072606043](.\image\1571072606043.png)

~~~
(2016年下半年试题5)
将一条指令的执行过程分解为取指、分析和执行三步，按照流水方式执行，若取指时间t取指=4△t、分析时间t分析=2△t、执行时间t执行=3△t，则执行完100条指令，需要的时间为（  ）△t。
（5）A．200
B.300
C.400
D.405
试题分析
         第一条指令执行时间+(指令数-1)*各指令段执行时间中最大的执行时间。
         4△t + 3△t + 2△t +（100-1）X 4△t = 405△t
试题答案
（5）D

~~~

![1572943825976](.\image\1572943825976.png)

![1572943856268](.\image\1572943856268.png)

~~~
(2015年下半年试题25-26)
假设磁盘块与缓冲区大小相同，每个盘块读入缓冲区的时间为15μs，由缓冲区送至用户区的时间是5μs，在用户区内系统对每块数据的处理时间为1μs，若用户需要将大小为10个磁盘块的Doc1文件逐块从磁盘读入缓冲区，并送至用户区进行处理，那么采用单缓冲区需要花费的时间为（  ）μs；采用双缓冲区需要花费的时间为（  ）μs。
（25）A．150 
B.151 
C.156 
D.201 

（26）A．150 
B.151 
C.156 
D.201
试题分析
 单缓冲区：(15+5)*10+1=201
 双缓冲区：15*10+5+1=156
试题答案
（25）D（26）C

~~~



#### 9.3 流水线吞吐率计算

​		流水线的吞吐率（Though Put rate，TP）是指在单位时间内流水线所完成的任务数量或输出的结果数量。

~~~
计算流水线吞吐率的最基本的公式：TP = 指令条数/流水线执行时间
流水线最大的吞吐量：TP = 1/△t
~~~



### 10.层次化存储结构

![1571072762109](.\image\1571072762109.png)



### 11. Cache 

#### 11.1 概念

​		在计算机的存储系统体系中，Cache是访问速度最快的层次（若有寄存器，则寄存器最快）。

​		使用Cache改善系统性能的依据是程序的局部性原理。

​		如果以h代表对Cache的访问命中率，t1表示Cache的周期时间，t2表示主存储器周期时间，以读操作为例，使用“Cache+主存储器”的系统的平均周期为t3，则：t3=h * t1+（1-h）* t2 其中，（1-h）又称为失效率（未命中率）。

~~~
以下关于Cache (高速缓冲存储器)的叙述中，不正确的是（  ）。

（6）A． Cache 的设置扩大了主存的容量
B. Cache 的内容是主存部分内容的拷贝
C. Cache 的命中率并不随其容量增大线性地提高
D. Cache 位于主存与 CPU 之间
试题分析
本题考查计算机组成原理中的高速缓存基础知识。高速缓存Cache有如下特点：它位于CPU和主存之间，由硬件实现；容量小，一般在几KB到几MB之间；速度一般比主存快5到10倍，由快速半导体存储器制成；其内容是主存内容的副本（所以Cache无法扩大主存的容量），对程序员来说是透明的；Cache既可存放程序又可存放数据。
Cache存储器用来存放主存的部分拷贝（副本）。控制部分的功能是：判断CPU要访问的信息是否在Cache存储器中，若在即为命中，若不在则没有命中。命中时直接对 Cache存储器寻址。未命中时，若是读取操作，则从主存中读取数据，并按照确定的替换原则把该数据写入Cache存储器中：若是写入操作，则将数据写入主存即可。
试题答案
（6）A

~~~

~~~~
(2015年下半年试题2)
虚拟存储体系由（  ）两级存储器构成。
（2）A．主存-辅存 
B.寄存器-Cache 
C.寄存器-主存 
D.Cache-主存
试题分析
虚拟存储器是一个容量非常大的存储器的逻辑模型，不是任何实际的物理存储器。它借助于磁盘等辅助存储器来扩大主存容量，使之为更大或更多的程序所使用。
虚拟存储器指的是主存-外存层次。它以透明的方式给用户提供了一个比实际主存空间大得多的程序地址空间。此时的程序的逻辑地址称为虚拟地址（虚地址），程序的逻辑地址空间称为虚拟地址空间。物理地址（实地址）由CPU地址引脚送出，它是用于访问主存的地址。设CPU地址总线的宽度为m位，那么物理地址空间的大小用2m来表示。
试题答案
（2）A

~~~~





#### 11.2 Cache映像

- 直接相联映像：硬件电路较简单，但冲突率很高
- 全相联映像：电路难于设计和实现，只适用于小容量的cache，冲突率较低。
- 组相联映像：直接相联与全相联的折中
- 地址映像是将主存与Cache的存储空间划分为若干大小相同的页（或称为块）。

例如，某机的主存容量为1GB，划分为2048页，每页512KB；Cache容量为8MB，划分为16页，每页512KB。

~~~~
(2017年下半年试题1)
在程序的执行过程中，Cache与主存的地址映射是由（  ）完成的。
（1）A．操作系统
B.程序员调度
C.硬件自动
D.用户软件
试题分析
在程序的执行过程中，Cache与主存的地址映射是由硬件自动完成的。
试题答案
（1）C
~~~~

~~~
(2016年下半年试题6)
以下关于Cache与主存间地址映射的叙述中，正确的是（  ）。
（6）A．操作系统负责管理Cache与主存之间的地址映射
B.程序员需要通过编程来处理Cache与主存之间的地址映射
C.应用软件对Cache与主存之间的地址映射进行调度
D.由硬件自动完成Cache与主存之间的地址映射
试题分析
在程序的执行过程中，Cache与主存的地址映射是由硬件自动完成的。
试题答案
（6）D

~~~





##### 11.2.1 直接相联映像

会排挤之前的，不能复用，容易冲突

![1571072983646](.\image\1571072983646.png)



##### 11.2.2 全相联映像

![1571073008302](.\image\1571073008302.png)



##### 11.2.3 组相联映像

![1571073041737](.\image\1571073041737.png)

~~~
(2016年上半年试题2)
主存与Cache的地址映射方式中，（  ）方式可以实现主存任意一块装入Cache中任意位置，只有装满才需要替换。
（2）A．全相联 
B.直接映射 
C.组相联 
D.串并联
试题分析
全相联映射是指主存中任一块都可以映射到Cache中任一块的方式，也就是说，当主存中的一块需调入Cache时，可根据当时Cache的块占用或分配情况，选择一个块给主存块存储，所选的Cache块可以是Cache中的任意一块。
试题答案
（2）A

~~~

~~~
(2015年上半年试题3)
Cache的地址映像方式中，发生块冲突次数最小的是（  ）。
（3）A．全相联映像 
B.组相联映像 
C.直接映像 
D.无法确定的
试题分析
全相联映像块冲突最小，其次为组相联映像，直接映像块冲突最大。
试题答案
（3）A

~~~





### 12. 主存-编址与计算

​		存储单元

- 卷按字编址：存储体的存储单元是字存储单元，即最小寻址单位是一个字端
- 按字节编址：存储体的存储单元是字节存储单元；即最小寻址单位是一个字节。

●根据存储器所要求的容量和选定的存储芯片的容量，就可以计算出所需芯片的总数，即：

~~~
总片数=总容量/每片的容量
~~~

例：若内存地址区间为4000H~43FFH，每个存储单元可存储16位二进制数，该内存区域用4片存储器芯片构成，则构成该内存所用的存储器芯片的容量是多少？

~~~
= 43FFH-4000H+1 = 400H
= 0100 0000 0000 = 2的10次方
= 1024 * 16bit / 4
~~~

~~~
(2017年下半年试题4)
计算机系统的主存主要是由（  ）构成的。
（4）A．DRAM
B.SRAM
C.Cache
D.EEPROM
试题分析
DRAM：动态随机存取存储器; SRAM: 静态随机存取存储器; Cache: 高速缓存; EEPROM: 电可擦可编程只读存储器。
试题答案
（4）A
~~~

~~~
(2017年上半年试题23)
某文件管理系统在磁盘上建立了位示图(bitmap) ，记录磁盘的使用情况。若计算机 系统的字长为 32 位，磁盘的容量为 300GB ，物理块的大小为4MB ，那么位示图的大小需要（  ）个字。

（23）A．1200
B.2400
C.6400
D.9600
试题分析
由于磁盘容量为300GB，物理块大小4MB，所以共有300*1024/4=75*1024块物理块，位示图用每1位表示1个磁盘块的使用情况，1个字是32位，所以1个字可以表示32块物理块使用情况，那么需要75*1024/32＝2400个字表示使用情况。
试题答案
（23）B
~~~

~~~
(2016年上半年试题25)
某磁盘有100个磁道，磁头从一个磁道移至另一个磁道需要6ms。文件在磁盘上非连续存放，逻辑上相邻数据块的平均距离为10个磁道，每块的旋转延迟时间及传输时间分别为100ms和20ms，则读取一个100块的文件需要（  ）ms。
（25）A．12060 
B.12600 
C.18000 
D.186000
试题分析
(6x10+100+20)x100=18000
试题答案
（25）C

~~~

~~~
(2015年下半年试题5)
内存按字节编址从B3000H到DABFFH的区域其存储容量为（  ）。
（5）A．123KB
B.159KB
C.163KB
D.194KB
试题分析
本题考查计算机组成基础知识。
直接计算16进制地址包含的存储单元个数即可。
DABFFH-B3000H+1=27C00H=162816=159k，按字节编址，故此区域的存储容量为159kB。
试题答案
（5）B

~~~



### 13. 总线

一条总线同一时刻仅允许一个设备发送，但允许多个设备接收。

总线的分类

- 数据总线（Data Bus）：在CPU与RAM之间来回传送需要处理或是需要储存的数据。传送数据信息，CPU一次传输的数据与数据总线带宽相等。
- 地址总线（Address Bus）：用来指定在RAM（Random Access Memory）之中储存的数据的地址。传送控制信号和时序信号，如读/写、片选、中断响应信号等 。
- 控制总线（Control Bus）：将微处理器控制单元（Control Unit）的信号，传送到周边设备，一般常见的为USBBus和1394 Bus。         传送地址，它决定了系统的寻址空间 。

~~~
(2016年上半年试题6)
以下关于总线的叙述中，不正确的是（  ）。
（6）A．并行总线适合近距离高速数据传输 
B.串行总线适合长距离数据传输 
C.单总线结构在一个总线上适应不同种类的设备，设计简单且性能很高 
D.专用总线在设计上可以与连接设备实现最佳匹配
试题分析
在单总线结构中，CPU与主存之间、CPU与I/O设备之间、I/O设备与主存之间、各种设备之间都通过系统总线交换信息。单总线结构的优点是控制简单方便，扩充方便。但由于所有设备部件均挂在单一总线上，使这种结构只能分时工作，即同一时刻只能在两个设备之间传送数据，这就使系统总体数据传输的效率和速度受到限制，这是单总线结构的主要缺点。
试题答案
（6）C
~~~

~~~
(2015年上半年试题5)
总线宽度为32bit，时钟频率为200MHz，若总线上每5个时钟周期传送一个32bit的字，则该总线的带宽为（  ）MB/S。
（5）A．40 
B.80 
C.160 
D.200
试题分析
200M/5*32bit /8bit=160MB/S

根据总线时钟频率为200MHz，得1 个时钟周期为1/200MHz=0.005μs
总线传输周期为0.005μs×5=0.025μs
由于总线的宽度为32 位=4B（字节）
故总线的数据传输率为4B/（0.025μs）=160MBps
试题答案
（5）C

~~~

~~~
(2014年下半年试题1)
三总线结构的计算机总线系统由（  ）组成。
（1）A．CPU总线、内存总线和IO总线 
B.数据总线、地址总线和控制总线 
C.系统总线、内部总线和外部总线 
D.串行总线、并行总线和PCI总线
试题分析
计算机内部总线为三总线结构，它们分别是地址总线、数据总线和控制总线。
数据总线：传送数据信息，CPU一次传输的数据与数据总线带宽相等
控制总线：传送控制信号和时序信号，如读/写、片选、中断响应信号等
地址总线：传送地址，它决定了系统的寻址空间 
试题答案
（1）B

~~~





### 14. 串联和并联系统

![1571073277139](.\image\1571073277139.png)

 串联公式 

~~~
R = R1 * R2 * ... * R3
~~~

并联公式 

~~~
R = 1 - (1 - R1) * (1 - R2 ) * ... * (1 - R3)
~~~

![1572938425731](.\image\1572938425731.png)

~~~
某系统由下图所示的冗余部件构成。若每个部件的千小时可靠度都为 R ，则该系 统的千小时可靠度为（  ）。
 
（4）A．(1-R3)(1-R2)
B.(1-(1-R)3)(1-(1-R)2)
C.(1-R3)+(1-R2)
D.(1-(1-R)3)+(1-(1-R)2)
试题分析
本题考查系统可靠度的概念。
串联部件的可靠度=各部件的可靠度的乘积。
并联部件的可靠度=1-部件失效率的乘积。
题目中给出的系统是“先并后串”。
此时先求出三个R并联可靠度为：1-（1-R）3
然后求出两个R并联可靠度为：1-（1-R）2
最终整个系统的可靠度是两者之积：（1-（1-R）3）*（1-（1-R）2）。
试题答案
（4）B

~~~

~~~
计算机系统的（  ）可以用MTBF/（1+MTBF）来度量，其中MTBF为平均失效间隔时间。

（34）A．可靠性
B.可用性
C.可维护性
D.健壮性
试题分析
这是可靠性的度量指标
试题答案
（34）A

~~~





### 15. N膜混合系统

![1571073584078](.\image\1571073584078.png)

~~~
R * (1 - (1 - R) * (1 - R) * (1 - R)) * (1 - (1 - R) * (1 - R) )
~~~



### 16. 校验码

#### 16.1 概念

​		码距：任何一种编码都由许多码字构成，任意两个码字之间最少变化的二进制位数就称为数据校验码的码距。

​		例如，用4位二进制表示16种状态，则有16个不同的码字，此时码距为1。
​		如0000与0001。

#### 16.2 奇偶校验

奇偶校验码的编码方法是：由若干位有效信息（如一个字节），再加上一个二进制位（校验位）组成校验码。

- 奇校验：整个校验码（有效信息位和校验位）中“1”的个数为奇数。
- 偶校验：整个校验码（有效信息位和校验位）中“1”的个数为偶数。

奇偶校验，可检查1位的错误，不可纠错。

#### 16.3 循环校验码CRC

​		什么是模2除法，它和普通的除法有何区别？

​		模2除法是指在做除法运算的过程中不计其进位的除法。

​		CRC校验，可检错，不可纠错。

- CRC的编码方法是：在k位信息码之后拼接r位校验码。应用CRC码的关键是如何从k位信息位简便地得到r位校验位（编码），以及如何从k+r位信息码判断是否出错。
- 循环沉余校验码编码规律如下：
  - ① 把待编码的N位有效信息表示为多项式M(X)；
  - ② 把M0左移K位，得到M(X) * X^k，这样空出了K位，以便拼装K位余数（即校验位）；
  - ③ 选取一个K+1位的产生多项式G(X)，对M(X) * X^k做模2除；
  - ④ 把左移K位以后的有效信息与余数R(X)做模2加减，拼接为CRC码，此时的CRC码共有N+K位。
- 把接收到的CRC码用约定的生成多项式G(X)去除，如果正确，则余数为0；如果某一位出错，则余数不为0。不同的位数出错其余数不同，余数和出错位序号之间有惟一的对应关

![1571074079135](.\image\1571074079135.png)



#### 16.4 海明校验码

##### 16.4.0  真题

1. （2018.上）海明码是一种纠错码， 其方法是为需要校验的数据位增加若干校验位， 使得校验位的值决定于某些被校位的数据，当被校数据出错时，可根据校验位的值的变化找到出错位，从而纠正错误。对于 32 位的数据，至少需要增加（  ）个校验位才能构成海明码。 
   以 10 位数据为例，其海明码表示为 D9D8D7D6D5D4P4D3D2D1P3D0P2P1中，其中 Di(0<=i<=9)表示数据位，Pj(1<=j<=4)表示校验位，数据位 D9 由 P4 P3 和 P2 进行校验（从右至左 D9 的位序为14，即等于 8+4+2，因此用第 8 位的 P4，第 4 位的 P3 和第 2 位的 P2 校验） ，数据位 D5 由（   ）进行校验。 
   A.3       B.4         C.5          D.6 
   A.P4P1     B.P4P2       C.P4P3P1      D.P3P2P1 

~~~
BD 解析:设数据位为m,校验位个数为k，如果满足2k -1>m+k (m+k 为编码后的数编总长度),则在理论上，k个校验码就可以判断是哪一位(包括信息码和校验码)出现了问题。解:2k-1>32+k得k=6。
D5的位序为10，即等于8+2，因此用第8位的P4和第2位的P2校验
~~~





##### 16.4.1 概念

​		海明校验，可检错，也可纠错。

​		海明校验码的原理是：在有效信息位中加入几个校验位形成海明码，使码距比较均匀地拉大，并把海明码的每个二进制位分配到几个奇偶校验组中。当某一位出错后，就会引起有关的几个校验位的值发生变化，这不但可以发现错误，还能指出错误的位置，为自动纠错提供了依据，如果为32位，那么m=32

~~~
2^r >= m + r + 1
~~~

![1571074190777](.\image\1571074190777.png)