---
title: 了解 Gatsby 的构建模块
typora-copy-images-to: ./
disableTableOfContents: true
---

在 [**上一章节**](/tutorial/part-zero/) 中，你安装了必要的软件，并使用 [**“hello world” starter**](https://github.com/gatsbyjs/gatsby-starter-hello-world) 创建了第一个 Gatsby 网站。你的本地开发环境已经准备就绪了。现在让我们深入了解 starter 生成的代码。

## 使用 Gatsby starters

在 [**第 0 章教程**](/tutorial/part-zero/) 里, 你使用了 “hello world” starter 和以下命令创建了一个新的网站：

```shell
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

当你新建一个 Gatsby 网站时，你可以用以下命令结构来创建一个网站，基于任何一个现有的 Gatsby starter：

```shell
gatsby new [SITE_DIRECTORY_NAME] [URL_OF_STARTER_GITHUB_REPO]
```

如果在命令的最后没有 URL，Gatsby 会基于 [**默认 starter**](https://github.com/gatsbyjs/gatsby-starter-default) 来为你创建网站。在这章教程中，我们将专注于你在第 0 章教程中已经创建好的 “Hello World” 网站。你可以在 [修改一个 starter](/docs/modifying-a-starter) 这篇文档中学习更多 starter 的相关知识。

### ✋ 打开代码

在你的代码编辑器里，打开你生成好的 “Hello World” 网站代码，看一看 ‘hello-world’ 目录中的各个子目录和文件，它们应该看起来像这样：

![Hello World 项目在 VS Code 中的演示](01-hello-world-vscode.png)

_注意: 再次说明一下, 这里展示的编辑器是 Visual Studio Code。如果你使用的是其他编辑器，看起来会略有不同。_

让我们先来看看让主页成功显示的代码。

> 💡 如果你在上一章运行了 `gatsby develop` 之后停止了开发服务器，现在请重新启动它——是时候对简单的 hello-world 网站做出些改变了！

## 熟悉 Gatsby 页面

在编辑器中打开 `/src` 目录，它里面只有一个目录：`/pages`。

打开 `src/pages/index.js`。这个文件中的代码生成了一个只包含一个 div 和一些文本的组件——正巧它就是：“Hello world!”

### ✋ 修改 “Hello World” 主页

1.  把 “Hello World!” 替换成 “Hello Gatsby!” 并保存文件。如果你的窗口是并排显示的，你在保存文件后几乎立即就能在浏览器中看到内容的改变。

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="./02-demo-hot-reloading.mp4"></source>
  <p>Sorry! Your browser doesn't support this video.</p>
</video>

> 💡 Gatsby 使用 **热重载** 来加快你的开发流程。当你运行 Gatsby 开发服务器的时候，Gatsby 的网站文件在后台被监听（watch）。所以每一次你保存文件的时候，浏览器就会立即显示出你所做的改动。你不需要强制刷新页面或重启开发服务器，你的改动会自动显示。

2.  现在你可以做点更显眼的改动。尝试把 `src/pages/index.js` 中的代码替换成下面这段，然后保存。你会看到文本有所变化——文字颜色变成紫色并且字体变大。

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Hello Gatsby!</div>
)
```

> 💡 我们会在教程的 [**第 2 章**](/tutorial/part-two/) 介绍更多 Gatsby 中的样式。

3.  删除字体大小的样式（fontSize），把 “Hello Gatsby!” 放到一个一级标题（h1）中，并且在标题下面增加一个段落（p）。

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  {/* highlight-start */}
  <div style={{ color: `purple` }}>
    <h1>Hello Gatsby!</h1>
    <p>What a world.</p>
  {/* highlight-end */}
  </div>
)
```

![更多有热重载的改动](03-more-hot-reloading.png)

4.  增加一张图片。 (这里我们随意选取了 Unsplash 上的一张图片).

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div style={{ color: `purple` }}>
    <h1>Hello Gatsby!</h1>
    <p>What a world.</p>
    {/* highlight-next-line */}
    <img src="https://source.unsplash.com/random/400x200" alt="" />
  </div>
)
```

