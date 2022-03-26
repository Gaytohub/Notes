# Java

### Java 可访问修饰符的作用范围：

public：允许任意类访问。

protected：允许被同一包内的类访问，还允许被子类使用。

无修饰符：只允许被同一包内的类访问。

private：只能在类的内部使用，不能在类外部使用。

**描述时，应当注意三个边界：类内与类外、包内与包外、是否是子类。**



### JDK 和 JRE 的区别：

**JRE (Java Runtime Enviroment)** ：Java 的运行时环境。面向的是 Java 程序的使用者，而不是开发者。JRE 中包含了 JVM 的标准实现及 Java 核心类库。

**JDK (Java Development Kit) **：Java 的开发工具包，提供了 Java 的开发环境（提供了 javac 等工具）和运行环境（提供了 JVM 和 Runtime 辅助包）。在完整安装 JDK 后，不仅可以开发 Java 程序，也同时拥有了运行 Java 程序的平台。



### 同步 IO 、异步 IO、阻塞IO、非阻塞IO

首先一个 IO 操作（read / write 系统调用）其实分为了两个步骤：1.发起 IO 请求 2.实际的 IO 读写（内核态与用户态的数据拷贝）

阻塞 IO 和非阻塞 IO 的区别在于第一步：发起 IO 请求的进程是否会被阻塞， 如果阻塞直到 IO 操作完成才返回那么就是传统的阻塞 IO，如果不阻塞，那么就是非阻塞 IO。

同步 IO 和异步 IO的区别在于第二步：实际的 IO 读写（内核态与用户态的数据拷贝）是否需要进程参与，如果需要进程参与则是同步 IO，不需要进程参与就是异步 IO。

在编程上，这种非阻塞 IO 一般都采用 IO 状态事件 + 回调方法的方式来处理 IO 操作。

如果是同步 IO，则状态事件为读写就绪，此时的数据仍在内核态中，但是已经准备就绪，可以进行 IO 读写操作。

如果是异步 IO，则状态事件为读写完成，此时的数据已经存在于应用进程的地址空间（用户态）中。



### NIO 为什么是同步非阻塞 IO？

同步：NIO 之所以是同步，是因为它的 accept / read / write 方法的内核 IO 操作都会阻塞当前线程。

非阻塞：发起请求不会阻塞请求者的线程。会通过不断的轮询，来实现获取已经准备好内核 IO 的操作流。



### Java NIO

NIO 主要有三大核心部分：Channel（通道），Buffer（缓冲区），Selector（选择器）。传统 IO 是基于字节流和字符流进行操作，而 NIO 基于 Channel 和 Buffer 进行操作。数据总是从通道读取到缓冲区中，或者从缓冲区中写入到通道。

Selector（选择器）用于监听多个通道的事件（比如：连接打开，数据到达）。因此，单个线程可以监听多个数据通道。

1. **Buffer（缓冲区）是一个用于存储特定基本数据类型的容器**。除了 Boolean 外，其余每种基本类型都有一个对应的 Buffer 类。Buffer 类的子类有 ByteBuffer，CharBuffer，DoubleBuffer，FloatBuffer，
2. Channel（通道）



### StackOverFlow 和 OutOfMemory 出现的原因：

**栈溢出**：每一个 JVM 线程都拥有一个私有的 JVM 线程栈，用于存放当前线程的 JVM 栈帧。**如果某个线程的线程栈空间被耗尽，没有足够资源分配给新创建的栈帧，就会抛出java.lang.StackOverflowError 错误。**

**可能产生的原因**：

1. 递归调用过深，没有跳出递归的条件。
2. 执行了大量的方法，导致线程栈空间耗尽。
3. 不断的创建线程。

**内存溢出**：创建对象的速度高于 GC 回收速度，或者内存泄露等都会导致内存溢出。简单地说内存溢出就是，程序运行过程中申请的内存大于系统能够提供的内存，导致无法申请到足够的内存。



### 抽象类和抽象方法的区别：

抽象类不可以被实例化，抽象方法所在的类一定是抽象类，抽象类可以没有抽象方法。

抽象方法：abstract 修饰的方法是抽象方法。抽象方法没有方法体，只保留方法的功能，具体的执行，交给继承抽象类的子类，由子类重写抽象方法。

**Notice**：如果子类继承抽象类，并重写了父类的所有抽象方法，则此子类不是抽象类，可以实例化。如果子类继承抽象类，但是没有重写父类所有的抽象方法，那么这个子类就必须申明为抽象的。



### 接口和抽象类的区别：

1. 接口的方法默认是 public，所有方法在接口中不能有实现，而抽象类可以有非抽象方法。抽象方法可以有 public、protected 和 default 这些修饰符。（抽象方法就是为了被重写，所以不能使用 private 关键字修饰）

2. 接口中除了 static、final 变量，不能有其他变量，而抽象类中则不一定。

3. 一个类可以实现多个接口，但只能继承一个抽象类。接口自己本身可以通过 extends 关键字扩展多个接口。

4. 从设计层面来讲，抽象是对类的抽象，是一种模版设计，而接口是对行为的抽象，是对行为的规范。

   

### sleep() 和 wait() 方法的区别：

两者都能暂停线程。但是 sleep 没有释放锁，而 wait 释放锁。 wait 常被用于线程间通信，sleep 则用来暂停执行。 wait 方法调用后，线程不会自动苏醒，需要别的线程调用同一对象的 notify 或 notifyAll 方法。sleep 方法执行完成后，线程会自动苏醒。



### 动态代理和静态代理的区别：

在代码运行之前，静态代理的 代理类.class 文件就已经存在，JVM 会读取 .class 文件，解析 .class 文件内的信息，加载到内存中。动态代理不不存在 代理类的 .class 文件，在运行时动态生成类字节码文件，并加载到 JVM 中。



