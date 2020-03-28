# sbt Build

本文包含了 sbt build 的最佳实践和标准。包括仓库，编译器开关，标准版本等。

## 1 设置全局仓库

使用国内仓库镜像可以加快下载速度。此处的设置参考了 [Library Dependencies](https://www.scala-sbt.org/1.x/docs/Library-Dependencies.html), [Library Management](https://www.scala-sbt.org/1.x/docs/Library-Management.html)。

### 第一步：设置全局仓库

在用户的根目录下创建 `~/.sbt/repositories` 文件。内容如下。其中的 `huaweicloud-maven: https://repo.huaweicloud.com/repository/maven/` 是国内镜像，也可以用 `aliyun-maven: http://maven.aliyun.com/nexus/content/groups/public`。其它的内容则来自[缺省的 sbt 仓库设置](https://github.com/sbt/sbt/blob/1.3.x/launch/src/main/input_resources/sbt/sbt.boot.properties)。

```text
[repositories]
local
local-preloaded-ivy: file:///${sbt.preloaded-${sbt.global.base-${user.home}/.sbt}/preloaded/}, [organization]/[module]/[revision]/[type]s/[artifact](-[classifier]).[ext]
local-preloaded: file:///${sbt.preloaded-${sbt.global.base-${user.home}/.sbt}/preloaded/}
huaweicloud-maven: https://repo.huaweicloud.com/repository/maven/
maven-central
sbt-maven-releases: https://repo.scala-sbt.org/scalasbt/maven-releases/, bootOnly
sbt-maven-snapshots: https://repo.scala-sbt.org/scalasbt/maven-snapshots/, bootOnly
typesafe-ivy-releases: https://repo.typesafe.com/typesafe/ivy-releases/, [organization]/[module]/[revision]/[type]s/[artifact](-[classifier]).[ext], bootOnly
sbt-ivy-snapshots: https://repo.scala-sbt.org/scalasbt/ivy-snapshots/, [organization]/[module]/[revision]/[type]s/[artifact](-[classifier]).[ext], bootOnly
```

### 第二步：设置所有项目都使用上面的全局仓库配置，忽略自身仓库配置

使用全局仓库配置需要通过 Java System Property 进行设置。SBT 提供了三种方法。

方法一（推荐的方法）： 修改 `sbt-1.3.x/conf/sbtopts` 文件（Windows 修改 `sbtconfig.txt`），在文件末尾增加一行：`-Dsbt.override.build.repos=true`。
方法二：设置环境变量。 `export SBT_OPTS="-Dsbt.override.build.repos=true"`
方法三：`sbt` 命令参数。 `sbt -Dsbt.override.build.repos=true`

### 第三步：确认使用全局仓库

创建一个 Scala 项目，运行 `sbt` 后在提示符下输入命令：`show overrideBuildResolvers` 应该看到结果为 `true`。

查看搜索的仓库配置，输入命令：`show fullResolvers`，确认配置的全局仓库。

编译项目 `compile` 然后确认下载的仓库资源。在 Mac `~/Library/Caches/Coursier/v1/https` 目录下应该可以看到 `repo.huaweicloud.com` 目录和下载的资源。

### 第四步：注意事项

正常情况下，`repositories` 文件应该在 `~/.sbt` 目录，如果放到别处，则应该在 `sbt-1.3.x/conf/sbtopts` 文件或 `sbt` 命令行给出路径。格式为 `-Dsbt.repository.config=<path-to-your-repo-file>`。

由于设置了 `sbt.override.build.repos=true`, 具体项目的 `resovlers` 或 `externalResolvers` 会不起作用了。新的仓库应该配置在 `~/.sbt/repositories`。

不在仓库的依赖可以直接给出具体的 `jar` 路径。比如：`libraryDependencies += "slinky" % "slinky" % "2.1" from "https://slinky2.googlecode.com/svn/artifacts/2.1/slinky.jar"`。

## `project/build.properties`

```text
sbt.version=1.3.8
```

## 2 `build.sbt`

### 2.1 基本配置

```scala
// ThisBuild means "entire build", its setting applies to all projects.
ThisBuild / organization := "com.example"
ThisBuild / scalaVersion := "2.13.1"
ThisBuild / version      := "0.1.0-SNAPSHOT"

// copied from https://nathankleyn.com/2019/05/13/recommended-scalac-flags-for-2-13/
scalacOptions ++= Seq(
  "-deprecation", // Emit warning and location for usages of deprecated APIs.
  "-explaintypes", // Explain type errors in more detail.
  "-feature", // Emit warning and location for usages of features that should be imported explicitly.
  "-language:existentials", // Existential types (besides wildcard types) can be written and inferred
  "-language:experimental.macros", // Allow macro definition (besides implementation and application)
  "-language:higherKinds", // Allow higher-kinded types
  "-language:implicitConversions", // Allow definition of implicit functions called views
  "-unchecked", // Enable additional warnings where generated code depends on assumptions.
  "-Xcheckinit", // Wrap field accessors to throw an exception on uninitialized access.
  "-Xfatal-warnings", // Fail the compilation if there are any warnings.
  "-Xlint:adapted-args", // Warn if an argument list is modified to match the receiver.
  "-Xlint:constant", // Evaluation of a constant arithmetic expression results in an error.
  "-Xlint:delayedinit-select", // Selecting member of DelayedInit.
  "-Xlint:doc-detached", // A Scaladoc comment appears to be detached from its element.
  "-Xlint:inaccessible", // Warn about inaccessible types in method signatures.
  "-Xlint:infer-any", // Warn when a type argument is inferred to be `Any`.
  "-Xlint:missing-interpolator", // A string literal appears to be missing an interpolator id.
  "-Xlint:nullary-override", // Warn when non-nullary `def f()' overrides nullary `def f'.
  "-Xlint:nullary-unit", // Warn when nullary methods return Unit.
  "-Xlint:option-implicit", // Option.apply used implicit view.
  "-Xlint:package-object-classes", // Class or object defined in package object.
  "-Xlint:poly-implicit-overload", // Parameterized overloaded implicit methods are not visible as view bounds.
  "-Xlint:private-shadow", // A private field (or class parameter) shadows a superclass field.
  "-Xlint:stars-align", // Pattern sequence wildcard must align with sequence component.
  "-Xlint:type-parameter-shadow", // A local type parameter shadows a type already in scope.
  "-Ywarn-dead-code", // Warn when dead code is identified.
  "-Ywarn-extra-implicit", // Warn when more than one implicit parameter section is defined.
  "-Ywarn-numeric-widen", // Warn when numerics are widened.
  "-Ywarn-unused:implicits", // Warn if an implicit parameter is unused.
  "-Ywarn-unused:imports", // Warn if an import selector is not referenced.
  "-Ywarn-unused:locals", // Warn if a local definition is unused.
  "-Ywarn-unused:params", // Warn if a value parameter is unused.
  "-Ywarn-unused:patvars", // Warn if a variable bound in a pattern is unused.
  "-Ywarn-unused:privates", // Warn if a private member is unused.
  "-Ywarn-value-discard", // Warn when non-Unit expression results are unused.
  "-Ybackend-parallelism",
  "8", // Enable paralellisation — change to desired number!
  "-Ycache-plugin-class-loader:last-modified", // Enables caching of classloaders for compiler plugins
  "-Ycache-macro-class-loader:last-modified" // and macro definitions. This can lead to performance improvements.
)
```

### 2.2 Akka 依赖

```scala
libraryDependencies ++= {
  val akkaVersion = "2.6.1"
  val httpVersion = "10.1.11"

  Seq(
    "com.typesafe.akka" %% "akka-stream" % akkaVersion,
    "com.typesafe.akka" %% "akka-http"  % httpVersion,
    "com.typesafe.akka" %% "akka-http-spray-json"  % httpVersion,
    "com.typesafe.akka" %% "akka-slf4j" % akkaVersion,
    "ch.qos.logback"    %  "logback-classic" % "1.2.3",
    "com.typesafe.akka" %% "akka-testkit" % akkaVersion % Test,
  )
}
```
