---
layout: post
title:  Linux 系统
category: other
keywords: [improvement,android,java,os]
---

补一波 OS 基础知识:

Unix 的输出哲学:  极简化,文本化,面向流,设备无关格式    

###  Linux

#### 万物皆文本

大道即简,万物皆文本   

文本化带来的通用化,对使用者友好,配置文本化,甚至硬件也文本化,只需要看文本遍可知道所有硬件的挂载状态,硬件的配置,硬件特性修改也只需要操作文本便可以完成; 

程序间的组合沟通,同样适用通用文本,文本只需要保证编解码一致性则可以保证程序间的独立性,下游不关心上游,上游不关注下游,彼此独立;

文本的解析可能带来一些性能方面的影响,但当无法用数据证明文本的解析对系统造成了影响成为性能瓶颈时,对其的改变便是过早优化 -- 用数据说话,而不是猜测 -- From 调试九法   

文本传输带来的空间尺度问题 - 在高效的数据压缩策略下,文本传输与二进制传输几乎无差别,但数据压缩的压缩算法同样对性能可能造成影响;

Linux 丰富的以文本形式存在的脚本,启动脚本,配置脚本,执行脚本,脚本作为胶水语言,用于弥补设计上的缺陷,事实上无论是自顶向下的设计还是自底向上的设计都存在其对应的缺陷;自顶向下的问题在于可能存在无法实现的下层,自底向上的缺陷在于存在过多与最终目的无关的设计与实现;随之衍生出"正确的自顶向下"以及依靠胶合层实现的双管齐下,上下层独立;

#### Linux 

Linux 有多重分离的发行版本,我常用的 CentOS,Ubuntu,RedHat..., 各版本甚至对于同一功能机制的实现都各不一样,如防火墙控制策略

Linux 中的丰富的 IPC 机制为 Linux 中机制与策略实现上下层分离提供了良好的基础支持: 信号,管道, IO 重定向,共享内存,Socket 等..

Linux 还有用庞杂的配置文件,在新人以及专家手中 Linux 甚至不是同一个系统,当然这也是 Linux 自由的一个体现;

#### Unix 哲学 

* 每个程序做好一件事 - 简洁  

* 你无法判断程序会在什么地方耗时,瓶颈也经常出现在意想不到的地方,在没有证实时不要随意修改代码;在没有对代码进行估量,找到最耗时的地方前,不要去优化速度,速度与性能的优化要基于实际数据,优化是可量化的;

* 清晰的设计胜于技巧的设计,设计应该透明可见以便于后期的调试;           
* 透明与简洁带来健壮性;            
* 策略与机制分离:  MVC

### fork   

进程与线程

线程通信依赖信号量,全局变量,以及锁等,而进程依赖 IPC;Linux 的线程实现:NPTL - 原生 POSIX线程库,内核创建与回收线程,线程与内核线程调度实体 1:1;

fork 赋予了进程生命,进程可以向细胞一样分裂扩展;

fork 后子进程继承父进程的资源,复制父进程内存页面, copy到自身所获取的操作系统赋予的内存中.此处的拷贝属于 COW 即为写时拷贝,子进程借此可以完成高效的进程创建以及父进程资源共享,只有到更改时才 copy 父进程的内存页面进而修改,在 copy 内存页面时子进程同时拷贝父进程页面表;

[分页和物理内存管理](http://airtrack.me/posts/2015/04/27/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%E5%AE%9E%E7%8E%B0%EF%BC%88%E4%BA%8C%EF%BC%89%EF%BC%9A%E5%88%86%E9%A1%B5%E5%92%8C%E7%89%A9%E7%90%86%E5%86%85%E5%AD%98%E7%AE%A1%E7%90%86/)

进程与线程比较: 进程相对而言更加独立通常即使一个进程崩溃也不会造成其他进程的影响,而线程则强调同步与资源共享,而为了资源共享的管理有序又需要引入同步管理机制,导致线程之间撕扯数据,一旦一个线程崩溃通常导致整个进程崩溃,当然也会导致其他线程崩溃;

对于 Linux 而言线程与进程在内核角度而言是基本等同管理的,同时进程特有的fork 引申的主从进程机制,主进程可以很容易的了解到子进程的运行状态,一旦工作子进程崩溃,主进程可以迅速感知并重启子进程,这些都是在系统层面的进程支持;

当然这些都是针对 Linux 系统而言,而对于Windows系统而言,则仿佛是为线程而生的,为工作线程的创建销毁保持了极大的弹性;

#### IPC    

* 信号: 信号是硬件中断机制在软件层次的模拟机制,信号是 进程 IPC 中唯一的异步通信机制,常用的有 kill 发送信号关闭进程, alarm 指定时间向进程本身发送信号;                    
* 管道: 管道实际上是内存中的一个固定大小的缓冲区,管道两端一端读一端写,管道中的数据是一次性的,如同流水一样,一旦有人读取其他人就无法再次获取;同时管道还能被阻塞,写阻塞是指管道中已经写满,在数据被读取进而有空闲空间前管道无法再被写入,同理,读阻塞则是指管道内容全部被读取,在管道有内容写入前,管道一端的读操作将被阻塞;管道的操作为 pipe, 事实上 pipe 线程间通信使用较少,使用更加广泛的是 FIFO 命名管道;

[Linux的进程间通信 － 管道](http://liwei.life/2016/07/18/pipe/)

### mmap    

mmap 在 Android 中的 Binder 传输中大量使用;

mmap 是 Linux 进行虚拟内存管理的核心**机制**.Linux 内核将其整个内存地址空间看作一系列不同文件的映射;mmap 提供的机制允许用户将某个文件映射到程序地址的某个部分,使用简单的内存访问指令即可进行对该文件的读写;

mmap 操作虚拟内存地址,各个进程可能操作的虚拟地址不一样,但在内核中转后被映射到同一物理内存地址,可以节省copy, 提升系统访问效率;

CPU 提供虚拟内存的硬件特性,该硬件模块成为 MMU 即为内存管理单元,而操作系统完成对虚拟内存的管理;虚拟内存技术方案实现的核心在于地址空间隔离,意在在多任务操作系统中隔离地址空间,完成地址空间的保护;

mmap 的核心是虚拟内存转换为物理内存的过程,也就是物理地址伪装的过程;其主要使用为**共享内存**,进程间可以通过 mmap 实现对于同一个普通文件的共享内存;

mmap的调用,内核会在进程地址空间中建立新的虚拟地址区域,并回调 mmap 驱动函数时将虚拟地址区域传递给驱动函数;而在用户空间,要对文件进行 mmap ,需要先获取文件描述符,进而调用mmap做内存映射,将文件内容映射到虚拟内存中,完成文件内容的镜像处理,事实上是一次快速 IO;

### epoll   

epoll 在 Android 中使用参见 MessageQueue实现机制;




### VFS   


### 并发  









---  

Quote :

<Unix&Linux 大学教程>

<深入理解计算机系统>

<Linux 就是这个范儿>