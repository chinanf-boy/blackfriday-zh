# russross/blackfriday [![explain]][source] [![translate-svg]][translate-list]

<!-- [![size-img]][size] -->

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
[size-img]: https://packagephobia.now.sh/badge?p=Name
[size]: https://packagephobia.now.sh/result?p=Name

「 desc 」

[中文](./readme.md) | [english](https://github.com/russross/blackfriday)

---

## 校对 🀄️

<!-- doc-templite START generated -->
<!-- repo = 'russross/blackfriday' -->
<!-- commit = '05f3235734ad95d0016f6a23902f06461fcf567a' -->
<!-- time = '2018-09-17' -->

| 翻译的原文 | 与日期        | 最新更新 | 更多                       |
| ---------- | ------------- | -------- | -------------------------- |
| [commit]   | ⏰ 2018-09-17 | ![last]  | [中文翻译][translate-list] |

[last]: https://img.shields.io/github/last-commit/russross/blackfriday.svg
[commit]: https://github.com/russross/blackfriday/tree/05f3235734ad95d0016f6a23902f06461fcf567a

<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---

### 目录

<!-- START doctoc -->
<!-- END doctoc -->

黑色星期五[![Build Status][buildsvg]][buildurl]

# [![Godoc][godocv2svg]][godocv2url]

Blackfriday是一个[降价][1]处理器实现于[走][2].它的输入是偏执的(所以你可以安全地提供用户提供的数据),它很快,它支持常见的扩展(表格,智能标点符号替换等).并且它对所有utf-8(unicode)输入都是安全的.

目前支持HTML输出以及Smartypants扩展.

它起源于C的翻译[日落][3].

## 安装

Blackfriday与任何现代Go版本兼容.安装Go和git:

```
go get -u gopkg.in/russross/blackfriday.v2
```

将下载,编译和安装包到您的`$GOPATH`目录层次结构

## 版本

目前维护和推荐版本的Blackfriday是`v2`.它正在自己的分支上开发:<https://github.com/russross/blackfriday/tree/v2>并且文档可在以下处获得<https://godoc.org/gopkg.in/russross/blackfriday.v2>.

它是`go get`通过[gopkg.in][6]在`gopkg.in/russross/blackfriday.v2`,但我们强烈建议使用包管理工具[dep][7]要么[滑行][8]并利用语义版本控制.使用包管理,您应该导入`github.com/russross/blackfriday`并指定您使用的是2.0.0版.

与v1相比,版本2提供了许多改进:

-   清理API
-   单独打电话给[`Parse`][4],它为文档生成一个抽象语法树
-   最新的错误修复
-   灵活地轻松添加自己的渲染扩展

潜在的缺点:

-   我们的基准测试表明v2比v1略慢.目前在球场约占15%.
-   API破损.如果您无法负担修改代码以遵守新API并且不太关心新功能,那么v2可能不适合您.
-   几个错误修复正在落后,仍然需要向前移植到v2.看问题[#348](https://github.com/russross/blackfriday/issues/348)用于跟踪.

如果你仍然对遗产感兴趣`v1`,你可以从中导入它`github.com/russross/blackfriday`.可以在此处找到旧版v1的文档:<https://godoc.org/github.com/russross/blackfriday>

### 已知问题`dep`

使用Blackfriday v1存在已知问题*及物动词*和`dep`.目前`dep`将semver版本优先于其他任何版本,并选择最新版本,加上它不适用`[[constraint]]`说明者可以传递包裹.因此,如果您使用的是使用Blackfriday v1的东西,但有些东西不能使用`dep`然而,你会得到Blackfriday v2,你的第一个依赖将无法建立.

它有几个修复,在这里记录:<https://github.com/golang/dep/blob/master/docs/FAQ.md#how-do-i-constrain-a-transitive-dependencys-version>

与此同时,`dep`团队正在研究对传递依赖性问题的约束的更通用的解决方案:<https://github.com/golang/dep/issues/1124>.

## 用法

### v1

对于基本用法,只需将输入输入字节切片并调用:

```
output := blackfriday.MarkdownBasic(input)
```

这使得它没有启用扩展.要获得更有用的功能集,请改用:

```
output := blackfriday.MarkdownCommon(input)
```

### v2

对于最明智的降价处理,只需将输入输入字节切片并调用:

```go
output := blackfriday.Run(input)
```

您的输入将被解析,并且输出使用一组最常用的扩展程序进行渲染.如果您想要最基本的功能集,与裸Markdown规范相对应,请使用:

```go
output := blackfriday.Run(input, blackfriday.WithNoExtensions())
```

### 清理不受信任的内容

Blackfriday本身无法防范恶意内容.如果您正在处理用户提供的markdown,我们建议您通过HTML清理程序运行Blackfriday的输出,例如[忧愁星期一][5].

以下是Blackfriday与Bluemonday一起使用的简单示例:

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

如果你想自定义选项集,首先得到一个渲染器(目前只有HTML输出引擎),然后用它来调用更一般的`Markdown`功能.例如,请参阅的实现`MarkdownBasic`和`MarkdownCommon`在`markdown.go`.

### 自定义选项,v2

如果要自定义选项集,请使用`blackfriday.WithExtensions`,`blackfriday.WithRenderer`和`blackfriday.WithRefOverride`.

### `blackfriday-tool`

你也可以看看`blackfriday-tool`有关如何使用它的更完整示例.使用以下命令下载并安装:

```
go get github.com/russross/blackfriday-tool
```

这是一个简单的命令行工具,允许您使用独立程序处理markdown文件.如果您只是在寻找一些示例代码,您还可以直接在github上浏览源代码:

-   <http://github.com/russross/blackfriday-tool>

请注意,如果您还没有这样做,请安装`blackfriday-tool`除了工具本身之外,还可以下载和安装blackfriday.工具二进制文件将安装在`$GOPATH/bin`.这是一个静态链接的二进制文件,可以将其复制到您需要的任何位置,而无需担心依赖项和库版本.

### 消毒的锚名称

Blackfriday包括用于创建与给定输入文本相对应的已消毒锚名称的算法.该算法用于为标题创建锚点`EXTENSION_AUTO_HEADER_IDS`已启用.该算法具有规范,因此其他包可以创建兼容的锚名称和到这些锚的链接.

规范位于<https://godoc.org/github.com/russross/blackfriday#hdr-Sanitized_Anchor_Names>.

[`SanitizedAnchorName`](https://godoc.org/github.com/russross/blackfriday#SanitizedAnchorName)公开此功能,并可用于创建与blackfriday生成的锚名称的兼容链接.该算法也在一个小的独立包中实现[`github.com/shurcooL/sanitized_anchor_name`](https://godoc.org/github.com/shurcooL/sanitized_anchor_name).它对于需要小包并且不需要blackfriday的完整功能的客户来说非常有用.

## 特征

支持日落的所有功能,包括:

-   **兼容性**.Markdown v1.0.3测试套件随附了`--tidy`选项.没有`--tidy`,差异主要在于空白和实体逃逸,其中blackfriday更加一致和清洁.

-   **常见的扩展**,包括表支持,围栏代码块,自动链接,删除线,非严格强调等.

-   **安全**.Blackfriday在解析时是偏执狂,可以安全地提供不受信任的用户输入而不用担心会发生不良事件.测试套件压力测试这个并且没有已知的输入使其崩溃.如果你找到一个,请告诉我,并把它输入给我.

    注意:在此上下文中的"安全"是指*仅限运行时安全*.为了保护自己免受不受信任内容中的JavaScript注入,请参阅[这个例子](https://github.com/russross/blackfriday#sanitize-untrusted-content).

-   **快速处理**.它足够快,可以在大多数Web应用程序中按需呈现,而无需缓存输出.

-   **线程安全**.您可以在不同的goroutine中运行多个解析器而不会产生不良影响.不依赖于全球共享状态.

-   **最小的依赖关系**.Blackfriday仅依赖于Go中的标准库包.源代码非常独立,因此很容易添加到任何项目,包括Google App Engine项目.

-   **符合标准**.使用适用于HTML 4.01和XHTML 1.0 Transitional的W3C验证工具成功验证输出.

## 扩展

除了标准的markdown语法之外,此包还实现了以下扩展:

-   **词内强调抑制**.该`_`在讨论代码时,字符常用于单词内部,因此将markdown解释为强调命令通常是错误的.Blackfriday允许您将所有强调标记视为正常字符,当它们出现在单词内部时.

-   **表**.可以使用简单的语法在输入中绘制表来创建表:

    ```
    Name    | Age
    --------|------
    Bob     | 27
    Alice   | 23
    ```

-   **受控代码块**.除了标记代码块的常规4空间缩进之外,您还可以显式标记它们并提供语言(以使语法高亮简化).只需将其标记为:

    ````
    ``` go
    func getTrue() bool {
        return true
    }
    ```
    ````

    您可以使用3个或更多反对来标记块的开头,并使用相同的数字来标记块的结尾.

    要在使用bluemonday HTML清理程序时保留受防护的代码块类,请使用以下策略:

    ```go
    p := bluemonday.UGCPolicy()
    p.AllowAttrs("class").Matching(regexp.MustCompile("^language-[a-zA-Z0-9]+$")).OnElements("code")
    html := p.SanitizeBytes(unsafe)
    ```

-   **定义列表**.一个简单的定义列表由单行术语后跟冒号和该术语的定义组成.

    ```
    Cat
    : Fluffy animal everyone likes

    Internet
    : Vector of transmission for pictures of cats
    ```

    术语必须用空行与前一个定义分开.

-   **脚注**.文本中的标记将成为上标数字;脚注定义将放在文档末尾的脚注列表中.脚注如下所示:

    ```
    This is a footnote.[^1]

    [^1]: the footnote text.
    ```

-   **Autolinking**.Blackfriday可以找到未明确标记为链接的URL并将其转换为链接.

-   **删除线**.使用两个波浪(`~~`)标记应该划掉的文本.

-   **硬线断裂**.启用此扩展程序(默认情况下,它已关闭`MarkdownBasic`和`MarkdownCommon`便利功能),输入中的换行符转换为输出中的换行符.

-   **聪明的报价**.支持Smartypants样式的标点替换,将普通的双引号和单引号转换为引号等.

-   **LaTeX风格的破折号解析**是一个额外的选择,在哪里`--`被翻译成`&ndash;`,和`---`被翻译成`&mdash;`.这与大多数smartypants处理器不同,后者将单个连字符转换为ndash,将双连字符转换为mdash.

-   **智能分数**,任何看起来像分数的东西都被翻译成合适的HTML(而不是像大多数智能处理器那样的一些特殊情况).例如,`4/5`变`<sup>4</sup>&frasl;<sub>5</sub>`,呈现为<sup>4</sup>/<sub>五</sub>.

## 其他渲染器

Blackfriday的结构允许替代渲染引擎.以下是一些注意事项:

-   [github_flavored_markdown](https://godoc.org/github.com/shurcooL/github_flavored_markdown):提供GitHub Flavored Markdown渲染器,带有围栏代码块突出显示,可点击的标题锚链接.

    它不可自定义,其目标是生成相当于的HTML输出[GitHub Markdown API端点](https://developer.github.com/v3/markdown/#render-a-markdown-document-in-raw-mode),除了渲染是在本地执行.

-   [markdownfmt](https://github.com/shurcooL/markdownfmt):像gofmt,但用于降价.

-   [乳胶输出](https://bitbucket.org/ambrevar/blackfriday-latex):将输出呈现为LaTeX.

-   [bfchroma](https://github.com/Depado/bfchroma/):提供方便的集成[浓度](https://github.com/alecthomas/chroma)代码突出显示库.bfchroma仅与Blackfriday的v2兼容,并提供可随时使用Blackfriday的嵌入式渲染器,以及进一步定制的选项和方法.

## 去做

-   更多单元测试
-   改进Unicode支持.它不了解所有Unicode规则(关于什么构成字母,标点符号等),因此在某些情况下它可能无法正确检测字边界.所有UTF-8输入都是安全的.

## 执照

[Blackfriday根据简化BSD许可证分发](LICENSE.txt)

[1]: https://daringfireball.net/projects/markdown/ "Markdown"

[2]: https://golang.org/ "Go Language"

[3]: https://github.com/vmg/sundown "Sundown"

[4]: https://godoc.org/gopkg.in/russross/blackfriday.v2#Parse "Parse func"

[5]: https://github.com/microcosm-cc/bluemonday "Bluemonday"

[6]: https://labix.org/gopkg.in "gopkg.in"

[7]: https://github.com/golang/dep/ "dep"

[8]: https://github.com/Masterminds/glide "Glide"

[buildsvg]: https://travis-ci.org/russross/blackfriday.svg?branch=master

[buildurl]: https://travis-ci.org/russross/blackfriday

[godocv2svg]: https://godoc.org/gopkg.in/russross/blackfriday.v2?status.svg

[godocv2url]: https://godoc.org/gopkg.in/russross/blackfriday.v2
