###Activity
####一 activity生命周期
* activity4中状态
	* running paused stopped(被完全覆盖 不可见 部分成员变量存在) killed 
* activity生命周期
	* Activity启动->onCreate->onStart(可见)->onResume(可见 用户可以操作)
	* 点Home回到主界面(Activity不可见)->onPause->onStop
	* 再次回到原Activity->onRestart(不可见->可见)->onStart->onResume
	* 退出当前Activity->onPause->onStop->onDestroy
* android进程优先级
	* 前台
	* 可见
	* 服务
	* 后台
	* 空
	
####二 android任务栈
####三 activity启动模式
* standard
* singletop栈顶复用
* singletask栈内复用 提到栈顶(此Activity) 调用onNewInstance
* singleinstance 独享一个栈

####四 scheme协议
###Fragment
####一 Fragment为什么被称为第五大组件
* 使用频率高
* 有自己的生命周期
* 动态灵活加载到activity中
	* 依附activity
	
##### Fragment加载到Activity的两种方式
* 添加Fragment到Activity布局文件中
* 动态在activity中添加fragment

##### FragmentPagerAdapter和FragmentStatePagerAdapter区别
* FragmentPagerAdapter页面较少  销毁时调用detach UI分离
* FragmentStatePagerAdapter页面较多 销毁时调用remove 回收内存

####Fragment生命周期
####Fragment通信
* 在Fragment中调用Activity的方法 getActivity()
* 在Activity中调用Fragment中方法 接口回调
* 在Fragment中调用Fragment中方法 findFragmentById
####Fragment的replace、add、remove

###Service
####一 service的应用场景，以及和Thread区别
#####Service是什么
Service是一个一种可以在后台执行长时间运行操作而没有用户界面的应用组件。
运行在主线程（不能执行耗时操作）
#####service和Thread的区别
* 定义 service运行在主线程 Thread相对独立
* 实际开发 Thread执行耗时操作
* 应用场景 Service后台播放音乐 天气预报通知 数据统计

####二 开启service的两种方式以及区别
#####1 startService 
通过startService方法启动Service会回调onStartCommand

* 定义一个类继承Service
* 在Manifest.xml文件中配置该Service
* 使用Context的startService(Intent)方法启动该Service
* 不再使用时，调用stopService(Intent)方法停止该Service

#####2 bindService
* 创建BindService服务端，继承自Service并在类中，创建一个实现IBinder接口的实例对象并提供公共方法给客户端调用
* 从onBind()回调方法返回次Binder实例
* 在客户端中，从onServiceConnected()回调方法接收Binder并使用提供的方法调用绑定服务

### Broadcast receiver
#### 广播
##### 广播的定义
在Android中，Broadcast是一种广泛运用的在应用程序之间传输信息的机制，Android中我们要发送的广播是一个Intent，这个Intent中可以携带我们要传送的数据

##### 广播的使用场景
* 同一app具有多个进程的不同组件之间的消息通信
* 不同app之间的组件之间消息通信

##### 广播种类
* Normal Broadcast：Context.sendBroadcast
* System Broadcast: Context.sendOrderedBaoadcast
* Local Broadcast: 只在自身app内传播

#### 实现广播-receiver
#####静态注册
* 注册完成就一直运行

#####动态注册
* 跟随activity的生命周期 在onDestroy解绑广播

#### 内部实现机制
* 自定义广播接受者BroadcastReceiver,并复写onReceive()方法
* 通过Binder机制向AMS(Activity Manger Service)进行注册
* 广播发送者通过Binder机制向AMS发送广播
* AMS查找符合相应条件(IntentFilter/Permission等)的BroadcastReceiver，将广播发送到Broadcast(一般情况下是Activity)相应的消息循环队列中
* 消息循环执行拿到此广播，回调BroadcastReceiver中的onReceive()方法

#### LocalBroadcastManger详解
* 使用它发送的广播将只在自身app内传播，因此你不必担心泄露隐私数据
* 其他app无法对你的app发送该广播，因为你的app根本就不可能接收到非自身应用发送的的该广播
* 比系统的全局广播更加高效

