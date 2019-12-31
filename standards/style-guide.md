# Scala 代码规范

尽可能在 IDE 和 sbt [Scalafmt](https://scalameta.org/scalafmt/) 设置代码规范.

## Scalac 选项

拷贝 [Recommended Scalac Flags For 2.13](https://nathankleyn.com/2019/05/13/recommended-scalac-flags-for-2-13/)。

## 命名规范

- 多数业务名称都会在多处使用，按产品和业务定义名词已经响应的英文名词。英文名词用于程序的各种文件、包、类已经变量命名。
- DDD（domain driven design）：每个子域有不同的定义，需要分布式维护清晰的定义。可以放入源文件库。
- Scala 源文件名用 PascalCase。比如 `CustomerOrder.scala`。
- 除非循环只有一句话，不要用 `i`。双重或以上循环禁用简写。Use `index` instead of `i`
- 常量名用 PascalCase。比如 `OutputFileTmpPath = "/tmp/"` 。

## Package Name

包命名通常有包含四层：`com.公司.产品.模块`。第三层是大的产品名称，第四层为顶层模块。根据需要可以有更多层，但是不建议超过 7 层。比如人力资源报表包名为 `com.tehang.hr.reports`。Play controllers 定义在 `com.tehang.hr.reports.controllers`, 公用的模块在 `com.tehang.hr.utility`。

## 大小

- 一行长度不超过 80（Scalafmt 的缺省值，明确定义）：`maxColumn = 80`。
- 一个函数实现一个目的明确的功能，不要超过 7 个操作，所有操作在同一抽象层 [SLA: Single Leval Abstraction](http://principles-wiki.net/principles:single_level_of_abstraction).
- 一个函数不要超过 30 行，不算注释，包括空行。超过的拆分。
- 一个类不要超过 7 个函数。超过的拆分。
- 一个文件通常只包含一个类及其伙伴对象，不要超过 200 行。

## 引入

- 用具体引入，避免通配符。如果引入过多，说明文件太大，应该拆分。
- 大类顺序采用： Scala/Java 标准库、框架库、公司和自己定义的类型三大类。
- 类中应用顺序：引入按字母排序。

## `class` & `trait`

- When a class extending a trait,
  only `override` for method with default method,
  do not use `override` for abstract method of the trait.

## 尽量使用 `final case class` 而不是 `class`

## 类型标注

- 类里所有公共值都需要显式标注数据类型。

## 不用 Null 和 Exception，使用

- `Option` 对于值可能为空
- `Either` 对于自己明白的错误
- `Try` 打包可能抛出异常的调用返回值

## 工具（待确定）

- scapegoat
- wartremover
- scala linter
- scalastyle
- scalafix
