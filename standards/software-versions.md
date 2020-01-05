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

- [sttp 2.0 or later](https://sttp.readthedocs.io/en/latest/) with [akka-http backend](https://sttp.readthedocs.io/en/latest/backends/akkahttp.html).

## 5 JSON Codec

Use [circe](https://circe.github.io/circe/) for its support for more than 22 data fields.