### 关于 Comparator 我必须知道的事儿

jdk 官方默认是升序，是基于：

```
< return -1
= return 0
> return 1
```

以上内容可以理解为硬性规定，也就是说，排序是由这三个参数同时决定的。

如果是降序就必须完全相反：

```
< return 1
= return 0
> return -1
```

所以，如果我们这样写就是降序：

```java
if (o1 < o2){
  return 1;
} else if (o1 > o2){
  return -1;
} else {
  return 0;
}
```

这样写就是升序：

```java
if (o1 < o2){
  return -1;
} else if (o1 > o2){
  return 1;
} else {
  return 0;
}
```



### 为什么不调用 run() ？ 而需要调用 start() ?

New Thread() 线程会进入新建状态，调用 start 方法会启动一个线程并使其进入就入就绪状态，等待分配 CPU 时间片就可以运行了。 start 会执行线程相应的准备工作，然后自动执行 run 方法的内容（真的多线程）。直接调用 run 方法相当于在主线程执行普通方法。



### JVM 运行时数据区域

**程序计数器**：字节码解释器通过改变程序计数器的值来选取下一条需要执行的命令。分支、循环、跳转、异常处理、线程恢复都依赖这个计数器完成。

**虚拟机栈**：线程私有，生命周期同线程相同，描述的是 Java 方法执行的内存模型。每次方法调用的数据都是通过栈传递的。**由栈帧组成， 栈帧中有局部变量表，操作数栈，动态链接和方法出口信息。**

**本地方法栈**：与虚拟机栈相似，虚拟机栈为虚拟机执行 Java 方法服务。本地方法栈则为虚拟机使用到的 Native 方法服务。

**堆**：Java虚拟机中管理内存最大的一块，唯一的目的就是存放对象实例，几乎所有对象以及数组都在这里分配内存。

**方法区**：各线程共享的内存区域。用于存储已被虚拟机加载的类信息、常量、静态变量，即时编译器编译后的代码等数据。



### JVM中哪些类可以作为GCRoot对象？

1. java虚拟机栈中的引用的对象。
2. 方法区中的类静态属性引用的对象。（一般指被static修饰的对象，加载类的时候就加载到内存中。）
3. 方法区中的常量引用的对象。
4. 本地方法栈中的JNI（Native方法）引用的对象。



### 类的加载机制

JVM 把描述类的数据从 class 文件加载到内存，并对数据进行校验、转换解析和初始化，最终形成可以被虚拟机直接使用的 Java 类型，这个过程被称作虚拟机的类加载机制。

### 类的生命周期

类从被加载到虚拟机内存中开始，到卸载出内存为止，**它的整个生命周期包括：加载、验证、准备、解析、初始化、使用、卸载这七个阶段。**其中验证、准备、解析 3 个部分统称为连接。



### Java 对象创建过程

1. 类加载检查：虚拟机遇到 new 关键字，就检查指令参数在常量池中是否存在符号引用，并且检查这个符号引用的类是否已被加载、解析、初始化过。如果没有，需要首先执行相应的类加载过程。
2. 分配内存：对象所需要的内存大小在类加载完成之后便可以确定，这一步就是在 Java 堆中划分出一块确定大小的内存。内存的分配方式有“指针碰撞”和“空闲列表”两种，选取哪种方式取决于 Java 堆是否规整。
3. 初始化零值：将分配到的内存空间（除对象头外）初始化零值。这一步操作保证了对象的实例字段在 Java 代码中可以不赋初始值就能使用（访问到的是数据类型对应的零值）。
4. 设置对象头：虚拟机对对象进行必要的设置，如这个对象是哪个类的实例，如何才能找到类的元数据信息，对象的哈希码，对象的 GC 分代年龄信息，这些信息都保存在对象头中。
5. 执行 init() 方法：按照程序猿的意愿进行初始化。



### 对象访问定位的两种方式？

Java 程序通过栈上的 Reference 数据来操作堆上的具体数据。目前主流的访问方式有两种：1.句柄 2.直接指针

1. 句柄：Java 堆会划分出一块内存作为句柄池，Reference 中存储的是对象的句柄地址，而句柄中包含了对象实例数据与类型数据各自的具体地址信息。
2. 直接指针：Reference 存储的是对象实例数据的地址，对象实例数据中存储对象类型数据地址。

优势：句柄：Reference 存储的是稳定的句柄地址，对象移动时只需要改变句柄中的数据指针。

直接指针：速度快，节省一次指针定位的时间开销。



### Java 的四种引用类型：

1. 强引用：Object o = new Object() 这种对象永远不会被回收，即使内存不足，JVM 抛出 OOM，也不会回收。

2. 软引用：SoftReference<Object> o = new SoftReference<Object(new Object)>

   ​               Object temp = o.get()	

   需要用 SoftReference 包裹对象，使用 get 获取这个对象。特点是当内存不足，不会第一时间收集。当GC后，内存仍不足，就会把软引用包裹的对象回收。（可以用来做网页图片缓存）

3. 弱引用：WeakReference<Object> o = new WeakReference<Object(new Object)>

   ​               Object temp = o.get()	

   特点：只要触发 GC，就会把弱引用包裹的对象回收。

4. 虚引用：特点：只要触发 GC ，就会把虚引用包裹的对象回收，并把回收的通知放到 ReferenceQueue 中（必须配合使用）



### 堆内存中对象的分配策略：

1. 对象优先在 eden 区分配 
2. 大对象直接进入老年代
3. 长期存活的对象将进入老年代



### 判断对象死亡的 2 种方法？

1. 引用计数法
2. 可达性分析法



### 垃圾回收算法

1. **标记 - 清除算法**：两轮遍历，第一轮标记存活对象，第二轮清除需要回收的对象。

   问题：效率低，空间问题（会产生大量不连续的碎片）

