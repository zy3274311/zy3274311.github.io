# OpenHarmony、HarmonyOS、HarmonyOS NEXT的关系

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=MWE3MTExZGY1MzEyYTIwOWM1MzUzMTc1NWRhMGZmMTlfaFBSd2dUSjlWSjNDRko1ekYybDdxYTlDOUcwR0VQQ01fVG9rZW46U0RPRWJQaEFjb2s4d2N4anNEZWNaY0hzbnNkXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

# 开发准备

- 申请开发者账号
- 申请开发者白名单
- 搭建开发环境：DevEco-Studio、模拟器
- 入门学习
- 获取能力证书

参考文档：[Harmony快速入门](https://nio.feishu.cn/docx/LMEJdQvK7oGf0sxSpKccn02Gnbb)

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=MDc0NTQyMzNhZWNkMzBjZWY5YzBmMjU4MGE0MDdmYTNfTHFmZHVOdk45bk00dm4zRmpKYk0wUGxYbTF0UERlcHBfVG9rZW46Szc1d2JKZURrb1BpRGx4bmZXTWMwUGlzbktnXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

# DevEco Studio

面向HarmonyOS应用/元服务开发者提供的集成开发环境(IDE)，提供环境配置、开发、运行、编译、调试、测试、发布功能

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2M5NDEzOTY5MTA5MzZlZTRkMjRkZTFlNWIzYTIzYjVfUm1ONVNqT3h4NGFVbnJRSDJCUWVKN0VDbzcyWVlvVm1fVG9rZW46UGhZSWJGTXd4b1pCZk94Z2RDVGNaZFZ0bnFlXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

参考文档：https://developer.huawei.com/consumer/cn/deveco-studio/

# 应用架构

## 应用包目录

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=MGNkY2M2OTI5MjhkNDFiNzYzZDNlMjZiODE5ZjRjNTFfUzlieXhKZWFGUmNVV1VqYXVxbGppaUo4cUpvRm03bURfVG9rZW46QzZVUWJlOHNNbzA2REx4cEZKMmM3VThTbmhiXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

## 资源访问

```XML
resources
|---base
|   |---element
|   |   |---string.json
|   |---media
|   |   |---icon.png
|   |---profile
|   |   |---test_profile.json
|---en_US  // 默认存在的目录，设备语言环境是美式英文时，优先匹配此目录下资源
|   |---element
|   |   |---string.json
|   |---media
|   |   |---icon.png
|   |---profile
|   |   |---test_profile.json
|---zh_CN  // 默认存在的目录，设备语言环境是简体中文时，优先匹配此目录下资源
|   |---element
|   |   |---string.json
|   |---media
|   |   |---icon.png
|   |---profile
|   |   |---test_profile.json
|---en_GB-vertical-car-mdpi // 自定义限定词目录示例，由开发者创建
|   |---element
|   |   |---string.json
|   |---media
|   |   |---icon.png
|   |---profile
|   |   |---test_profile.json
|---rawfile // 其他类型文件，原始文件形式保存，不会被集成到resources.index文件中。文件名可自定义。
```

## Stage模型

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=OTBkNWJkYmIwOTEyNTM1NDJhNWZlMWI3ZDBkMDc3M2VfZlZhRWZrdW5wVDI2UEJkMjR4MUk5WDNRWkhnMm1GVGtfVG9rZW46UmR2S2JORjI0b3BIT2V4Qm1XTWNDeW02blNlXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

## 应用包结构

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=MTM0OTY5NDczYzM5ZWY0NDMyMGViOWY5NTc1M2ExNDVfV29ZQkNaQnptV1NJeE9Eb1JJNUtZYjhqc2ZsbEZERVVfVG9rZW46Uk1CVGJ4b3Znbzg5M2Z4VXdLU2NWblJubmxmXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

## 多hap机制

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=ZWJlMWZmOWU2ZTI0MmUwYTdhYzA1ZjhjNmQxY2E3ZjRfVjlsMUZrbUhTR1dHSFRpTnppQ1NTUXhMT2pDR1owNjhfVG9rZW46R3RyMGJVdlZDb2k0TWR4VkN3N2NXOWU0blRmXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

## 共享包

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=OGI5NDk2ZDkxMzM3N2YyYjlmZjE0ZGRkMzYyMzYxYTFfSG53bDl2TjViUHFNOUt4cGtZaGg3NTFCZTc5dml6SVlfVG9rZW46UXVrVWI1U245b0xtTmd4NDVZMGM0SUZSbkNiXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

# ArkTS

- ArkTS提供了声明式UI范式、状态管理支持等相应的能力，让开发者可以以更简洁、更自然的方式开发应用。
- 同时，它在保持TypeScript（简称TS）基本语法风格的基础上，进一步通过规范强化静态检查和分析，使得在程序运行之前的开发期能检测更多错误，提升代码健壮性，并实现更好的运行性能。
- 针对JavaScript（简称JS）/TS并发能力支持有限的问题，ArkTS对并发编程API和能力进行了增强。
- ArkTS支持与JS/TS高效互操作，兼容JS/TS生态。

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=NjZhNWY4MzUxN2Q3NmVkNWJiMWJlZWQyMmJlNDlmYmFfQ1RwZFVrUTluZlNJaVFWTTd3Q1RyR1NXUXRxcGx6VVNfVG9rZW46VlpIM2J0Vmtlb2w5Nm94V1JZbWNwUzQ0bnJkXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

## ArkTS**相比**TS**特性差异**

- 不支持在运行时更改对象布局
- 对象字面量须标注类型
- 不支持structural typing

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=ZTM2MmE3NWUwMzYxNDVhNTYwN2YwNGUyZDg0ZTUyY2NfSGd4dFZlMDZrUDFQRnY0MmpnT09jU0pxTjBWbkJ6UGVfVG9rZW46WUpvOWJnaVhBbzJUOEN4T1RNQ2NUNGtjbmhjXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

## 基础类库

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=NTY4Mzc1OWI4YTMxYzdlNTY0MWZlMDZjMGM4ZTQyNDFfbVJYenBhQTJSUkNGQnU0amdQamRSOWxvSTY4cFNRb3pfVG9rZW46UkZoc2JEeW1Eb01vZW94bkdkYWNtUkY5bk1nXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

### 并发机制

ArkCompiler运行时在HarmonyOS上提供了Taskpool和Worker支持并发编程。在运行时实例内存隔离的基础上，ArkCompiler通过共享运行实例中的不可变或者不易变的对象、内建代码块、方法字节码等技术手段，优化了并发运行实例的启动性能和内存开销。

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=ODRkM2UwOWIyYjllMTJjMmFhMmJmMDQ3YjA2ODg1ZmZfdE5ycUYyYjREMGlnM01rUDdBZ2o5amx5azAwWlJmQzBfVG9rZW46RTYxWmJONVpLb2Q1Y0p4bkptdGNYcEhNbkFiXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

# ArkUI

ArkUI是一套构建分布式应用界面的声明式UI开发框架。它使用简洁的UI信息语法、丰富的UI组件、以及实时界面预览工具，帮助您提升HarmonyOS应用界面开发效率30%。您只需使用一套ArkTS API，就能在多个HarmonyOS设备上提供生动而流畅的用户界面体验。

## 基本概念

- **UI****：**即用户界面。开发者可以将应用的用户界面设计为多个功能页面，每个页面进行单独的文件管理，并通过页面路由API完成页面间的调度管理如跳转、回退等操作，以实现应用内的功能解耦。
- **组件****：**UI构建与显示的最小单位，如列表、网格、按钮、单选框、进度条、文本等。开发者通过多种组件的组合，构建出满足自身应用诉求的完整界面。

## 开发范式

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=N2U4OWM3NTViZDM3MDZhMjk1MzI1ODU0OGY5MDkyYzJfNHZldEZDaTJoVm00VVI0Wk1XRm5aQUY4Y3pQc1JPM2RfVG9rZW46QkxPY2JDZFlsb2FIY3B4dTNWWmNyOTlubmhnXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

- **声明式开发范式**：采用基于TypeScript声明式UI语法扩展而来的ArkTS语言，从组件、动画和状态管理三个维度提供UI绘制能力。
- **类Web开发范式**：采用经典的HML、CSS、JavaScript三段式开发方式，即使用HML标签文件搭建布局、使用CSS文件描述样式、使用JavaScript文件处理逻辑。该范式更符合于Web前端开发者的使用习惯，便于快速将已有的Web应用改造成方舟开发框架应用。参考[兼容JS的类Web开发范式API](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V2/js-components-common-attributes-0000001427744868-V2)文档
  - ![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=MTFlZDdhNjk4ZWVlYmEyMGU5MjY5OWU2OGEwOTViYzRfYnp6UkszUE15Sk5MbDROTmJLWFo2N3lOOGl6Nk9yTDdfVG9rZW46UmltdWJ2Q2hSb3NuQ1h4VE5TZWNKemRlbm9jXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

## 声明式UI

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=NTZmZjE5MDZkZDI0NzI5ZWUzNzdmNTExYzk4MWZkMmJfZXk1bENRa01OQllkU0liUG5DMUdGanBvWjJhblFEVmVfVG9rZW46WTFBaWJkMnpWb3lpU2V4bkI4UmN2eUsxbnJkXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

- 装饰器： 用于装饰类、结构、方法以及变量，并赋予其特殊的含义。如上述示例中@Entry、@Component和@State都是装饰器，@Component表示自定义组件，@Entry表示该自定义组件为入口组件，@State表示组件中的状态变量，状态变量变化会触发UI刷新。
- UI描述：以声明式的方式来描述UI的结构，例如build()方法中的代码块。
- 自定义组件：可复用的UI单元，可组合其他组件，如上述被@Component装饰的struct Hello。
- 系统组件：ArkUI框架中默认内置的基础和容器组件，可直接被开发者调用，比如示例中的Column、Text、Divider、Button。
- 属性方法：组件可以通过链式调用配置多项属性，如fontSize()、width()、height()、backgroundColor()等。
- 事件方法：组件可以通过链式调用设置多个事件的响应逻辑，如跟随在Button后面的onClick()。
- 系统组件、属性方法、事件方法具体使用可参考基于ArkTS的声明式开发范式。

除此之外，ArkTS扩展了多种语法范式来使开发更加便捷：

- @Builder/@BuilderParam：特殊的封装UI描述的方法，细粒度的封装和复用UI描述。
- @Extend/@Styles：扩展内置组件和封装属性样式，更灵活地组合内置组件。
- stateStyles：多态样式，可以依据组件的内部状态的不同，设置不同样式。

## stage模型

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=MmFlMTUyNDEyODQ0ZGIxM2VkMzA2MThkZDJmY2U1ZTJfakVOYVJhaFdCOGNaUzRYNkJtZThPRW9xT3BBSkpIVGxfVG9rZW46V2YzdGJsMk1Vb2RuRE54UnIwQWNQaUZYbkZlXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

## 生命周期

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=OWViZGU3ODBlZDRlNzMyNGI4OWU0YzkxNWZmMzcxNGVfcnRCUERyM0pQWHJuT25SaU5KNHZqNUhqT3V1b25rWm1fVG9rZW46UEtGbWJZUG50b0tkcUt4RU93RWNkY2Q0bk9jXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

### 页面生命周期

即被@Entry装饰的组件生命周期，提供以下生命周期接口：

- onPageShow：页面每次显示时触发一次，包括路由过程、应用进入前台等场景。
- onPageHide：页面每次隐藏时触发一次，包括路由过程、应用进入后台等场景。
- onBackPress：当用户点击返回按钮时触发。

### 组件生命周期

一般用@Component装饰的自定义组件的生命周期，提供以下生命周期接口：

- aboutToAppear：组件即将出现时回调该接口，具体时机为在创建自定义组件的新实例后，在执行其build()函数之前执行。
- aboutToDisappear：在自定义组件析构销毁之前执行。不允许在aboutToDisappear函数中改变状态变量，特别是@Link变量的修改可能会导致应用程序行为不稳定。

## 状态管理

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=MDA0ZWQ1Yzc3MmYwYjY1MGYxNDgyZTQ5Y2Q4YmZkMzFfSENqQVRoZG5WcVlFRDN0RnlQaWlHMFBDdEZBVFhNU1hfVG9rZW46RDliRmJpZmhjb0RTcG14dUpLRGNmNG44bjhkXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

### 组件状态

仅能在页面内，即一个组件树上共享状态变量

- @State：@State装饰的变量拥有其所属组件的状态，可以作为其子组件单向和双向同步的数据源。当其数值改变时，会引起相关组件的渲染刷新。
- @Prop：@Prop装饰的变量可以和父组件建立单向同步关系，@Prop装饰的变量是可变的，但修改不会同步回父组件。
- @Link：@Link装饰的变量和父组件构建双向同步关系的状态变量，父组件会接受来自@Link装饰的变量的修改的同步，父组件的更新也会同步给@Link装饰的变量。
- @Provide/@Consume：@Provide/@Consume装饰的变量用于跨组件层级（多层组件）同步状态变量，可以不需要通过参数命名机制传递，通过alias（别名）或者属性名绑定。
- @Observed：@Observed装饰class，需要观察多层嵌套场景的class需要被@Observed装饰。单独使用@Observed没有任何作用，需要和@ObjectLink、@Prop连用。
- @ObjectLink：@ObjectLink装饰的变量接收@Observed装饰的class的实例，应用于观察多层嵌套场景，和父组件的数据源构建双向同步。
  - 仅@Observed/@ObjectLink可以观察嵌套场景，其他的状态变量仅能观察第一层，详情见各个装饰器章节的“观察变化和行为表现”小节。

### 应用状态

要实现应用级的，或者多个页面的状态数据共享，就需要用到应用级别的状态

- LocalStorage：页面级UI状态存储，通常用于UIAbility内、页面间的状态共享。
  -  LocalStorage根据与@Component装饰的组件的同步类型不同，提供了两个装饰器：

  - @LocalStorageProp：@LocalStorageProp装饰的变量和与LocalStorage中给定属性建立单向同步关系。
  - @LocalStorageLink：@LocalStorageLink装饰的变量和在@Component中创建与LocalStorage中给定属性建立双向同步关系。
- AppStorage：特殊的单例LocalStorage对象，由UI框架在应用程序启动时创建，为应用程序UI状态属性提供中央存储；
  - 建立AppStorage和自定义组件的联系，需要使用@StorageProp和@StorageLink装饰器。使用@StorageProp(key)/@StorageLink(key)装饰组件内的变量，key标识了AppStorage的属性。
  - 不建议开发者使用@StorageLink和AppStorage的双向同步的机制来实现事件通知，AppStorage是和UI相关的数据存储，改变会带来UI的刷新，相对于一般的事件通知，UI刷新的成本较大。开发者可以使用emit订阅某个事件并接收事件回调，可以减少开销，增强代码的可读性，状态装饰器装饰的变量，改变会引起UI的渲染更新，如果改变的变量不是用于UI更新，只是用于消息传递，推荐使用 emitter方式。
- PersistentStorage：持久化存储UI状态，通常和AppStorage配合使用，选择AppStorage存储的数据写入磁盘，以确保这些属性在应用程序重新启动时的值与应用程序关闭时的值相同；
  - AppStorage属性借助PersistentStorage持久化。UI和业务逻辑不直接访问PersistentStorage中的属性，所有属性访问都是对AppStorage的访问，AppStorage中的更改会自动同步到PersistentStorage。
  - LocalStorage和AppStorage都是运行时的内存，但是在应用退出再次启动后，依然能保存选定的结果，是应用开发中十分常见的现象，这就需要用到PersistentStorage。
  - number, string, boolean, enum 等简单类型。
  - 可以被JSON.stringify()和JSON.parse()重构的对象。例如Date, Map, Set等内置类型则不支持，以及对象的属性方法不支持持久化。
  - 不支持嵌套对象（对象数组，对象的属性是对象等）。因为目前框架无法检测AppStorage中嵌套对象（包括数组）值的变化，所以无法写回到PersistentStorage中。
  - 不支持undefined 和 null 。
  - 避免持久化大型数据集，好是小于2kb。
  - 避免持久化经常变化的变量。
  - 如果需要存储大量的数据，建议使用数据库api。
  - 支持的接口PersistProp、DeleteProp、PersistProps
- Environment：应用程序运行的设备的环境参数，环境参数会同步到AppStorage中，可以和AppStorage搭配使用。
  - 如果需要应用程序运行的设备的环境参数，以此来作出不同的场景判断，比如多语言，暗黑模式等，需要用到Environment设备环境查询。
  - Environment的所有属性都是不可变的（即应用不可写入），所有的属性都是简单类型。
  - @StorageProp关联的环境参数可以在本地更改，但不能同步回AppStorage中，因为应用对环境变量参数是不可写的，只能在Environment中查询。

### 其他状态

- @Watch用于监听状态变量的变化。
  - @Watch用于监听状态变量的变化，当状态变量变化时，@Watch的回调方法将被调用。@Watch在ArkUI框架内部判断数值有无更新使用的是严格相等（===），遵循严格相等规范。当在严格相等为false的情况下，就会触发@Watch的回调。
  - 避免循环的产生，建议不要在@Watch的回调方法里修改当前装饰的状态变量；
- $$运算符：给内置组件提供TS变量的引用，使得TS变量和内置组件的内部状态保持同步。
  - 当前$$支持基础类型变量，以及@State、@Link和@Prop装饰的变量。
  - 当前$$仅支持Refresh组件的refreshing参数。
  - $$绑定的变量变化时，会触发UI的同步刷新

## 渲染控制

- if/else：条件渲染
- ForEach：循环渲染
- LazyForEach：数据懒加载

# ArkCompiler

ArkCompiler是华为自研的统一编程平台，包含编译器、工具链、运行时等关键部件，支持高级语言在多种芯片平台的编译与运行，并支撑应用和服务运行在手机、个人电脑、平板、电视、汽车和智能穿戴等多种设备上的需求。

- ark_js_vm：运行abc文件的可执行程序。
- es2panda：将ArkTS文件转换生成ArkCompiler字节码文件的工具。

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=ZmE0NjJhOTRkYjdiY2M5NGRmNzJkYmNlNTAwMjhlMWJfMm45QlRGZDNxRVBLdkQ0NVF4WkFzODRFNG83Q0tKb3BfVG9rZW46VWg4WGJJMXBwb0V1M3h4YmF0b2NaT0ZIbm9jXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

参考文档：https://gitee.com/openharmony/docs/blob/master/zh-cn/device-dev/subsystems/subsys-arkcompiler-guide.md

# AppGallery Connect

AppGallery Connect（以下简称AGC）是华为应用市场推出的应用一站式服务平台，致力于为开发者提供应用创意、开发、分发、运营、分析全生命周期服务，构建全场景智慧化的应用生态。

![img](https://nio.feishu.cn/space/api/box/stream/download/asynccode/?code=YmQzM2I5MjNhYmYyNTIyM2Y5MmJlZTk0OTEzNGZkYzlfd0N3N3Y3WmZoOVRUaU01aG92bk9VMHF6dlFXaktkZ1hfVG9rZW46WlhTZ2IzMG9Zb0xJcmd4c3ZFYmNvQ1BIblpkXzE3MTkwMzExMDY6MTcxOTAzNDcwNl9WNA)

- 应用管理
  - 证书、Profile管理：需要使用证书和profile对应用签名后才可以发布
- 应用分发
- 质量监控
- 数据分析
- 增长
- 盈利
- Serverless

入口地址：https://developer.huawei.com/consumer/cn/service/josp/agc/index.html#/

# DevEco Service

- OHPM中心仓
- OHPM私仓工具

入口地址：https://developer.huawei.com/consumer/cn/next/deveco-service/

# 元服务

登录AppGallery Connect， 点击“我的应用”，点击应用列表右侧的“新建”。

# Harmony Design

提供设计资源库，包含图标、色彩、文字、音效等丰富的资源，并且提供多种效率组件和界面模版，帮助快速准确地设计 HarmonyOS 应用。

参考文档：https://developer.huawei.com/consumer/cn/design