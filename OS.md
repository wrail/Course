#                                                                                                                                        引论

## 操作系统的目标和作用

### 目标

* 方便性：方便用户
* 有效性：提高资源利用率，提高系统吞吐量
* 可扩充性：微内核结构能方便的增添新的模块和功能和对原来功能和模块的修改
* 开放性：遵循世界标准规范

### 作用

可以从下边这几个角度进行分析

1. OS作为用户与计算机硬件之间的接口

   可以让用户在OS的帮助下简单可靠的操纵硬件和运行自己的程序

2. OS作为资源的管理者

   对处理机，存储器，I/O设备和文件这四大类资源进行有效的管理

   处理机管理：分配和控制处理机

   存储管理：内存分配和回收

   I/O设备管理：对I/O设备的分配和回收

   文件管理：文件读取，共享和保护

3. OS实现了对计算机资源的抽象

   通过对硬件层面覆盖管理软件，让用户不用注意其底层的实现细节，抽象层次越高，用户使用起来就越方便。

## OS的发展过程

> Os的发展过程当然不可能是一蹴而就的，是逐渐从原始走上高级

### 未配置OS的计算机系统

1. 人工操作方式

   根据纸带上的孔来输入给计算机数据或程序，用完取走计算结果。

2. 脱机输入/输出方式

   为了改进人工操作浪费太多CPU资源，加入了外围机来进行I/O处理。相对于人工操作减少了CPU空闲，提高了I/O速度。

   脱机输入：将打好孔的纸带通过外围机输入到磁盘，当CPU需要这些数据的时候高速调入内存。

   脱机输出：从内存高速调出到磁带，利用外围机输入结果。

### 单道批处理系统

> 为了充分提高计算机利用率，保持系统的连续运行，在处理完一个作业后，紧接着处理下一个作业。就产生了批处理这个概念。

#### 单道批处理系统的处理流程

![1558442757936](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1558442757936.png)

#### 单道批处理系统的缺点

可以从上图中看到程序的循环执行，在CPU每次只能执行一个作业，每执行一个都需要从磁盘上拿数据，在拿数据的时候CPU是空闲的，CPU的利用率就大大降低了，因此就有了多道批处理系统。

### 多道批处理系统

多道批处理并不是单核CPU同一时间运行多个程序，单核CPU同一时间只能运行一个程序，但是与此同时还可以根据一定的调度算法进行其他程序的I/O操作，等CPU当前程序运行结束，可能其他程序I/O处理也就完成了，就会直接拿到CPU去运行。（和单道批处理的改进之处也就是可以在I/O过程中可以运行程序或运行程序时进行其他程序的I/O操作）

#### 和单道处理系统的对比

1. 单道每次一个作业，顺序执行。多道每次多个作业，不按顺序执行，需要顺序调度算法。
2. 多道相比于单道，资源利用率高，不仅可以提高CPU利用率，也可以提高I/O利用率
3. 多道吞吐量大，但是也造成了周转时间长，没有交互能力。

> 看到这，设计者一想这样不是办法啊，这响应时间和交互性可是个大问题啊，然后就又想出了分时系统。

### 分时系统

前面说到多道处理是改善了资源利用率和吞吐量，分时系统就是在多道上对交互，和响应时间进行了改进。

#### 分时系统概念

一台主机连接了多个配有显示器和键盘的终端组成的系统，并允许多用户同时使用终端和主机交互，也就是一台计算机供很多用户用享使用。

因此在分时系统就有了多人共享主机，因此响应时间不能太长，不然对于多用户来说是效率不高的。

#### 为了实现分时系统而实现的关键问题

1. 及时接收

   为了及时接收到多个用户键入的命令和数据，在系统配置了一个多路卡（实现分时多路复用）

2. 及时处理

   需要及时处理，就要改变从磁盘调取数据，在及时处理上就要让作业直接进入内存（在磁盘的话用户都不知道自己的程序是那个，而且需要调入内存影响及时性），并且对一些大作业引入了时间片的概念（不能让大作业占用过多的CPU时间而影响及时处理）

