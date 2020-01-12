# 使用的软件版本

更新时间 2019-12-25

## 1 开发工具、环境

- JDK 11
- [Scala 2.13.1 or later](https://www.Scala-lang.org/download/)
- [sbt 1.3.4 or later](https://www.scala-sbt.org/1.x/docs/)
- The latest VS Code and [Metals](https://scalameta.org/metals/docs/editors/vscode.html)

## 2 数据存储

- Mysql 5.7
- Redis 4.0

## 3 Akka

- Akka 2.6.1 or later
- Akka Http 10.11 or later
- Play 2.8.0 or later

## 4 Http Client

- 使用 Play 的用其内含的 WSClient。
- 使用 Akka Http 的用其内含 HTTP client API。

不建议使用 sttp 是因为其封装了很多不必要的高级函数式模式。比如过长的类型、各种返回模式对于初级程序员不友好，而且多一层封装也带来了理解和处理的额外工作。

## 5 JSON Codec

如果使用的类库有缺省的 JSON Codec， 比如 Play、Akka Http 和 Lagom 都有建议的JSON codec，最好使用推荐的。自己做集成适配会带来不必要的工作量和理解难度。如果上述不能满足要求，可以考虑 [circe](https://circe.github.io/circe/)。
