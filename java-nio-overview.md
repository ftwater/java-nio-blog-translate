# Java NIO Overview
[原文连接](http://tutorials.jenkov.com/java-nio/overview.html)

Java NIO由一下核心组件构成：

- Channels
- Buffers
- Selectors

Java NIO中的类和组件比这些要多，但是Channel，Buffer和Selectors这些组件的API构成了NIO的核心。剩下的组件，比如Pipe和FileLock之类的仅仅是作为关联这些核心组件的工具类。因此，在概览中我会重点关注这3个组件。其他的组件会在各自的介绍中进行详细解释。可以根据目录进行查看。
## Channel 和 Buffers
在NIO中，所有的IO都是从Channel开始的。Channel就像一个流，可以通过Channel读取数据到Buffer，或者从Buffer写入数据到Channel。示意图如下：

![示意图](http://tutorials.jenkov.com/images/java-nio/overview-channels-buffers.png)

NIO中的Channel实现有以下类：

- FileChannel
- DatagramChannel
- SocketChannel
- ServerSocketChannel

这些channels包括了UDP+TCP的网络IO和文件IO。
NIO中还有一些有意思的接口和类，但是简单起见，我不会在概览中介绍他们。他们会在其他相关章节进行介绍。

下面是NIO中Buffer的实现类列表：

- ByteBuffer
- CharBuffer
- DoubleBuffer
- FloatBuffer
- IntBuffer
- LongBuffer
- ShortBuffer

这些Buffer的实现类支持了基础的数据类型，你可以通过IO传递像byte、short、int、long、float、double和characters。

Java NIO还有一个MappedByteBuffer类，这个类是用来和内存映射文件一起使用的，本次概述也不会详细介绍，留在对应章节介绍。

## Selectors

Selector支持用单线程处理大量Channel。如果你的程序有大量连接，但是每个连接上的传输量并不大，比如聊天服务器，这种方式将会很方便。

比如一个Selector处理3个Channel的示意图如下：

![一个Selector处理3个Channel](http://tutorials.jenkov.com/images/java-nio/overview-selectors.png)

如果需要使用Channel，需要将Channel注册到Selector上，然后通过调用select()方法获取。这个方法将会阻塞线程，直到注册到这个Selector上的某个Channel出现ready事件，当select()方法返回后，线程可以处理这些事件，比如连接成功和收到数据等。