> 这是分时系统的设计，可以看到这是为普通人使用的系统，而对于一些响应速度特别快的还是不能满足，因此后面又出现了实时系统。

### 实时系统

实时系统要求响应时间要特别快，在规定时间完成对某件事的处理。一般用于实时性比较强的应用上。

#### 实时任务的分类

1. 周期性实时任务和非周期性实时任务

   周期性实时任务：外部设备周期性发信号给计算机，要求按照指定周期循环执行。

   非周期性实时任务：无明显周期性，但必须要又截止时间（开始截止时间/结束截止时间）。

2. 硬实时任务和软实时任务

   硬实时任务：系统必须在规定时间完成某项任务，否则可能会出现无法预测的后果。

   软实时任务：也有时间要求，但是偶尔超时也不会造成太大的影响。

## 操作系统的基本特征

### 并发（Concurrency）

在一段时间内，交替使用CPU资源

### 共享（Sharing）

1. 互斥共享：一个时间点只能被一个进程访问
2. 同时访问：在一段时间可以多个线程同时访问（微观下还是交替访问）

### 虚拟（Virtual）

通过某种技术（时分复用技术和空分复用技术）将一个物理实体变为若干个逻辑上的对应物。

### 异步（Asynchronism）

由于一些资源因素的限制，进程是以人们不可预知的速度向前推进。但是可以利用一些同步机制（比如信号量机制等）保证一个作业多次运行的结果相同的异步是被允许的。

 ## 操作系统的主要功能

在前面也提到过OS可以管理四大资源等一些最基本的功能。

### 处理机管理

* 进程控制：主要是为作业创建进程和撤销进程。
* 进程同步（或线程）
  *  进程互斥方式：对临界资源的互斥访问。
  * 进程同步方式：最常用的是信号量机制。
* 进程通信：一般利用消息队列
* 调度
  * 作业调度：按照算法选若干个作业，为其分配资源，在调入内存后为其建立进程，使它有可能成为就绪进程。
  * 进程调度：按照调度算法选一个进程将处理机分配给它，并为它设置所欲要的准备环境。

### 存储器管理

* 内存分配

  分为静态分配（在装入时就已经确定）和动态分配（基本内存空间在装入前确定，在运行时可以继续申请和移动）

* 内存保护

  保证每道程序都在自己的内存空间运行，互不干扰。

  不允许用户访问OS的程序和数据，也不允许用户程序转移到非共享的其他程序中运行。

* 地址映射

  能够将逻辑地址转换为物理地址

* 内存扩充

  也可以说是虚拟内存，可以通过请求调入（如果发现需要的数据没有在内存中可以向OS发出请求，将数据和程序从磁盘调入到内存）和置换（将暂时不用的程序和数据调到硬盘，腾出内存空间，然后将需要的从磁盘装入内存）来实现。

### 设备功能管理

主要任务就是完成用户进程提出的I/O请求，分配I/O设备，完成I/O操作并且提高I/O和CPU的效率（在I/O和CPU之间引入多级缓冲）

设置缓冲的原理时间性原理和空间性原理。

### 文件管理

* 文件存储空间管理
* 目录管理
* 文件读写管理和保护

### 接口

* 用户接口：用户使用的命令接口，方便用户管理自己的作业
* 程序接口：为用户程序执行中访问资源而设置的

## OS结构设计

### 传统操作系统结构

#### 无结构OS结构

这个就很像我们刚开始学习编程，想到哪写到哪，这样当然是不行的

#### 模块化结构OS

比如对OS根据进程管理或内存管理等进行模块化划分，当然可以更好的管理和维护，正确性也提高，但是灵活性不高

#### 分层结构OS

在OS和裸机间有n个中间层，可以保证可维护，灵活，可扩展等等，但是正因为分层多而导致效率降低

### C/S模式（Client/Server Model）

C（Client）：可以和服务器交互信息的有自主处理能力的计算机

