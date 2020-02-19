---
title: 创建嵌套的布局组件
typora-copy-images-to: ./
disableTableOfContents: true
---

欢迎来到 Gatsby 第三部分教程！

## 本教程的内容

在这一部分中，你将学习 Gatsby 插件的使用和创建 “布局” 组件。

Gatsby 插件是用于向 Gatsby 网站添加功能的 JavaScript 包。 Gatsby 被设计为可扩展的，这意味着插件也是像 Gatsby 一样是可以扩展和修改的。

布局组件用于你要在多个页面上共享的网站公共部分。例如，站点通常有公共的页眉和页脚的布局组件。其他常见的要添加到布局的公共的内容是是侧边栏或导航菜单。例如，在此页面上，顶部标题是 gatsbyjs.org 的布局组件的一部分。

让我们深入了解第三部分教程。

## 使用插件

你很可能已经熟悉插件的概念。许多软件系统都支持添加自定义插件来添加新功能，甚至可以修改软件的核心功能。Gatsby 插件的工作方式也一样。

社区成员（像你一样！）可以贡献插件（只需少量 JavaScript 代码），其他人可以在构建 Gatsby 网站时使用你的插件。

> 目前已经有数百个插件！ 了解 Gatsby [插件库](/plugins/)。

我们的插件的目标是使它们易于安装和使用。你可能会在几乎所有构建的 Gatsby 网站中使用到插件。在学习本教程的其余部分时，你将有很多机会练习安装和使用插件。

对于使用插件的初步介绍，我们将为 Typography.js 安装并使用 Gatsby 插件。

[Typography.js](https://kyleamathews.github.io/typography.js/) 是一个 JavaScript 库，可为你网站的排版生成全局基本样式。该库具有一个 [相应的Gatsby插件](/packages/gatsby-plugin-typography)，可以在 Gatsby 网站中配合使用。

### ✋ 新建一个 Gatsby 网站

正如我们在 [第二部分](/tutorial/part-two/) 中提到的那样，此时最好关闭本教程前面部分的命令行终端窗口和项目文件，以使你桌面的内容保持整洁。 然后打开一个新的命令行终端窗口并运行以下命令，在名为 `tutorial-part-three` 的目录中创建一个新的 Gatsby 站点，然后定位到该新目录：

```shell
gatsby new tutorial-part-three https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-three
```

### ✋ 安装和配置 `gatsby-plugin-typography`

使用插件有两个主要步骤：安装和配置。

1. 命令行使用 npm 安装 `gatsby-plugin-typography` 依赖包。

```shell
npm install --save gatsby-plugin-typography react-typography typography typography-theme-fairy-gates
```

> 注意：Typography.js 需要一些其他的 javascript 依赖包，因此说明中包括了这些 javascript 依赖包。每个插件的安装指引中都会列出类似的其他要求。

2. 编辑项目根目录下的文件 `gatsby-config.js` ，如下：

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

`gatsby-config.js` 文件是 Gatsby 会自动识别的另一个特殊文件。 要在这里添加插件和网站配置。

> 如果需要，请查看 [关于 gatsby-config.js 的文档](/docs/gatsby-config/) 以了解更多信息。

3. Typography.js 需要一个配置文件。 在 `src` 目录中创建一个名为 `utils` 的新目录。 然后在 `utils` 中添加一个名为 `typography.js` 的新文件，并将以下内容复制到该文件中：

```javascript:title=src/utils/typography.js
import Typography from "typography"
import fairyGateTheme from "typography-theme-fairy-gates"

const typography = new Typography(fairyGateTheme)

export const { scale, rhythm, options } = typography
export default typography
```

4. 启动开发服务器。

```shell
gatsby develop
```

加载网站后，如果你使用 Chrome 开发人员工具检查生成的 HTML，你会发现排版插件向 `<head>` 元素添加了一个带生成的 CSS 样式的 `<style>` 元素：

![typography-styles](typography-styles.png)

### ✋ 进行一些内容和样式更改

将以下内容复制到你的 `src/pages/index.js` 文件中，以便你更好的看到 Typography.js 生成的 CSS 样式的效果。

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div>
    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
    <p>
      What do I like to do? Lots of course but definitely enjoy building
      websites.
    </p>
  </div>
)
```

你的网站现在应如下所示：

![no-layout](no-layout.png)

让我们快速改进。许多网站在页面中间居中显示一列文本。 要创建此样式，请将以下样式添加到 `src/pages/index.js` 中的 `<div>` 元素中。

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  // highlight-next-line
  <div style={{ margin: `3rem auto`, maxWidth: 600 }}>
    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
    <p>
      What do I like to do? Lots of course but definitely enjoy building
      websites.
    </p>
  </div>
)
```

