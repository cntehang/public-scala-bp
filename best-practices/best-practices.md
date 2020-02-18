# Scala Best Practices

Best practices for Scala programming. Unless you have a strong reason and get approval from an experienced developer, please just follow these practices.

## 1. 不使用面向对象编程的类继承层级

除了定义 sum types 这一特殊场景，不要在自己的代码用定义类的继承层级。需要使用多态的地方请用 type class。

Only use subtypes (or subclasses in Java) in one well-defined scenario: to define [sum types](https://alvinalexander.com/scala/fp-book/algebraic-data-types-adts-in-scala). 虽然用的是类或对象的继承关系，其实只是用仅有一层的继承语法定义了数据的类型 （is-a 关系 ），相关的数据操作，通常定义到相应的 companion object 里面。

## 2. 尽量使用函数式编程，尽量避免类（class）

尽量避免面向对象的 class 而尽可能采用函数式编程。用 case class 或 case object 来定义数据，用 object 或 companion object 定义操作。
