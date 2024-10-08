[合集 \- 上位机开发(2\)](https://github.com)1\.终于有人把Modbus讲明白了09\-13[2\.RS485与ModbusRTU09\-10](https://github.com/xiketangedu/p/18406808):[westworld加速](https://tianchuang88.com)收起
大家好！我是付工。


2012年开始接触Modbus协议，至今已经有10多年了，从开始的懵懂，到后来的顿悟，再到现在的开悟，它始终岿然不动，变化的是我对它的认知和理解。


今天跟大家聊聊关于Modbus协议的那些事。


**发展历史**


Modbus于1979年诞生，已经历经了40多年。


Modbus诞生在一个特定的时期。1969年，第一台PLC的发明，解决了数字电路代替传统继电器控制的问题，10年之后，Modbus的发明，主要用于解决PLC之间通信的问题。


**这些年，它凭借了免费开放、简单易懂等特点，广泛应用在工业自动化领域的各种产品中。**


莫迪康当初发明Modbus时，主要针对的是串口设备，即ModbusRTU和ModbusASCII协议，后来施耐德在其基础上发明了针对以太网设备的ModbusTCP。


**Modbus协议的诞生与发展，是工业自动化领域技术进步的必然结果，各种工业设备之间的数据交互，必然需要一个高效可靠的协议来支持。**


即使1979年莫迪康没有发明Modbus，也许1989年恩迪康也会发明一个Nodbus出来。
**协议基础**


Modbus协议可以说是所有协议的基础，学习上位机开发自然也离不开它。


**我认为，学习Modbus有两个层面，第一个是应用层面，第二个是报文层面。**


应用层面可以让我们借助开源通信库很轻松实现设备通信；而报文层面，可以让我们自己写通信库。


可能有人会这么问，既然有开源通信库了，我们是不是就可以不用学Modbus协议报文，直接用现成的通信库？


初期也许可以，但是从长远角度来看，既然选择了上位机这条路，未来必然还会遇到各种各样的协议，而Modbus协议恰恰是一个非常好的学习和练手的机会。


我们把它当作一个跳板，我们学习它，不仅仅是为了使用它，也为理解其他协议奠定一个扎实的基础。所以初学时，一定不要错过这个机会，否则，你会折返跑的。


**存储区分类**


我喜欢站在协议制定者角度，并结合身边的一些事物来介绍Modbus协议。


**首先我们要明确，协议的目的是为了实现数据交互。**那


么，我们先从【数据】入手，数据必然需要一个载体，自然就有了存储区的概念，这个存储区类似于我们电脑的硬盘。


**硬件要分区，存储区也要分类。**


**至于如何分类，首先我们想到根据数据类型来分，但是不可能每个数据类型分一个，那样太多了，我们将布尔和非布尔分开，因此就有了线圈和寄存器的概念。**



> 在电气回路中，接触器和中继都是靠线圈得电和失电来控制，因此用线圈来表示布尔，而寄存器在PLC中也是用来存储数据的，因此用寄存器来表示非布尔，一个寄存器就表示一个Word。


Modbus更类似于日系和国产PLC，线圈存储区类似于X、Y、M存储区，寄存器存储区类似于D、W、H存储区。


X和Y同样是线圈存储区，X表示的是输入，Y表示的是输出，输入意味着该存储区数据由外部设备接入，是只读的，输出表示输出给外部设备，是可读可写的。


**因此，Modbus的线圈和寄存器存储区，还需要按照读写特性，进一步细分，因此形成了Modbus的4个存储区，如下表所示：**




| 序号 | 读写 | 存储类型 | 存储区名称 |
| --- | --- | --- | --- |
| 1 | 只读 | 线圈 | 输入线圈 |
| 2 | 读写 | 线圈 | 输出线圈 |
| 3 | 只读 | 寄存器 | 输入寄存器 |
| 4 | 读写 | 寄存器 | 保持型寄存器 |


**存储区代号**



存储区名称是一个完整称呼，实际应用的时候会比较麻烦，因此我们会给这些存储区取一个代号，这个和PLC是一样的，PLC我们只说X区、Y区、D区，只不过PLC使用的是字母作为代号，而Modbus使用的是数字，于是便有了存储区代号表：


| 存储区名称 | 存储区代号 |
| --- | --- |
| 输入线圈 | 1区 |
| 输出线圈 | 0区 |
| 输入寄存器 | 3区 |
| 保持型寄存器 | 4区 |



> 这个存储区代号中是没有2区的，这个其实没有理由，也许就是莫迪康单纯的不喜欢2这个数字，这一点和我们国人一样。


**Modbus地址**


任何一个存储区都是有范围的，比如西门子的M区只有8192个字节，三菱的D区有8000个字，高端系列有18000个字，我们电脑硬盘也是，以前500G很大了，现在动辄1T、2T，都终究有个范围，因此Modbus的存储区也是有范围的，不可能无限大。


Modbus协议是这么规定的，每个存储区最多可能存放65536个线圈或寄存器，这个范围已经很大了。存储区地址是从0开始的，那么对于每个存储区来说，地址范围则从0到65535。




| 存储区名称 | 存储区地址 |
| --- | --- |
| 输入线圈 | 0\-65535 |
| 输出线圈 | 0\-65535 |
| 输入寄存器 | 0\-65535 |
| 保持型寄存器 | 0\-65535 |


这时候会遇到一个问题，比如你跟别人说地址100，别人是不知道是哪个存储区的100，因为每个存储区都有100，那么如何解决这个问题呢？


我们来看下PLC是如何定义的，首先看一个PLC的变量地址，比如D100，这个D100是由D\+100组合而成，D是存储区代号，100是地址偏移量，这样的地址模型就直接包含了存储区，这里的D100我们可以理解为绝对地址，而后面的地址偏移量100可以理解为相对地址。



> 所谓绝对地址，就是通过地址名称，就能明确知道是什么存储区的第几个位置的数据，而相对地址就是地址偏移量，因此绝对地址是唯一的，而相对地址，每个存储区都有。


**Modbus仍然遵守这个公式：绝对地址\=存储区代号\+相对地址。**


Modbus和PLC有两个地方不同：


1、PLC的存储区代号是字母，所以可以直接拼接，但是Modbus的存储区代号是数字，如果直接拼接，会导致地址混乱，比如4区的第10个地址，叫410，而0区的410地址也是410，因此必须要保证总长度固定，相对地址始终占5位，不足补0，于是便有了下面的表格，该表格只是当前理解下的表格，并不是最终正确的表格：


![](https://img2024.cnblogs.com/blog/1719657/202409/1719657-20240913115858560-1662920691.webp)


 


**2、Modbus协议规定：以保持型寄存器存储区为例，第一个地址不是400000，而是400001，这个是由Modbus规约决定的，其他存储区也是同样的道理。**因此正确的Modbus存储区范围如下表所示：


![](https://img2024.cnblogs.com/blog/1719657/202409/1719657-20240913115907798-1386758117.webp)


前面提到过，65536是一个非常大的范围，在实际使用中，我们可能根本用不到这么多地址。于是为了使用方便，还有一种短地址模型，即5位地址模型，前面的称为长地址模型，即6位地址模型，短地址模型存储区范围如下表所示：


![](https://img2024.cnblogs.com/blog/1719657/202409/1719657-20240913115941029-1251889723.webp)


 **直到这里，我们才看到了熟悉的40001，40001这个地址是这样逐步演变出来的。**


**功能码**


我们回到原点，协议的目的是为了实现数据交互。


**前面一直在围绕【数据】，下面围绕【交互】说明。**


交互即读写。


我们已经有了4个不同的存储区，那么我们对这些存储区的读写，必然会产生很多不同的行为，比如读取输出线圈和写入输出线圈，即为2种不同的行为。我们给这些行为取个代号，即为功能码。


**功能码就是Modbus读写行为的代号。**


那么会有多少种不同的行为呢？


读取和写入是2种不同的动作，而对象即为4个存储区，排列组合即为2\*4\=8个，但是输入线圈和输入寄存器是不能写入的，因此8\-2\=6，如下图所示：




| 序号 | 具体行为 |
| --- | --- |
| 1 | 读取输入线圈 |
| 2 | 读取输出线圈 |
| 3 | 读取输入寄存器 |
| 4 | 读取保持寄存器 |
| 5 | 写入输出线圈 |
| 6 | 写入保持型寄存器 |



Modbus协议规定：对写入输出线圈和写入保持型寄存器进行细分，分为单个写入和多个连续写入，因此前面的6种行为又变成了8种形成，同时给每个行为取个代号，即形成了我们常说的8大功能码，如下图所示：


| 功能码 | 功能说明 |
| --- | --- |
| 0x01 | 读取输出线圈 |
| 0x02 | 读取输入线圈 |
| 0x03 | 读取保持寄存器 |
| 0x04 | 读取输入寄存器 |
| 0x05 | 写入单个线圈 |
| 0x06 | 写入单个寄存器 |
| 0x0F | 写入多个线圈 |
| 0x10 | 写入多个寄存器 |



> Modbus协议除了这8种常用的读写功能码，还有一些用于诊断异常的功能码，但是一般很少使用，了解即可。


**协议分类**


Modbus协议是一个统称，有三个协议家族，分别是ModbusRTU、ModbusASCII和ModbusTCP。


我们常说A和B之间进行Modbus通信，这句话是不严谨的，应当明确指出具体使用哪种通信协议。


**一般情况下，ModbusRTU和ModbusASCII用于串行通信，ModbusTCP用于以太网通信，但是这并不是绝对的，因为Modbus协议只是一种应用层的协议，并没有指定物理层，比如，ModbusRTU协议也可以使用在以太网中进行数据传输。**


如果准确划分，应该有7种不同的通信方式，我们实际主要使用ModbusRTU和ModbusTCP，其他的使用较少。


![](https://img2024.cnblogs.com/blog/1719657/202409/1719657-20240913120018968-2062304350.webp)


**报文格式**


针对ModbusRTU、ModbusASCII、ModbusTCP这三种不同的协议，在学习时，并不需要学习三次，只要把某一种弄明白，其他两种很容易上手，一般我们以ModbusRTU作为入口，先学习ModbusRTU协议，ModbusASCII了解即可，再学习ModbusTCP协议，下面分别对这三种协议的报文格式进行说明：



1、ModbusRTU的通用报文格式如下：第一部分：从站地址，占1个字节
第二部分：功能码，占1个字节


第三部分：数据部分，占N个字节



第四部分：校验部分，CRC校验，占2个字节
2、ModbusASCII的通用报文格式如下：


第一部分：开始字符（:）


第二部分：从站地址，占2个字节


第三部分：功能码，占2个字节


第四部分：数据部分，占N个字节


第五部分：校验部分，LRC校验，占2个字节



第六部分：结束字符（CR LF）
3、ModbusTCP的通用报文格式如下：


第一部分：事务处理标识符，占2个字节


第二部分：协议标识符，占2个字节


第三部分：长度，占2个字节


第四部分：单元标识符，占1个字节


第五部分：功能码，占1个字节



第六部分：数据部分，占N个字节
具体报文内容通过后面的文章进行阐述。


Modbus学习成本很低，因为协议是公开免费的，而且有很丰富的调试工具，甚至可以在不购买任何硬件的情况下，把Modbus协议学得很透彻。



当然如果有条件，购买一些硬件配合学习，效果更佳。
