# 安卓
## APK编译流程
* Gradle
* APK内部结构
* 应用发布

## 应用架构
* MVVM
* MVP
* MVC

## 应用启动流程
* Zygote
* Application
* ActivityThread
* 主线程构建
* Activity生命周期
* AMS
* WMS
* 绘制流程
* 事件

## 线程通信
* Handler
* MessageQueue
* Message
* Looper
* AIDL的调用线程问题

## 进程通信
* AIDL
* Socket

## NDK
### NDK编译
* ndk-build
* cmake
* autocanf
* make
* ndk集成到自己的构建系统
### NDK API
* GLES
* opensl
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
### 调试
* Trace

## 问题排查
* ANR
* java crash
* native carsh
* 卡顿

## 第三方库及框架
* retrofit
* glide
* rxjava
* okhttp

## 安卓独有数据结构

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
* Hilt
* LifeCycle