S（Server）：通常是规模较大的机器（我们平常玩的云服务器就是一些小型的），一般是被动接受客户端请求并进行相应。

### 面向对象程序设计

最基本的就是对象的提取和继承，可以增加重用和易扩展性

### 微内核OS结构

在OS的一步步发展过程中，逐渐的就产生了微内核结构

#### 微内核OS的概念

微内核概念中集中了前面的几种结构，它有**足够小的内核**，**基于C/S模式**，把最基本的部分放入内核，把OS绝大部分放在微内核外面的一组服务器（进程）中实现，应用“**机制与策略分离**”原理，并且采用**面向对象**的技术

#### 微内核的缺点

OS处理请求会导致大量的上下文切换，因为客户端和服务端，服务端和服务端，客户端和客户端的通信都需要通过微内核

# 进程

## 进程的概念

进程是具有独立功能的程序在一个数据集合上运行的过程，是系统进行资源分配的基本单位。是资源分配和调度的独立单位

## 进程的特征

### 动态

正因为它是一个执行过程，因此是动态的

### 并发

多个进程存在内存中可以在一段时间内同时运行

### 独立

进程是一个获取资源和调度的独立单位

### 异步

进程按照各自独立，不可预知的速度向前推进

## 进程的状态和转换

### 就绪（Ready）

已准备好，就差获得CPU了

### 运行(Running)

正在使用CPU

### 阻塞(Block)

正在执行的程序因为某些事件，比如I/O请求，缓冲区申请失败等而导致不得不退出CPU导致受阻，在系统中一般也会存在阻塞队列，在较大的系统会有多个阻塞队列。

### 创建

1. 先申请一个新的PCB
2. 向PCB填入进程的信息
3. 为该进程分配运行时所必须的资源
4. 将该进程转为就绪态，并插入的就绪队列

### 终止

在终止的时候OS要对该进程做一些善后操作，然后再将他的PCB清空并返回给OS。

### 状态转换图

![1561041876772](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1561041876772.png)

### 挂起

挂起其实就是将当前的进程静止。如果是正在运行的进程，挂起后将暂停执行，原本是就绪的话，挂起后则不接受任何的调度

### 激活

就是和挂起相反，从静止到活跃 

### 引入挂起后的状态转换图

![1561048024698](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1561048024698.png)

## 进程管理中的一些数据结构

### PCB（Process Control Block）

进程控制块，是一个记录型数据结构，PCB是用于描述进程当前状况和管理进程的全部信息。

* 独立运行的基本单位
* 能实现间断性运行方式（可以保存信息）
* 提供进程管理所需的重要信息
* 提供调度的重要信息
* 实现进程间通信

### PCB的组织方式

* 线性方式：将所有PCB组织的一个线性表中
* 链接方式：把具有相同状态的PCB放在一个链表上，也可以将优先级高的进程排在队前
* 索引方式：根据不同的进程状态创建不同索引表对应到PCB的线性表的地址

 ## 进程控制

### 进程的创建

#### 引起创建进程的事件

1. 用户登陆
2. 作业调度
3. 提供服务
4. 应用请求

#### 创建过程

1. 申请一个空白的PCB
2. 为进程分配所需的资源，包括一些物理和逻辑资源
3. 初始化PCB（初始化标识信息，处理机状态信息，处理及控制信息）
4. 如果就绪队列未满就插入

> 可以使用面向对象思想来看进程的创建，可以创建一个新的进程，也可以创建依赖于父进程的子进程

### 进程的终止

#### 引起终止的条件

1. 正常结束：进程任务完成
2. 异常结束：运行期间发生了某种错误
3. 外界干预：进程应外界请求而终止运行

#### 终止过程

1. 根据终止进程的标识符找到对应的PCB
2. 若被终止进程正处于执行状态，就立即停止
3. 如果有子孙进程，也应该立即停止，防止成为僵尸进程
4. 将中的进程的所有资源归还给父进程或者系统
5. 将被终止的进程的PCB从链表移除。

