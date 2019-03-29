# russross/blackfriday [![translate-svg]][translate-list]

<!-- [![explain]][source]  -->

<!-- [![size-img]][size] -->

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
[size-img]: https://packagephobia.now.sh/badge?p=Name
[size]: https://packagephobia.now.sh/result?p=Name

「 Blackfriday{黑色星期五}是一个[markdown][1]处理器，用[Go][2]实现. 」

[中文](./readme.md) | [english](https://github.com/russross/blackfriday)

---

## 校对 🀄️

<!-- doc-templite START generated -->
<!-- repo = 'russross/blackfriday' -->
<!-- commit = '05f3235734ad95d0016f6a23902f06461fcf567a' -->
<!-- time = '2018-09-17' -->
翻译的原文 | 与日期 | 最新更新 | 更多
---|---|---|---
[commit] | ⏰ 2018-09-17 | ![last] | [中文翻译][translate-list]

[last]: https://img.shields.io/github/last-commit/russross/blackfriday.svg
[commit]: https://github.com/russross/blackfriday/tree/05f3235734ad95d0016f6a23902f06461fcf567a

<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[If help, **buy** me coffee —— 营养跟不上了，给我来瓶营养快线吧! 💰](https://github.com/chinanf-boy/live-need-money)

---

# Blackfriday[![Build Status][buildsvg]][buildurl][![Godoc][godocv2svg]][godocv2url]

Blackfriday{黑色星期五}是一个[markdown][1]处理器，用[Go][2]实现. 它对输入是偏执严格的(所以你可以安全地提供用户提供的数据),且它很快,它支持常见的扩展(表格,智能标点符号替换等).并且它对所有 utf-8(unicode)输入都是安全的.

目前支持 HTML 输出以及 Smartypants 扩展.

> 该 Smartypants 扩展，将 ascii 中的引号，连接号，省略号转换为对应的 HTML 等价表示。

它起源于 C 的[sundown][3], 并翻译翻译成 Go.

### 目录

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [安装](#%E5%AE%89%E8%A3%85)
- [版本](#%E7%89%88%E6%9C%AC)
  - [`dep` 已知问题](#dep-%E5%B7%B2%E7%9F%A5%E9%97%AE%E9%A2%98)
- [用法](#%E7%94%A8%E6%B3%95)
  - [v1](#v1)
  - [v2](#v2)
  - [清理不受信任的内容](#%E6%B8%85%E7%90%86%E4%B8%8D%E5%8F%97%E4%BF%A1%E4%BB%BB%E7%9A%84%E5%86%85%E5%AE%B9)
  - [自定义选项,v1](#%E8%87%AA%E5%AE%9A%E4%B9%89%E9%80%89%E9%A1%B9v1)
  - [自定义选项,v2](#%E8%87%AA%E5%AE%9A%E4%B9%89%E9%80%89%E9%A1%B9v2)
  - [`blackfriday-tool`](#blackfriday-tool)
  - [消毒的 anchor 名称](#%E6%B6%88%E6%AF%92%E7%9A%84anchor%E5%90%8D%E7%A7%B0)
- [特征](#%E7%89%B9%E5%BE%81)
- [扩展](#%E6%89%A9%E5%B1%95)
- [其他渲染器](#%E5%85%B6%E4%BB%96%E6%B8%B2%E6%9F%93%E5%99%A8)
- [TODO](#todo)
- [执照](#%E6%89%A7%E7%85%A7)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 安装

Blackfriday 与任何现代 Go 版本兼容. 安装 Go 和 git 后:

```
go get -u gopkg.in/russross/blackfriday.v2
```

将下载,编译和安装包到您的`$GOPATH`目录层次结构

## 版本

目前维护和推荐版本的 Blackfriday 是`v2`.它正在自己的分支上开发:<https://github.com/russross/blackfriday/tree/v2>, 并且文档可在以下处获得<https://godoc.org/gopkg.in/russross/blackfriday.v2>.

若使用`go get`获取，它是通过[gopkg.in 网站][6]的`gopkg.in/russross/blackfriday.v2`, 但我们强烈建议使用包管理工具[dep][7]要么[Glide][8]并利用语义版本控制. 使用包管理,您应该导入`github.com/russross/blackfriday`并指定您使用的是 2.0.0 版.

与 v1 相比,版本 2 提供了许多改进:

- 清理-Cleaned up API
- 可单独运行[`Parse`][4],它为文档生成一个抽象语法树 AST
- 最新的错误修复
- 灵活地轻松添加自己的渲染扩展

潜在的缺点:

- 我们的基准测试表明`v2`比`v1`略慢.目前约占球场 15%.
- API 破损.如果您无法负担修改代码，以遵守新 API 并且不太关心新功能,那么`v2`可能不适合您.
- 几个错误的修复拉下了,仍需要向前移植到`v2`.看问题[#348](https://github.com/russross/blackfriday/issues/348)跟踪.

如果你仍然对遗产`v1`感兴趣,你可以从`github.com/russross/blackfriday`中导入它.可以在此处找到旧版 v1 的文档:<https://godoc.org/github.com/russross/blackfriday>

### `dep` 已知问题

存在 Blackfriday `v1`*以及*与`dep`的已知问题.目前`dep`将 semver 版本优先于其他任何版本,并选择最新版本,加上它不适用`[[constraint]]`说明者可以传递包裹.因此,如果您使用的是 Blackfriday v1 的东西,则有些东西不能使用`dep`然而,你会得到 Blackfriday v2, 你的第一个依赖将无法建立.

它有几个修复,在这里记录:<https://github.com/golang/dep/blob/master/docs/FAQ.md#how-do-i-constrain-a-transitive-dependencys-version>

与此同时,`dep`团队正在研究对传递依赖性问题的约束的更通用的解决方案:<https://github.com/golang/dep/issues/1124>.

## 用法

### v1

对于基本用法,只需将输入，input 字节切片并调用:

```
output := blackfriday.MarkdownBasic(input)
```

这使得它没有启用扩展.要获得更有用的功能集,请改用:

```
output := blackfriday.MarkdownCommon(input)
```

### v2

对于最明智的 markdown 处理,只需输入，input 字节切片并调用:

```go
output := blackfriday.Run(input)
```

上面的 input 将被解析,并且输出使用一组最常用的扩展程序进行渲染 . 如果您想要最基本的功能集,与裸 Markdown 规范相对应,请使用:

```go
output := blackfriday.Run(input, blackfriday.WithNoExtensions())
```

### 清理不受信任的内容

Blackfriday 本身无法防范恶意内容.如果您正在处理用户提供的 markdown,我们建议您通过 HTML 清理程序运行 Blackfriday 的输出,例如[Bluemonday 忧愁星期一][5].

以下是 Blackfriday 与 Bluemonday 一起使用的简单示例:

```go
import (
    "github.com/microcosm-cc/bluemonday"
    "gopkg.in/russross/blackfriday.v2"
)

// ...
unsafe := blackfriday.Run(input)
html := bluemonday.UGCPolicy().SanitizeBytes(unsafe)
```

### 自定义选项,v1

如果你想自定义选项集,首先得到一个渲染器(目前只有 HTML 输出引擎),然后用它来调用更通用`Markdown`功能.例如,请参阅的实现`MarkdownBasic`和`MarkdownCommon`在`markdown.go`.

### 自定义选项,v2

如果要自定义选项集,请使用`blackfriday.WithExtensions`,`blackfriday.WithRenderer`和`blackfriday.WithRefOverride`.

### `blackfriday-tool`

你也可以看看`blackfriday-tool`，有关如何使用它的更完整示例. 使用以下命令下载并安装:

```
go get github.com/russross/blackfriday-tool
```

这是一个简单的命令行工具,允许您使用独立程序处理 markdown 文件.如果您只是在寻找一些示例代码,您还可以直接在 github 上浏览源代码:

- <http://github.com/russross/blackfriday-tool>

**请注意**,如果您还没有 Blackfriday, 那可以先安装`blackfriday-tool`，因为除了工具本身之外,还会下载和安装 blackfriday. 工具二进制文件将安装在`$GOPATH/bin`.这是一个静态链接的二进制文件,可以将其复制到您需要的任何位置,而无需担心依赖项和库版本.

### 消毒的 anchor 名称

> 作用如: `This is a header` => `this-is-a-header`

Blackfriday 包括，一个用于创建与给定输入文本相对应的已消毒 anchor 名称的算法 .该算法用于为标题创建 anchor 点，若`EXTENSION_AUTO_HEADER_IDS`已启用. 该算法具有规范, 因此其他包可以创建兼容的 anchor 名称和到这些 anchor 的链接.

规范位于<https://godoc.org/github.com/russross/blackfriday#hdr-Sanitized_Anchor_Names>.

[`SanitizedAnchorName`](https://godoc.org/github.com/russross/blackfriday#SanitizedAnchorName)正是公开的函数,并可用于创建 blackfriday 生成的 anchor 名称的兼容链接.该算法也在一个小的独立包中实现[`github.com/shurcooL/sanitized_anchor_name`](https://godoc.org/github.com/shurcooL/sanitized_anchor_name).它对于需要小包，并且不需要 blackfriday 的完整功能的客户来说非常有用.

## 特征

支持[sundown][3]的所有功能,包括:

- **兼容性**. 通过 Markdown v1.0.3 测试套件，且带`--tidy`选项. 若没有`--tidy`,差异主要在于空格和 HTML 实体转义,其中 blackfriday 更一致和清洁.

- **常见的扩展**,包括表格支持,围栏代码块,自动链接,删除线,非严格强调(加粗)等.

- **安全**.Blackfriday 在解析时是偏执狂,可以安全地提供不受信任的用户输入，而不用担心会发生不良事件.测试套件压力会测试这个，且结果没有已知的输入使其崩溃. 如果你找到一个,请告诉我,并把它输入给我.

  注意:在此上下文中的"安全"是指*仅限运行时安全*.为了保护自己免受不受信任内容中的 JavaScript 注入,请参阅[这个例子](https://github.com/russross/blackfriday#sanitize-untrusted-content).

- **快速处理**.它足够快,可以在大多数 Web 应用程序中按需呈现,而无需缓存输出.

- **线程安全**.您可以在不同的 goroutine 中运行多个解析器，而不会产生不良影响.不依赖于全局共享状态.

- **最小的依赖关系**.Blackfriday 仅依赖于 Go 中的标准库包.源代码非常独立,因此很容易添加到任何项目,包括 Google App Engine 项目.

- **符合标准**.使用适用于 HTML 4.01 和 XHTML 1.0 Transitional 的 W3C 验证工具成功验证输出.

## 扩展

除了标准的 markdown 语法之外,此包还实现了以下扩展:

- **词内强调抑制**.在梳理代码时,`_`字符常用于单词内部(如:`hello_world`),因此将 markdown 解释为强调命令通常是错误的.Blackfriday 允许您将所有强调标记视为正常字符, 当它们出现在单词内部时.

- **表格**.可以使用简单的语法在输入中绘制表来创建表:

  ```
  Name    | Age
  --------|------
  Bob     | 27
  Alice   | 23
  ```

- **受控代码块**.除了标记代码块的常规 4 空格缩进之外,您还可以显式标记它们并提供语言(以使语法高亮简化).只需将其标记为:

  ````
  ``` go
  func getTrue() bool {
      return true
  }
  ```
  ````

  您可以使用 3 个或更多反引号来标记块的开头,并使用相同的数字来标记块的结尾.

  要在使用 bluemonday HTML 清理程序时，保留被保护的代码块类,请使用以下策略:

  ```go
  p := bluemonday.UGCPolicy()
  p.AllowAttrs("class").Matching(regexp.MustCompile("^language-[a-zA-Z0-9]+$")).OnElements("code")
  html := p.SanitizeBytes(unsafe)
  ```

- **定义列表**.一个简单的定义列表由单行术语后跟冒号，和该术语的定义组成.

  ```
  Cat
  : Fluffy animal everyone likes

  Internet
  : Vector of transmission for pictures of cats
  ```

  术语必须用`空行`与前一个定义分开.

- **脚注**.文本中的标记将成为上标数字;脚注定义将放在文档末尾的脚注列表中.脚注如下所示:

  ```
  This is a footnote.[^1]

  [^1]: the footnote text.
  ```

- **自动链上**.Blackfriday 可以找到未明确标记为链接的 URL，并将其转换为链接.

- **删除线**.使用两个波浪(`~~`)标记应该划掉的文本.

- **行线断裂**.启用此扩展程序(默认情况下,它已关闭`MarkdownBasic`和`MarkdownCommon`便利功能),输入中的换行符，转换为输出中的换行符. (也就是各个换行)

- **聪明的报价**.支持 Smartypants 样式的标点替换,将普通的双引号和单引号转换为引号等.

- **LaTeX 风格的破折号解析**是一个额外的选择,这里`--`被翻译成`&ndash;`,和`---`被翻译成`&mdash;`.这与大多数 smartypants 处理器不同,后者将单个连字符转换为`ndash`,将双连字符转换为`mdash`.

- **智能分数**,任何看起来像分数的东西都被翻译成合适的 HTML(而不是像大多数智能处理器那样的一些特殊情况).例如,`4/5`变`<sup>4</sup>&frasl;<sub>5</sub>`,呈现为<sup>4</sup>/<sub>五</sub>.

## 其他渲染器

Blackfriday 的结构允许替代渲染引擎.以下是一些关注的项目:

- [github_flavored_markdown](https://godoc.org/github.com/shurcooL/github_flavored_markdown):提供 GitHub Flavored Markdown 渲染器,带有围栏代码块突出显示,可点击的标题 anchor 链接.

  它不可自定义,其目标是生成 HTML 输出与[GitHub Markdown API 端点](https://developer.github.com/v3/markdown/#render-a-markdown-document-in-raw-mode)对上,除了渲染是在本地执行.

- [markdownfmt](https://github.com/shurcooL/markdownfmt):像 gofmt,但用于 markdown.

- [LaTeX output](https://bitbucket.org/ambrevar/blackfriday-latex):将输出呈现为 LaTeX.

- [bfchroma](https://github.com/Depado/bfchroma/):提供方便的代码高亮显示库[chroma](https://github.com/alecthomas/chroma)集成.bfchroma 仅与 Blackfriday 的 v2 兼容,并提供与 Blackfriday 随时使用的嵌入式渲染器,以及进一步定制的选项和方法.

## TODO

- 更多单元测试
- 改进 Unicode 支持.它不了解所有 Unicode 规则(关于什么构成字母,标点符号等),因此在某些情况下它可能无法正确检测字的边界.所有 UTF-8 输入都是安全的.

## 执照

[Blackfriday 根据简化 BSD 许可证分发](LICENSE.txt)

[1]: https://daringfireball.net/projects/markdown/ 'Markdown'
[2]: https://golang.org/ 'Go Language'
[3]: https://github.com/vmg/sundown 'Sundown'
[4]: https://godoc.org/gopkg.in/russross/blackfriday.v2#Parse 'Parse func'
[5]: https://github.com/microcosm-cc/bluemonday 'Bluemonday'
[6]: https://labix.org/gopkg.in 'gopkg.in'
[7]: https://github.com/golang/dep/ 'dep'
[8]: https://github.com/Masterminds/glide 'Glide'
[buildsvg]: https://travis-ci.org/russross/blackfriday.svg?branch=master
[buildurl]: https://travis-ci.org/russross/blackfriday
[godocv2svg]: https://godoc.org/gopkg.in/russross/blackfriday.v2?status.svg
[godocv2url]: https://godoc.org/gopkg.in/russross/blackfriday.v2