2. **标记 - 整理算法**：第一轮标记存活对象，后续不直接对可回收对象回收，而是让所有存活对象向一端移动，直接清理掉端边界以外垃圾对象。

3. **复制算法**：解决了效率问题。将内存分为两个大小相同的块，每次使用一块，当一块内存使用完后，就将还存活的对象复制到另一块去，然后再把使用的空间一次性处理掉。

4. **分代收集**：根据对象存活周期不同，将内存分为几块（新生代，老年代），各代根据特点选择合适的垃圾收集算法。

   新生代：大量对象死亡，有辅助空间，需要高效回收，使用复制算法。

   老年代：存活率高，没有额外的辅助空间，使用标记 - 清除， 标记 - 整理算法。



### 常见的垃圾收集器

...

Serial 收集器：只使用一条垃圾收集线程去完成垃圾收集工作，并且在进行垃圾收集工作的时候，必须暂停其他的工作线程（stop-the-world），直到它结束。

Par New 收集器：Serial 收集器的多线程版本。

Parallel Scavenge 收集器：关注吞吐量，高效率运用 CPU

Serial Old 收集器：Serial 收集器的老年代版本。

Parallel Old 收集器：Parallel Scavenge 收集器的老年代版本，使用多线程的标记 - 整理算法。

CMS 收集器：使用的是标记-清除算法。更多关注用户线程的停顿时间，提高用户体验，第一次实现了让垃圾收集线程与用户线程（基本上）同时工作。整体过程分为 4 步：

第一步：初始标记：暂停所有其他线程，并记录下直接与 root 相连的对象。

第二步：并发标记：同时开启 GC 和用户线程，用一个闭包结构去记录可达对象。但是在这个阶段结束后，闭包结构不能保证包含当前所有可达对象（因为用户线程可能会不断的更新引用域，所以 GC 线程无法保证可达性分析的实时性）。

第三步：重新标记：修正并发标记期间因用户程序继续运行而导致标记产生的变动。

第四步：并发清除：开启用户线程，同时 GC 线程开始对未标记区域进行清除。

优点：实现了并发收集和低停顿。

缺点：1.对 CPU 敏感。 2.无法处理**浮动垃圾** 3.采用 “标记 - 清除” 算法产生空间碎片。

G1的整体过程：

第一步：初始标记：暂停所有其他线程，并记录下直接与 GC Root 相连的对象。

第二步：并发标记：同时开启 GC 和用户线程，进行可达性分析，找出存活的对象。

第三步：最终标记：停顿用户线程，修正并发标记期间因用户程序继续运行而导致标记产生的变动。

第四步：筛选回收：对各个 Region 的回收价值和成本进行排序，根据用户期望的 GC 停顿时间来制定回收计划。

优点：

1.充分利用多核CPU，使用多个CPU来缩短 stop-the-world停顿时间。

2.空间整合，使用的是标记-整理算法，局部可以看成是复制算法，不会产生内存碎片。

3.可预测停顿时间。

| 特征                     | G1   | CMS  |
| ------------------------ | ---- | ---- |
| 并发和分代               | 是   | 是   |
| 最大化释放堆内存         | 是   | 否   |
| 低延时                   | 是   | 是   |
| 吞吐量                   | 高   | 低   |
| 压实                     | 是   | 否   |
| 可预测性                 | 是   | 否   |
| 新生代和老年代的物理隔离 | 否   | 是   |



#### 创建线程的几种方式：

1. 继承 Thread 类，并重写 run() 方法
2. 实现 Runnable 接口，并实现 run() 方法
3. 实现 Callable 接口，实现 call() 方法，并结合 FutureTask 实现。 call() 方法是带有返回值的。
4. 通过线程池创建线程。



### ThreadPoolExecutor 参数：

1. corePoolSize ： 核心线程数量，它的数量决定了添加的任务是开辟新线程执行还是放到 workQueue 队列中。

2. maximunPoolSize ：线程池中的最大线程数量。

3. KeepAliveTime ：超出 corePoolSize 的线程能够存活的时间。

4. Unit ：KeepAliveTime 参数的单位。

5. workQueue ：任务队列，有四种取值。

   ArrayBlockingQueue ： 有界数组队列，先进先出，创建时必须指定大小。

   SynchronousQueue ：同步阻塞队列，不保存提交的任务，直接新建线程执行新来的任务。

   LinkedBlockingQueue ：基于链表的先进先出序列，支持有界 / 无界队列。

   PriorityBlockingQueue ：优先队列，可针对任务进行排序。

6. ThreadFactory ：线程创建的工厂。

7. Handler ：拒绝策略，当达到最大线程量，并且队列已满时触发。

   AbortPolicy ：抛出 RejectedExecutionException，拒绝新任务处理。

   CallerRunsPolicy ：调用执行自己的线程运行任务。

   DiscardPolicy ：不处理新任务，直接丢弃掉。

   DiscardOldestPolicy ：丢弃最早的未处理的任务请求。

#### CPU 密集型 （CPU BOUND）

CPU 密集型，即计算密集型，大部分的状况是 CPU 负载 100%，CPU 要读写IO，但 IO 很短时间可以完成，而 CPU 还要花很多时间计算。

#### IO 密集型（IO BOUND）

大部分状况是 CPU 在等 IO 操作完成，CPU 负载并不高！

CPU 密集型任务配置应尽可能小的线程，如 CPU 数目 + 1（加一个线程主要是为了当某线程偶尔出现故障时，额外的线程能保证 CPU 时钟周期不浪费）。

IO 密集型任务并不是一直在执行任务，则应该配置尽可能多的线程，如 2 * CPU 数目，让这些线程做 IO 操作去。



### 并发编程中的三个概念：

