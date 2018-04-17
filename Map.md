[TOC]

# Map(元组)

K-V集合

## 使用

### 初始化

```scala
①非可变Map
val map = Map("name" -> "zhangsan", "sex" -> "man", "age" -> 23)
②可变Map
val map2 = scala.collection.mutable.Map("name" -> "li", "sex" -> "women", "age" -> 18)
```

## 常用方法

```scala
// 添加k->v
map2 += ("city" -> "beijing", "nickname" -> "wangshisuuifeng")
// 修改值
map2("name") = "lisimeimei"
// 遍历
for ((k, v) <- map2)
```

如果是不可变得的map，直接Map(key->value),如map，但是如果是可变的map,则必须要类型明确标明scala.collection.mutable.Map，否则编译器不会报错，但是执行的话会报错误。