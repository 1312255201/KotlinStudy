# 函数

函数的具体定义：

> 函数是完成特定任务的独立程序代码单元。

## 创建和使用函数

Kotlin函数使用`fun`关键字声明：

```kotlin
fun 函数名称([函数参数...]): 返回值类型 {
    //函数体
}
```

我们要调用这个函数也很简单，只需要像下面这样就可以了：

```kotlin
fun main() {
    hello()     //调用函数只需使用 函数名() 即可
}
```

不过，有些时候，我们可能需要外部传入一些参数来使用，比如：

```kotlin
fun say(message: String){   //在定义函数时，可以将参数写到
    println("我说：$message")
}
```

这里我们在函数的小括号中填入的就是形式参数，这代表调用函数时需要传入的数据，比如这里就是我们要打印的字符串，而实际在调用函数时，填入的内容就是实际参数：

```kotlin
fun main() {
  	//在调用带参数的函数时，必须填写实参，否则无法编译通过
  	//这里填入的内容就是实际参数
    say("你干嘛")
  	//也可以将变量作为实际参数传入
  	val str: String = "哎哟"
    say(str)
}
```

还有一些时候，我们的函数可能需要返回一个计算的结果给调用者，我们也可以设定函数的返回值：      

```kotlin
//这个函数用于计算两个Int数之和
fun sum(a: Int, b: Int) : Int {
    return a + b  //使用return语句将结果返回
}
```

带返回值的函数，调用之后得到的返回值，可以由变量接收，或是直接作为其他函数的参数：

```kotlin
fun main() {
    var result = sum(1, 2)   //获取函数返回值
    println(result)
    println(sum(2, 4))  //直接打印函数返回值
}
```

注意这个`return`关键字在执行之后，是不会继续执行之后的内容的：

```kotlin
fun main() {
    println(test(-2))
    println(test(10))
}

fun test(i: Int): String{
    if(i > 0) return "Hello"
  	println("继续")
    return "World"   //如果满足上面条件，在执行return之后，后续无论有没有执行完，都不会再往下了
}
```

有些时候，也可以设计一些参数带有默认值的函数，如果在调用函数时不填入参数，那么就使用我们一开始设置好的默认值作为实际传入的参数：

```kotlin
fun main() {
    test()   //调用函数时，如果对应参数有默认值，可以不填
}

fun test(text: String = "我是默认值"){
    println(text)
}
```

在调用函数时，我们可以手动指定传入的参数对应的是哪一个形式参数：

```kotlin
fun main() {
    test(b = 3)  //这里如果只想填写第二个参数b，我们可以直接指定吧实参给到哪一个形参
  	test(3)   //这种情况就是只填入第一个实参
}

fun test(a: Int = 6, b: Int = 10): Int {
    return a + b
}
```

对于一些内容比较简单的函数，比如上面仅仅是计算两个参数的和，我们可以直接省略掉花括号，像这样编写：

```kotlin
fun test(a: Int = 6, b: Int = 10): Int = a + b   //函数的结果直接缩减为 = a + b 效果跟之前是一样的
fun test(a: Int = 6, b: Int = 10) = a + b  //返回类型可以自动推断，这里可以吧返回类型省掉
```

这里还需要注意一下，函数的形式参数默认情况下为常量，无法进行修改，只能使用：

![image-20240902111331206](assets/image-20240902111331206.png)

比较奇葩的是，函数内部也可以定义函数：

```kotlin
fun outer(){
    fun inner(){
        //函数内部定义的函数，无限套娃
    }
}
```

函数内的函数作用域是受限的，我们只能在函数内部使用：

```kotlin
fun outer(){
    fun inner(){
    }

    inner()
}
```

内部函数可以访问外部函数中的变量：

```kotlin
fun outer(){
    val a = 10;
    fun inner(){
        println(a)
    }
}
```

最后，我们不能同时编写多个同名函数，这会导致冲突：

![image-20240902111345307](assets/image-20240902111345307.png)

但是，如果多个同名函数的参数不一致，是允许的： 

```kotlin
fun test() = println("A")
fun test(str: String) = println("B")  //参数列表不一致
```

