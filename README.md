## Android 

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
 - Stack

### Intent: 在Activity, Service, Broadcast之间传递消息
   - 显式：指定组件类名
   - 隐式：指定操作名，如：ACTION_SEND 发送邮件，指定了此IntentFilter的Activity会被响应
   - 数据：如果是ACTION_SEND，则需要包含URI，格式为：<scheme>://<host>:<port>/<path> 比如 content://com:200/folder/etc
   - Extra：附加数据，比如Bundle
   - PendingIntent：用于Notification
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

### ContentProvider

### Service
 - [Service | IntentService](https://developer.android.com/guide/components/services)
   - Service is not in a seperate thread.
   - [When use Service/IntentService?](https://stackoverflow.com/questions/15524280/service-vs-intentservice)
   - non-sticky/sticky//////
   - AlarmManager

### Handler
  - [Handler definition](https://developer.android.com/reference/android/os/Handler)
  - [Communicate With UI thread](https://developer.android.com/reference/android/os/Handler)

### AsyncTask
  - [AsyncTask definition](https://developer.android.com/reference/android/os/AsyncTask)
  - [Service, IntentService, AsyncTask and Thread](https://marsic.info/2016/03/01/android-service-intent-asynctask-thread/)

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