1. 原子性：一个或多个操作，要么都执行，要么都不执行。

2. 可见性：当多线程访问一个变量时，一个线程修改了变量值，其他线程应当能立即看到修改后的值。

3. 有序性：程序执行的顺序按照代码顺序执行。

   可能发生指令重排，（处理器为提效，可能对输入代码做优化，不保证程序中各语句的执行先后顺序同代码相同，但是保证最后结果同顺序执行结果一致），指令重排在单线程的情况下没有问题，多线程并发则会出现问题。

   一线程：

   ```
   context = loadContext();
   
   inited = true; (指令重排，先执行这句话)
   ```

   二线程：

   ```
   while(inited){
   	sleep();
   }
   doSomething();
   ```

   上例会导致一线程还未初始化，二线程便 doSomething()，这是不应该的。



### volatile 关键字

volatile ：一个变量被 volatile 修饰后，便具备两层语义：

一、保证不同线程对这个变量操作的可见性。

        1. 使用 volatile 关键字会强制将修改的值写入内存。
        2. 线程二修改变量值，会使线程一缓存的变量值无效。
        3. 由于线程一缓存变量无效，而重新从内存中读取。

二、禁止指令重排

​	保证在指令优化时，volatile 前的语句不会在 volatile 后执行。volatile 后的语句也不会在 volatile 前执行。



volatile ---> 汇编代码出现 lock 指令 ---> 施加内存屏障

1. 执行到内存屏障时，保证之前的语句都完成。
2. 对缓存修改立即写入内存。
3. 写操作导致缓存无效。



### synchronized 关键字

synchronized 的三种使用形式：

1. 修饰普通的方法：锁当前实例对象
2. 修饰静态方法：锁当前类 class 对象  （两个不同的锁，可同时获取）
3. 修饰代码块：锁括号里配置的对象

Notice ：

1. synchronized 修饰非静态方法和代码块，同一类不同对象有自己的锁，所以不阻塞。
2. synchronized 修饰实例对象，若正在访问实例对象的同步方法，则不能访问该实例的其他同步方法。
3. 线程 A 访问非静态 synchronized 方法， 线程 B 访问静态 synchronized 方法时并不互斥，获取的是两个不同的锁。

synchronized 锁的四种状态：无锁，偏向锁，轻量级锁，重量级锁

初次执行到 synchronized 代码块时，锁对象变成偏向锁（通过 CAS 修改对象头的锁标志位）。执行完代码不会主动释放锁，当第二次到达同步代码块时，判断此时持有锁的对象是否是自己？是的话就正常执行（没有锁释放和加锁操作，如果自始至终只有一个使用锁的线程，那么几乎没有额外开销，性能很高）。

Notice ： 偏向锁撤锁或升级锁的过程 ： 1.等待全局安全点  ---> 暂停偏向锁线程 ---> 检查锁对象锁定状态 ---> 线程不活动 ---> 设置对象头（无锁或轻量锁）

当锁是偏向锁，却被其他线程访问，偏向锁会升级为轻量级锁，其他线程会通过自旋的形式获取锁。

长时间自旋非常消耗 CPU 资源，一个线程持有锁，其他线程只能空耗 CPU，无法执行有效任务（忙等现象）。但是在轻微锁竞争的情况下，允许段时间的忙等，换取线程在用户态和内核态之间频繁切换的开销。

忙等是有限度的（计数器记录自旋次数），达到一定的次数，轻量级锁会升级为重量级锁（通过 CAS 修改锁标志）当一个线程获取锁后，其余所有等待该锁的线程会处于阻塞状态（控制权交由操作系统，会出现频繁对线程状体的切换、挂起、唤醒从而消耗大量资源）。



### synchronized 和 volatile 的区别：

1. volatile 是线程同步的轻量级实现，所以 volatile 的性能优于 synchronized 。但是 volatile 只能用于变量，而 synchronized 可以修饰方法、代码块。
2. volatile 能保证数据可见性，但不能保证数据原子性，synchronized 两者都能保证。
3. volatile 关键字主要解决变量在多个线程之间的可见性。synchronized 主要解决多线程访问资源的同步性。



### synchronized 和 Lock 的区别：

1. synchronized 编码简单，锁机制由 JVM 实现，竞争不激烈的情况下性能更好。Lock 功能更强大更灵活，竞争激烈时性能更好。
2. 锁机制不同，synchronized 是 JVM 层面实现，Lock 是 JDK 代码实现，需要手动释放（在 finally 中释放 unlock() ）
3. synchronized 能够用于代码块、方法。 Lock 只能写在代码里，并且 Lock 支持公平锁，非公平锁，读写锁等。



### CAS （Compare And Swap） 乐观锁

每次不加锁，假设没有冲突去完成某项工作，因冲突失败就重试，直到成功。

CAS 原理：在内存值和期望值相同时，将内存值更新为需要的值。内存值 V，期望值 A，更新值 B   ----> if V == A, UPDATE V = B;

应用：java.util.concurrent.atomic 包中的原子类

例子：内存值 V = 10，线程一相加 1，即 V = 10， A = 10，B = 11

但是线程二提前修改，使得 V = 11，此时线程一，V != A，无法完成更新。

线程一重新读取，V = 11，A = 11，B = 12，此时 V == A，更新 V = 12

**优点**：非阻塞的轻量级乐观锁，在资源竞争不激烈的情况下，性能高，相比 synchronized 省去了加锁、解锁、唤醒等操作。

**缺点**：

1. ABA 问题：现有线程一二。线程二将 A 的值改为 B，后又将 B 的值改为 A，此时线程一认为 A 的值没有被修改过。**解决**：追加版本号，1A --> 2B --> 3A
2. 自旋时间过长，消耗 CPU 资源



### 反射

**反射：程序运行时动态加载类并获取类的信息，从而操纵类或对象的属性和方法。**