我们在调用这个函数时，编译器会根据我们传入的实参自动匹配使用的函数是哪一个：

```kotlin
...

fun main() {
    test("")  //结果为B
}
```

以上适用于形参列表不同的情况，如果仅仅是返回值类型不同的情况，同样是不允许的：

![image-20240902111400148](assets/image-20240902111400148.png)

像这种编写同名但不同参数的函数，称为*函数的重载*。

## 再谈变量

之前的变量都是在main函数中使用的局部变量，我们也可以将变量的作用域进行提升，将其直接变成一个顶级定义：

```kotlin
var str: String = "尊嘟假嘟"   //跟定义函数一样，直接写在Kt文件中

fun main() {
    ...
}
```

此时，这个变量可以被所有的函数使用：

```kotlin
var str: String = "尊嘟假嘟"

fun main() = println(str)  //作用域的提升，使得变量可以被随意使用
fun test() = println(str)
```

以上也只是对变量的一些简单使用，现在变量的作用域被提升到顶层，它可以具有更多的一些特性，那么，我们就再来重新认识一下变量，声明一个变量的完整语法如下：

```kotlin
var <propertyName>[: <PropertyType>] [= <property_initializer>]
    [<getter>]
    [<setter>]
```

对于这种顶层定义的变量（包括后面类中会用到的成员属性变量）可以具这两个可选的函数，它们本质上是一个get和set函数：

- getter：用于获取这个变量的值，默认情况下直接返回当前这个变量的值
- setter：用于修改这个变量的值，默认情况下直接对这个变量的值进行修改

我们在使用这种全局变量时，对于变量的获取和设定，本质上都是通过其getter和setter函数来完成的，只不过默认情况下不需要我们去编写，程序编译之后，有点像这样的结果：

```kotlin
var name: String = "小明"

fun getName() : String {   //编译时自动生成了对应变量的get函数
    return this.name
}
  
fun setName(name: String) {  //编译时自动生成了set函数
   this.name = name;
}
```

而对于其使用，在编译之后，会变成这样：    

```kotlin
fun main() {
    println(getName())   //获取name时本质上是调用getName函数
}
```

这其实是为了后续多态的一些性质而设计的

可以看到，在默认情况下，变量的获取就是直接返回，设置就是直接修改，不过有些时候我们可能希望修改这些变量获取或修改时执行的操作，我们可以手动编写：

```kotlin
var str: String = "尊嘟假嘟"
    get() = field + field   //使用filed代表当前这个变量(字段)的值，这里返回值拼接的结果
```

> 这里使用的field准确的说应该是Kotlin提供的"后备字段"，因为我们使用getter和setter本质上替代了原有的获取和修改方式，使其变得更像是函数的调用，因此，为了能够继续像之前使用一个变量那样去操作它本身，就有了这个后备字段。

最后得到的就是：

![image-20240902112714355](assets/image-20240902112714355.png)

甚至还可以写成这样，在获取的时候执行一些操作：

```kotlin
var str: String = "尊嘟假嘟"
    get() {
        println("获取变量的值：")   //获取的时候打印一段文本
        return field + "666"
    }

fun main() = println(str)
```

同样的，设置的时候也可以自定义：

```kotlin
var str: String = "尊嘟假嘟"
    get() = field + field
    set(value) {    //这里的value就是给过来的值
        println("设置变量的值")
        field = value   //注意，对于val类型的变量，没有set函数，因为不可变
    }
```

因此，一个变量有些时候可能会写成这样：

```kotlin
val str get() = "你干嘛"
```

当然，默认情况下其实没有必要去重写get和set除非特殊需求。

## 递归调用

我们前面学习了如何调用函数，实际上函数自己也可以调用自己。    

```kotlin
fun test(){
    test()   //我自己调用自己
}
```

函数自己调用自己有什么意义？反而还会导致函数无限的调用下去，无穷无尽，确实，如果不加限制地让函数自己调用自己：

![image-20240902114215133](assets/image-20240902114215133.png)

就会出现这种`爆栈`的情况，这是因为程序的内存是有限的，不可能无限制的继续调用下去，因此，在自我调用到一定的深度时，会被强制终止。所以说这玩意有啥用呢？如果我们对递归函数加以一些限制，或许会有意想不到的发现： 