##### 源码
* LocalBroadcastManger高效的原因主要是因为它内部通过Handler实现的，它的sendBroadcast()方法含义并非和我们平时所用的一样,它的sendBroadcast()方法其实是通过Handler发送一个Message实现的
* 既然是它内部通过Handler来实现广播的发送的，那么相比系统广播通过Binder实现那肯定是更高效了，同时使用Handler来实现，别的应用无法向我们的应用发送该广播，而我们应用发送的广播也不会离开我们的应用
* LocalBroadcastManger内部协作主要是靠这两个Map集合：mReceivers和mActions，当然还有一个List集合mPendingBroadcasts，这个主要是存储待接收的广播对象

### Webview
#### Webview常见的一些坑
* Android API level 16以及之前的版本存在远程代码执行安全漏洞，该漏洞源于程序没有正确限制使用WebView.addJavascriptInterface方法，远程攻击者可通过使用Java Reflection API利用该漏洞执行任意Java对象的方法
* webview在布局文件的使用：webview写在其他容器时 销毁webview时候 先移除容器中的webview 在调用webview.removeAllView destroy
* jsbridge 
* webviewClient.onPageFinished->webChromeClient.onProgressChanged
* 后台耗电 onDestroy 调用System.exit(0);
* webview硬件加速导致页面渲染问题 android3.0开始  解决办法 关闭硬件加速

#### 关于webview的内存泄漏问题
* 独立进程，简单暴力，不过可能涉及到进程间通信
* 动态添加webview，对传入webview中使用的context使用弱引用，动态添加webview意思在布局创建个ViewGroup用来放置webview，Activity创建时add进来，在Activity停止时remove掉

### Binder
#### Linux内核基础知识
* 进程隔离/虚拟地址空间
* 系统调用 
* binder驱动
#### Binder通信机制
##### 为什么使用Binder
* Android使用的Linux内核拥有着很多的跨进程通信机制
* 性能
* 安全

##### Binder通信模型
* 通信录：binder驱动

##### 到底什么是binder
* 通常意义下，Binder是一种通信机制
* 对于Service进程来说，Binder指的是Binder本地对象/对与Client来说，Binder指的是Binder代理对象
* 对于传输过程而言，Binder是可以跨进程传递的对象
#### AIDL
### Handler
#### 什么是Handler
handler通过发送和处理Message和Runnable对象来关联对应线程的MessageQueue
* 可以让对应的Message和Runnable在未来的某个时间点进行相应处理
* 让自己想要处理的耗时操作放在子线程，让更新ui的操作放在主线程

#### Handler的使用方法
* post(runnable) 底层还是调用sendMessage
* sendMessage(message)

#### Handler机制的原理
#### Handler引起的内存泄漏以及解决办法
* 原因：静态内部类持有外部类的匿名引用，导致外部activity无法释放
	* handler内部持有外部activity的弱引用
	* 把handler改为static内部类
	* onDestroy时候mHandler.removeCallback()

### AsyncTask
#### 什么是AsyncTask
它本质上就是一个封装了线程池和handler的异步框架
#### AsyncTask的使用方法
* 3个参数
	* Integer 
	* Integer 进度
	* String result
* 5个方法

#### AsyncTask机制原理
* AsyncTask的本质是一个静态的线程池，AsyncTask派生出的子类可以实现不同的异步任务，这些任务都是提交到静态的线程池中执行
* 线程池中的工作线程执行doInBackground(mParams)方法执行异步任务
* 当任务状态改变之后，工作线程会向UI线程发送消息，AsyncTask内部的InternalHandler响应这些消息，并调用相关的回调函数
#### AsyncTask注意事项
* 内存泄漏
	* 非静态匿名内部类持有外部引用
* 生命周期
	* 在onDestroy 中销毁AsyncTask
* 结果丢失
	* 屏幕旋转等导致activity被重新创建 导致AsyncTask的引用无效
* 并行or串行

### handlerThread
#### handlerThread是什么
handler+thread+looper
是一个thread 内部有looper

* 本质上是一个线程类，继承了Thread
* 有自己的内部Looper对象，可以进行looper循环 
* 通过获取HandlerThread的looper对象传递给Handler对象，可以在handlerMessage方法中执行异步任务
* 优点是不会有堵塞，减少了对性能的消耗，缺点是不能同时进行多任务的处理，需要等待进行处理，处理效率低
* 与线程池注重并发不同，HandlerThread是一个串行队列，HandlerThread背后只有一个线程
	
#### handlerThread源码  

