# 建议使用的软件和版本

更新时间 2020-08-07

后端技术栈选取原则有二个：

- 第一是尽量靠近 Scala 语言本身（第一性原则），外加最简单的 FP 概念。避免引入复杂的抽象而美丽的概念如 Monad, Higher Kinded Type，Transformer 等等。
- 第二是新的东西需要自己完全可以掌控（团队具备源代码级的理解和修改能力）。

## 1 开发语言与运行环境

[Scala 2.13 or later](https://www.Scala-lang.org/download/)。 运行环境为 JDK 11 or later.

学习 Scala 需要掌握一些基本的 FP 编程概念， [Functional Programming, Simplified](https://alvinalexander.com/scala/functional-programming-simplified-book/) 是很好的介绍。

另外就是需要了解 `case class` 的概念，在 Scala 编程里用途广泛。 [Type class from the ground up 的 Youtub 视频](https://youtu.be/3BM4IEziqIM) 给出了非常好的介绍，建议一定看懂理解。

[Functional Programming with Effects](https://youtu.be/30q6BkBv5MY) 介绍了函数式编程的一些概念。

## 2 IDE

The latest VS Code and [Metals](https://scalameta.org/metals/docs/editors/vscode.html)。基本成熟，尤其方便多语言全栈编程。

遇到特别难解决的问题，就删除 `.bloop` 和 `.metals` 重新 build。

## 3 Build 工具

[Mill](http://www.lihaoyi.com/mill/) 0.7.3 or later. 是 Scala 原生的 build tool。吸收了 Bazel 等工具的有点。概念清晰，使用简单方便，也容易定制所需的功能。缺点是还不到 1.0 并且插件较少。但是大方向非常好。

最好的学习资料就是官方文档 [Mill](http://www.lihaoyi.com/mill/)，[演示代码](https://www.lihaoyi.com/post/WorkingwithDatabasesusingScalaandQuill.html) 和 [源代码](https://github.com/lihaoyi/mill) 。

Haoyi 的[介绍视频](https://youtu.be/j6uThGxx-18)。

## 4 FP Lib

采用 [ZIO](https://zio.dev/)。利用文档了解其基本功能后，需要了解模块化依赖（DI）工具 [`ZLayer`](https://zio.dev/docs/howto/howto_use_layers)。

学习资源有：

- [A Tour of ZIO](https://youtu.be/TWdC7DhvD8M)
- [Getting sstarted with ZIO](https://youtu.be/6A1SA5Be9qw)

## 5 Logging

采用基于 Slf4j 和 Logback 的 [BlindSight](https://tersesystems.github.io/blindsight/)， 并在其上开发相应的 ZIO logging API。不要使用 ZIO 自己的 logging 库，完全是一场灾难。

## 6 数据库访问

[quill](https://getquill.io/) 用于生成 SQL， 比 Slick 设计要好，更新也快。[官方文档](https://getquill.io/) 是最好的学习资源。

[Dualistic Quotations in Quill](https://youtu.be/sqyAa4W7GDo) 视频介绍了其实现机理和一些功能。

## 7 Web Server

使用异步 Java 库（不用其 Scala API， 更新不及时而且有些累赘） [Vert.X Core](https://vertx.io/docs/vertx-core/java/) 和 [Vert.x Web](https://vertx.io/docs/vertx-web/java/)。最好的学习资料就是其官网文档，比较清晰。

我们在此之上集成了 ZIO 做应用基本的异步并发处理。 REST API 可以尝试其生态库的其它库 -- 待定。

客户端 HTML 生成则采用 [Scalatags](https://www.lihaoyi.com/scalatags/)。原生的 Scala 代码生成 HTML 非常方便、高效而且灵活。

## 8 测试

待定。

## 9 其它工具

- JSON 序列化：先尝试 [uPickle](https://www.lihaoyi.com/upickle/)。如果不能满足需求可以尝试 [circe](https://circe.github.io/circe/)。
- 配置管理： 用 [PureConfgu](https://pureconfig.github.io/)。

## 10 不建议采用

- Akka 技术栈包括：Akka Actor， Akka HTTP, Play, Lagom。过于复杂难用。
- Cats / Scalaz： 这二大类生态太多高级概念，而且缺乏代码规范。
- Scalajs： 配置和类型转换太累。
- sttp：作为 Http clien 类型比较复杂。
- cask web framework: 虽然比 Play 好，但是不成熟而且功能有限。
