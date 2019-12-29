# Scala 代码规范

尽可能在 IDE 和 sbt [Scalafmt](https://scalameta.org/scalafmt/) 设置代码规范.

## Scalac 选项

拷贝 [Recommended Scalac Flags For 2.13](https://nathankleyn.com/2019/05/13/recommended-scalac-flags-for-2-13/)。

## 命名

- Scala 源文件名用 PascalCase。比如 `CustomerOrder.scala`。
- 除非循环只有一句话，不要用 `i`。双重或以上循环禁用简写。Use `index` instead of `i`
- 常量名用 PascalCase。比如 `OutputFileTmpPath = "/tmp/"` 。

## 大小

- Code blocks should not exceed 30 lines.

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
