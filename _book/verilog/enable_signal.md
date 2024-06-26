## 什么是使能信号?

使能信号(Enable Signal), 顾名思义, 就是控制某个电路/芯片开启的信号. 

我们以本次实验中开发板上的七段数码管显示模块为例, 来了解一下使能信号在具体模块中的用法.

<div align ="center"><img src="/img/verilog/03.png" alt="电路图" style="zoom:80%;" /><div align ="center">
<center style="color:#000000;font-size:10pt;">电路图</center>

上图为六位七段数码管的引脚图, 其中 DIG1-6 用来控制这六位数码管的亮灭, 高电平熄灭, 低电平点亮. 例如我们给 DIG1-6 赋值 111110, 则是第六位数码管点亮, 其余的数码管熄灭.

在这儿模块中, 只要 DIG 引脚的电平为高, 对应的数码管就不会点亮, 所以 DIG1-6 可以看作是六位数码管的使能信号. 

### 为什么需要使能信号?

还是来看六位数码管显示这个例子.

> #### hint::六位数码管的使能信号
>
> 有些同学可能看到数码管的引脚图会感到困惑, 一位数码管有 8 段 LED 构成, 一共有六个数码管, 如果我们想让六个数码管同时显示的话, 不是应该有 6 * 8 个引脚吗? 电路图上只有 6 + 8 个引脚, 怎么实现同时显示 6 个数字呢?
>
> 事实上, 六位数码管的段选信号是连在一起的, 如果点亮所有数码管, 在同一时刻只能显示相同的数字. 我们看到的六位数码管 "同时" 显示六个不同的数字, 其实是利用视觉暂留效果做到的. 通过轮流点亮数码管,  并在不同的时间给出不同的段选信号, 只要速度合适, 在人眼中就相当于六位数码管同时显示不同的数字. 
>
> 所以, 使能信号在这里起到的作用就显而易见了. 由于我们同一时间只能输出一个数据, 所以我们需要通过使能信号来**选择**哪一位数码管输出什么数据. 

<!--  -->

> #### fread::数码管动态显示的更多资料
>
> 了解更多:
>
> [[走近FPGA\]之数码管动态显示](https://zhuanlan.zhihu.com/p/195150754)

接下来, 我们来看另外一个例子. 

> #### hint::存储器的使能信号
>
> 在[LAB3: "灯, 等灯, 等灯"-流水灯的几种点法](../lab3/introduction.md)实验中, 我们需要用到两个存储器. 然而, 我们却只有一条数据总线. 当我们想要写或者读存储器的数据时, 应该怎么做呢?
>
> 由于所有的存储器都共用一条数据总线, 我们不可能同时对不同的存储器写入不同的数据. 如果想要对不同的存储器写入数据, 我们需要借助于[总线译码器](./decoder.md), 根据 CPU 发出的地址, 确定我们要对哪一个存储器进行操作, 此时, 存储器的使能如果有效, 总线就可以对其进行读写了. 
>
> 所以, 这是个有点类似于数码管显示的问题. 在数码管显示中, 我们只有公用的一组段选信号, 而在本例中, 我们只有一条数据总线. 使能信号在这里用于确定被选择的存储器能否进行工作.

### 使能信号还有什么作用?

使能信号的用途非常广泛, 除了可以作为计算机系统中的存储芯片选通信号和作为显示系统中的控制信号外, 其核心的一点就是使能信号控制了芯片的功能是否能被正常的启用. 在本次实验中, 我们将会更多的见到使能信号的作用.