#### 为什么需要反射？

1. 某些时候只有 .class，没有 .java
2. 有的类用到的时候再动态加载，能减少 JVM 启动时间。
3. 几乎所有通用框架的开发都离不开反射。

**反射的实现主要借助于四个类：**Class，Constructor，Field，Method



### 序列化和反序列化

**序列化**：把对象转换成有序的字节流，以便在网络上传输或存在本地文件中。

**反序列化**：根据获取到的字节流保存的对象状态和描述信息，重建对象。



### 双亲委派模型

四级类加载器：启动类加载器、扩展类加载器、应用类加载器、自定义类加载器

#### 什么是双亲委派模型？

如果一个类加载器收到了类加载请求，它首先不会自己去尝试加载这个类，而是把这个类的加载请求委派给父类加载器去完成，每一个层次的加载器都是如此，因此所有的类加载请求都会传给顶层的启动类加载器，只有当父加载器反馈自己无法完成类加载请求时，子加载器才会尝试自己去加载。

#### 为什么需要双亲委派模型？

1. 双亲委派模型的方式，可以避免类的重复加载，当父加载器已经加载过某一个类的时候，子加载器就不会重新再加载这个类。
2. 通过双亲委派的方式，还保证了安全性。像启动类加载器只会加载 JAVA_HOME 中的 jar 包里的类，这样可以避免有人自定义一个有破坏功能的类被加载，可以有效防止核心 Java API 被篡改。

#### 破坏双亲委派模型的例子？（记住前俩）

1. 实现热插拔热部署的工具。为了让代码能够动态加载而无需重启，实现方式就是把模块连同类加载器一起换掉实现代码的热替换。
2. JDBC 4.0之后实际上是不需要调用 Class.forName 来加载驱动，只需要把驱动的 jar 包放到工程的类加载路径里， 驱动就会被自动加载（这种自动加载采用的技术叫 SPI）。
3. Tomcat。对于各个webapp中的class和lib，需要相互隔离，不能出现一个应用加载的类库影响另一个应用的情况（不同应用程序可能依赖同一个第三方类库的不同版本，但是不同版本的类库中某一个类的全路径可能是一样的）。Tomcat造了一堆自己的classloader（当然Tomcat也有热部署的原因）。



### HashMap

#### HashMap 扩容：

**HashMap 扩容时机**：当 map 中包含的 Entry 的数量大于等于 threshold = loadFactor * capacity 的时候，且新建的 Entry 刚好落在一个非空的桶上，此时触发扩容机制，将其容量扩大为 2 倍。

**扩容步骤**：

1. 扩容数组容量到原来的两倍（如果扩容前数组容量已经达到了 2 的 20 次方，就会修改阈值到 int 的最大值，之后也不会扩容）。
2. 转移现有数据到新的 Entry 数组里（1.7 的时候，会重新计算每个元素在新数组中的位置，1.8 之后是判断，当前元素是否需要移位，移位后的位置就是当前位置加扩容容量）。



### ConcurrentHashMap 源码解析

#### ConcurrentHashMap 中几个重要概念

``````java
private static final int MAXIMUM_CAPACITY = 1 << 30;
private static final int DEFAULT_CAPACITY = 16;
static final int TREEIFY_THRESHOLD = 8;
static final int UNTREEIFY_THRESHOLD = 6;
static final int MIN_TREEIFY_CAPACITY = 64;

//表示正在转移
static final int MOVED = -1;

//表示已经转换成树
static final int TREEBIN = -2;

//默认没初始化的数组，用来保存元素
transient volatile Node<K,V>[] table;

//转移的时候用的数组
private transient volatile Node<K,V>[] nextTable;

//用来控制初始化和扩容的。默认值为 0 ，当在初始化的时候指定了大小，这会将这个大小保存在 sizeCtl 中，
//大小为数组的 0.75 。
//当为负的时候，说明正在初始化或扩张。
//    -1 表示初始化
//    -（1 + n），其中 n 表示活动的扩张线程
private transient volatile int sizeCtl;
``````

#### ConcurrentHashMap 中几个重要的类

``````java
//Node<K, V> 这是构成每个元素的基本类
static class Node<K, V> implements Map.Entry<K, V>{
  
}

//TreeNode<K, V> 构造树的节点
static class TreeNode<K, V> extends Node<K, V>{
  
}

//TreeBin 用作树的头节点，只存储 root 和 first 节点，不存储节点的 key, value 值
static class TreeBin<K, V> extends Node<K, V>{
  
}

//ForwardingNode 在转移的时候放在头部的节点，是一个空节点
static class ForwardingNode<K, V> extends Node<K, V>{
  
}
``````

#### ConcurrentHashMap 中几个重要的方法

``````java
//用来返回节点数组指定位置节点的原子操作
static final <K, V> Node<K, V> tabAt(Node<K, V>[] tab, int i){
  
}

//cas 操作，在指定位置设定值
static final <K, V> boolean casTabAt(Node<K, V>[] tab, int i, Node<K, V> c, Node<K, V> v){
  
}

//原子操作，在指定位置设定值
static final <K, V> void setTabAt(Node<K, V>[] tab, int i, Node<K, V> v){
  
}
``````

#### ConcurrentHashMap 的 put 操作详解

`````java
//下面是 put 方法的源码
public V put(K key, V value){
  return putVal(key, value, false);
}

//单纯的调用 putVal 方法，并且 putVal 的第三个参数设置为 false
//当设置为 false 的时候表示这个 value 一定会设置
//当设置为 true 的时候，只有当这个 key 的 value 为空的时候才会设置
`````

