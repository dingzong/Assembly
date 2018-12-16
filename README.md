# 汇编语言基础
&ensp;&ensp;&ensp;&ensp;*对于汇编这门语言来说，了解是很必要的。无论外面的世界高级语言如何变幻，谁强又谁弱，哪个被淘汰哪个又兴起，作为汇编这门语言来说，云淡风轻，因为偏向底层，和CPU拉手手的，就是不一样哈~大家都是靠它来完成与硬件的对接，逻辑的实现。所以简单介绍与学习一下这门语言。*

## 目录

***一、基础介绍***

  1. [汇编语言简介](#jichu1)
  1. [80×86 计算机组织](#jichu2)
  1. [80×86 的指令系统和寻址方式](#jichu3)
  1. [汇编语言程序格式](#jichu4)
  1. [汇编语言程序的运行](#jichu5)
  1. [子程序结构](#jichu6)
  
***二、汇编实验***
  1. [打印输出"Hello World!"](#shiyan1)
  1. [双精度数加减法](#shiyan2)
  1. [四则运算](#shiyan3)
  1. [串操作](#shiyan4)
  1. [数组求和](#shiyan5)
  1. [n的阶乘](#shiyan6)
  1. [大小写转换](#shiyan7)

***三、例题讲解***

<a name="jichu1"> </a>
## 汇编语言简介
&ensp;&ensp;&ensp;&ensp;汇编语言是大学许多基础课程的基础（先行课），并不说明汇编语言很基础，而是它很重要！它作为C语言、计算机组成原理、操作系统等大学课程的基础课，不仅能为我们打下牢固的计算机学习地基，也能让我们建立起一个完整的计算机学习构架。

&ensp;&ensp;&ensp;&ensp;学来只是为了过渡？of course not！它也可以用作程序开发（虽然语法很不友好），由于偏下底层，所以效率极高，作为高效率程序开发是个不二之选。但是如果选它其实还是蛮二的...不说这个，由于偏向底层硬件，所以在嵌入式开发中仍是有一席之地，如果你对黑客这一行感兴趣，汇编语言是一定得去了解的，这是一切语言的基础，用高级语言能做到的，用汇编一定能做到。总之，汇编语言很重要！

&ensp;&ensp;&ensp;&ensp;在学习中，我采用的是网上[Masm for Windows](http://www.skycn.com/soft/appid/86368.html)汇编编译器（点击跳到下载页面），语言嘛，工具不重要，学习到其中的东西才是最主要的。当然后续的实际开发中工具合不合适，上不上手才是程序员最应该考虑的，当前学生阶段还是不要太挑了。

&ensp;&ensp;&ensp;&ensp;后面不罗嗦了，大家一起加油吧！

[◀返回目录](#目录)

<a name="jichu2"> </a>
## 80×86 计算机组织
&ensp;&ensp;&ensp;&ensp;计算机主要由[CPU](https://baike.baidu.com/item/中央处理器/284033?fromtitle=ＣＰＵ&fromid=368184&fr=aladdin)（运算器和控制器集成在的一个芯片）、[存储器](https://baike.baidu.com/item/存储器)、[输入输出设备](https://baike.baidu.com/item/输入输出设备) 构成，80×86 就是这样一组微处理器系列。

### 80×86寄存器组
**通用寄存器**——AX、BX、CX、DX、SP、BP、DI、SI，8个通用寄存器（只限于8086/8088）。

其中AX、BX、CX、DX可称为数据寄存器，用来暂时存放计算过程中所用到的操作数、结果或其他信息。它们都可以以字（16位）的形式访问，或者也可以以字节（8位）的形式访问。例如，对AX可以分别访问高位字节AH或者低位字节AL。
+ AX（accumulator）作为累加器用，所以它是算术运算的主要寄存器。
+ BX（base）可以作为通用寄存器使用。此外在计算存储器地址时，它经常用作基址寄存器。
+ CX（count）可以作为通用寄存器使用。此外常用来保存计数值。
+ DX（data）可以作为通用寄存器使用。一般在作双字长运算时把DX和AX组合在一起存放一个双字长数，DX用来存放高位字。

其中SP、BP、SI、DI这四个16位寄存器可以像数据寄存器一样在运算过程中存放操作数，但它们只能以字（16位）位单位使用。因为它们更经常的用途是在存储器寻址时，提供偏移地址，所以它们亦称为指针或变址寄存器。
+ SP（stack pointer）称为堆栈指针寄存器，用来指示段顶的偏移地址。
+ BP（base pointer）称为基址指针寄存器。
+ SI（source index）称为源变址寄存器，用来确定段中某一存储单元的地址，有自动增量和自动减量的功能。
+ DI（destination index）称为目的变址寄存器，用来确定段中某一存储单元的地址，有自动增量和自动减量的功能。

**专用寄存器**——IP、SP、FLAGS、（后续为标志寄存器）OF、SF、ZF、CF、AF、PF、等...
+ IP（instruction pointer）为指令指针寄存器，它用来存放代码段中的偏移地址。在程序运行的过程中，它始终指向下一条指令的首地址，与段寄存器CS联用确定下一条指令的物理地址。IP寄存器是计算机中很重要的一个控制寄存器。
+ SP为堆栈指针寄存器。它与堆栈段寄存器联用来确定堆栈段中栈顶的地址。
+ FLAGS为标志寄存器，又称程序状态寄存器（program status word，PSW）。这是一个存放条件码标志、控制标志和系统标志的寄存器。

下面6个条件码标志用来记录程序中运行结果的状态信息，它们是根据有关指令的运行结果由CPU自动设置的。
+ OF（overflow flag）为溢出标志。在运算过程中，操作数超出了机器能表示的范围称为溢出，此时OF位置为1，否则置为0.
+ SF（sign flag）为符号标志。记录运算结果的符号，结果为负时置为1，否则置为0。
+ ZF（zero flag）为零标志。运算结果为0时置为1，否则置为0。
+ CF（carry flag）为进位标志。记录运算时从最高有效位产生的进位值。例如，执行加法指令时，最高有效位有进位时置为1，否则置为0。
+ AF（auxiliary carry flag）为辅助进位标志。记录运算时第3位（半个字节）产生的进位值。进位时置为1，否则置为0。
+ PF（parity flag）为奇偶标志。当结果操作数中1的个数为偶数时置为1，否则置为0。

**段寄存器**——CS、DS、SS、ES，4个，后续还增加了两个FS和GS就不加以说明了。

段寄存器也是一种专用寄存器，它们专用于存储器寻址，用来直接或间接地存放段地址。段寄存器长度为16位。
+ CS（code segment）为代码段。
+ DS（data segment）为数据段。
+ SS（stack segment）为堆栈段。
+ ES（extra segment）为附加段。

[◀返回目录](#目录)

<a name="jichu3"> </a>
## 80×86 的指令系统和寻址方式
&ensp;&ensp;&ensp;&ensp;何为指令系统？指令嘛，执行来使计算机解决实际问题的。每种计算机都有一组指令集供给用户使用，这组指令集就称为计算机的指令系统。计算机中的指令由操作码字段和操作数字段两部分组成，其中操作码字段指示计算机所要执行的操作。用一句来体现80×86指令的格式：`[标号:]指令的助记符[操作数1[,操作数2]][;注释]`

&ensp;&ensp;&ensp;&ensp;何为寻址方式？寻址嘛，找到要执行的数的地址。准确来说，确定指令中用于说明操作数所在地址的方法称为寻址方法。8086/8088有七种基本的寻址方式。

**1. 立即寻址方式**
  
&ensp;&ensp;&ensp;&ensp;操作数就包含在指令中，它作为指令的一部分，跟在操作后存放在代码段，这种操作数称为立即数（可以是8位，也可以是16位）。

例如：
```asm
MOV AX,1234H
```

<div align="center">
    <img src="/pics/jichu3.1.png" width="400px">
</div>

**2. 寄存器寻址方式**

&ensp;&ensp;&ensp;&ensp;操作数在CPU内部的寄存器中，指令指定寄存器号。对于16位操作数，寄存器可以是：AX、BX、CX、DX、SI、DI、SP、BP。对于8位操作数，寄存器可以是：AL、AH、BL、BH、CL、CH、DL、DH。这种寻址方式由于操作数就是寄存器中，不需要访问外部存储器来取得操作数，因而可以取得较高的运算速度。例如：
```asm
MOV AX,BX
```
假设指令执行前：(AX)=3064H，(BX)=1234H。指令执行后：(AX)=1234H，(BX)保持不变。

**3. 直接寻址方式**

&ensp;&ensp;&ensp;&ensp;操作数在寄存器中，指令直接包含有操作数的有效地址（偏移地址）。操作数一般存放在数据段，其低地址由DS加上指令中直接给出的16位偏移得到。

例如：假设(DS)=2000H
```asm
MOV AX,[8054H]
```

<div align="center">
    <img src="/pics/jichu3.2.png" width="400px">
</div>

**4. 寄存器间接寻址方式**

&ensp;&ensp;&ensp;&ensp;操作数在存储器中，操作数有效地址在SI、DI、BX、BP这四个寄存器之一中。在一般情况下，如果有效地址在SI、DI和BX中，则默认段位DS段。如果有效地址在BP中，则默认段为SS段。

例如：假设(DS)=5000H，(SI)=1234H
```asm
MOV AX,[SI]
```

<div align="center">
    <img src="/pics/jichu3.3.png" width="400px">
</div>

**5. 寄存器相对寻址方式**

&ensp;&ensp;&ensp;&ensp;操作数在存储器中，操作数的有效地址是一个基址寄存器（BX、BP）或变址寄存器（SI、DI）内容加上指令中给定的8位或16位位移量之和。在一般情况下，如果SI、DI或BX之内容作为有效地址的一部分，那么引用的段寄存器是DS。如果BP的内容作为有效地址的一部分，那么引用的段寄存器是SS。

<div align="center">
    <img src="/pics/jichu3.4.png" width="400px">
</div>

例如：假设(DS)=5000H，(DI)=3678H
```asm
MOV AX,[DI+1234H]
```
则物理地址=50000+3678+1223=5489BH，如果该字存储单元的内容如下，则(AX)=55AAH

<div align="center">
    <img src="/pics/jichu3.5.png" width="400px">
</div>

**6. 基址加变址寻址方式**

&ensp;&ensp;&ensp;&ensp;操作数在存储器中，操作数的有效地址是由：基址寄存器之一的内容与变址寄存器之一的内容相加。即：

<div align="center">
    <img src="/pics/jichu3.6.png" width="400px">
</div>

&ensp;&ensp;&ensp;&ensp;在一般情况下，如果BP的内容作为有效地址的一部分，则引用的段寄存器是SS，否则是DS。

例如：假设(DS)=2100H，(BX)=0158H，(DI)=10A5H
```asm
MOV AX,[BX][DI]
```
假设该字存储单元的内容如下，则(AX)=1234H

<div align="center">
    <img src="/pics/jichu3.7.png" width="400px">
</div>

**7. 相对基址加变址寻址方式**

&ensp;&ensp;&ensp;&ensp;操作数在存储器中，其有效地址是由：基址寄存器之一的内容与变址寄存器之一的内容及指令中给定的8位或16位位移量相加得到。即：

<div align="center">
    <img src="/pics/jichu3.8.png" width="400px">
</div>

&ensp;&ensp;&ensp;&ensp;在一般情况下，如果BP的内容作为有效地址的一部分，则引用的段寄存器是SS，否则是DS。

例如：假设(DS)=5000H，(BX)=1223H，(DI)=54H，(51275)=54H，(51276)=76H
```asm
MOV AX,[BX+DI-2]
```
则物理地址=50000+1223+0054+FFFFE=51275H，指令执行后，(AX)=7654H

相对基址加变址表示方法多种多样，下面这四种表示方法均是等价的：
```asm
MOV AX,[BX+DI+1234H]
MOV AX,1234H[BX][DI]
MOV AX,1234H[BX+DI]
MOV AX,1234H[DI][BX]
```

---

&ensp;&ensp;&ensp;&ensp;除了这7种基本的寻址方式外，8086/8088还提供了4种基于转移地址的寻址方式（左边为段内，右边为段间）：

<div align="center">
    <img src="/pics/jichu3.9.png" width="800px">
</div>

[◀返回目录](#目录)

<a name="jichu4"> </a>
## 汇编语言程序格式

&ensp;&ensp;&ensp;&ensp;汇编语言有多种类型的语句——指令语句，伪指令语句、宏指令语句。汇编语言在对源程序进行汇编时，把指令语句翻译成机器指令，而伪指令语句没有与之相对应的机器指令，换句话说，伪指令语句实际属于一种说明语句，给编译器执行，不由CPU执行。

&ensp;&ensp;&ensp;&ensp;指令语句格式：`(标号)指令助记符(操作数(,操作数))(;注释)`

&ensp;&ensp;&ensp;&ensp;伪指令语句格式：`(名字)伪指令定义符(参数,...,参数)(;注释)`

&ensp;&ensp;&ensp;&ensp;汇编语言中标号和名字不能是汇编语言的保留字，如不能是“MOV”。汇编语言不区分保留字中字母的大小写。

&ensp;&ensp;&ensp;&ensp;下面介绍一下常见的伪指令/伪操作。

**（1） 段定义语句**

&ensp;&ensp;&ensp;&ensp;segment和ends是一对成对使用的伪指令，这是在写可被编译器编译的汇编程序时，必须要用到的一对伪指令。segment说明一个段开始，ends说明一个段结束。一个汇编程序是由多个段组成的，这些段被用来存放代码、数据或被当作栈空间来使用。使用格式为：
```asm
段名 SEGMENT
段名 ENDS
```
一个简单的段示例：
```asm
DSEG SEGMENT
    MESS DB ‘HELLO’ , 0DH , 0AH , ‘$’
DSEG ENDS
```

&ensp;&ensp;&ensp;&ensp;汇编程序根据段开始语句和段结束语句判断出源程序的段划分，为了有效地产生目标代码，汇编程序还要了解各程序段与段寄存器间的对应关系。这种对应关系由段使用设定语句说明。使用格式为：
```asm
ASSUME 段寄存器名:段名[,段寄存器名:段名…]
```
段寄存器名可以是CS、DS、SS、ES。例如：CS寄存器对应CSEG段，DS寄存器对应DSEG段。
```asm
ASSUME CS:CSEG,DS:DSEG
```
下面感受一段较为完整的段定义：
```asm
DSEG1 SEGMENT
    VARW DW 12
DSEG1 ENDS
DSEG2 SEGMENT
    XXX DW 0
DSEG2 ENDS
CSEG SEGMENT
    ASSUME CS:CSEG , DS: DSG1 , ES : DSG2
    MOV AX , DSEG1
    MOV DS , AX
    MOV AX , DSEG2
    MOV ES , AX
    ……
    ASSUME DS: DSG2 , ES :NOTHING    ;NOTHING表示该ES寄存器不与任何段有对应关系
    MOV AX , DSEG2
    MOV DS , AX
    ……
DSEG ENDS
```

**（2） 数据定义及存储器分配伪操作**

&ensp;&ensp;&ensp;&ensp;数据定义语句是最常用的伪指令语句，一般格式如下：
```asm
[变量名] 数据定义符 表达式[,表达式,...,表达式][;注释]
```
例如：
```asm
VARB DB 3
VARW DW -1234
BUFF DB 100, 3+4, 5*6
```
DB——定义字节数据项，DW——定义字数据项，DD——定义双字数据项。如何定义没有初值的数据项呢？如果数据定义语句中的表达式只是一个问号（？），则表示不预置对应的变量初值，而仅仅是给变量分配存储单元。例如：
```asm
INBUFF DB 5, ?, ?, ?, 8, ?
VARW DW ?
OLDV DD ?
```
上面定义的都是数值型变量，而如果要定义字符串该怎么表示？定义字节数据的伪指令DB也可以用于定义字符串，字符串要用引号括起来（不区分单引号或者双引号，只要求配对），例如：
```asm
MESS DB 'HELLO!'
```
上述语句等价于如下：
```asm
MESS DB 'H','E','L','L','O','!'
```

有时需要定义数组，有时还需要定于数据缓冲区。例如：
```asm
BUFFER DB 0, 0, 0, 0, 0, 0, 0, 0
```
以上操作太不方便了有没有！为此，汇编语言提供了在数据定义语句中使用的重复操作符DUP，下面这条语句等价于上面的语句：
```asm
BUFFER DB 8 DUP(0)
```
来看看关于重复操作符DUP的一些其他用法案例：
```asm
BUFFER1  DB 5 , 0 , 5 DUP(?)
BUFFER2  DB 100 DUP(0, 2 DUP(1, 2), 0, 3)
BUFFER3  DB 256 DUP('ABCDE')
```

**（3） 程序开始与结束伪操作**

&ensp;&ensp;&ensp;&ensp;一段汇编程序代码从哪儿开始被执行，又从哪儿结束，都由我们END伪操作来执行，其格式为：`END [标号]`。其中标号表示程序开始执行的起始地址，即程序是从END所指的“标号”开始执行，遇到END指令后结束。如果END没有指定标号，则程序从头开始运行。下面由两个对比的汇编程序段来说明END伪操作的用处。
```asm
CODES SEGMENT                                       CODES SEGMENT
START:                                                  MOV DL , '1'
    MOV DL , '1'                                        MOV AH , 2
    MOV AH , 2                                          INT 21H
    INT 21H                                         START:
    MOV DL , '2'                                        MOV DL , '2'
    MOV AH , 2                                          MOV AH , 2
    INT 21H                                             INT 21H
    
    MOV AH , 4CH                                        MOV AH , 4CH
    INT 21H                                             INT 21H
CODES ENDS                                          CODES ENDS 
    END START                                           END START
```
上面的程序格式，包括数据定义我们都讲过了，现在不用去理解这段代码到底要干嘛。观察区别，我们发现了这两个程序都有END这个伪操作符，说明是这个程序结束的地方，而同时END后面指明了标号START，说明这段程序被执行的第一行应该是从START出开始的，所以左边的代码比右边的代码多执行了三句操作。

[◀返回目录](#目录)

<a name="jichu5"> </a>
## 汇编语言程序的运行
&ensp;&ensp;&ensp;&ensp;有了前面的知识作铺垫，那么一段汇编程序是如何在计算机中运行的呢？在DOS中，可执行文件中的程序P1若要运行，必须有一个正在运行的程序P2将P1从可执行文件中加载入内存，将CPU的控制权交给它，此时P2处于挂起状态，P1才能得以运行。当P1运行完毕后，应该将CPU的控制权交还给使它得以运行的程序P2。如果要清楚的了解执行电脑中某个exe可执行文件时，是哪个正在运行的程序将它载入内存的，必须先了解一个操作系统的外壳的概念。

> **操作系统的外壳：** 操作系统是由多个功能模块组成的庞大、复杂的软件系统。任何通用的操作系统，都要提供一个称为shell（外壳）的程序。用户（操作人员）使用这个程序来操作计算机系统工作。DOS中有一个程序command.exe（cmd.exe），这个程序在DOS中称为命令解释器，也就是DOS系统的shell。

&ensp;&ensp;&ensp;&ensp;我们在DOS中直接执行exe可执行文件时，是正在运行的command程序将exe中的程序加载入内存。command设置CPU的CS:IP指向程序的第一条指令（即程序的入口），从而使程序得以运行。程序运行结束后，返回到command中，CPU继续运行command。

汇编程序从写出到执行的整个过程：
```markdown
 编程  -> .asm -> 编译  -> .obj -> 链接  -> .exe -> 加载  -> 内存中的程序 -> 运行
(edit)          (masm)           (link)         (command)                 (CPU)
```
由于不是针对零基础编程的同学，所以编写程序、编译链接这些作用不再赘述。

&ensp;&ensp;&ensp;&ensp;本来想再细致说明一些汇编常用的`INT 21H`系统调用、寄存器用法、以及一些常用的汇编指令。但我们还是只针对考试以及了解为主，程序中重要的指令我将在第二节[汇编实验](#目录)中一一说明。

[◀返回目录](#目录)

<a name="jichu6"> </a>
## 子程序结构

[◀返回目录](#目录)

<a name="shiyan1"> </a>
## 打印输出"Hello World!"
```asm
DATAS  SEGMENT                           ;定义一个DATAS段
    STR1  DB  'Hello World!',13,10,'$'   ;具体看下面解答部分↓
DATAS  ENDS                              ;DATAS段结束

CODES  SEGMENT                           ;定义一个CODES段
    ASSUME    CS:CODES,DS:DATAS          ;关联代码段寄存器CODES和数据段寄存器CS、DATAS和DS
START:                                   ;程序开始标号处
    MOV  AX,DATAS                        ;先将段DATAS中立即数存到通用寄存器AX中作为中转
    MOV  DS,AX                           ;将立即数送到段寄存器DS中
    LEA  DX,STR1                         ;调用字符串开始地址
    MOV  AH,9                            ;调用DOS系统9号功能：显示字符串
    INT  21H                             ;调用DOS功能中断
	
    MOV  AH,4CH                          ;调用DOS系统4C号功能：结束程序
    INT  21H                             ;调用DOS功能中断
CODES  ENDS                              ;CODES段结束
    END  START                           ;汇编程序运行结束
   
;-----------------------------------------------------------------------------
;1. 如何理解第二段中“STR1  DB  'Hello World!',13,10,'$'”这句指令？

;答：由于STR是汇编一个关键字指令，则命名应该避免否则会报错；
;    伪指令DB用于字符串中定义字节数据，字符串用引号括起来；
;    13，10分别是回车符与换行符的ASCII值，执行的结果是回车换行，$是字符串结束的标志。
;-----------------------------------------------------------------------------
;2. 如何理解第八、九段中MOV的操作？

;答：MOV指令不允许将立即数直接送给段寄存器，通常是借助通用寄存器中转。
;-----------------------------------------------------------------------------
;3. 什么是立即数？

;答：汇编语言中中操作数有三种：寄存器操作数、存储器操作数和立即数；
;    其中立即数相当于高级语言中的常量（常数），它是直接出现在指令中的数，
;    不用存储在寄存器或存储器中。如指令ADD AL,06H中的06H即为立即数。
```

[◀返回目录](#目录)

<a name="shiyan2"> </a>
## 双精度数加减法
***双精度数加法***
```asm
DATAS SEGMENT                      ;定义一个DATAS段
    X DW 1,2                       ;给字变量X赋值，具体看下面解答部分↓0001H，0002H 
    Y DW 3,4                       ;给字变量Y赋值，其中X和Y为加数 
    Z DW 0,0                       ;给字变量Z赋值，其中Z为X和Y的和 
DATAS ENDS                         ;DATAS段结束

CODES SEGMENT                      ;定义一个CODES段
    ASSUME CS:CODES,DS:DATAS       ;关联代码段寄存器CODES和数据段寄存器CS、DATAS和DS
START:                             ;程序开始标号处
    MOV AX,DATAS                   ;先将段DATAS中立即数存到通用寄存器AX中作为中转
    MOV DS,AX                      ;将立即数送到段寄存器DS中
    
    MOV AX,X                       ;将X的低位部分存放在AX寄存器中
    MOV DX,X+2                     ;将X的高位部分存放在DX寄存器中
    
    ADD AX,Y                       ;将X的低位部分和Y的低位部分相加，结果存放在AX寄存器中
    ADC DX,Y+2                     ;将X的高位部分和Y的高位高位相加，结果存放在DX寄存器中
    
    MOV Z,AX                       ;将AX中X和Y低位部分相加得到的结果存放在Z的低位部分中
    MOV Z+2,DX                     ;将DX中X和Y高位部分相加得到的结果存放在Z的高位部分中
    
    MOV DX,Z+2
    MOV DL,DH
    ADD DL,'0'                     ;输出Z的高位
    MOV AH,02H                     ;调用dos系统的02号功能
    INT 21H                        ;调用DOS功能中断
    MOV DX,Z+2
    ADD DL,'0'
    MOV AH,02H                     ;调用DOS系统的02号功能：显示一个字符
    INT 21H                        ;调用DOS功能中断
    
    MOV DX,Z
    MOV DL,DH
    ADD DL,'0'          ;输出Z的高位
    MOV AH,02H                     ;调用DOS系统的02号功能：显示一个字符
    INT 21H                        ;调用DOS功能中断
    MOV DX,Z
    ADD DL,'0'
    MOV AH,02H
    INT 21H                        ;调用DOS功能中断
    
    MOV AH,4CH                     ;调用DOS系统4C号功能：结束程序
    INT 21H                        ;调用DOS功能中断
CODES ENDS                         ;CODES段结束
    END START                      ;汇编程序运行结束
    
;-----------------------------------------------------------------------------
;1. 在DATAS段中，如何理解“X DW 1,2”这个赋值过程？

;答：DW全称为：define word，其中一个word字占四个字节。一个字节又占八位，
;    所以，一个DW子类型变量占32位，即32个长度的二进制数。1，2是十进制表
;    示法，而如果要用我们常用的16进制，则应该表示为：0001H,0002H，其中
;    0001H在低半位，0002H在高半位。因为4位二进制等价于1位十六进制，所以
;    4个长度的十六进制表示16位二进制。
;-----------------------------------------------------------------------------
;2. DW的高低位是分别存放什么寄存器之中？

;答：双字长运算时用来存放高位字的寄存器为DX，存放低位字的寄存器一般是AX。
;-----------------------------------------------------------------------------
;3. 在程序第14段，为什么X+2就能代表X的高位（同理Y和Z）？

;答：X是变量名，本身也是地址。X+2表示基于X的地址多加了两个字节长度（16位），
;    而第1问中讲了DW一共由32位二进制来表示，所以第16位上下分别位高低部分。
;-----------------------------------------------------------------------------

```

***双精度数减法***
```asm
DATAS SEGMENT
    X DW 4,6                  ;给双精度X赋值 
    Y DW 2,3                  ;给双精度Y赋值
    Z DW 0,0                  ;给双精度Z赋值
DATAS ENDS

STACKS SEGMENT
    ;此处输入堆栈段代码
STACKS ENDS

CODES SEGMENT
    ASSUME CS:CODES,DS:DATAS,SS:STACKS
START:
    MOV AX,DATAS
    MOV DS,AX
    
    MOV AX,X         ;把X的低位放在AX寄存器中
    MOV DX,X+2       ;把X的高位放在DX寄存器中
    
    SUB AX,Y          ;把X与Y的低位相减，存在AX中
    SBB DX,Y+2        ;把X与Y的高位相减，存在DX中
    MOV Z,AX         ;低位相减得到的值放在Z的地位
    MOV Z+2,DX       ;高位相减得到的值放在Z的高位
    
    MOV DX,Z+2
    MOV DL,DH
    ADD DL,'0'          ;输出Z的高位
    MOV AH,02H       ;调用dos系统2号功能
    INT 21H            ;中断
    MOV DX,Z+2
    ADD DL,'0'
    MOV AH,02H
    INT 21H
    
    MOV DX,Z
    MOV DL,DH
    ADD DL,'0'          ;输出Z的低位
    MOV AH,02H       ;调用dos系统2号功能
    INT 21H            ;中断
    MOV DX,Z
    ADD DL,'0'
    MOV AH,02H
    INT 21H
    ;此处输入代码段代码
    MOV AH,4CH
    INT 21H
CODES ENDS
    END START
```

[◀返回目录](#目录)

<a name="shiyan4"> </a>
## 串操作
***串拷贝***
```asm
DATAS SEGMENT
    ;此处输入数据段代码 
    STR1 DB 'fmw2017110110 $'     ;定义一串字符
    
    STR2 DB 20 DUP(?)    ;定义作为拷贝对象
DATAS ENDS

STACKS SEGMENT
    ;此处输入堆栈段代码
STACKS ENDS

CODES SEGMENT
    ASSUME CS:CODES,ES:DATAS
START:
    MOV AX,DATAS    
    MOV ES,AX         
    ;此处输入代码段代码
    
    LEA SI,STR1     ;把STR1存在SI中
    LEA DI,STR2     ;把STR2存在DI中
    MOV CX,20      
    CLD            ;使变址寄存器SI或DI的地址指针自动增加
    REP MOVS ES:BYTE PTR[DI],ES:[SI]   ;表明是操作的是字节
    
    LEA DX,STR2   ;把STR2放入到DX中
    MOV AX,ES        
    MOV DS,AX     ;把AX里的传到DS
    MOV AH,9      ;输出字符串
    INT 21H         
    
    MOV AH,4CH
    INT 21H        
CODES ENDS
    END START
```

***串扫描***
```asm
DATAS SEGMENT
    ;此处输入数据段代码 
    STR1 DB 'fmw2017110110'       ;定义字符串
DATAS ENDS


STACKS SEGMENT
    ;此处输入堆栈段代码
STACKS ENDS

CODES SEGMENT
    ASSUME CS:CODES,ES:DATAS
START:
    MOV AX,DATAS       
    MOV ES,AX
    ;此处输入代码段代码
    
    LEA DI,STR1     ;把STR1的有效地址传送到DI中
    MOV AL,'m'      ;将m的ACSII码送到AL
    MOV CX,20       ;计数
    CLD             ;使变址寄存器SI或DI的地址指针自动增加
    REPNE SCASB     ;串扫描
    
    MOV DL,ES:[DI]   ;若相等就停止比较
    MOV AH,2		 ;输出
    INT 21H     
    
    MOV AH,4CH
    INT 21H     
CODES ENDS
END START
```

***串比较***
```asm
DATAS1 SEGMENT
    ;此处输入数据段代码 
    str1 DB 'fanmaowei'    ;定义字符串1
DATAS1 ENDS

DATAS2 SEGMENT 
	str2 DB 'fammaowei'     ;定义字符串2
DATAS2 ENDS

STACKS SEGMENT
    ;此处输入堆栈段代码
STACKS ENDS

CODES SEGMENT
    ASSUME CS:CODES,DS:DATAS1,ES:DATAS2
START:
    MOV AX,DATAS1    
    MOV DS,AX
    MOV AX,DATAS2
    MOV ES,AX
    ;此处输入代码段代码
    
    LEA SI,str1    ;把STR1的有效地址传送到SI中
    LEA DI,str2    ;把STR1的有效地址传送到SI中
    MOV CX,9       ;计数
    CLD            ;使变址寄存器SI或DI的地址指针自动增加
    REPE CMPSB     ;串比较
    
    MOV DL,ES:[DI]     ;相等则停止
    MOV AH,2      
    INT 21H            ;输出
    
    MOV AH,4CH
    INT 21H        
CODES ENDS
    END START
```

***从串中取元素***
```asm
DATAS SEGMENT
    ;此处输入数据段代码 
    STR1 DB 'fmw2017110110' ;定义字符串,后面只取前三位'fmw'
DATAS ENDS


STACKS SEGMENT
    ;此处输入堆栈段代码
STACKS ENDS

CODES SEGMENT
    ASSUME CS:CODES,DS:DATAS
START:
    MOV AX,DATAS
    MOV DS,AX
    ;此处输入代码段代码
    
    LEA SI,STR1     ;把STR1的有效地址传送到SI中
    CLD             ;使变址寄存器SI或DI的地址指针自动增加
    LODSB           ;从串取
    MOV DL,AL
    MOV AH,2        ;输出
    INT 21H
    
    LODSB           ;从串取
    MOV DL,AL
    MOV AH,2        ;输出
    INT 21H
    
    LODSB           ;从串取
    MOV DL,AL
    MOV AH,2        ;输出
    INT 21H
    
    MOV AH,4CH
    INT 21H        
CODES ENDS
END START
```

***将元素存入串***
```asm
DATAS SEGMENT
    ;此处输入数据段代码 
    STR1 DB 3 dup(?),'$'   ;给定未知内容STR1空间
DATAS ENDS

STACKS SEGMENT
    ;此处输入堆栈段代码
STACKS ENDS

CODES SEGMENT
    ASSUME CS:CODES,ES:DATAS
START:
    MOV AX,DATAS
    MOV ES,AX
    ;此处输入代码段代码
    
    LEA DI,STR1
    MOV AL,'f'    ;将f的ACSII码送到AL
    MOV CX,3      ;与串的大小一致
    CLD           ;使变址寄存器SI或DI的地址指针自动增加
    REP STOSB     ;存放入字节当中
    LEA DX,STR1   ;将STR1放到DX中
    MOV AX,ES
    MOV DS,AX     
    MOV AH,9      ;输出
    INT 21H       
    
    MOV AH,4CH     
    INT 21H        
CODES ENDS
END START
```

[◀返回目录](#目录)

<a name="shiyan5"> </a>
## 数组求和
