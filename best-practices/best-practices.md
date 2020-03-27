# Scala 最佳实践

请遵守下面的最佳实践。如果有非常强烈的理由以及经过团队同意方可有例外， 并需要将例外情况写入此文档。

## 0 动机

下面的建议出自创建一个 Scala 开发团队的视频 [Buding a Company on Scala](https://youtu.be/lTL4TflgEWc):

- 使用框架: 我们确定的有 ZIO, Akka http, Akka Stream, slick， Play
- 在工作中总结最佳实践 (Best Practices： BPs): 每日工作的组成部分。
- 不需要成为 FP 专家: 压出车辙，团队按既有模式高效合作。
- 不要被 Scala 的能力吓到或诱惑。
- 早点决定框架和最佳实践。
- 工具是手段，不是目的，一定要看应用场景。用 VS Code。慎重避免 Akka actors. 评估 Lagom.
- 异步或同步：尝试 Akka Streams with back pressure. 在响应式和同步之间不要三心二意。全异步同时隔离同步操作到固定线程池。
- 不要追求简介，可维护性更重要。
- 不要过早抽象。遇到三次才是模式。
- 基于团队进行培训、讨论、开发和改进。

## 1 不使用面向对象编程的类继承层级

[John A De Goes 总结](http://degoes.net/articles/fp-vs-oop-part1) 说：OO 的继承和子类只用于 Sum types， module 和 Type class。

## 2 尽量使用函数式编程，尽量避免类（class）

尽量避免面向对象的 class 而尽可能采用函数式编程。用 case class 或 case object 来定义数据，用 object，companion object 或 case class 来定义操作。