``````java
/*
* 当添加一对键值对的时候，首先会去判断 tab 数组是否初始化了。
* 如果没有的话，就首先初始化数组
*		然后计算该 key 值应当放在数组的哪个位置。
* 如果这个位置为空，使用 CAS 的方式进行直接添加。 
* 如果这个位置不为空，则取出这个节点来
* 如果取出来节点的 hash 值是 MOVED(-1)的话，则表示当前正在对这个数组进行扩容（复制到新数组），
* 则当前线程也去帮助扩容。
* 最后一种情况就是，如果这个节点，不为空，也不在扩容，则通过 synchronized 来加锁，再进行添加操作。
*		此时需要判断当前取出的节点位置存放的是链表还是树
* 	如果是链表，则遍历整个链表，取出每个节点的 key 和要插入的 key 值比较，如果 key 相等，则说明是同一个
* 			key，则覆盖原有的 value，否则的话就添加到链表的末尾。
* 	如果是树的话，就调用 putTreeVal 方法，把这个元素添加到树中去。
* 最后添加完成后，会判断在该节点处共有多少个节点（添加节点前），如果达到 8 个及以上了，
* 就调用 treeifyBin 方法来尝试将该处的链表转换为树，或是扩容数组。
*/
``````

#### ConcurrentHashMap 扩容

当数组长度小于 64 的时候，使用 tryPresize 函数将数组长度扩展一倍。

否则，对需要扩张的头节点进行加锁，再将链表转化为树。

由于帮助扩容机制的存在：每个 CPU 最少处理 16 个长度的数组元素，也就是说如果一个数组的长度只有 16，那么只有一个线程会对数组进行扩容的复制移动操作。每处理完一个节点会在头部设置一个 fwd 节点，这样其他线程就会跳过它。

#### ConcurrentHashMap 的 get 操作

get 函数本身不加锁， 支持并发操作。get 首先通过计算 key 的 hash 值来确定该元素在数组的哪个位置，然后该位置的所有节点，存在 key 相同的就返回 value，否则返回 null。

hash 值小于 0 的时候有两种情况，一个是为 -1，说明正在扩容，该节点是 FWD 节点，需要调用 FWD 的 find 方法到 nextTable 中找。 另一个是为 -2，说明该节点是红黑树，需要使用红黑树的查找算法来进行查找。



### AbstractQueuedSynchronizer 源码解析

``````java
//头节点，可以当作当前持有锁的线程
private transient volatile Node head;

//阻塞的尾节点，每个新的节点进来，都插到最后，也就形成了一个链表
private transient volatile Node tail;

//代表当前锁的状态，0 代表没有被占用，大于 0 代表当前有线程持有锁
//这个值可以大于 1，以为锁可以重入，每次重入都加上 1
private volatile int state;

//代表当前持有独占锁的线程。
private transient Thread exclusiveOwnerThread;
``````

#### 线程抢锁

`````java
static final class FairSync extends Sync {
  private static final long serialVersionUID = -3000897897090466540L;

  //争锁
  final void lock() {
    acquire(1);
  }
  
  public final void acquire(int arg) {
    //首先调用 tryAcquire(1)，尝试获取锁，如果直接就获取到了就不需要进队列排队了。
    if (!tryAcquire(arg) &&
        //tryAcquire(1) 没有成功，这个时候需要把当前线程挂起，放到阻塞队列中。
        acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
      selfInterrupt();
  }
  
  //返回true：1.没有线程在等待锁；2.重入锁，线程本来就持有锁。
  //获取当前线程和锁的 state 状态。
  //如果 state 值为 0，则判断等待队列中是否有线程在等待，
  //如没有，则使用 CAS 的方式尝试获取锁，获取成功，就标记一下锁被当前线程占有，并返回 true
  //	如果使用 CAS 尝试获取锁失败，则说明几乎同一时间，另外一个线程获取到了锁资源，则返回 false
  //等待队列中有线程就返回 false
  //如果 state 不是零，就判断占有锁的线程是不是当前线程。
  //是的话，就更新 state 值，并返回 true
  //不是的话，就返回 false
  protected final boolean tryAcquire(int acquires) {
    final Thread current = Thread.currentThread();
    int c = getState();
    if (c == 0) {
      if (!hasQueuedPredecessors() &&
          compareAndSetState(0, acquires)) {
        setExclusiveOwnerThread(current);
        return true;
      }
    }
    else if (current == getExclusiveOwnerThread()) {
      int nextc = c + acquires;
      if (nextc < 0)
        throw new Error("Maximum lock count exceeded");
      setState(nextc);
      return true;
    }
    return false;
  }
  
  //把当前线程包装成 NODE，同时使用 CAS 的方式添加到队列末尾。
  private Node addWaiter(Node mode) {
    Node node = new Node(Thread.currentThread(), mode);
    Node pred = tail;
    if (pred != null) { 
      node.prev = pred; 
      if (compareAndSetTail(pred, node)) {
        pred.next = node;
        return node;
      }
    }
    //到这一步说明 ： pred == null 或者 CAS 失败（有线程在竞争入队）
    enq(node);
    return node;
  }
  
  //等待队列为空 或者 有线程竞争入队，导致进入该函数。
  //该函数使用自旋的方式入队。
  private Node enq(final Node node) {
    for (;;) { 
      Node t = tail; 
      //如果是因为等待队列为空，则先初始化队列，再入队。
      if (t == null) { // Must initialize 
        if (compareAndSetHead(new Node())) 
          tail = head; 
      } else { 
        node.prev = t; 
        if (compareAndSetTail(t, node)) { 
          t.next = node; 
          return t; 
        } 
      } 
    } 
  }
  
  final boolean acquireQueued(final Node node, int arg) { 
    boolean failed = true; 
    try { 
      boolean interrupted = false; 
      for (;;) { 
        final Node p = node.predecessor(); 
        //如果 p == head，说明当前节点是阻塞队列的第一个，
        //这种情况可以尝试抢一下锁
        if (p == head && tryAcquire(arg)) { 
          setHead(node); 
          p.next = null; // help GC 
          failed = false; 
          return interrupted; 
        } 
        //如果当前 node 不是阻塞队列第一个，
        //或者 tryAcquire(arg) 没有抢赢
        if (shouldParkAfterFailedAcquire(p, node) &&
            parkAndCheckInterrupt()) 
          interrupted = true; 
      } 
    } finally { 
      if (failed) cancelAcquire(node); 
    } 
  }
  
  //当前线程没有抢到锁，是否需要挂起当前线程？
  //首先获取前驱节点的 waitStatus
  //如果前驱节点的 waitStatus == -1，说明前驱节点状态正常，当前线程需要挂起。返回true
  //如果前驱节点的 waitStatus > 0，说明前驱节点取消了排队，
  //（而挂起线程的唤醒需要由前驱节点完成）
  //就去找 waitStatus <= 0 的节点，把它设为前驱节点。
  // 进入 else 说明前驱节点的 waitStatus 是 0（初始化节点后从来未动过），
  // 则使用 CAS 的方式把前驱节点的 waitStatus 设置为 -1.
  private static boolean shouldParkAfterFailedAcquire(Node pred, Node node) { 
    int ws = pred.waitStatus; 
    if (ws == Node.SIGNAL) 
      return true; 
    if (ws > 0) { 
      do { 
        node.prev = pred = pred.prev; 
      } while (pred.waitStatus > 0); 
      pred.next = node; 
    } else { 
      compareAndSetWaitStatus(pred, ws, Node.SIGNAL); 
    } 
    return false; 
  }
  
  // shouldParkAfterFailedAcquire 返回 true
  // 则执行对当前线程的挂起
  private final boolean parkAndCheckInterrupt() { 
    LockSupport.park(this); 
    return Thread.interrupted(); 
  }
`````

