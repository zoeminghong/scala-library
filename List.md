[TOC]

# List

同类型的数据列表

## 使用

### 初始化

```scala
// ①
val fruit=List("Apple","Banana","Orange")
// ②
val fruit=List.apply("Apple","Banana","Orange")
// ③
val nums = 1 :: (2 :: (3 :: (4 :: Nil)))
> List(1,2,3,4)
```

> List一旦创建即为不可变，不能为其添加新值

### 常用方法

```scala
//判断是否为空
scala> nums.isEmpty
res108: Boolean = false

//取第一个无素
scala> nums.head
res109: Int = 1

//取除第一个元素外剩余的元素，返回的是列表
scala> nums.tail
res114: List[Int] = List(2, 3, 4)

//取列表第二个元素
scala> nums.tail.head
res115: Int = 2

//插入排序算法实现
def isort(xs: List[Int]): List[Int] =
if (xs.isEmpty) Nil
else insert(xs.head, isort(xs.tail))

def insert(x: Int, xs: List[Int]): List[Int] =
if (xs.isEmpty || x <= xs.head) x :: xs
else xs.head :: insert(x, xs.tail)

//List连接操作
scala> List(1,2,3):::List(4,5,6)
res116: List[Int] = List(1, 2, 3, 4, 5, 6)

//取除最后一个元素外的元素，返回的是列表
scala> nums.init
res117: List[Int] = List(1, 2, 3)

//取列表最后一个元素
scala> nums.last
res118: Int = 4

//列表元素倒置
scala> nums.reverse
res119: List[Int] = List(4, 3, 2, 1)

//一些好玩的方法调用
scala> nums.reverse.reverse==nums
res120: Boolean = true

scala> nums.reverse.init
res121: List[Int] = List(4, 3, 2)

scala> nums.tail.reverse
res122: List[Int] = List(4, 3, 2)

//丢弃前n个元素
scala> nums drop 3
res123: List[Int] = List(4)

scala> nums drop 1
res124: List[Int] = List(2, 3, 4)

//获取前n个元素
scala> nums take 1
res125: List[Int] = List(1)

scala> nums.take(3)
res126: List[Int] = List(1, 2, 3)

//将列表进行分割
scala> nums.splitAt(2)
res127: (List[Int], List[Int]) = (List(1, 2),List(3, 4))

//前一个操作与下列语句等同
scala> (nums.take(2),nums.drop(2))
res128: (List[Int], List[Int]) = (List(1, 2),List(3, 4))

//Zip操作
scala> val nums=List(1,2,3,4)
nums: List[Int] = List(1, 2, 3, 4)

scala> val chars=List('1','2','3','4')
chars: List[Char] = List(1, 2, 3, 4)

//返回的是List类型的元组(Tuple）
scala> nums zip chars
res130: List[(Int, Char)] = List((1,1), (2,2), (3,3), (4,4))

//List toString方法
scala> nums.toString
res131: String = List(1, 2, 3, 4)

//List mkString方法
scala> nums.mkString
res132: String = 1234

//转换成数组
scala> nums.toArray
res134: Array[Int] = Array(1, 2, 3, 4)
```

### 伴生对象方法

```scala
//apply方法
scala>  List.apply(1, 2, 3)
res139: List[Int] = List(1, 2, 3)

//range方法，构建某一值范围内的List
scala>  List.range(2, 6)
res140: List[Int] = List(2, 3, 4, 5)

//步长为2
scala>  List.range(2, 6,2)
res141: List[Int] = List(2, 4)

//步长为-1
scala>  List.range(2, 6,-1)
res142: List[Int] = List()

scala>  List.range(6,2 ,-1)
res143: List[Int] = List(6, 5, 4, 3)

//构建相同元素的List
scala> List.make(5, "hey")
res144: List[String] = List(hey, hey, hey, hey, hey)

//unzip方法
scala> List.unzip(res145)
res146: (List[Int], List[Char]) = (List(1, 2, 3, 4),List(1, 2, 3, 4))

//list.flatten，将列表平滑成第一个无素
scala> val xss =
     | List(List('a', 'b'), List('c'), List('d', 'e'))
xss: List[List[Char]] = List(List(a, b), List(c), List(d, e))
scala> xss.flatten
res147: List[Char] = List(a, b, c, d, e)

//列表连接
scala> List.concat(List('a', 'b'), List('c'))
res148: List[Char] = List(a
, b, c
```

