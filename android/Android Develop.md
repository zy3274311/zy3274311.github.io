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
* mKeys、mValues双数组数据结构
* 通过二分查找确定index位置
* 删除使用Delete标记字段
* put位置为Delete则覆盖，否则后移元素后插入value
* 如数组已满则扩容再执行移动元素插入value操作,扩容size*2
* gc有Delete标记的数据，元素前移
### ArrayMap