```kotlin
fun main() {
    test(5)  //计算0-5的和
}

//这个函数实现了计算0-n的和的功能
fun test(n: Int): Int{
    if(n <= 0) return 0  //当n等于0的时候就不再向下，而是直接返回0
    return n + test(n - 1)  //n不为0就返回当前的n加上test参数n-1的和
}
```

这个函数最终调用起来就像这样：

> test(5) = 5 + test(4) = 5 + 4 + test(3) =  ...  = 5 + 4 + 3 + 2 + 1 + 0

只要合理使用递归函数，加以一定的结束条件，反而能够让我们以非常简洁的形式实现一个需要循环来完成的操作。

我们可以再来看一个案例：

> 斐波那契数列是一个非常经典的数列，它的定义是：前两个数是1和1，之后的每个数都是前两个数的和。
>
> 斐波那契数列的前几个数字依次是：1, 1, 2, 3, 5, 8, 13, 21, 34, 55, ...

对于求解斐波那契数列第N个数这类问题，我们也可以使用递归来实现：

```kotlin
fun main() {
    println(fib(5))
}

fun fib(n: Int): Int{
    if(n <= 2) return 1   //我们知道前两个一定是1，所以直接返回
    return fib(n - 1) + fib(n - 2)   //当前fib(n)的结果就是前两个结果之和，直接递归继续找
}
```

不过，这种函数的效率就非常低了，相比循环来说，使用递归解决斐波那契问题，时间复杂度会呈指数倍增长，且n大于20时基本可以说很卡了（可以想象一下，每一个fib(n)都会分两个出去，实际上这个中间存在大量重复的计算）

可以将这种尾部作为返回值进行递归的操作优化一下使用`tailrec`关键字来实现：

```kotlin
tailrec fun test(n: Int, sum: Int = 0): Int {
    if(n <= 0) return sum   //到底时返回累加的结果
    return test(n - 1, sum + n)  //不断累加
}
```

实际上在编译之后，会变成这样：

![image-20240902114228497](assets/image-20240902114228497.png)

它变成了一个普通的循环操作，这也是编译器的功劳，同样的，对于斐波那契数列：

```kotlin
tailrec fun fib(n: Int, prev: Int = 0, next: Int = 1): Int {
    return if (n == 0) prev else fib(n - 1, next, prev + next)  //从0和1开始不断向后，直到n为0就返回
}
```

## 实用函数库介绍

Kotlin为我们内置了大量实用的库函数，我们可以使用这些库函数来快速完成某些操作。

比如我们前面使用的`println`就是Kotlin提供的库函数，我们可以使用这个函数快速进行数据打印：    

```kotlin
fun main() {
    println("Hello World")  //这里其实就是在调用函数，传入了一个String类型的参数
}
```

那既然现在有输出，能不能让用户输入，然后我们来读取呢？

```kotlin
fun main() {
    val text = readln()
    println("读取到用户输入：$text")
}
```

我们可以在控制台输入一段文本，然后回车结束：

![image-20240902114335019](assets/image-20240902114335019.png)

Kotlin提供的运算符实际上只能进行一些在小学数学中出现的运算，但是如果我们想要进行乘方、三角函数之类的高级运算，就没有对应的运算符能够做到，而此时我们就可以使用数学工具类来完成。 

```kotlin
import kotlin.math.*    //我们需要使用import来引入某些库，这样才能使用库函数

fun main() {
    1.0.pow(4.0)  //我们可以使用pow方法直接计算a的b次方
    abs(-1);    //abs方法可以求绝对值
    max(19, 20);    //快速取两个数的最大值
    min(2, 4);   //快速取最小值
    sqrt(9.0);    //求一个数的算术平方根
}
```

当然，三角函数肯定也是安排上了的：

```kotlin
fun main() {
    //这里我们可以直接使用库中预设好的PI
    sin(PI / 2);     //求π/2的正弦值，这里我们可以使用预置的PI进行计算
    cos(PI);       //求π的余弦值
    tan(PI / 4);    //求π/4的正切值

    asin(1.0);     //三角函数的反函数也是有的，这里是求arcsin1的值
    acos(1.0);
    atan(0.0);
}
```