![with-layout2](with-layout2.png)

很好。你已经安装并配置了第一个 Gatsby 插件！

## 创建布局组件

现在让我们继续学习布局组件。为了准备好这一部分，请在项目中添加几个新页面：“关于我们” 页面和 “联系我们” 页面。

```jsx:title=src/pages/about.js
import React from "react"

export default () => (
  <div>
    <h1>About me</h1>
    <p>I’m good enough, I’m smart enough, and gosh darn it, people like me!</p>
  </div>
)
```

```jsx:title=src/pages/contact.js
import React from "react"

export default () => (
  <div>
    <h1>I'd love to talk! Email me at the address below</h1>
    <p>
      <a href="mailto:me@example.com">me@example.com</a>
    </p>
  </div>
)
```

让我们看看 “关于我们” 页面的新外观：

![about-uncentered](about-uncentered.png)

嗯，如果两个新页面的内容像首页一样居中，那就对了。拥有某种全局导航会很好，因此访问者可以轻松找到并访问每个子页面。

你将通过创建第一个布局组件来实现这些效果。

### ✋ 创建你的第一个布局组件

1. 创建一个新目录 `src/components`。

2. 在上面的目录中创建一个非常基本的布局组件文件 `src/components/layout.js`：

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children}
  </div>
)
```

3. 将此新的布局组件引入到你的 `src/pages/index.js` 页面组件中：

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout" // highlight-line

export default () => (
  <Layout> {/* highlight-line */}
    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
    <p>
      What do I like to do? Lots of course but definitely enjoy building
      websites.
    </p>
  </Layout> {/* highlight-line */}
)
```

![with-layout2](with-layout2.png)

太好了，布局有效！首页的内容仍居中。

但是，请尝试导航至 `/about/` 或 `/contact/`。 这些页面上的内容仍不会居中。

4. 将布局组件引入 `about.js` 和 `contact.js` 中（就像上一步 `index.js` 文件一样）。

有了这个公共布局组件，你所有三个页面的内容都居中！

### ✋ 添加网站标题

1. 将以下代码添加到新的布局组件：

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    <h3>MySweetSite</h3> {/* highlight-line */}
    {children}
  </div>
)
```

如果你进入三个页面中的任何一个页面，都会看到添加的是相同的标题，例如 `/about/` 页面：

![with-title](with-title.png)

### ✋ 在页面之间添加导航链接

1. 将以下内容复制到布局组件文件中：

```jsx:title=src/components/layout.js
import React from "react"
// highlight-start
import { Link } from "gatsby"

const ListLink = props => (
  <li style={{ display: `inline-block`, marginRight: `1rem` }}>
    <Link to={props.to}>{props.children}</Link>
  </li>
)
// highlight-end

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    {/* highlight-start */}
    <header style={{ marginBottom: `1.5rem` }}>
      <Link to="/" style={{ textShadow: `none`, backgroundImage: `none` }}>
        <h3 style={{ display: `inline` }}>MySweetSite</h3>
      </Link>
      <ul style={{ listStyle: `none`, float: `right` }}>
        <ListLink to="/">Home</ListLink>
        <ListLink to="/about/">About</ListLink>
        <ListLink to="/contact/">Contact</ListLink>
      </ul>
    </header>
    {/* highlight-end */}
    {children}
  </div>
)
```

![with-navigation2](with-navigation.png)

很好！ 一个三个子页面的网站，具有基本的全局导航。

_扩展：_ 使用新的 “布局组件” 功能，尝试向 Gatsby 网站添加页眉、页脚、全局导航和侧边栏等！

## 下一步

继续阅读 [本教程的第四部分](/tutorial/part-four/)，你将在此开始学习 Gatsby 的数据层并以编码的方式创建页面！
