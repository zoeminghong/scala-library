[TOC]

# String

字符串类型

## 使用

### 初始化

```scala
val s = "Hello, world"
```

### 常用方法

```scala
// 获取字符串长度
s.length
// 连接字符串
val s = "Hello" + " world"
// 多行连接
 val foo = """This is 
     | a multiline
     | String"""
// 左对齐第一行之后的每一行
val speech = """Four score and
     | |seven years age""".stripMargin
// 结果
Four score and
seven years age

// 自定义对其符
val speech = """Four score and
     | #seven years ago""".stripMargin('#')
// 字符串替换
val speech = """Four score and
     | |seven years ago
     | |our fathers""".stripMargin.replace("\n", " ")
// 判断是否相等，不用equal，null时候也不会抛空指针
s1 == s2
s1.toUpperCase == s2.toUpperCase
a.equalsIgnoreCase(b)
// 遍历字符串
"Hello".foreach(println)
for(c <- "hello") println(c)
// 使用算子
"hello world".filter(_ != '1')
// drop(n)方法是从集合头部开始，丢弃n个元素，并保留剩余元素的集合方法
"scala".drop(2)  
// take(n)方法会保留所给集合的头n个元素，然后丢弃剩余的部分
"scala".take(2)
// 分割
"hello world".split(" ")
// 字符串中的变量替换，Scala中基础的字符串插值就是在字符串前加字幕‘s’，然后在字符串中放入变量，每个变量都应以‘$’开头。字符串前加字母‘s’时，其实是在创建一个处理字符串字面量
println(s"$name is $age years old, and weights $weight pounds.")
println(s"${hannah.name} has a score of ${hannah.score}")
```

TODO

https://blog.csdn.net/GnahzNib/article/details/52344888