可能在某些情况下，计算出来的浮点数会得到一个很奇怪的结果：

```kotlin
fun main() {
    println(sin(Math.PI));
}
```

![image-20240902114408195](assets/image-20240902114408195.png)

正常来说，sinπ的结果应该是0才对，为什么这里得到的是一个很奇怪的数？这个E是干嘛的，这其实是科学计数法的10，后面的数就是指数，上面的结果其实就是：

- 1.2246467991473532×10−161.2246467991473532×10−16

其实这个数是非常接近于0，这是因为精度问题导致的，所以说实际上结果就是0。

我们也可以计算对数函数：

```kotlin
fun main() {
    ln(E)    //e为底的对数函数，其实就是ln，我们可以直接使用Math中定义好的e
    log10(100.0)    //10为底的对数函数
  	log2(8.0)    //2为底的对数函数
    //利用换底公式，我们可以弄出来任何我们想求的对数函数
    val a = ln(4.0) / ln(2.0) //这里是求以2为底4的对数，log(2)4 = ln4 / ln2
    println(a)
}
```

还有一些比较特殊的计算：

```kotlin
fun main() {
    ceil(4.5) //通过使用ceil来向上取整
    floor(5.6) //通过使用floor来向下取整
}
```

向上取整就是找一个大于当前数字的最小整数，向下取整就是砍掉小数部分。注意，如果是负数的话，向上取整就是去掉小数部分，向下取整就是找一个小于当前数字的最大整数。

## 高阶函数与lambda表达式

Kotlin中的函数属于一等公民，它支持很多高级特性，甚至可以被存储在变量中，可以作为参数传递给其他高阶函数并从中返回，就想使用普通变量一样。 为了实现这一特性，Kotlin作为一种静态类型的编程语言，使用了一系列函数类型来表示函数，并提供了一套特殊的语言结构，例如lambda表达式。

> 正是得益于函数可以作为变量的值进行存储，因此，如果一个函数接收另一个函数作为参数，或者返回值的类型就是一个函数，那么该函数称为高阶函数。

要声明函数类型，需要按照以下规则：

- 所有函数类型都有一个括号，并在括号中填写参数类型列表和一个返回类型，比如：`(A, B) -> C ` 表示一个函数类型，该类型表示接受类型`A`和`B`的两个参数并返回类型`C`的值的函数。参数类型列表可为空的，比如`() -> A`，注意，即使是`Unit`返回类型也不能省略。

我们可以像下面这样编写：  

```kotlin
//典型的函数类型 (参数...) -> 类型  小括号中间是一个剪头一样的符号，然后最后是返回类型
var func0: (Int) -> Unit  //这里的 (Int) -> Unit 表示这个变量存储的是一个有一个int参数并且没有返回值的函数
var func1: (Double, Double) -> String   //同理，代表两个Double参数返回String类型的函数
```

同样的，作为函数的参数也可以像这样表示：

```kotlin
fun test(other: (Int) -> String){
}
```

函数类型的变量，我们可以将其当做一个普通的函数进行调用：

```kotlin
fun test(other: (Int) -> String){
    println(other(1))  //这里提供的函数接受一个Int参数返回string，那么我们可以像普通函数一样传入参数调用它
}
```

由于函数可以接受函数作为参数，所以说你看到这样的套娃场景也不奇怪：

```kotlin
var func: (Int) -> ((String) -> Double)
```

不过这样写可能有些时候不太优雅，我们可以为类型起别名来缩短名称：

```kotlin
typealias HelloWorld = (String) -> Double

fun main() {
    var func: HelloWorld
}
```

那么，函数类型我们知道如何表示了，如何具体表示一个函数呢？我们前面都是通过`fun`来声明函数：  

```kotlin
fun test(str: String): Int {
    return 666
}
```

而现在我们的变量也可以直接表示这个函数：   

```kotlin
fun main() {
  	//这个变量表示的也是(String) -> Int这种类型的函数
    var func: (String) -> Int = ::test   //使用双冒号来引用一个现成的函数（包括我们后续会学习的成员函数、构造函数等）
}

//这个函数正好与上面的变量表示的函数类型一致
fun test(str: String): Int {
    return 666
}
```