### 进程的阻塞和唤醒

 进程的阻塞是一种自身行为，是该线程由于一些原因而不能正常的运行，从而进入阻塞。

#### 引起阻塞和唤醒的事件

* 向系统申请资源失败
* 等待某操作的完成
* 等待某些数据/任务的到来

#### 阻塞的过程

如果发生上面的事件，就会自身发出阻塞请求，将状体由运行改为阻塞状态，并且保存状态，并将PCB加入对应事件的阻塞队列，然后转调度程序进程重新调度，将处理机分配给另外的已经就绪的进程。

#### 唤醒的过程

如果被阻塞的进程所需的资源能拿到了，就会被有关进程唤醒，然后从阻塞队列中拿出来让在就绪队列并将PCB插入就绪队列。

### 进程的挂起与激活

挂起一般就是将该进程移到外存，并且不受OS的调度。激活和此相反。

#### 挂起

OS利用原语将指定的进程挂起

执行过程：先检查状态，如果是活动就绪状态就转为静止就绪，如果是活动阻塞就转为静止阻塞。

#### 激活

激活和挂起相反。

## 进程同步

进程同步就是对多个并发执行的程序在次序上进程协调，使得程序能很好的合作，保证程序的可在线性。

### 相关术语

#### 临界资源

一次只允许一个进程访问的资源

#### 临界区

所有访问临界资源的程序的代码段，因此只要保证临界区的互斥就能保证临界资源的互斥使用它。

### 同步机制遵循的规则

所有同步机制都要遵循下面几条规则

* 空闲让进
* 忙则等待
* 有限等待
* 让权等待：当前进程不能进入自己的临界区的时候，应该立即释放处理机资源，以免陷入“忙等”。

### 硬件同步机制

1. 关中断：在未加锁的时候关闭中断，加锁后打开中断，保证只有一个程序进入
2. 使用Test-adn-set：和cas相似
3. Swap指令：可以利用一个全局的flag变量利用交换指令实现同步

### 信号量机制

#### 整数型信号量

一次只能处理一个任务，如果资源被占用，其余的程序来的时候就会在wati（s）中一直等待，等待时间结束才退出，这就导致了忙等。（未遵循让则等待原则）

#### 记录型信号量

在上面的基础上加了一个链表，用来连接等待进程

```c
//P V 操作

//先减后加  1->0
wait(Semaphore *s){
    s->value--;
    if(s->value<0) block(s->list);
}
//先加后减 0->1  0代表正在运行
singal(Semaphore *s){
    s->value++;
    if(s->value<=0) wakeup(s->list);
}

```

#### AND型信号量

可以将进程运行过程中的所需要的全部资源一次性分配，用完一起释放

#### 信号量集

不仅仅可以满足需求的种类，也可以满足需求的数量

### 管程机制

这些P V操作要是单独操作起来就给系统带来很大的麻烦，因此就是出现了一种进程同步的管理机制——管程

 管程采取了面向对象思想，让操作忽略其最底层的东西，可以更好的管理进程。

## 经典进程同步问题

### 生产者和消费者问题

### 哲学家进餐问题

### 读写者问题

## 进程通信

### 进程通信的类型

#### 共享存储系统

在内存中划分出一段区域进程进程间的高级通信，过程基本就是线程1——>内存——>线程二。

#### 管道通信系统

管道文件：是一个读进程和一个写进程通信的共享文件名称为Pipe文件

在通信上就是发送线程和接受线程之间交互，因此成为管道通信。

#### 消息传递系统

一个格式化的消息为单位，使用一组通信命令，在进程中进程消息传递，完成数据的交换。

用途：

* 在计算机网络中：消息又称报文
* 在微内核OS中，微内核和OS也都采用的消息传递系统
* 在分布式中也在大量的使用消息传递中间件

#### C/S系统

基于客户机和服务器的通信机制

##### Scoket

套接字就是一个标识通信类型的数据结构，包含了目的地址，使用的端口，传输协议，进程所在的网络地址，和不同的系统调用。

