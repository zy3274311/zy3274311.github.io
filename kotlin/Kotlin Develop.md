# Kotlin Develop

## coroutines

#### 超时

```kotlin
withTimeout(1300L) {
    repeat(1000) { i ->
        println("I'm sleeping $i ...")
        delay(500L)
    }
}
```

#### 取消

```kotlin
val job = launch {
    repeat(1000) { i ->
        println("job: I'm sleeping $i ...")
        delay(500L)
    }
}
delay(1300L) // 延迟一段时间
println("main: I'm tired of waiting!")
job.cancel() // 取消该作业
job.join() // 等待作业执行结束
println("main: Now I can quit.")
```

取消计算任务

协程的取消是 协作 的。一段协程代码必须协作才能被取消。 所有 kotlinx.coroutines 中的挂起函数都是 可被取消的 。它们检查协程的取消， 并在取消时抛出 CancellationException。 然而，如果协程正在执行计算任务，并且没有检查取消的话，那么它是不能被取消的

```kotlin
val startTime = System.currentTimeMillis()
val job = launch(Dispatchers.Default) {
    var nextPrintTime = startTime
    var i = 0
    while (isActive) { // 可以被取消的计算循环
        // 每秒打印消息两次
        if (System.currentTimeMillis() >= nextPrintTime) {
            println("job: I'm sleeping ${i++} ...")
            nextPrintTime += 500L
        }
    }
}
delay(1300L) // 等待一段时间
println("main: I'm tired of waiting!")
job.cancelAndJoin() // 取消该作业并等待它结束
println("main: Now I can quit.")
```

#### 顺序调用

```kotlin
val time = measureTimeMillis {
    val one = doSomethingUsefulOne()
    val two = doSomethingUsefulTwo()
    println("The answer is ${one + two}")
}
println("Completed in $time ms")
```

打印输出

```
The answer is 42
Completed in 2017 ms
```

#### 并发

```kotlin
val time = measureTimeMillis {
    val one = async { doSomethingUsefulOne() }
    val two = async { doSomethingUsefulTwo() }
    println("The answer is ${one.await() + two.await()}")
}
println("Completed in $time ms")
```

打印输出

```
The answer is 42
Completed in 1017 ms
```

#### 惰性启动async

可选的，async 可以通过将 start 参数设置为 CoroutineStart.LAZY 而变为惰性的。 在这个模式下，只有结果通过 await 获取的时候协程才会启动，或者在 Job 的 start 函数调用的时候。

```kotlin
val time = measureTimeMillis {
    val one = async(start = CoroutineStart.LAZY) { doSomethingUsefulOne() }
    val two = async(start = CoroutineStart.LAZY) { doSomethingUsefulTwo() }
    // 执行一些计算
    one.start() // 启动第一个
    two.start() // 启动第二个
    println("The answer is ${one.await() + two.await()}")
}
println("Completed in $time ms")
```

打印输出

```
The answer is 42
Completed in 1017 ms
```



## class

sealed class

data class

## 基础语法

### 可变长参数函数

```kotlin
fun vars(vararg v:Int){
    for(vt in v){
        print(vt)
    }
}

fun main(args: Array<String>) {
    vars(1,2,3,4,5)  // 输出12345
}
```



### 区间

`rangeTo()`函数操作符形式的 `..` 创建两个值的区间。 通常会辅以 `in` 或 `!in` 函数。

```kotlin
if (i in 1..10) { // 等同于 1 <= i && i <= 10
    println(i)
}
```

迭代区间

```kotlin
for (i in 1..4) print(i) // 输出“1234”
```

要反向迭代数字 downTo

```kotlin
for (i in 4 downTo 1) print(i) // 输出"4321"
```

通过任意步长（不一定为 1 ）迭代数字 step

```kotlin
for (i in 1..4 step 2) print(i) // 输出“13”
```

迭代不包含其结束元素的数字区间 until

```kotlin
for (i in 1 until 4) {   // i in [1, 4) 排除了 4
	print(i)
}
```



### lambda

匿名函数

```kotlin
fun main(args: Array<String>) {
    val sumLambda: (Int, Int) -> Int = {x,y -> x+y}
    println(sumLambda(1,2))  // 输出 3
}
```



### 函数

#### 中缀函数

标有 infix 关键字的函数也可以使用中缀表示法（忽略该调用的点与圆括号）调用。中缀函数必须满足以下要求：

- 它们必须是成员函数或扩展函数；
- 它们必须只有一个参数；
- 其参数不得接受可变数量的参数且不能有默认值。

```kotlin
infix fun Int.shl(x: Int): Int { …… }
// 用中缀表示法调用该函数
1 shl 2
// 等同于这样
1.shl(2)
```

#### 单表达式函数

```kotlin
fun double(x: Int) = x * 2
```

#### 闭包

闭包即在外部作用域中声明的变量

```kotlin
var sum = 0
ints.filter { it > 0 }.forEach {
    sum += it
}
print(sum)
```

#### 匿名函数

```kotlin
ints.filter(fun(item) = item > 0)
```

#### 内联函数

### 委托

#### 类委托

#### 属性委托