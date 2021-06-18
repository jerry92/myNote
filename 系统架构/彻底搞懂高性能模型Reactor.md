在高性能网络技术中，大家应该经常会看到Reactor模型。并且很多开源软件中都使用了这个模型，如：Redis、Nginx、Memcache、Netty等。
刚开始接触时可能一头雾水，这到底是个什么东东？一查英文解释：“反应堆”，感觉更加唬人了。那么，今天我们来一起看看这个Reactor到底是个啥。

其实通俗点讲，Reacotr = IO多路复用 + 池化技术。是“大神”们将IO多路复用技术结池化技术（线程池进程池）结合的一种模式。IO多路服用负责统一监听事件，收到事件后派发给资源池中的某个线程或进程。

其中根据Reacotr的数量和资源池中资源的数量和类型，Reactor有以下3种典型实现方案。其中“多Reactor单进程/线程”实现方案相比“单 Reactor 单进程”方案，既复杂又没有性能优势，因此实际没有应用。

1. 单Reactor + 单进程/单线程
2. 单Reactor + 多线程
3. 多Reactor + 多进程/多线程

下面我们逐一介绍一下这3个方案，及他们适用的场景。

### 单Reactor + 单进程/单线程

该方案示意图如下（以进程举例）：
![](https://cdn.jsdelivr.net/gh/jerry92/imageHost1/20210617183209.png)

Reactor 对象通过 select 监控连接事件，收到事件后通过 dispatch 进行分发。

如果是连接建立的事件，则由 Acceptor 处理，Acceptor 通过 accept 接受连接，并创建一个 Handler 来处理连接后续的各种事件。

如果不是连接建立事件，则 Reactor 会调用连接对应的 Handler（第 2 步中创建的Handler）来进行响应。Handler 会完成 read-> 处理 ->send 的完整业务流程。

这种优点很明显，就是简单，不用考虑进程间通信、线程安全、资源竞争等问题。但是也有自身的局限性，就是无法利用多核资源，**只适用于业务处理非常快速的场景**，Redis就是采用的这种方案。

### 单Reactor + 多线程

该方案示意图如下：
![](https://cdn.jsdelivr.net/gh/jerry92/imageHost1/20210617185507.png)

与第一种方案相比，不同的是：Handler只负责响应事件，并不负责处理事件，Handler读取数据后会发送给Processor进行处理。Processor在子线程中完成业务处理，然后将结果发送给Handler。由Handler将结果返回给client。

你可能主要到没有列出单Reactor + 多进程方案，主要因为如果采用多进程，就要考虑进程间通信的问题，比如子进程处理完成后需要通知父进程将结果返回给对应的client，处理比较复杂。但多线程之间数据是共享的，复杂度相对比较低。

另外，这种方案下，主线程承担了所有的事件监听和响应。瞬间高并发时可能会成为性能瓶颈。这时就需要多Reactor的方案了。

### 多Reactor + 多进程/多线程

该方案示意图如下（以进程举例）：
![](https://cdn.jsdelivr.net/gh/jerry92/imageHost1/20210617193906.png)

父进程中 mainReactor 对象通过 select 监控连接建立事件，收到事件后通过 Acceptor接收，将新的连接分配给某个子进程。

子进程的 subReactor 将 mainReactor 分配的连接加入连接队列进行监听，并创建一个Handler 用于处理连接的各种事件。

当有新的事件发生时，subReactor 会调用连接对应的 Handler（即第 2 步中创建的Handler）来进行响应。

Handler 完成 read→处理→send 的完整业务流程。

目前著名的开源系统 Nginx 采用的是多 Reactor 多进程，采用多 Reactor 多线程的实现有Memcache 和 Netty。不过需要注意的是 Nginx 中与上图中的方案稍有差异，具体表现在主进程中并没有mainReactor来建立连接，而是由子进程中的subReactor建立。

### Proactor模式

以上就是Reactor模式中的几种常见方案，另外除了Reactor模式还有Proactor模式。Reactor 是非阻塞同步网络模型，因为真正的 read 和 send 操作都需要用户进程同步操作。这里的“同步”指用户进程在执行 read 和 send 这类 I/O 操作的时候是同步的，如果把 I/O 操作改为异步就能够进一步提升性能，这就是异步网络模型 Proactor。

理论上 Proactor 比 Reactor 效率要高一些，但在 Linux 系统下的异步并不完善，因此在 Linux 下实现高并发网络编程时都是以 Reactor 模式为主。所以今天就不对 Proactor 模式进行过多介绍了。


参考资料
- https://time.geekbang.org/column/article/8805
---
如果以上对你有帮助，欢迎关注铁柱，一起成长。

![](https://cdn.jsdelivr.net/gh/jerry92/imageHost1/qrcode_for_gh_9c4eb6c9e91b_258.jpg)