因此可以基于Socket进程通信

##### RPC

RPC（Remote Procedure Call）就是远程过程调用，是一个通信协议，通过网络连接的系统。允许一台在本机，另外一台是远程主机。

###### RPC调用的过程

![img](https://img2018.cnblogs.com/blog/739231/201901/739231-20190113163552755-1990586909.png)

1. 调用者（客户端Client）以本地调用的方式发起调用；
　　2. Client stub（客户端存根）收到调用后，负责将被调用的方法名、参数等打包编码成特定格式的能进行网络传输的消息体；
　　3. Client stub将消息体通过网络发送给服务端；
　　4. Server stub（服务端存根）收到通过网络接收到消息后按照相应格式进行拆包解码，获取方法名和参数；
　　5. Server stub根据方法名和参数进行本地调用；
　　6. 被调用者（Server）本地调用执行后将结果返回给server stub；
　　7. Server stub将返回值打包编码成消息，并通过网络发送给客户端；
　　8. Client stub收到消息后，进行拆包解码，返回给Client；
　　9. Client得到本次RPC调用的最终结果。

## 线程

### 线程简介

线程又称轻量级进程，线程是最小的执行单元，而进程由至少一个线程组成。如何调度进程和线程，完全由操作系统决定，程序自己不能决定什么时候执行，执行多长时间。

进程拥有进程控制块（PCB）来记录信息。当然线程也又自己的线程控制块（TCB）。

### 线程的分类

#### 用户线程

不需要内核支持而在用户程序中实现的线程，其不依赖于操作系统核心，应用进程利用线程库提供创建、同步、调度和管理线程的函数来控制用户线程。

不需要用户态/核心态切换，速度快，**操作系统内核不知道多线程的存在，因此一个线程阻塞将使得整个进程（包括它的所有线程）阻塞**。由于这里的处理器**时间片分配是以进程为基本单位**，所以每个线程执行的时间相对减少

#### 内核级线程

内核线程就是**内核的分身**，**一个分身可以处理一件特定事情**。内核线程的调度由内核负责，一个内核线程处于**阻塞状态时不影响其他的内核线程**，因为其是调度的基本单位。

这与用户线程是不一样的。因为内核线程只运行在内核态。用户是不能被内核察觉到的。

# 处理机调度

## 处理机调度的类型

处理机调度程序按照某种算法将处理机分配给某个进程，这就叫处理机调度。总体而言，按层次分，有三种类型：

### 作业调度（又称高级调度、长程调度）

作业调度的本质就是根据某种算法，把**外存上的作业调入内存，并为之创建进程，分配处理机并执行**。这里有两个概念：

#### 作业（Job）

在操作系统概述中，我们了解过操作系统的发展历程，在单道批处理系统和多道批处理系统中粗略了解过作业的概念。作业是一个比程序更广泛的概念，可以包含多个程序和数据，还包含一份作业说明书，处理机根据作业说明书来对作业中的程序进行控制。一般而言，批处理系统中才会有高级调度。

#### 作业步（Job Step）

作业步的本质就是程序。作业运行过程中的每一个步骤可以称为一个作业步。

典型的作业可分为三个作业步：编译作业步->连续装配作业步->运行作业步。相当于我们的程序代码的整个执行步骤。

### 进程调度（又称低级调度、短程调度）

进程调度的本质就是根据某种算法，把处理机分配给进程。

进程调度首先会保存处理机现场。将程序计数器等指定寄存器中的内容保存到PCB中。然后将按照某种算法从就绪队列中选取进程，把处理机分配给进程。最后，把指定进程的PCB中的处理机现场信息恢复到处理机中，处理机分配给进程执行。

#### 进程调度中的三个基本机制

- 排队器。将所有的就绪进程按照一定方式 （如优先级）排成一个队列，以便调度程序找到。
- 分派器。把从就绪队列中取出的进程，处理机上下文切换后，把处理机分配给该进程执行。
- 上下文切换机制。

#### 进程调度的两种调度方式

- 非抢占式（Nonpreemptive Mode）
- 抢占式（Preemptive Mode）

### 中级调度

中级调度的本质就是让暂时不能运行的进程挂起，释放内存资源，并把它们调到外存上去等待。什么是外存？外存就是硬盘、磁盘等存储设备。后边会有一些置换算法等等。

## 调度算法

### 先来先服务调度算法（FCFS）

来的先进入内存或占用处理机。对于作业调度，就是从后备作业队列中选择一个或多个最先进入队列的作业，将其调入内存。对于进程调度就是从就绪队列选择最新进入的进程，为之分配处理机。

 

### 短作业优先调度算法（SPF）

就是在选择作业或进程的时候，先估算每个作业、进程的服务时间，选择其中最短的优先获得处理机。 

### 高优先权优先调度算法和高响应比调度算法

优先权有两种类型，

一种是静态的，即每个进程、作业的优先权在它创建的时候就已经确定，此后都不能改变。

另一种是动态的也叫**高响应比调度算法**，即进程、作业的优先权是可以改变的。最常见的做法就是进程、作业在等待中，优先权以一定速率随时间增长，这样等待时间越长，被调用的可能性就越大。 

响应比的优先权计算公式



### 基于时间片的轮转调度法。

这就是分时系统中采用的调度算法。原理就是把所有的就绪队列进程按先来先服务的原则排成队列。每次都把CPU分配给队首，让其执行一个时间片，执行完毕，调度器中断进程，并把该进程移至就绪队列的队尾，然后再取一个队首进程，继续执行下一个时间片。时间片是什么，就是一段很短的CPU时间，几毫秒到几百毫秒不等。

### 多级反馈队列调度算法

这是当下公认的比较好的，**使用最广泛的调度算法**。对作业调度分级到不同的就绪队列上去。如果上一级的任务没有执行完，那就降低优先级并且拆入到下一个就绪队列中去。

例如，某计算机采用多级反馈队列调度算法，设置了5个就绪队列。第一个就绪队列优先级最高，时间片为2ms。第二个就绪队列优先级第二，时间片为4ms，其余队列也一样，优先级依次递减，时间片依次增加。如果某个进程进入就绪队列，首先把它放在第一个就绪队列的末尾，轮到它执行就执行2ms，时间片结束，若该进程还没有执行完毕，就把该进程移入第二个就绪队列的末尾。只有当第一个队列的进程都执行完时间片，才会执行第二个队列。如此依次执行，若该进程服务时间很长，将被移到最后一个就绪队列。在最后一个就绪队列，进程将按照时间片轮转调度法执行。处理机执行过程中，只有当优先级高的队列中的线程都执行完毕，才会执行优先级低的队列。如图所示（懒得自己画一遍了，直接从书上拿过来）：

 ![img](https://images0.cnblogs.com/blog/580631/201412/301835054976319.png)

## 死锁

多个进程在运行过程中因争夺资源造成的一种僵局。

### 产生死锁的必要原因

1. 资源互斥条件。即一段时间内，某资源只能由一个进程占用。这段时间，其他的进程只能等待。

2. 请求和保持条件。进程已经保持了至少一个资源，但又提出了新的资源请求，而该资源又已被其它进程占有，此时请求进程阻塞，但又对自己已获得的其它资源保持不放。
3.  不剥夺条件。进程已获得的资源，在未使用完之前，不能被剥夺
4.  环路等待条件。一条等待的循环链。

### 预防死锁

怎么预防死锁，只需要对上面的四个条件的一个进行破坏就可以了。

### 避免死锁

避免死锁一般采用的是银行家算法，在银行家算法尝试分配资源的时候，再进行安全性检测算法，尝试找到一条安全序列，安全序列很多时候不止一条，若能找到一条就说明不会出现死锁。

### 死锁的检测

可以根据死锁定理加上资源分配图，根据已有的资源对这个资源分配图进行化简，**如果可以化简为全孤立节点**，那就不会出现死锁，否则就会有死锁。

# 存储器管理