#### 解锁操作

`````java
public void unlock() { 
  sync.release(1); 
}

public final boolean release(int arg) { 
  if (tryRelease(arg)) { 
    Node h = head; 
    if (h != null && h.waitStatus != 0) 
      unparkSuccessor(h); 
    return true; 
  } 
  return false; 
}

protected final boolean tryRelease(int releases) { 
  int c = getState() - releases; 
  if (Thread.currentThread() != getExclusiveOwnerThread()) 
    throw new IllegalMonitorStateException(); 
  boolean free = false; 
  if (c == 0) { 
    free = true; 
    setExclusiveOwnerThread(null); 
  } 
  setState(c); 
  return free; 
}

private void unparkSuccessor(Node node) { 
  int ws = node.waitStatus; 
  if (ws < 0) 
    compareAndSetWaitStatus(node, ws, 0); 
  Node s = node.next; 
  if (s == null || s.waitStatus > 0) { 
    s = null; 
    for (Node t = tail; t != null && t != node; t = t.prev) 
      if (t.waitStatus <= 0) 
        s = t; 
  } 
  if (s != null) 
    LockSupport.unpark(s.thread); 
}
`````



### JDBC 连接池

为了避免频繁地创建和销毁 JDBC 连接，可以通过连接池复用已经创建好的连接。

常见的 JDBC 连接池有：HikariCP，C3P0，BoneCP，Druid



### Java 中的内存泄漏

Java 中的内存泄漏，广义通俗的说，就是：不再会被使用的对象的内存空间无法被回收，就是内存泄漏。

对象都是有生命周期的，有的长，有的短。**如果长生命周期的对象持有短生命周期对象的引用，就很有可能出现内存泄漏。**



### 消息队列常用的使用场景：

1. 异步处理
2. 应用解耦：订单系统：用户下单，订单系统完成持久化处理，将消息写入消息队列，返回用户订单下单成功。库存系统：订阅下单的消息，采用拉 / 推的方式，获取下单的信息，库存系统根据下单信息，进行库存操作，实现了两者的解耦。
3. 流量削峰：秒杀活动一般会因为流量过大，导致流量的暴增，应用挂掉。该问题可通过加入消息队列实现。用户的请求，服务器接收之后，首先写入消息队列。假如消息队列长度超过最大数量，则直接抛弃用户请求或跳转到错误页面。秒杀业务会根据消息队列中的请求消息，再做后续处理。
4. 日志处理
5. 消息通讯：消息队列一般内置了高效的通信机制，因此也可以用在纯的消息通讯。



### 使用队列的一个实例：

**订单超时未支付自动关闭** 可以使用消息队列来完成。如 RocketMQ ，RabbitMQ 等消息队列支持延时消息。

延时消息的实现原理如下：**时间轮算法**

<img src="https://raw.githubusercontent.com/Gaytohub/notesPic/main/imgNotes/202201252328190.png" alt="image-20220125232825109" style="zoom:50%;" />

时间轮算法可以类比于时钟，如上图箭头（指针）按某一个方向按固定频率轮动，每一次跳动称为一个 tick。这样可以看出定时轮由个3个重要的属性参数，ticksPerWheel（一轮的 tick 数），tickDuration（一个 tick 的持续时间）以及 timeUnit（时间单位），例如当 ticksPerWheel = 60，tickDuration = 1，timeUnit = 秒，这就和现实中的始终的秒针走动完全类似了。

如果当前指针指在 1 上面，我有一个任务需要 4 秒以后执行，那么这个执行的线程回调或者消息将会被放在 5 上。那如果需要在20秒之后执行怎么办，由于这个环形结构槽数只到 8，如果要 20 秒，指针需要多转2圈。位置是在2圈之后的 5 上面（20 % 8 + 1）。

### RocketMQ 的持久化机制

涉及的核心角色：

