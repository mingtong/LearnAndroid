## Android 

### UI
 - Layout
 - ConstraintLayout
 - Fragment
 - ListFragment
 - ListView
 - ViewPager
 - RecyclerView
 - DialogFragment
 - Custom control
 - AppCompat
 - Theme, Style
 - Animation
 - Material design
 - Card
 - Float acting button
 - snackbar
 
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
 - TCP/UDP

### Storage
 - Serialization
 - JSON
 - SQLite
 
### SDK version
 - Min SDK
 - Target SDK
 - Compile SDK

### Debug
 - ANR
  - Reason
  - Analyze log
 - Crash

### Architecture
 - MVC
 - MVP
 - MVVM (Data Binding)

### Boardcast
 - App可以接收到系统或者其他App的broadcast。
 - 类似 发布-订阅 模式(观察者模式)
 - 可以用于跨App的消息系统，也可以只在本App内发送。
 - 如果onReceive时间过长，会阻塞主线程，应该用[goAsync()去转成异步]https://developer.android.com/guide/components/broadcasts#effects-process-state。
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

### 3rd party
 - EventBus
 - RxJava

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

### Java

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
 - 
