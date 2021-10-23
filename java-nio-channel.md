# Java NIO Cahnnel

[原文链接](http://tutorials.jenkov.com/java-nio/channels.html)

Java NIO Channels 和流基本一直，但存在一些小的差别：

- Channels 可以同时支持读写，而流通常只能支持读或写一种方式
- Channels 支持异步读写
- Channels 的读写需要通过 Buffer

正如刚才提到的，对 Channels 的读写需要通过 Buffer，如图所示：
![Channels的读写需要通过Buffer](http://tutorials.jenkov.com/images/java-nio/overview-channels-buffers.png)

## Channel 的实现类

在 Java NIo 中有一些比较重要的 Channel 实现类：

- FileChannel
- DatagramChannel
- SocketChannel
- ServerSocketChannel

FileChannel 读取文件中的数据。

DatagramChannel 能够通过 UDP 网络连接读写数据。

SocketChannel 能够通过 TCP 网络连接读写数据。

ServerSocketChannel 允许监听 TCP 连接，就像 web 服务器那样。每当有连接进来，都会
创建一个 ServerSocketChannel。

## 简单 Channel 实例

这是一个使用 FileChannel 读取数据到到 Buffer 的例子：

```java
    @Test
    public void test() throws IOException {
        RandomAccessFile accessFile = new RandomAccessFile("nio-data.txt", "rw");
        FileChannel inChannel = accessFile.getChannel();
        ByteBuffer buf = ByteBuffer.allocate(1024);
        int bytesRead = inChannel.read(buf);
        while (bytesRead != -1) {
            System.out.println("Read" + bytesRead);
            buf.flip();
            while (buf.hasRemaining()) {
                System.out.println((char) buf.get());
            }
            buf.clear();
            bytesRead = inChannel.read(buf);
        }
        accessFile.close();
    }
```

注意 buf.filp()调用，首先需要把数据写入 Buffer，然后在翻转，此时你可以把写入的内
容读出来。我将在后面的 Buffer 文章中具体介绍它。
