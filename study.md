## 📑 目录

* [➕ C/C++](#cc)
* [💻 操作系统](#os)
* [☁️ 计算机网络](#computer-network)
* [🌩 网络编程](#network-programming)
* [💾 数据库](#database)
* [⚡️ 算法](#algorithm)
* [📏 设计模式](#design-pattern)
* [⚙️ 链接装载库](#link-loading-library)
* [📚 书籍](#books)
* [🔱 C/C++ 发展方向](#cc-development-direction)
* [💯 复习刷题网站](#review-of-brush-questions-website)
* [📝 面试题目经验](#interview-questions-experience)
* [📆 招聘时间岗位](#recruitment-time-post)
* [👍 内推](#recommend)
* [👬 贡献者](#contributor)
* [🍭 支持赞助](#support-sponsor)
* [📜 License](#license)

<a id="cc"></a>

# ➕ C/C++

### 变量的内存分配
* 堆栈：局部变量
* bss: 未初始化的全局变量，static变量，extern
* 静态存储区：全局数据区：全局变量，static变量，extern，直接通过地址访问
* 静态存储区：常量区：const全局变量
>[变量的内存分配](https://wenku.baidu.com/view/6a3a04185427a5e9856a561252d380eb629423cc.html)

### const
1. 修饰变量，说明该变量在当前进程运行期间不可以被改变；
2. 修饰指针，分为指向常量的指针（pointer to const）和自身是常量的指针（常量指针，const pointer）；
3. 修饰引用，指向常量的引用（reference to const），用于形参类型，即避免了拷贝，又避免了函数对值的修改；
4. 修饰成员函数，说明该成员函数内不能修改成员变量。

### const 内存分配
* 局部变量，分配在栈区。类似宏定义，直接用常量进行了替代。并不会访问变量的地址。
* 全局变量，分配在常量区

#### 宏定义 #define 和 const 常量 

宏定义 #define|const 常量
---|---
宏定义，相当于字符替换|常量声明
预处理器处理|编译器处理
无类型安全检查|有类型安全检查
不分配内存|要分配内存
存储在代码段|存储在数据段
可通过 `#undef` 取消|不可取消

### static用法
* static修饰类成员是存放在全局变量区的，直接通过它的地址访问。而不是像访问其他类成员是通过类对象地址+偏移。

### inline 内联函数

#### 特征

* 复制代码：把内联函数里面的代码复制到调用内联函数处；
* 节约开销：省去了函数调用开销；
* 类型检查：相较于宏函数的优点；
* 编译器一般不内联包含循环、递归、switch 等复杂操作的内联函数；
* 在类声明中定义的函数，除了虚函数的其他函数都会自动隐式地当成内联函数。

#### 优点

1. 节约函数调用开销，省去了参数压栈、栈帧开辟与回收，结果返回等，从而提高程序运行速度。
2. 做安全检查或自动类型转换（同普通函数），而宏定义则不会。 
3. 在类中声明同时定义的成员函数，自动转化为内联函数，因此内联函数可以访问类的成员变量，宏定义则不能。
4. 内联函数在运行时可调试，而宏定义不可以。

#### 缺点

1. 代码膨胀。内联是以代码膨胀（复制）为代价，消除函数调用带来的开销。如果执行函数体内代码的时间，相比于函数调用的开销较大，那么效率的收获会很少。另一方面，每一处内联函数的调用都要复制代码，将使程序的总代码量增大，消耗更多的内存空间。
2. inline 函数无法随着函数库升级而升级。inline函数的改变需要重新编译，不像 non-inline 可以直接链接。
3. 是否内联，程序员不可控。内联函数只是对编译器的建议，是否对函数内联，决定权在于编译器。

#### 虚函数（virtual）可以是内联函数（inline）吗？

* 虚函数可以是内联函数，内联是可以修饰虚函数的，但是当虚函数表现多态性的时候不能内联。
* 内联是在编译期建议编译器内联，而虚函数的多态性在运行期，编译器无法知道运行期调用哪个代码，因此虚函数表现为多态性时（运行期）不可以内联。
* `inline virtual` 唯一可以内联的时候是：编译器知道所调用的对象是哪个类（如 `Base::who()`），这只有在编译器具有实际对象而不是对象的指针或引用时才会发生。

### 静态联编与动态联编
* 静态联编。当访问一个函数，明确知道它的函数入口地址（函数的入口地址分布在代码区）。编译的时候，直接将函数入口地址放入到汇编后的指令。
* 编译期并不知道函数入口地址。汇编指令放的堆栈相对地址（例如esp+8）.只有运行时才知道这个堆栈的区域存放的值，也就是函数入口地址。

### 右值引用
* 左值是locator value, 指可以被寻址的数据。右值是read value, 只提供值，但不能被寻址（寄存器）

### 尽量少使用 `using 指示` 污染命名空间

> 一般说来，使用 using 命令比使用 using 编译命令更安全，这是由于它**只导入了指定的名称**。如果该名称与局部名称发生冲突，编译器将**发出指示**。using编译命令导入所有的名称，包括可能并不需要的名称。如果与局部名称发生冲突，则**局部名称将覆盖名称空间版本**，而编译器**并不会发出警告**。

### new、delete

1. new / new[]：完成两件事，先底层调用 malloc 分配了内存，然后调用构造函数（创建对象）。
2. delete/delete[]：也完成两件事，先调用析构函数（清理资源），然后底层调用 free 释放空间。
3. new 在申请内存时会自动计算所需字节数，而 malloc 则需我们自己输入申请内存空间的字节数。

#### 定位 new

定位 new（placement new）允许我们向 new 传递额外的地址参数，从而在预先指定的内存区域创建对象。

### 如何定义一个只能在堆上（栈上）生成对象的类？

> [如何定义一个只能在堆上（栈上）生成对象的类?](https://www.nowcoder.com/questionTerminal/0a584aa13f804f3ea72b442a065a7618)

### 静态建立对象
> 例如 A a。静态建立一个类对象，是由编译器为对象在栈空间中分配内存，是通过直接移动栈顶指针，挪出适当的空间，然后在这片内存空间上调用构造函数形成一个栈对象。使用这种方法，直接调用类的构造函数。

### 动态建立对象
> 例如 a = new A()。第一步，使用operator new()函数，在堆空间中搜索合适的内存并进行分配；第二步是调用构造函数构造对象，初始化这片内存空间。这种方法，间接调用类的构造函数。

### 智能指针
* unique_ptr, 一个指针独占拥有一个对象。
* shared_ptr, 多个智能指针可以共享同一个对象，对象的最末一个拥有着有责任销毁对象。
* week_ptr,  多个指针共享但不拥有某一个对象，通过shared_ptr来构造的。

### 重载，重写和隐藏的作用范围

### 详细解释动态绑定

### 抽象类与纯虚函数

### 内存泄漏相关，不用delete，访问越界

### 类的内存计算
>[C++类对象的内存结构](https://blog.csdn.net/MOU_IT/article/details/89045103)

### C++11新特性，右值引用，移动语义

### 字符串
* string 本质上是一个STL容器。c_str()是一个char*指针指向一个在堆区分配的char数组。 构造过程先malloc分配内存，然后进行拷贝操作。
* const char* 是静态字符串，存放在静态存储区的只读常量
* char[] 是字符串数组，分配在栈区的局部变量。没有构造函数和析构函数
* string_view 是string的可读视图，存放一个const char*,和长度。substr并不会有额外的内存分配和拷贝操作。

### C++新特性
* C++17 string_view
* C++20 range库,增加view视图，view的关系转化用符号“|”串联起来，类似于Linux管道。
* C++20 Coroutines协程 co_yield <= next()

<a id="os"></a>
# 💻 操作系统

### 进程与线程的区别
* 进程负责资源的分配和管理，进程可以创建多个线程，由线程负责CPU的调度和执行，多个线程可以并发执行。
* 进程拥有独立的虚拟地址空间。进程之间地址是相互隔离的。线程共享进程的虚拟地址空间。
* 进程切换的开销大，线程切换开销小。

### 进程的通信方式
* 管道，有名管道用于任意进程，无名管道用于父子进程
* 信号量，对共享资源进行计数，用于同步进程
* 信号，告知一个进程发生了某个事件
* 消息队列，存放在内核的数据结构
* 共享内存，将一部分内存空间映射到特定的虚拟地址段
* 套接字，传输的数据为字节，需要对数据解析转换成应用级数据

> 进程之间的通信方式以及优缺点来源于：[进程线程面试题总结](http://blog.csdn.net/wujiafei_njgcxy/article/details/77098977)

> 多进程与多线程间的对比、优劣与选择来自：[多线程还是多进程的选择及区别](https://blog.csdn.net/lishenglong666/article/details/8557215)

### 协程
* 协程，相当于更加轻量化的线程，或者是用户线程。相较于OS线程来说，协程在用户态上进行调度，切换的上下文空间更加轻量。
* 协程需要与一个线程绑定

> [GMP原理与调度](https://www.topgoer.cn/docs/golang/chapter09-11)
### 死锁预防

* 打破互斥条件：改造独占性资源为虚拟资源，大部分资源已无法改造。
* 打破不可抢占条件：当一进程占有一独占性资源后又申请一独占性资源而无法满足，则退出原占有的资源。
* 打破占有且申请条件：采用资源预先分配策略，即进程运行前申请全部资源，满足则运行，不然就等待，这样就不会占有且申请。
* 打破循环等待条件：实现资源有序分配策略，对所有设备实现分类编号，所有进程只能采用按序号递增的形式申请资源。
* 有序资源分配法
* 银行家算法

### 多进程是怎么工作的，fork实现

### 内存共享的实现
共享内存是指将一片物理空间映射到特定的一段虚拟地址空间。是最快的IPC方式，一般配合信号量进行同步。
>[Linux的管道和共享内存](https://blog.csdn.net/ych9527/article/details/115355389)

### mmap的零拷贝IO
>[mmap的零拷贝IO](https://blog.csdn.net/hellozhxy/article/details/115312608)
>[页缓存](https://blog.csdn.net/qq_23929673/article/details/103802583)
### 两个进程通过mmap映射普通文件实现共享内存通信

### 进程上下文的切换
* CPU上下文切换，将CPU寄存器的数据（esp），程序计数器(pc)的值，保存到内核空间的栈
* 地址空间切换，指页表切换，将新进程的页全局目录的虚拟地址转换为物理地址，存放到页基址寄存器。
* 清空TLB页表，导致重新遍历多级页表，增加磁盘IO开销
* ASID机制一定程度上减轻了TLB清空导致的开销。通过ASID版本号来区分每个进程。
* 后台SWAP守护进程,将空闲页置换到Swap分区。
* 如果是跨CPU调度，之前热起来的TLB、通过局部性原理缓存的数据都失效了，导致穿透到内存的IO增加。

### 五种IO模型

### select
* socket本质是一种文件，通过文件管理系统来管理的。socket的内存结构包括接收缓冲区、发送缓存区、等待队列。分布在内核空间的内存。
* 当数据包到达网卡，驱动程序通过DMA拷贝将数据拷贝到内存。进行CPU中断，CPU将数据再拷贝到内核空间的socket缓冲区。数据包从网卡到内核空间的内存由内核态自动实现的。
用户态需要关注的是什么时候将数据从内核空间拷贝到用户空间。
* 同步阻塞IO是调用recv函数进行阻塞。通过一个内核线程轮询内核空间的socket缓冲区，当有数据就返回，解除关联的进程的阻塞状态。同步非阻塞IO是调用recv函数直接返回
* select方法，创建一个socket集合，调用select函数，将socket集合拷贝到内核空间，遍历集合阻塞socket关联的进程。然后不停的轮询集合的socket缓冲区是否有新数据，若没有就不返回。
直到来了数据。返回之后，还需要遍历一次socket集合，找出哪些socket收到数据的。然后进行数据的拷贝。
>[select](https://blog.csdn.net/qq_32649581/article/details/123918450)

### epoll
* epoll_create 创建一个eventpoll对象叫epfd。eventpoll的结构体包含了wq(等待队列)，rbr（红黑树根节点）和rdlist（双向链表存放的也是epitem）
* epoll_ctl 将要监听的socket添加到epfd里，往红黑树插入节点，节点是epitem结构体，相当于是对socket描述符的间接引用。然后，给socket的等待队列添加回调函数
* epoll_wait 调用后， 监听的socket所关联的进程会被阻塞，也就是将他们加入等待队列。轮询epfd的rdlist,如果为空，就把自己阻塞，下次唤醒后继续轮询rdlist。
* 当数据包到达网卡后，DMA拷贝完后，软中断处理网络帧，CPU最后会将数据拷贝到socket的缓冲区。之后访问socket的等待队列，得到回调函数。然后执行回调函数，将自己的epitem加入到epfd的就绪链表。
* 访问epfd的等待队列，唤醒里面的进程。
* epoll_wait 返回之后，用户态就可以根据epfd的就绪链表对应的socket描述符，来进行数据拷贝

>[epoll](https://blog.csdn.net/armlinuxww/article/details/119603605)

### 为什么要内核缓冲区，内核空间和用户空间数据拷贝的意义
* 主要是为了防Time of Check to Time of Use攻击，用户态的内存数据不可信，内核在访问操作之前要做必要的检查，但是如果只检查不拷贝在原地操作的话，在检查和使用之间会有一个小的时间窗口，这个窗口内用户修改这段内存的话就导致之前的检查无效，所以需要拷贝到内核空间再做检查，然后操作

作者：大荒落
链接：https://www.zhihu.com/question/450331813/answer/2210019907
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
### Linux 后台进程和守护进程

### Linux 进程回收
<a id="computer-network"></a>

# ☁️ 计算机网络

### TCP与UDP
* TCP需要建立连接，有三次握手和四次挥手
* TCP数据包有序列号和确认号，需要按序发送和按序接收。
* TCP拥有可靠传输机制，UDP是尽最大努力交付。
* TCP有超时重传，丢包重传
* TCP有流量控制和拥塞控制。发送方会根据接收方的缓存空间大小，和网络拥堵情况，动态调整发送数据包的速度。UDP一般以恒定的速度发送数据包。
* TCP面向字节流传输，UDP面向报文传输
* TCP首部开销至少20字节，UDP只有8字节

### TCP粘包
* 固定包长，比较方便，灵活性差
* 添加特殊符号来标记边界，比如FTP使用'\r\n'
* 包头定长，包头指明包体的长度。

### TCP三次握手
![TCP三次握手](./src/TCP%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B.png)

### TCP四次挥手
![TCP四次挥手](./src/TCP%E5%9B%9B%E6%AC%A1%E6%AC%A1%E6%8F%A1%E6%89%8B.png)

### Time-Wait 的必要性
* 四次挥手中，最后的 ACK 是由「主动关闭连接」的一端发出的，如果这个 ACK 丢失，则对方会重发 FIN 请求；
* 等待旧报文消失。TCP报文可能会延迟到达，为了避免「延迟到达的 TCP 报文」被误认为是「新 TCP 连接」的数据，则需要在允许新创建 TCP 连接之前，保持一个不可用的状态，等待所有延迟报文的消失，一般设置为 2 倍的 MSL（报文的最大生存时间），解决「延迟达到的 TCP 报文」问题；

### 加密算法

### 网络攻击

### HTTP

### CA证书
![CA证书](./src/CA%E8%AF%81%E4%B9%A6.png)

### 安全套接字层SSL

### 从输入网址到获取页面
* 查询DNS
    * 浏览器自身的DNS缓存
    * 操作系统的DNS缓存，host文件
    * 通过DNS服务器
* 建立TCP/IP连接，三次握手
* TLS握手
    * 客户端发送ClientHello消息给服务器，消息包含了自身的TLS版本，可用的加密算法和压缩算法
    * 服务器返回ServerHello消息，消息还额外包括了CA签发的服务器公开证书，证书中包含了公钥。
    * 客户端验证证书是否可信。验证成功后，随机生成一个对称密钥，使用证书中的公钥进行加密。
    * 客户端发送Finish消息，加密后消息摘要，和加密后的对称密钥，给服务器
    * 服务器使用私钥解密对称密钥，解密消息摘要，计算Finish消息的散列值是否对应。 返回一个Finish消息
    * 之后都使用这个对称密钥进行加密通信，传输HTTP的内容
* HTTP请求和响应
    * 服务器接收到HTTP请求后，根据请求方法、请求参数和路径参数，路由到对应的后端处理函数。最终返回一个HTTP响应，里面包含生成好的HTML代码。
    * 浏览器根据HTTP响应，拿到的资源进行解析和渲染。如果遇到一些外部引用，比如js，css，图片之类的，重复上述的HTTP请求。

### HTTP2.0
* 二进制分帧。将HTTP消息分为多个帧，由二进制进行编码。多个帧可以乱序发送，根据帧首部的流标识重新组装。
* 多路复用。同一个域名只需要一个TCP连接就能并发多个HTTP请求和响应。
* 服务器推送。服务器可以主动推送js，css这些外部引用文件给客户端。不需要等到客户端解析HTML在发送请求。如果客户端有这些文件的缓存也可以拒收。
* 头部压缩。采用HPACK对头部进行压缩，节约带宽。

### HTTP3.0

<a id="database"></a>

# 💾 数据库

### 索引失效

1.有or必全有索引;
2.复合索引未用左列字段;
3.like以%开头;
4.需要类型转换;
5.where中索引列有运算;
6.where中索引列使用了函数;
7.如果mysql觉得全表扫描更快时（数据少）;

### 创建和使用索引的注意事项
* 列的类型尽量小，增加一个数据页的记录，减少磁盘IO操作
* 覆盖索引，当索引包含了所有查找的列，就不需要回表操作
* 数据页已满，插入主键值在其中的记录。设置自增长可以避免

### Redis持久化AOF
* AOF日志保留数据库的写入命令，启动数据库时顺序执行AOF日志的命令来恢复数据库数据
* 当每执行一条写命令，就往AOF日志最加这条命令。有三种回写策略
* AOF日志重写机制，通过扫描数据库所有键值对生成对应的写操作命令。由后台子进程进行，只有当`写时复制`和信号处理函数才会阻塞主进程。
![AOF](./src/AOF.png)
<a id="algorithm"></a>

### Redis持久化RDB
* 将某一刻的内存数据保存为RDB快照(二进制)
* 优点是读入速度快，缺点是保存快照耗时，容错率低
* 混合AOF和RDB, AOF日志前半部分是RDB快照，后半部分是AOF重写的增量数据

>[Redis杂谈](https://copyfuture.com/blogs-details/20211207093851557m)

# ⚡️ 算法

### 归并排序时间复杂度推导

>[递归式求解时间复杂度](https://blog.csdn.net/flying_all/article/details/94412552)

### 快速排序手写

>[快速排序](https://leetcode.cn/problems/sort-an-array/)


<a id="project"></a>

# 项目

### 项目压测

<a id="regex"></a>

# 正则表达式

<a id="linux"></a>

# Linux

<a id="problem"></a>

# 场景题目

### 位图法记录登录状态

### 如何从几亿个数中找到唯一出现的一个数

### 秒杀
>[秒杀](https://blog.csdn.net/weixin_45304503/article/details/125237792)

# 软实力

<a id="problem"></a>