1. CommitLog：消息真正的存储文件。
2. CommitQueue：消息消费的逻辑队列，类似数据库索引。
3. IndexFile：消息索引文件，主要存储 Key 和 offset 对应关系，提升消息检索速度。

刷盘时机和策略：

1. 异步刷盘（默认）：消息写入 PageCache 中，就立刻给客户端返回成功。当 PageCache 中消息累积到一定量时，触发写操作。
2. 同步刷盘：消息写入内存中的 PageCache 后，立即通知刷盘线程刷盘，然后等待刷盘完成，执行完成后唤醒等待线程，返回成功状态。

### RocketMQ 和 Kafka 的区别：

数据可靠性：RocketMQ 支持同步刷盘和异步刷盘。Kafka 仅支持异步刷盘。

性能对比：Kafka 性能较 RocketMQ 更好。

消费失败重试：Kafka 不支持消费失败重试机制，需要自己实现。RocketMQ 消费失败支持定时重试。

分布式事务消息：Kafka 不支持分布式事务消息。RocketMQ 支持。

单机支持的队列数：Kafka 在队列数超过 64 的时候，负载会明显增高。RocketMQ 单机最高 5w 队列，负载变化不明显。



### 分布式 CAP

C ：一致性，所有节点同一时间的数据应完全一致（每次写操作之后的读操作，都必须返回该值）。

A ：可用性，只要收到用户的请求，服务器就必须给出回应。（用户可以选择向分区1或是分区2发起读的操作。不管是哪个服务器收到了读的请求，就必须告诉用户，应当读取到的内容）。

P ：分区容错，分布式系统在某节点或网络分区故障的时候，仍能对外提供满足一致性和可用性的服务。



**CAP三性只能同时满足两个**

CA without P ：如果不要求 P（不允许分区，无异于单机），C（强一致性）和 A（可用性）是可以得到保证的。

CP without A ：如果不要求A（可用性），可以保证各分区之间强一致性，但会导致分区之间同步时间的无限延长（例如网络故障时的只读不写）。

AP without C ：放弃一致性，来保证节点的高可用性和分区容错。但是放弃一致性就导致了全局数据的不一致。



### 分布式 CAP 中的 C  和 数据库 ACID 中的 C 有区别吗？

分布式 CAP 中的 C 是指所有分区在同一时间的数据应当是完全一致的，也就是强一致性。而数据库中的 CAP 是指数据库中的数据应当是从某种一致性状态进入另一种一致性状态。



### AbstractQueuedSynchronizer （AQS）

AbstractQueuedSynchronizer：抽象的队列式同步器，AQS 定义了一套多线程访问共享资源的同步器框架，许多同步类实现都依赖于它，如常用的 ReentrantLock / Semaphore / CountDownLatch

**原理**：AQS 核心思想是，如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制 AQS 是使用队列锁实现的，即将暂时获取不到锁的线程加入到队列中。

**整体实现框架**：它维护了一个 volatile int state（代表共享资源，使用 volatile 修饰保证其在各个线程之间的可见性）和一个 FIFO 线程等待队列（多线程争用资源被阻塞时会加入此队列）。

有关 state 的访问方式有三种：getState()，setState()，和 compareAndSetState()。

#### AQS定义了两种资源共享的方式：

**Exclusive（独占）**：只有一个线程能执行 ，如 ReentrantLock，又可以分为公平锁和非公平锁。**公平锁**：按照线程在队列中的排队顺序，先到者先拿到锁。**非公平锁**：当线程要获取锁时，无视队列顺序直接去抢锁，抢不到锁再加入队列。

**Share（共享）**：多个线程能够同时执行，如 Semaphore / CoundownLatch。



不同的自定义同步器争用共享资源的方式也不同。**自定义同步器在实现时只需要实现共享资源 state 的获取与释放方式即可。** 自定义同步器实现时主要实现以下几种方法：

isHeldExclusive()：该线程是否正在独占资源，只有用到 condition 才需要实现它。

tryAcquire(int): 独占方式。尝试获取资源，成功返回true，失败返回false。

tryRelease(int): 独占方式。尝试释放资源，成功返回true，失败返回false。

tryAcquireShared(int): 共享方式。尝试获取资源。负数表示失败；0表示成功，但没有剩余可用资源；正数表示成功，且有剩余资源。

tryReleaseShared(int): 共享方式。尝试释放资源，如果释放后允许唤醒后续等待节点则返回true，否则返回false。



以 ReentrantLock 为例，state 初始化为 0，表示未锁定状态。A 线程 lock() 时，会调用 tryAcquire() 独占该锁并将 state += 1。此后，其他线程再 tryAcquire() 时就会失败，直到 A 线程 unlock() 到 state = 0（即释放锁），其他线程才有机会获取该锁。当然，释放锁之前，A 线程自己是可以重复获取此锁的（state 会累加，这就是可重入锁的概念）。但是要注意的是，获取多少次就要释放多少次，这样才能保证 state 是能回到 0 的。



以 CountDownLatch 为例，任务分为 N 个子线程去执行，state 也初始化为 N （注意 N 要与线程个数一致）。这 N个子线程是并行执行的，每个子线程执行完后 countDown() 一次，state 会以 CAS 的方式减1，等到所有子线程都执行完后 state = 0.



### 异常处理的时候，finally 代码块的重要性：

finally 是异常处理的一部分，用于释放资源。一般来说，finally 代码块中的代码一定会执行。

有时候，程序在 try 块中打开了一些物理资源（例如数据库连接、网络连接和磁盘文件等），这些物理资源必须显示回收。（Java 的垃圾回收机制不会回收任何物理资源，垃圾回收机制只能回收堆内存中对象占用的内存）。

为了保证一定能回收 try 块中打开的物理资源，异常处理机制提供了 finaly 块。不管 try 块中的代码是否出现异常，也不管哪个 catch 块被执行，finally 总是执行。