![增加图片](04-add-image.png)

### 等等…… HTML 在 JavaScript 文件里?

_如果你对 React 和 JSX 有所了解，你可以跳过这一段。_ 如果你没有使用 React 框架的经验，你可能会想知道 HTML 是怎么在 JavaScript 函数中起作用的。或者想知道为什么我们在第一行引入（import）了 `react`，但好像并没有在哪里使用它。这个 “HTML-in-JS” 的混合实际上是一种 JavaScript 的语法扩展，在 React 中叫 JSX。即使你没有接触过 React，你也可以顺着这个教程继续下去。但如果你很好奇这部分，可以看看这段简单入门：

这是我们 `src/pages/index.js` 文件的原始内容:

```jsx:title=src/pages/index.js
import React from "react"

export default () => <div>Hello world!</div>
```

如果使用纯 JavaScript, 这个文件就变成:

```javascript:title=src/pages/index.js
import React from "react"

export default () => React.createElement("div", null, "Hello world!")
```

现在你能找到在哪里使用了引入的 `'react'` 了！ 等等，你写的是 JSX，不是纯 HTML 和 JavaScript，浏览器是怎么成功读取的？答案很简单：没必要。Gatsby 有工具已经为你把源代码转换成浏览器可以解析的代码了。

## 用组件来构建

你刚刚编辑的主页是通过定义一个页面组件创建出来的。那到底什么是 “组件”（component）？

从广义上定义，组件是你网站的构建模块；它是能描述一个 UI（用户界面）部分的完备的代码片段。

Gatsby 是以 React 为基础的。当我们谈论使用和定义 **组件** 的时候，我们实际上是在谈论 **React 组件**——一个完备的代码片段（通常是用 JSX 编写）。它能接收输入，并返回 React 元素。其中 React 元素描述了一个 UI 部分。

当你开始通过组件构建的时候（如果你已经是一位开发者了），你要做好这样一个思维转换：现在 CSS、HTML 和 JavaScript 三者紧紧耦合在一起，并且通常位于同一个文件里。

有时候一个看似简单的改动，会对你思考如何构建网站产生深远的影响。

就拿创建一个自定义按钮来举例：在之前，你会创建一个 CSS class（比如 `.primary-button`），为其编写你自己的样式，然后就能在任何时候应用这些样式，比如：

```html
<button class="primary-button">Click me</button>
```

In the world of components, you instead create a `PrimaryButton` component with your button styles and use it throughout your site like:

而在组件的世界里，你创建的是一个 `PrimaryButton` 组件，并为其加入一些按钮样式。在你的网站中要像这样使用它：

<!-- prettier-ignore -->
```jsx
<PrimaryButton>Click me</PrimaryButton>
```

这时组件就成了你的网站的基础构建模块，而不是局限于使用浏览器提供的构建模块（比如 `<button />`）。现在你可以轻松创建一个新的构建模块并且优雅地满足你的项目需求。

### ✋ 使用页面组件

每个定义在 `src/pages/*.js` 中的 React 组件，在浏览器里都是一个页面。让我们动手来看看。

你现在已经有了一个 `src/pages/index.js` 文件。它是通过 “Hello World” starter 生成的。让我们来创建一个 About 页面。

1.  新建一个文件，路径是 `src/pages/about.js`。把以下代码复制到这个新文件里并保存。

```jsx:title=src/pages/about.js
import React from "react"

export default () => (
  <div style={{ color: `teal` }}>
    <h1>About Gatsby</h1>
    <p>Such wow. Very React.</p>
  </div>
)
```

2.  访问这个页面 http://localhost:8000/about/。

![新的 About 页面](05-about-page.png)

