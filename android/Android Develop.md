# 安卓
## APK编译流程

## 应用架构

## 应用启动流程

## 应用分发

## AMS

## WMS

## 绘制流程

## 事件

## 线程通信

## 进程通信
* AIDL
* Socket

## NDK

## 动态加载技术
* 热修复
* 插件化

## 优化
* 性能优化
* 包体积优化
* 内存优化

## 问题排查
* ANR
* java crash
* native carsh
* 卡顿

## 第三方库及框架
* retrofit

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

