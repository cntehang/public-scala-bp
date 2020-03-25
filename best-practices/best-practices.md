# Scala Best Practices

Best practices for Scala programming. Unless you have a strong reason and get approval from an experienced developer, please just follow these practices.

## 1. 不使用面向对象编程的类继承层级

[John A De Goes 总结](http://degoes.net/articles/fp-vs-oop-part1) 说：OO 的继承和子类只用于 Sum types， module 和 Type class。

## 2. 尽量使用函数式编程，尽量避免类（class）

尽量避免面向对象的 class 而尽可能采用函数式编程。用 case class 或 case object 来定义数据，用 object，companion object 或 case class 来定义操作。