仅仅在 `src/pages/about.js` 文件中放入了一个 React 组件，你就有了一个通过 `/about` 访问的新页面。

### ✋ 使用子组件

假设主页和 About 页面都很复杂，你写了很多东西在里面。你可以使用子组件，把 UI 划分成可复用的片段。你的两个页面里都有 `<h1>` 标题——那就新建一个描述一个 `Header` 的组件。

1.  新建一个目录于 `src/components` ，然后新建一个文件在这个目录里，命名这个文件为 `header.js`。
2.  把以下代码添加到新的 `src/components/header.js` 文件里。

```jsx:title=src/components/header.js
import React from "react"

export default () => <h1>This is a header.</h1>
```

3.  修改 `about.js` 文件，为其引入 `Header` 组件。替换 `h1` 标记为  `<Header />`。

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header" // highlight-line

export default () => (
  <div style={{ color: `teal` }}>
    <Header /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![添加 Header 组件](06-header-component.png)

在浏览器里，“About Gatsby” 这个标题文本现在应该被替换成了 “This is a header.”。但是你不想要 “About” 页面显示 “This is a header.” 作为标题，你要的是：“About Gatsby”。

4.  回到 `src/components/header.js` 文件里，做以下改动：

```jsx:title=src/components/header.js
import React from "react"

export default props => <h1>{props.headerText}</h1> {/* highlight-line */}
```

5.  回到 `src/pages/about.js` 文件里，做以下改动：

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Header headerText="About Gatsby" /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![传递标题数据](07-pass-data-header.png)

你应该重新看到了你想要的 “About Gatsby” 标题！

### 什么是 “props”?

你刚刚定义了 React 组件作为描述 UI 的可复用片段。要使这些可复用片段动态化，你需要给它们提供不同的数据。你可以通过一种叫做 “props" 的输入。Props，正如其名，是给 React 组件使用的属性（property）。

在 `about.js` 里，你为引入的 `Header` 子组件传入了一个值为 `"About Gatsby"` 的 `headerText` prop：
```jsx:title=src/pages/about.js
<Header headerText="About Gatsby" />
```

在 `header.js` 中，header 组件期望接收到 `headerText` prop（因为你在 `about.js` 里已经写好它是这么期望的）。所以可以这样使用这个 prop：

```jsx:title=src/components/header.js
<h1>{props.headerText}</h1>
```

> 💡 在 JSX 中, 你可以嵌入任意 JavaScript 表达式，只要用 `{}` 包裹起来。这就是这段代码如何从  “props”  对象获取到 `headerText` 属性（或 “prop!”）的。

如果你向 `<Header />` 组件传入另一个 prop，像这样：

```jsx:title=src/pages/about.js
<Header headerText="About Gatsby" arbitraryPhrase="is arbitrary" />
```

……类似地，你就能使用 `arbitraryPhrase` prop 了：`{props.arbitraryPhrase}`。

6.  为了着重解释这样做如何使得你的组件可复用：在 about 页面中添加一个额外的 `<Header />` 组件，然后把下面这段代码复制到你的 `src/pages/about.js` 文件里并保存。

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Header headerText="About Gatsby" />
    <Header headerText="It's pretty cool" /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![复制 header 来展示可复用性](08-duplicate-header.png)

现在你就有了第二个 header。你所做的仅是利用 props 传入不同的数据，并没有重写任何代码。

### 使用布局组件

布局组件的作用，是使得不同页面能够共享某些部分。比如大多数 Gatsby 网站都有一个布局组件来共享页头和页脚。侧边栏和导航菜单栏也是常出现在布局中的。

你将在 [**第 3 章**](/tutorial/part-three/) 中详细探索布局组件。

## 连接不同页面

你会经常需要把页面连接到别的页面——让我们来看看 Gatsby 网站的路由功能。

### ✋ 使用 `<Link />` 组件

1. 打开 index 页面组件（`src/pages/index.js`），从 Gatsby 中引入`<Link />` 组件，在 header 上面添加一个 `<Link />` 组件，并且给这个新组件添加一个值为 `"/contact/"` 的 `to` 属性来指定路径名：

```jsx:title=src/pages/index.js
import React from "react"
import { Link } from "gatsby" // highlight-line
import Header from "../components/header"

export default () => (
  <div style={{ color: `purple` }}>
    <Link to="/contact/">Contact</Link> {/* highlight-line */}
    <Header headerText="Hello Gatsby!" />
    <p>What a world.</p>
    <img src="https://source.unsplash.com/random/400x200" alt="" />
  </div>
)
```

当你点击主页上新的 "Contact" 连接，你会看到……

![Gatsby 的 404 开发页面](09-dev-404.png)

……Gatsby 的 404 开发页面。为什么？因为你在尝试连接到一个并不存在的页面。

2. 现在你必须为你的新页面 "Contact" 创建一个页面组件： `src/pages/contact.js` 。并把它连接回到主页：

```jsx:title=src/pages/contact.js
import React from "react"
import { Link } from "gatsby"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Link to="/">Home</Link>
    <Header headerText="Contact" />
    <p>Send us a message!</p>
  </div>
)
```

保存这个文件后，你应该能成功看到 contact 页面，并且能点击链接回到主页。

<video controls="controls" loop="true">
  <source type="video/mp4" src="./10-linking-between-pages.mp4"></source>
  <p>Sorry! Your browser doesn't support this video.</p>
</video>

`<Link />` 这个 Gatsby 组件的作用，是在你的网站里连接不同页面。对于那些不是你的 Gatsby 网站所处理的外部链接，请使用常规的 HTML `<a>` 标签。

## 部署 Gatsby 网站

Gatsby.js 是一个 _现代网站生成器_。这意味着部署既不需要设置服务器，也不需要部署复杂的数据库。相反地，Gatsby 的 `build` 命令仅仅生成了一个包含静态 HTML 和 JavaScript 文件的目录。你只需要把这个目录部署到一个静态网站托管服务中。

不妨试一试使用 [Surge](http://surge.sh/) 来部署你的第一个 Gatsby 网站。Surge 是众多能够部署 Gatsby 网站的 “静态网站托管” 服务中的一个。

如果你还没有安装并设置好 Surge，打开一个终端窗口并安装他们的命令行工具：

```shell
npm install --global surge

# Then create a (free) account with them
surge login
```

接下来，在终端中定位到你的网站的根目录，通过运行以下代码来构建的你的网站文件（提示：确保在你的网站的根目录里运行这个命令，对于这篇教程所演示的项目，这个目录是 hello-world 文件夹。在你运行 `gatsby develop` 的这个窗口内新建一个标签，即可确保你在网站的根目录）：

```shell
gatsby build
```

构建的时间大概在 15~30 秒左右。构建完成后，不妨看看 `gatsby build` 为你准备好的将要部署的文件，这些文件很有趣。

要查看生成文件的列表，只需在终端中网站根目录下输入这条命令，你会看到 `public` 目录的文件列表：

```shell
ls public
```

最后，通过发布生成文件到 surge.sh，部署你的网站。

```shell
surge public/
```

运行结束后，你应该会在终端中看到类似这样的内容：

![使用 Surge 发布 Gatsby 网站的截图](surge-deployment.png)

打开最后一行显示的网址（在这张图里是 `lowly-pain.surge.sh`），你就能看到你新发布的网站了！干的漂亮！

## ➡️ 下一步

在本节中，你：

- 学习了 Gatsby starters 和如何使用它们来构建新项目
- 学习了 JSX 语法
- 学习了组件
- 学习了 Gatsby 页面组件和子组件
- 学习了 React “props” 和 React 组件的复用

现在，继续阅读如何 [**为你的网站添加样式**](/tutorial/part-two/)！
