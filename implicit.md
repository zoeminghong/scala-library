# Implicit

Implicit 是Scala语言中处理编译类型错误的一种修复机制，利用该机制，我们可以编写出任意参数和返回值的多态方法(这种多态也被称为`Ad-hoc polymorphism` -- 任意多态)，实现任意多态，我们通常使用`Magnet Pattern`磁铁模式；同时还可以给其他类库的类型添加方法来对其他类库进行扩展，通常将这种技术称之为`Method Injection`。

## 用法

### 隐式类

```scala
object Helpers {
  implicit class IntWithTimes(x: Int) {
    def times[A](f: => A): Unit = {
      def loop(current: Int): Unit =
        if(current > 0) {
          f
          loop(current - 1)
        }
      loop(x)
    }
  }
}

//使用
scala> import Helpers._
import Helpers._

scala> 5 times println("HI")
HI
HI
HI
HI
HI
```

#### 要求

- implicit只能在trait和类内部使用，不能单独直接声明（Helpers类就不能使用implicit）
- 构造函数只能携带一个非隐式参数

```scala
 implicit class RichDate(date: java.util.Date) // 正确！
 implicit class Indexer[T](collecton: Seq[T], index: Int) // 错误！
 implicit class Indexer[T](collecton: Seq[T])(implicit index: Index) // 正确！
```

> 虽然我们可以创建带有多个非隐式参数的隐式类，但这些类无法用于隐式转换。

- 在同一作用域内，不能有任何方法、成员或对象与隐式类同名（意味着隐式类不能是`case class`）

```scala
object Bar
implicit class Bar(x: Int) // 错误！

val x = 5
implicit class x(y: Int) // 错误！

implicit case class Baz(x: Int) // 错误！
```

### 隐式转换

如果不存在隐式转换，代码是这样的

```scala
object Test {
  // 定义打印操作Trait
  trait PrintOps {
    val value: String
    def printWithSeperator(sep: String): Unit = {
      println(value.split("").mkString(sep))
    }
  }

  // 定义针对String的隐式转换方法
  def stringToPrintOps(str: String): PrintOps = new PrintOps {
    override val value: String = str
  }

  stringToPrintOps("hello,world") printWithSeperator "*"
}
```

加上implicit之后，是这样的

```scala
object Test {
  // 定义打印操作Trait
  trait PrintOps {
    val value: String
    def printWithSeperator(sep: String): Unit = {
      println(value.split("").mkString(sep))
    }
  }

  // 定义针对String的隐式转换方法
  implicit def stringToPrintOps(str: String): PrintOps = new PrintOps {
    override val value: String = str
  }

  "hello,world" printWithSeperator "*"
}
```

#### 要求

- 标记规则：只会去寻找带有`implicit`标记的方法，这点很好理解，在上面的代码也有演示，如果不申明为`implicit`只能手工去调用。

- 作用域范围规则：

  - 只会在当前表达式的作用范围之内查找，而且只会查找**单一标识符**的函数，上述代码中，如果是伴生对象也是有效的
  - 当前作用域上下文的隐式转换方法优先级高于伴生对象内的隐式方法

  > 单一标志符：直接可以调用的，不用`对象.方法`方式，而是直接`方法`即可

- 不能有歧义原则：在相同优先级的位置只能有一个同一类型的隐式的转型方法，否则Scala编译器无法选择适当的进行转型，编译出错。

```scala
// 已存在
implicit def stringToPrintOps(str: String): PrintOps = new PrintOps {
    override val value: String = str
}
// 可以
implicit def intToPrintOps(i: Int): PrintOps = new PrintOps {
    override val value: String = i+""
}
// 不可以
implicit def strToPrintOps(str: String): PrintOps = new PrintOps {
    override val value: String = str
}
// 参数类型要不一致
```

- 只应用转型方法一次原则：Scala编译器不会进行多次隐式方法的调用，比如需要`C`类型参数，而实际类型为`A`，作用域内存在`A => B`,`B => C`的隐式方法，Scala编译器不会尝试先调用`A => B` ,再调用`B => C`
- 显示方法优先原则：如果方法被重载，可以接受多种类型，而作用域中存在转型为另一个可接受的参数类型的隐式方法，则不会被调用，Scala编译器优先选择无需转型的显式方法

```scala
def m(a: A): Unit = ???
def m(b: B): Unit = ???

val b: B = new B

//存在一个隐式的转换方法 B => A
implicit def b2a(b: B): A = ???

m(b) //隐式方法不会被调用，优先使用显式的 m(b: B): Unit
```



### 隐式参数

```scala
object SomeApp extends App {
  //这是我们的全局配置类
  class Setting(config: Config) {
    def host: String = config.getString("app.host")
  }
  // 申明一个隐式的配置对象
  implicit val setting = new Setting(ConfigFactory.load)

  // 申明隐式参数
  def startServer()(implicit setting: Setting): Unit = {
    val host = setting.host
    println(s"server listening on $host")
  }

  // 无需传入隐式参数
  startServer()
}
```

#### 要求

- 一定要存在与隐式参数同名的implicit标识的变量或常量存在，否则会报`could not find implicit value for parameter`错误（`implicit val setting = new Setting(ConfigFactory.load)`是必须的）