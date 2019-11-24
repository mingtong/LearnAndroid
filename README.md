## Android 

### UI
 - View
    - Activity -> PhoneWindow -> DecorView -> TitleView -> ContentView
    - 绘制流程: Measure -> Layout -> Draw
 - Layout
    - LinearLayout
    - RelativeLayout
    - FrameLayout
    - PercentFrameLayout: 2015年 Percent Support Library 引入
    - TabLayout，Tab分组布局
 - ConstraintLayout
 - Fragment
 - ListFragment
 - ListView 网络加载卡顿的处理
 - ViewPager
 - RecyclerView
 - DialogFragment
 - Custom control
 - AppCompat
 - Theme, Style
 - Material design
 - Card
 - Float acting button，浮动操作按钮
 - Snackbar，带按钮的Toast提示
 - 事件传递机制
    - Activity: DispachTouchEvent -> OnTouchEvent 
    - ViewGroup: DispachTouchEvent -> onInterceptTouchEvent -> OnTouchEvent
    - View: DispachTouchEvent -> OnTouchEvent
 - 动画
    - FrameAnimation，帧动画，多张图片组成
    - TweenAnimation，补间动画，关键帧动画(指定开始和结束两个点)
    - PropertyAnimation，属性动画，改变View的属性
    - TransitionAnimation，过渡动画，
 
### Activity
 - Lifecycle: onCreate, onStart, onResume, onPause, onStop, onDestroy
 - (onStop)onSaveInstanceState -> Bundle -> onCreate(Restore)
 - launchMode(通过Manifest指定)
    - standard: 
    - singleTop: 如果目标activity在栈顶，则调用目标activity的onNewIntent，不启动新的activity
    - singleTask: 如果目标activity在当前task栈中，则把此activity上面的所有activity销毁。如果不存在于当前task栈中，则把activity创建到新的task栈中，如果目标activity已经单独在另一个task的栈中，则把另一个task全部栈带到当前task的栈顶
    - singleInstance: 如果目标activity不存在任何task栈中，则创建新的task栈，此栈中只有这个activity。如果存在，则复用。
 - launchMode(通过Intent指定)
    - NEW_TASK: 同singleTask
    - SINGLE_TOP：同singleTop
    - CLEAR_TOP：如果目标activity在当前task栈中，则把此activity上面的所有activity销毁。
    - NO_HISTORY

### Intent: 在Activity, Service, Broadcast之间传递消息
   - 显式：指定组件类名
   - 隐式：指定操作名，如：ACTION_SEND 发送邮件，指定了此IntentFilter的Activity会被响应
   - 数据：如果是ACTION_SEND，则需要包含URI，格式为：<scheme>://<host>:<port>/<path> 比如 content://com:200/folder/etc
   - Extra：附加数据，比如Bundle
   - PendingIntent：用于Notification

### Media 
 - Media Player
 - SoundPool
 - Camera
 - SurfaceView(TextureView)
 
### Localizatition
 - i18n
 
### Network
 - HTTP request
    - 长连接
    - 断点续传
 - HTTPS
 - TLS
 - TCP，有连接，基于流，三次握手，拥塞控制，流控制，来保证可靠的传输
    - 粘包：https://www.cnblogs.com/panchanggui/p/9748204.html
    - 分包：包头加包长度。
 - UDP：无连接，基于数据包，更在意延迟率，相比丢包率
 - RTP
 - RTSP
 - RTMP
 - WebSocket

### Storage
 - Serialization
 - JSON
 - SQLite
    - 升级的处理步骤
       - 将现有表命名为临时表
       - 创建新表
       - 将临时表导入新表
       - 删除临时表
 
### SDK version
 - Min SDK
 - Target SDK
 - Compile SDK

### Debug
 - ANR
     - 3个主要根源
        - Service: 20秒无响应  “serviceTimeout”
        - Broadcast: 10秒无响应
        - UI: 5秒无响应 “dispatchTimeout Wait queue”
     - 分析日志
        - LogCat，通过检索”am_anr”关键字，可以找到发生ANR的应用
        - main log，通过检索”ANR in“关键字，可以找到ANR的信息，日志的上下文会包含CPU的使用情况
        - dropbox，通过检索”anr”类型，可以找到ANR的信息
        - traces.txt, 发生ANR时，各进程的函数调用栈信息
     - 分析原因
        - 监测机制: 在启动服务、输入事件分发时，植入超时检测，用于发现ANR；
        - 报告机制: 当ANR被发现后，两个很重要的日志输出是：CPU使用情况和进程的函数调用栈，这两类日志是我们解决ANR问题的利器；
        - 监测ANR的核心原理是消息调度和超时处理
        - 只有被ANR监测的场景才会有ANR报告以及ANR提示框
     - 避免
        - StrictMode
        - BlockCanary
 - Crash
    - 捕获 Java Crash: 实现全局的UncaughtExceptionHandler接口
    - 捕获 NDK Crash: 
       - 捕捉软中断信号
       - Google Breakpad工具，写入minidump文件，用addr2line对应代码。
 - 内存 Leak
    - 原因
       - 内部类/匿名类无法释放（持有外部强引用）
       - 单例持有activity
       - 持有static的view引用
       - 持有static的Context引用
       - AsyncTask（当任务完成时没有取消，或者Activity结束时没有还完成任务）
       - 错误的重写了finalize
    - 解决办法
       - 使用static 内部类，弱引用
       - 考虑HandlerThread/IntentService/AsyncTask
       - 不单纯依赖GC
       - LeakCanary