除了引用现成的函数之外，我们也可以使用匿名函数，这是一种没有名称的函数：

```kotlin
fun main() {
    val func: (String) -> Int = fun(str: String): Int {  //这里写了fun关键字后，并没有编写函数名称，这种函数就是匿名函数，因为在这里也不需要什么名字，只需要参数列表函数体
        println("这是传入的内容$str")
        return 666
    }
}
```

匿名函数除了没名字之外，其他的用法跟函数是一样的。

最后，我们来看看今天的重量级嘉宾，不要小看了Kotlin的语法，我们也可以使用Lambda表达式来表示一个函数实例：     

```kotlin
fun main() {
    var func: (String) -> Int = {  //一个Lambda表达式只需要直接在花括号中编写函数体即可
        println("这是传入的参数$it")   //默认情况下，如果函数只有一个参数，我们可以使用it代表传入的参数
        666   //跟之前的if表达式一样，默认最后一行为返回值
    }
  	func("HelloWorld!")
}
```

是不是感觉特别简便？

![image-20230730230512284](assets/fsxB2UhpXZewk4Q.jpg)

对于参数有多个的情况，我们也可以这样进行编写：

```kotlin
fun main() {
    val func: (String, String) -> Unit = { a, b ->   //我们需要手动添加两个参数这里的形参名称，不然没法用他两
        println("这是传入的参数$a, 第二个参数$b")   //直接使用上面的形参即可
    }
  	val func2: (String, String) -> Unit = { _, b ->
        println("这是传入的第二个参数$b")   //假如这里不使用第一个参数，也可以使用_下划线来表示不使用
    }
    func("Hello", "World")
}
```

![image-20230730230633880](assets/R9WjhIUinaP465p.jpg)

是不是感觉玩的非常高级？还有更高级的在后面呢！

我们接着来看，如果我们现在想要调用一个高阶函数，最直接的方式就是下面这样：

```kotlin
fun main() {
    val func: (Int) -> String = { "收到的参数为$it" }
    test(func)
}

fun test(func: (Int) -> String) {
    println(func(66))
}
```

当然我们也可以直接把一个Lambda作为参数传入作为实际参数使用：

```kotlin
fun main() {
    test({ "收到的参数为$it" })
}
```

不过这样还不够简洁，在Kotlin中，如果函数的最后一个形式参数是一个函数类型，可以直接写在括号后面，就像下面这样：

```kotlin
test() { "收到的参数为$it" }
```

由于小括号里面此时没有其他参数了，还能继续省，直接把小括号也给干掉：

```kotlin
test { "收到的参数为$it" }   //干脆连小括号都省了，这语法真的绝
```

当然，如果在这之前有其他的参数，只能写成这样了：

```kotlin
fun main() {
    test(1) { "收到的参数为$it" }
}

//这里两个参数，前面还有一个int类型参数，但是同样的最后一个参数是函数类型
fun test(i: Int, func: (Int) -> String) {
    println(func(66))
}
```

这种语法也被称为 尾随lambda表达式，能省的东西都省了，不过只有在最后一个参数是函数类型的情况下才可以，如果不是最后一位，就没办法做到尾随了。

最后需要特别注意的是，在Lambda中没有办法直接使用`return`语句返回结果，而是需要用到之前我们学习流程控制时用到的标签：

```kotlin
fun main() {
    val func: (Int) -> String = test@{
        //比如这里判断到it大于10就提前返回结果
        if(it > 10) return@test "我是提前返回的结果"
        println("我是正常情况")
        "收到的参数为$it"
    }
    test(func)
}

fun test(func: (Int) -> String) {
    println(func(66))
}
```

如果是函数调用的尾随lambda表达式，默认的标签名字就是函数的名字：

```kotlin
fun main() {
    testName {  //默认使用函数名称
        if(it > 10) return@testName "我是提前返回的结果"
        println("我是正常情况")
        "收到的参数为$it"
    }
}

fun testName(func: (Int) -> String) {
    println(func(66))
}
```

## 内联函数

