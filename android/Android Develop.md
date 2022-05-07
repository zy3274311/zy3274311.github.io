# 安卓
## 应用启动
* Zygote进程或fork的子进程都由脚本启动，init.zygote32_64.rc脚本使用了两个Service类型语句启动了两个Zygote进程
    ```shell
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
    ```java
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
  ![安卓应用启动流程](/Users/zhangying/Desktop/workspace/zy3274311.github.io/android/assets/安卓应用启动流程.jpg)

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
  
## 动态加载

### ClassLoader

- loadClass

  ```java
  protected Class<?> loadClass(String name, boolean resolve)
          throws ClassNotFoundException {
      // First, check if the class has already been loaded
      Class<?> c = findLoadedClass(name);
      if (c == null) {
          try {
              if (parent != null) {
                  c = parent.loadClass(name, false);
              } else {
                  c = findBootstrapClassOrNull(name);
              }
          } catch (ClassNotFoundException e) {
              // ClassNotFoundException thrown if class not found
              // from the non-null parent class loader
          }
  
          if (c == null) {
              // If still not found, then invoke findClass in order
              // to find the class.
              c = findClass(name);
          }
      }
      return c;
  }
  ```

- findLoadedClass

  ```java
  protected final Class<?> findLoadedClass(String name) {
      ClassLoader loader;
      if (this == BootClassLoader.getInstance())
          loader = null;
      else
          loader = this;
      return VMClassLoader.findLoadedClass(loader, name);
  }
  ```

### BootClassLoader

```java
class BootClassLoader extends ClassLoader
```

- system classloader

  ```java
  private static ClassLoader createSystemClassLoader() {
      String classPath = System.getProperty("java.class.path", ".");
      String librarySearchPath = System.getProperty("java.library.path", "");
      ……
      return new PathClassLoader(classPath, librarySearchPath, BootClassLoader.getInstance());
  }
  ```

- loadClass	

  ```java
  @Override
  protected Class<?> loadClass(String className, boolean resolve)
    throws ClassNotFoundException {
      Class<?> clazz = findLoadedClass(className);
      if (clazz == null) {
          clazz = findClass(className);
      }
      return clazz;
  }
  ```

- findClass

  ```java
  @Override
  protected Class<?> findClass(String name) throws ClassNotFoundException {
      return Class.classForName(name, false, null);
  }
  ```

### BaseDexClassLoader

```java
public class BaseDexClassLoader extends ClassLoader
```

```java
public BaseDexClassLoader(String dexPath,
                          String librarySearchPath, ClassLoader parent, ClassLoader[] sharedLibraryLoaders,
                          ClassLoader[] sharedLibraryLoadersAfter,
                          boolean isTrusted) {
    super(parent);
    // Setup shared libraries before creating the path list. ART relies on the class loader
    // hierarchy being finalized before loading dex files.
    this.sharedLibraryLoaders = sharedLibraryLoaders == null
      ? null
      : Arrays.copyOf(sharedLibraryLoaders, sharedLibraryLoaders.length);
    this.pathList = new DexPathList(this, dexPath, librarySearchPath, null, isTrusted);
    this.sharedLibraryLoadersAfter = sharedLibraryLoadersAfter == null
      ? null
      : Arrays.copyOf(sharedLibraryLoadersAfter, sharedLibraryLoadersAfter.length);
    // Run background verification after having set 'pathList'.
    this.pathList.maybeRunBackgroundVerification(this);
    reportClassLoaderChain();
}
```

- findClass

  ```java
  @Override
  protected Class<?> findClass(String name) throws ClassNotFoundException {
      // First, check whether the class is present in our shared libraries.
      if (sharedLibraryLoaders != null) {
          for (ClassLoader loader : sharedLibraryLoaders) {
              try {
                  return loader.loadClass(name);
              } catch (ClassNotFoundException ignored) {
              }
          }
      }
      // Check whether the class in question is present in the dexPath that
      // this classloader operates on.
      List<Throwable> suppressedExceptions = new ArrayList<Throwable>();
      Class c = pathList.findClass(name, suppressedExceptions);
      if (c != null) {
          return c;
      }
      // Now, check whether the class is present in the "after" shared libraries.
      if (sharedLibraryLoadersAfter != null) {
          for (ClassLoader loader : sharedLibraryLoadersAfter) {
              try {
                  return loader.loadClass(name);
              } catch (ClassNotFoundException ignored) {
              }
          }
      }
      if (c == null) {
          ClassNotFoundException cnfe = new ClassNotFoundException(
                  "Didn't find class \"" + name + "\" on path: " + pathList);
          for (Throwable t : suppressedExceptions) {
              cnfe.addSuppressed(t);
          }
          throw cnfe;
      }
      return c;
  }
  ```

### DexPathList

```java
DexPathList(ClassLoader definingContext, String dexPath,
            String librarySearchPath, File optimizedDirectory, boolean isTrusted) {
    if (definingContext == null) {
        throw new NullPointerException("definingContext == null");
    }
    if (dexPath == null) {
        throw new NullPointerException("dexPath == null");
    }
    if (optimizedDirectory != null) {
        if (!optimizedDirectory.exists()) {
            throw new IllegalArgumentException(
                    "optimizedDirectory doesn't exist: "
                            + optimizedDirectory);
        }
        if (!(optimizedDirectory.canRead()
                && optimizedDirectory.canWrite())) {
            throw new IllegalArgumentException(
                    "optimizedDirectory not readable/writable: "
                            + optimizedDirectory);
        }
    }
    this.definingContext = definingContext;
    ArrayList<IOException> suppressedExceptions = new ArrayList<IOException>();
    // save dexPath for BaseDexClassLoader
    this.dexElements = makeDexElements(splitDexPath(dexPath), optimizedDirectory,
            suppressedExceptions, definingContext, isTrusted);
    // Native libraries may exist in both the system and
    // application library paths, and we use this search order:
    //
    //   1. This class loader's library path for application libraries (librarySearchPath):
    //   1.1. Native library directories
    //   1.2. Path to libraries in apk-files
    //   2. The VM's library path from the system property for system libraries
    //      also known as java.library.path
    //
    // This order was reversed prior to Gingerbread; see http://b/2933456.
    this.nativeLibraryDirectories = splitPaths(librarySearchPath, false);
    this.systemNativeLibraryDirectories =
            splitPaths(System.getProperty("java.library.path"), true);
    this.nativeLibraryPathElements = makePathElements(getAllNativeLibraryDirectories());
    if (suppressedExceptions.size() > 0) {
        this.dexElementsSuppressedExceptions =
                suppressedExceptions.toArray(new IOException[suppressedExceptions.size()]);
    } else {
        dexElementsSuppressedExceptions = null;
    }
}
```

- findClass

  ```java
  public Class<?> findClass(String name, List<Throwable> suppressed) {
      for (Element element : dexElements) {
          Class<?> clazz = element.findClass(name, definingContext, suppressed);
          if (clazz != null) {
              return clazz;
          }
      }
      if (dexElementsSuppressedExceptions != null) {
          suppressed.addAll(Arrays.asList(dexElementsSuppressedExceptions));
      }
      return null;
  }
  ```

### 热修复

### 插件化

## 优化
### 线程优化
* 通过合理控制线程能提升性能（Thread、Runable、Executors、HandlerThread、AsyncTask、ThreadPoolExecutor）
  
### 包体积优化

### 内存管理
* 内存泄漏问题（显示引用、隐示引用（内部类、匿名内部类））
  
### 电量优化
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