### 性能优化
 - 布局优化
    - 不嵌套过多无用层
    - 避免过度绘制
    - 使用 include 重用 layout
    - 使用 ViewStub 延迟加载
    - merge标签减少层次
    - 用Style复用xml样式
 - 内存优化
    - 工具: Meomor Monitor, Profile
 - 图片优化
 - 电量优化
 - 网络优化
 
### Architecture
 - MVC
 - MVP
 - MVVM (Data Binding)
    - 数据绑定
    - 事件绑定
 - 依赖注入
    - ButterKnife，View注入
    - Dagger2
    - RoboGuice
 - AOP
    - AspectJ

### Boardcast
 - App可以接收到系统或者其他App的broadcast。
 - 类似 发布-订阅 模式(观察者模式)
 - 可以用于跨App的消息系统，也可以只在本App内发送。
 - 如果onReceive时间过长，会阻塞主线程，应该用[goAsync()去转成异步](https://developer.android.com/guide/components/broadcasts#effects-process-state)
 - 接收广播的几种方式：
    - 在 Manifest 文件中声明要接收的IntentFilter，实现 BroadcastReceiver子类的onReceive方法。
    - 创建 BroadcastReceiver 的对象，设置IntentFilter，并registerReceiver()。生命周期与context一样，比如App或者Activity。
    - 注册本地Receiver: LocalBroadcastManager.registerReceiver(BroadcastReceiver, IntentFilter)，只接收App内的广播。
 - 发送广播的几种方式:
    - sendOrderedBroadcast(Intent, String): 顺序发送广播。
    - sendBroadcast(Intent): 乱序发送广播。
    - LocalBroadcastManager.sendBroadcast(): App内发送广播。

### ContentProvider
 - 用于：跨应用分享数据；通过搜索框架提供定制的搜索建议。
 - 以一个或多个表的形式将数据呈现给其他应用。
 - ContentResolver: 获取数据
 - URI: 类似格式 content://user_dictionary/words

### [Service](https://developer.android.com/guide/components/services)
 - Service: 依附于主线程
    - startService
    - 通信方式
       - bindService: 先实现ServiceConnection，然后与Activity通信。注意：应该在onStart()中进行绑定，并在onStop()中解除绑定。以免多次绑定。
       - Messager: 可跨进程，在Service中使用Handler接收Message队列，在Activity发送Message
       - AIDL: 跨进程且多线程时，才考虑AIDL。不跨进程应该考虑用Binder，跨进程不多线程应该考虑用Messager。实现步骤：创建.aidl文件，实现接口，公开接口
 - IntentService: 是一个单独的线程
    - START_NOT_STICKY: 如果系统在 onStartCommand() 返回后终止服务，则除非有待传递的挂起 Intent，否则系统不会重建服务。
    - START_STICKY: 如果系统在 onStartCommand() 返回后终止服务，则其会重建服务并调用 onStartCommand()，但不会重新传递最后一个 Intent。
    - START_REDELIVER_INTENT: 如果系统在 onStartCommand() 返回后终止服务，则其会重建服务，并通过传递给服务的最后一个 Intent 调用 onStartCommand()。
    - 可以 Handle 前台的工作请求，并顺序执行(onHandleIntent)。
    - 可以通过用 LocalBroadcastManager.getInstance(this).sendBroadcast(localIntent); 向前台发送广播消息，前台receive广播。
 - [什么时候使用 Service/IntentService?](https://stackoverflow.com/questions/15524280/service-vs-intentservice)
    - Service适合短任务，长任务应该用Thread，而IntentService可以执行长任务但是不能直接与UI通信，需要用broadcast与UI通信。
    - Service会阻塞主线程，IntentService不能执行并行任务。
 - AlarmManager: 可以用来定时启动service(即使程序已经不在前台)。

### [Handler](https://developer.android.com/reference/android/os/Handler)
  - 两个作用:
     - 定时执行某个runnable或者任务: post(Runnable r), postAtTime(Runnable r, Time), postDelay(Runnable r, Time)
     - 在另一个线程顺序执行任务: sendMessage -> handleMessage
  - 四部分
     - Handler: 处理消息
     - Looper: 消息泵
     - MessageQueue: 消息队列
     - Message: 消息体
  - 子线程使用 Handler 应该考虑 HandlerThread（也有内置Looper）
  - 主线程创建多个Handler，对应一个Looper（一个线程只有一个），使用msg.target区分handler.
  - 什么时候用Handler.post(Runnable)? 想在UI线程执行操作的时候，比如改变UI控件的值。
  - 与UI线程通信的机制:
     - Handler
     - AsyncTask
     - Activity.runOnUIThread(Runnable)，在Runnable中写后台任务且可以操作UI控件。
     - View.Post(Runnable)，限定指定UI控件。
     - View.PostDelayed(Runnabe,long)，可以设制延时     

### [AsyncTask](https://developer.android.com/reference/android/os/AsyncTask)
  - 让你可以在后台执行任务，在UI线程发布结果，而不用操作多线程或者Handler，可以被取消。
  - 四个接口(依次执行)
     - onPreExecute(), 在UI线程执行，在任务开始前，一般用于准备任务，比如设置进度条画面
     - doInBackground(Params...)，必须有返回值，返回给onPostExecute(Result)，也可以执行publishProgress(Progress)把进度汇报给onProgressUpdate。
     - onProgressUpdate(Progress...)，在UI线程执行，一般用于显示进度。
     - onPostExecute(Result)，在UI线程执行，接收任务结果。
  - 应该用于短时的操作。
  - 内部是一个共享的静态的ThreadPoolExecutor 和一个 LinkedBlockingQueue。
  - 要注意屏幕旋转时的处理。
  - 如果activity结束了，AsyncTask并不会被取消，需要手动取消，否则会一直运行。
  - 执行网络请求时如果有异常，需要手动处理。
  - [什么时候用Service、IntentService,、AsyncTask、Thread](https://marsic.info/2016/03/01/android-service-intent-asynctask-thread/)
     - Service 像是一个没有UI的Activity
     - Thread 就是Thread，不能操作UI。
     - AsyncTask是一个智能线程，可以操作UI。
     
### Android IPC(进程间通信)
 - Broadcast
 - ContentProvider
 - Messager
 - AIDL
 - Binder: 高性能(只拷贝一次内存)，稳定(基于C/S架构)，安全(用户空间)
 
### 3rd party
 - EventBus: 事件总线
 - RxJava: 用于异步操作
 - Volly: 不适合大型下载或流式操作, 封装了 ImageLoader 
 - Retrofit: 基于OHttp, 解耦的更彻底，适合 RESTful
 - Todo-MVP
 - greenDAO
 - Glide
 - Fresco
 - ImageLoader
 
### 热更新
 - PathClassLoader: 加载安装后的apk文件，即data/app下的文件
 - DexClassLoader: 可以加载任意目录下的dex,jar,apk,zip等。
 - 原理: 利用PathClassLoader和DexClassLoader去加载与bug类同名的类，替换掉bug类，进而达到修复bug的目的，原理是在app打包的时候阻止类打上CLASS_ISPREVERIFIED标志，然后在热修复的时候动态改变BaseDexClassLoader对象间接引用的dexElements，替换掉旧的类。
 - tinker: Tencent开源框架 https://github.com/Tencent/tinker
 - Dexposed: Alibaba，
 - AndFix:  Alibaba，
 - Nuwa: QQ空间

### 插件化
 - 它将某个功能独立提取出来，独立开发，独立测试，再插入到主应用中。依次来较少主应用的规模。
 - 开源框架
    - 
### 安全
 - Prograd混淆
 - 加密
 - 签名
 - 证书
 - 敏感数据保存在gradle.properties中
### Gradle
 - [Gralde Build](https://developer.android.com/studio/build/index.html)

### CMake & NDK
 - [Difference](https://stackoverflow.com/questions/39589427/difference-between-cmake-and-ndk-build-in-android-studio-project)

### Extenral USB Camera
 - [Definition](https://source.android.com/devices/camera/external-usb-cameras)
 - [USB Host](https://developer.android.com/guide/topics/connectivity/usb/host)
 - [USB Manager](https://developer.android.com/reference/android/hardware/usb/UsbManager)
 - [UVC Camera](https://github.com/saki4510t/UVCCamera)
 - [Lib UVC](https://github.com/ktossell/libuvc)
 - [Lib USB](https://github.com/libusb/libusb)
 
### Android Framework
 - Android 源码编译，打包 
 - AMS, ActivityManageService
 - PMS, PackageManageService
 - WMS, WindowManageService
 - InputManagerService
 - Audio, AudioFlinger


### Java重点
 - 内部类
 - JVM Runtime 数据区
    - 程序计数器(Program Counter Register): 线程私有，当前线程的字节码的行号指示器。如果是Native方法，则计算器值是undefined.
    - JVM栈(JVM stack): 线程私有，存储局部变量，操作数栈，动态链接，方法出口等。
    - 本地方法栈(Native Method Stack): 为调用Native方法服务(JNI))。
    - Java堆(Java Heap): 存放所有的对象实例和数组，GC的主要管理区域。
    - 方法区(Method Area): 各线程共享区域，存储JVM加载的类，常量，静态变量，JIT代码。
 - StackOverflow: 线程申请的栈深度大于JVM允许的最大深度。
 - OutOfMemoryError: JVM在扩展栈时无法申请到足够的内存空间。
 - GC 对象是否该回收算法
    - 引用计数
    - 可达性分析: GC Root(JVM栈，方法区中的静态变量、常量，JNI对象) 不可达之处
 - GC 回收算法
    - 标记-清除(mark-swap): 容易产生碎片
    - 复制算法(copying): 回收新生代
    - 标记-整理(mark-compact):
    - 分代收集: 按新生代-老年代收集
 - GC 收集器
    - Serial 收集器: 老的单线程收集器
    - ParNew 收集器: Serial的多线程版本
    - Parallel 收集器: 并行的新生代收集器，使用复制算法。
    - Serial Old 收集器: 收集老年代。
    - Parallel Old 收集器: 并行收集老年代。
    - CMS 收集器: 最短时间停顿，基于标记-清除算法。
    - G1 收集器: 面向服务端
 - 类加载机制:  
    - 流程: 加载 -> 验证 -> 准备 -> 解析 -> 初始化 -> 使用 -> 卸载
 - 类加载器(Class loader)
    - Bootstrap ClassLoader
    - Extension ClassLoader
    - Application ClassLoader
 - 委派机制(Parents delegate): 加载类时，不会自己先加载，而是请求父类的加载器去完成，直到顶层，父类无法加载时，子加载器才会加载。
    - Hack此机制: 1，自己写findClass()。2，线程上下文类加载器通过setContextClassLoader()方法设置，如果创建线程时还没有设置，就从父线程中继承。
    - OSGI: 自定义类加载机制
 - 内存模型 JMM
 - 泛型 类型擦除
 - 反射及动态代理
 - JVM优化
 - 4种引用
    - 强引用，GC不回收被引用的对象
    - 软引用，OOM前GC回收，
    - 弱引用，GC一定回收
    - 虚引用，GC回收时收到通知
 - JNI 回收
    - Local reference, （gc不会回收) native方法return时，会被自动释放
    - global reference，（gc不会回收)，需要手动释放，跨线程，跨方法调用 NewGlobalRef, deleteLocalRef
    - Weak reference，（gc会回收)  DeleteGlobalWeakRef
 - JNI 异常处理
    - ExceptionOccurred(): 判断是否发生异常
    - ExceptionClear(): 用来清理当前JNI层中发生的异常
    - ThrowNew(): 向Java层抛出异常
 - JNI 与Java通信
    - GetObjectClass() -> GetMethodID() -> CallVoidMethod()
    - GetObjectClass() -> GetStaticMethodID() -> CallStaticVoidMethod()
    - GetObjectClass() -> GetFieldID() -> GetObjectField()
    - GetObjectClass() -> GetStaticFieldID() -> GetStaticObjectField()

#### Multithread safe
 - synchronized: 保证代码块原子操作，其他线程阻塞，同线程可重入
 - volatile: 保证变量对其他线程可见（以便让其他线程读取一最新的值），防止编译器（优化）重排列指令
 - ThreadLocal: 线程封闭，防止共享singleton对象或全局对象
 - final: 使对象的状态保持不变
 - static 初始化对象
 - ConcurrentHashmap: Java5 引入
 - CopyOnWriteArrayList: 写入时复制
 - Deque: Java6 引入
 - FutureTask: 闭锁的一种实现，表示异步任务，可以用new Thread()调用
 - Semaphore: 信号量
 - BlockingDeque： Java6 引入
 - Lock: 抽象类 Lock.lock(); Lock.unlock();
 - ReentrantLock: 实现了Lock接口。公平锁(按照请求的顺序来获得锁)，可中断，可轮询(tryLock())
 - ReadWriteLock: 读写锁。允许多个读操作同时进行，但每次只能一个写操作。

#### Concurrent
 - Executor: thread pool

#### Dead Lock
 - 4 conditions
    - mutual exclusion. 互斥
    - hold and wait or partial allocation. 请求与保持
    - no pre-emption. 不可剥夺
    - esource waiting or circular wait. 循环等待