使用高阶函数会可能会影响运行时的性能：每个函数都是一个对象，而且函数内可以访问一些局部变量，但是这可能会在内存分配（用于函数对象和类）和虚拟调用时造成额外开销。

为了优化性能，开销可以通过内联Lambda表达式来消除。使用`inline`关键字会影响函数本身和传递给它的lambdas，它能够让方法的调用在编译时，直接替换为方法的执行代码，什么意思呢？比如下面这段代码：

```kotlin
fun main() {
    test()
}

//添加inline表示内联函数
inline fun test(){
    println("这是一个内联函数")
  	println("这是一个内联函数")
  	println("这是一个内联函数")
}
```

由于test函数是内联函数，在编译之后，会原封不动地把代码搬过去：

```kotlin
fun main() {
    println("这是一个内联函数")   //这里是test函数第一行，直接搬过来
  	println("这是一个内联函数")
  	println("这是一个内联函数")
}
```

同样的，如果是一个高阶函数，效果那就更好了：   

```kotlin
fun main() {
    test { println("打印：$it") }
}

//添加inline表示内联函数
inline fun test(func: (String) -> Unit){
    println("这是一个内联函数")
    func("HelloWorld")
}
```

由于test函数是内联的高阶函数，在编译之后，不仅会原封不动地把代码搬过去，还会自动将传入的函数参数贴到调用的位置：

```kotlin
fun main() {
    println("这是一个内联函数")   //这里是test函数第一行
  	val it = "HelloWorld"  //这里是函数内传入的参数
    println("打印：$it")  //第二行是调用传入的函数，自动贴过来
}
```

内联会导致编译出来的代码变多，但是同样的换来了性能上的提升，不过这种操作仅对于高阶函数有显著效果，普通函数实际上完全没有内联的必要，也提升不了多少性能。

注意，内联函数中的函数形参，无法作为值给到变量，只能调用：

![image-20240902130659710](assets/image-20240902130659710.png)

同样的，由于内联，导致代码被直接搬运，所以Lambda中的return语句可以不带标签，这种情况会导致直接返回：

```kotlin
fun main() {
    test { return }  //内联高阶函数的Lambda参数可以直接写return不指定标签
    println("调用上面方法之后")
}

inline fun test(func: (String) -> Unit){
    func("HelloWorld")
    println("调用内联函数之后")
}
```

上述代码的运行结果就是，直接结束，两句println都不会打印，这种情况被称为**非局部返回**。

回到上一节最后我们提出的问题，实际上，在Kotlin中Lambda表达式支持一个叫做"标签返回"（labeled return）的特性，这使得你能够从一个Lambda表达式中返回一个值给外围函数，而不是简单地返回给Lambda表达式所在的最近的封闭函数，就像下面这样：

```kotlin
fun main() {
    test { return@main }  //标签可以直接指定为外层函数名称main来提前终止整个外部函数
    println("调用上面方法之后")
}

inline fun test(func: (String) -> Unit){
    func("HelloWorld")
    println("调用内联函数之后")
}
```

效果跟上面是完全一样的，为了避免这种情况，我们也可以像之前一样将标签写为@test来防止非局部返回。

```kotlin
fun main() {
    test { return@test }  //这样就只会使test返回，而不会影响到外部函数了
    println("调用上面方法之后")
}
```

有些时候，可能一个内联的高阶函数中存在好几个函数参数，但是我们希望其中的某一个函数参数不使用内联，能够跟之前一样随意当做变量使用：

```kotlin
fun main() {
    test({ println("我是一号：$it") }, { println("我是二号：$it") })
}

//在不需要内联的函数形参上添加noinline关键字，来防止此函数的调用内联
inline fun test(func: (String) -> Unit, noinline func2: (Int) -> Unit){
    println("这是一个内联函数")
    func("HelloWorld")
  	var a = func2  //这样就不会报错，但是不会内联了
    func2(666)
}
```

最后编译出来的结果，类似于：

```kotlin
fun main() {
    println("这是一个内联函数")
    val it = "HelloWorld"
    println("打印：$it")
  	//第二个参数由于不是内联，这里依然作为Lambda使用
    val func2: (Int) -> Unit = { println("我是二号：$it") }
    func2(666)
}
```
