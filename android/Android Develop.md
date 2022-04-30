# 安卓
## 应用启动
* Zygote进程或fork的子进程都由脚本启动，init.zygote32_64.rc脚本使用了两个Service类型语句启动了两个Zygote进程
    ```
    service zygote /system/bin/app_process32 -Xzygote /system/bin --zygote --start-system-server --socket-name=zygote
    class main
    priority -20
    user root
    group root readproc reserved_disk
    socket zygote stream 660 root system
    onrestart write /sys/android_power/request_state     wake
    onrestart write /sys/power/state on
    onrestart restart audioserver
    onrestart restart cameraserver
    onrestart restart media
    onrestart restart netd
    onrestart restart wificond
    writepid /dev/cpuset/foreground/tasks 
    
    service zygote_secondary /system/bin/app_process64 -Xzygote /system/bin --zygote     --socket-name=zygote_secondary
    class main
    priority -20
    user root
    group root readproc reserved_disk
    socket zygote_secondary stream 660 root system
    onrestart restart zygote
    writepid /dev/cpuset/foreground/tasks
    ```
    
* Zygote进程和由它fork出来的子进程都会进入app_main.cpp的main函数中
    ```
    int main(int argc, char* const argv[]) {
    ...
    while (i < argc) {
        const char* arg = argv[i++];
        if (strcmp(arg, "--zygote") == 0) {
            zygote = true;
            niceName = ZYGOTE_NICE_NAME;
        } else if (strcmp(arg, "--start-system-server") == 0) {
            startSystemServer = true;
        } else if (strcmp(arg, "--application") == 0) {
            application = true;
        } else if (strncmp(arg, "--nice-name=", 12) == 0) {
            niceName.setTo(arg + 12);
        } else if (strncmp(arg, "--", 2) != 0) {
            className.setTo(arg);
            break;
        } else {
            --i;
            break;
        }
    }
    
    ...
    
    if (zygote) {
        runtime.start("com.android.internal.os.ZygoteInit", args, zygote);
    } else if (className) {
        runtime.start("com.android.internal.os.RuntimeInit", args, zygote);
    } else {
        fprintf(stderr, "Error: no class name or --zygote supplied.\n");
        app_usage();
        LOG_ALWAYS_FATAL("app_process: no class name or --zygote supplied.");
    }
    }
    ```
    ![](assets/16511124082433.jpg)
    
* startActivity启动过程
  ![安卓应用启动流程 ](assets/%E5%AE%89%E5%8D%93%E5%BA%94%E7%94%A8%E5%90%AF%E5%8A%A8%E6%B5%81%E7%A8%8B.jpg)

* AMS启动应用进程
  ![AMS启动进程流程图 -1-](assets/AMS%E5%90%AF%E5%8A%A8%E8%BF%9B%E7%A8%8B%E6%B5%81%E7%A8%8B%E5%9B%BE%20-1-.png)

* 应用启动流程
    * ApplicationThread
    * ActivityThread

    ![ActivityThread启动流程 -4-](assets/ActivityThread%E5%90%AF%E5%8A%A8%E6%B5%81%E7%A8%8B%20-4-.png)

## Activity生命周期

ApplicationThread调用scheduleThranaction,通过TransactionExecutor.execute(ClientTransaction transaction),executeLifecycleState执行后会自动补全中间的生命周期，launch Activity后会发送下个生命周期状态为resume，从onCreate到onResume中间的生命周期会补全

```java
public void execute(ClientTransaction transaction) {
  ……
  executeCallbacks(transaction);
  executeLifecycleState(transaction);
  ……
}
```



* WMS
* View绘制
* View事件

## 线程通信
* Handler
* MessageQueue
* Message
* Looper
* AIDL的调用线程问题

## 进程通信
* AIDL
* Socket

## APK编译流程
* Gradle
* APK内部结构
* 应用发布

## 应用架构
* MVVM
* MVP
* MVC
* 模块分层
* 模块隔离
* 模块通信

## NDK
### NDK编译
* ndk-build
* cmake
* autocanf
* make
* ndk集成到自己的构建系统
* JNI
  
### NDK API
* GLES
* OpenSL
* OpenMax
* ……
  
### NDK调试
* Sampleperf
* ndk-stack
* adb logcat
* trace
  
## 动态加载技术
* 热修复
* 插件化

## 优化
### 线程优化
* 通过合理控制线程能提升性能（Thread、Runable、Executors、HandlerThread、AsyncTask、ThreadPoolExecutor）
  
### 包体积优化

### 内存管理
* 内存泄漏问题（显示引用、隐示引用（内部类、匿名内部类））
  
### 电量优化
### 内存管理
### 问题排查
* ANR
* java crash
* native carsh
* 卡顿
  
### 调试
* Trace

## 第三方库及框架
* retrofit
* glide
* rxjava
* okhttp
  
## 安卓特有数据结构

### SparseArray
* int[] mKeys、Object[] mValues双数组数据结构
* 通过二分查找确定index位置
* 删除使用Delete标记字段
* put位置为Delete则覆盖，否则后移元素后插入value
* 如数组已满则创建新数组扩容，拷贝数据后再执行移动元素插入value操作,新数组长度size*2
* gc有Delete标记的数据，元素前移
  
### ArrayMap
* int[] mHashes、Object[] mArray双数组数据结构
* mArray长度为mHashes的2倍
* mArray[index<<1]存储Key，mArray[index<<1+1]存储Value
* put操作覆盖原值，如index位置key不一致则后移元素后插入Value，mHashes后移一位，mArray后移两位
* 如数组已满则进行扩容调用allocArrays(n)，然后调用freeArrays释放原数组，扩容数组长度为8、4分别使用缓存数组
* 扩容的旧数组先对所有Value元素置空，然后存放缓存数组
  
## androidx
* ViewModel、AndroidViewModel
* LiveData
* Hilt、Dagger
* LifeCycle
