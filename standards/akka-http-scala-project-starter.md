# Akka-HTTP-Scala 项目搭建

## 1 开发工具、环境、主要依赖

- JDK 11
- [Scala 2.13.1 or later](https://www.Scala-lang.org/download/)
- [sbt 1.3.4 or later](https://www.scala-sbt.org/1.x/docs/)
- The latest VS Code and [Metals](https://scalameta.org/metals/docs/editors/vscode.html)
- Mysql 5.7
- Redis 4.0

- Akka 2.6.1 or later
- Akka Http 10.11 or later

## 2 sbt 项目目录

```text
├── src
│　 ├── main
│　 │　 ├── java
│　 │　 ├── resources
│　 │　 └── scala
│　 ├── test
│　 │　 ├── java
│　 │　 ├── resources
│　 │　 └── scala
├── build.sbt: the build file
├── project: sbt plugins and build helper code
│　 ├── build.properties
│　 ├── plugins.sbt
```

## 3 初始化项目-复杂版

sbt 没有类似 sbt init 的功能，无法自动创建符合 sbt 配置的项目结构。

### 3.1 创建文件结构

本地创建项目文件夹 your-project-name 及如第 2 点所述的标准 sbt 项目目录，重点关注如下两个文件并填充内容：

- project/build.properties
- build.sbt

project/build.properties 示例内容：

```text
sbt.version = 1.2.8
```

build.sbt 示例内容：

```sbt
// 定义一些通用的build 设置，可所有Scala服务共用
lazy val commonSettings = Seq(
  organization := "com.tehang", // 代码组织，我们属于特航
  scalaVersion := "2.13.0" //  Scala 的版本号
)

// 当前项目本身的一些build配置
lazy val root = (project in file("."))
  .settings(
    commonSettings,
    name := "your-project-name", //  项目的名字
    version := "0.0.1", // 当前项目的版本
  )

libraryDependencies ++= {
  val akkaVersion = "2.5.23"
  val httpVersion = "10.1.8"
  Seq(
    "com.typesafe.akka" %% "akka-stream" % akkaVersion,
    "com.typesafe.akka" %% "akka-http" % httpVersion,
    "com.typesafe.akka" %% "akka-http-spray-json" % httpVersion,

    // For testing
    "com.typesafe.akka" %% "akka-stream-testkit" % akkaVersion % Test,
    "com.typesafe.akka" %% "akka-http-testkit" % httpVersion % Test,
    "com.typesafe.akka" %% "akka-testkit" % akkaVersion % Test,
    "org.scalatest" %% "scalatest" % "3.0.8" % Test,

    // Using slf4j with logback as backend
    "com.typesafe.akka" %% "akka-slf4j" % akkaVersion,
    "ch.qos.logback" % "logback-classic" % "1.2.3",

    // Using jackson for (un)marshalling
    "de.heikoseeberger" %% "akka-http-jackson" % "1.27.0"

  )
}
```

### 3.2 书写业务逻辑

在 IDE/TEXT EDITOR 中打开刚才创建的文件夹，在 src/main/scala 路径下新建 Application.scala 文件，书写 WEB SERVER 启动类，示例如下：

```scala
import akka.actor._
import akka.stream._
import akka.stream.scaladsl._
import akka.http.scaladsl.model._
import akka.http.scaladsl.Http
import akka.http.scaladsl.server.Directives._
import scala.concurrent._


object HelloHttp extends App {
  implicit val httpSys = ActorSystem("httpSys")
  implicit val httpMat = ActorMaterializer()
  implicit val httpEc = httpSys.dispatcher

  val (host,port) = ("localhost",8088)

  val services: Flow[HttpRequest, HttpResponse, Any] = path("hello") {
    get {
      complete(HttpEntity(ContentTypes.`text/html(UTF-8)`,"<h> Hello World! </h>"))
    }
  }

  val futBinding: Future[Http.ServerBinding] = Http().bindAndHandle(services,host,port)

  println(s"Server running at $host $port. Press any key to exit ...")

  scala.io.StdIn.readLine()

  futBinding.flatMap(_.unbind())
    .onComplete(_ => httpSys.terminate())
}
```

### 3.3 编译构建运行

使用 IDE：

- 运行 Application.scala 文件的方法即可启动 web server

使用 sbt：

- Compile: `sbt compile`
- Test: `sbt test`
- Run: `sbt run`
- Usage: `curl http://localhost:8088/hello`

sbt 的指令之间有依赖关系，比如 run 指令会先执行 compile 作为前置条件。

## 4 初始化项目-简单版

sbt 虽然没有类似 maven 自动初始化项目结构的功能，但是内嵌（sbt 0.13.13 及后版本）了借助 [Gilter8](http://www.foundweekends.org/giter8/) 工具导入项目模版的功能。基于 Lightbend 官方给出的一个托管在 github 上的 scala 项目模版，可经由如下步骤使用：

- 执行命令：`sbt new scala/scala-seed.g8`
- 根据提示输入项目名
- 在 IDE 或者文本编辑器中打开创建好的项目文件夹（如 IDEA open），书写业务逻辑

前两步可以简化为使用命令 `sbt new scala/scala-seed.g8 --name=your-project-name`。基于这种思想，我们可以开发自己的项目模版，保持各项目的统一。对于 Akka HTTP 项目可参考的模版有 [akka-http-quickstart-scala.g8](https://github.com/akka/akka-http-quickstart-scala.g8)
