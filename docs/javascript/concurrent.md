# JavaScript并发与事件模型
## 运行时概念
### 栈（Stack）
栈是一种后进先出的数据结构。

每次**函数调用**时都会形成一个由若干[帧](../../docs/javascript/栈帧.md)组成的栈。

### 堆（Heap）
堆通常可以看为是一个完全二叉树的数组对象

堆通常用于
1. 事先不知道程序所需对象的数量和大小
2. 对象太大，不适合使用堆栈分配器

在JavaScript中，对象被分配在堆中。

### 队列（Queue）
队列是一种先进先出的数据结构，比如我们去超市买东西，排队付款，同一个队列中，排在前面的人先完成。

JavaScript运行时包含了一个待处理消息的消息队列。每一个消息都关联着处理这个消息的回调函数（callback function）。

## 事件循环
一个JavaScript运行时会同步地等待消息到达（如果当前没有任何消息等待被处理）

只有每一个消息被完整地执行后，其他消息才可以被执行。

JavaScript中，`setTimeout`函数可以将一个消息添加到队列中，参数1是需要添加的消息，参数2是最少延迟的时间。

`setTimeout`函数第二个参数的时间仅仅代表最小延迟的时间，即使设置为0，也不会立即执行，它会等待当前队列中所有的消息都处理完毕后才能执行，即使超出了第二个参数所指定的时间。

### 多个运行时互相通信
两个不同的运行时只能通过`postMessage`方法进行通信。另一个运行时需要取侦听message事件