---
title: 在 Gatsby 中使用 CSS 样式
typora-copy-images-to: ./
disableTableOfContents: true
---

<!-- Idea: Create a glossary to refer to. A lot of these terms get jumbled -->

<!--

  - Global styles
  - Component css
  - CSS-in-JS
  - CSS Modules

-->

欢迎来到 Gatsby 第二部分教程！

## 概述

在这一部分中，你将学习到如何在 Gatsby 中使用 CSS 样式，并更深入地研究如何使用 React 组件来构建网站。

## 使用全局样式

每个站点都有某种统一风格。 其中包括网站的排版和背景颜色。 这些样式用于设置网站的整体风格，就像墙壁的颜色和纹理用于设置房间的整体风格一样。

### 使用标准 CSS 文件创建全局样式

向网站添加全局样式的最直接的方法之一就是使用全局 `.css` 样式表。

#### ✋ 新建一个Gatsby网站

首先新建一个 Gatsby 网站。 如果你不熟悉命令行工具，最好关闭了用于 [第一部分](/tutorial/part-one/) 的命令行终端窗口，并打开一个新的命令行终端窗口。

打开一个新的终端窗口，新建一个 “hello world” gatsby 站点，并启动本地开发服务器：

```shell
gatsby new tutorial-part-two https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-two
```

现在，基于 Gatsby 的 “hello world” 启动模版，你将新建一个具有以下目录结构的 Gatsby 网站：

```text
├── package.json
├── src
│   └── pages
│       └── index.js
```

#### ✋ 将样式添加到 CSS 文件

1. 在新项目中创建一个 .css 文件：

```shell
cd src
mkdir styles
cd styles
touch global.css
```

> 注意：如果你愿意，你也可以使用代码编辑器随意创建这些目录和文件。

你现在应该具有这样的文件目录结构：

```text
├── package.json
├── src
│   └── pages
│       └── index.js
│   └── styles
│       └── global.css
```

2. 在 global.css 文件中定义一些样式：

```css:title=src/styles/global.css
html {
  background-color: lavenderblush;
}
```

> 注意：示例中的 css 文件放置在 `/src/styles/` 文件夹中的位置是任意的。

#### ✋ 将样式表导入 `gatsby-browser.js` 中

1. 创建 `gatsby-browser.js` 文件

```shell
cd ../..
touch gatsby-browser.js
```

你项目的文件目录结构现在应如下所示：

```text
├── package.json
├── src
│   └── pages
│       └── index.js
│   └── styles
│       └── global.css
├── gatsby-browser.js
```

> 💡 `gatsby-browser.js` 文件是什么？ 不必太担心这一点，现在，只需知道 `gatsby-browser.js` 文件是 Gatsby 会自动扫描并使用的少数特殊文件之一（如果存在）。 在这里，文件的命名很重要。 如果你确实想现在想了解更多，请查看 [文档](/docs/browser-apis/)。

2. 将你刚刚创建的样式表导入 “gatsby-browser.js” 文件中：

```javascript:title=gatsby-browser.js
import "./src/styles/global.css"

// 或者:
// require('./src/styles/global.css')
```

> 注意：CommonJS 格式的（`require`）和 ES Module 格式的（`import`）语法都可以在这里使用。 如果你不确定选择哪个，通常最好使用 `import`。 但是，当处理仅在 Node.js 环境中运行的文件时（如 `gatsby-node.js`），则需要使用 `require`。

3. 启动本地开发服务器：

```shell
gatsby develop
```

在浏览器中查看，应该会看到淡紫色背景并已应用了 “hello world” 模板的启动页面：

![Lavender Hello World!](global-css.png)

> 提示：本教程这一部分重点介绍了最快和最直接的设置 Gatsby 网站 CSS 样式的方法 - 使用 `gatsby-browser.js` 直接导入标准 CSS 样式文件。 在大多数情况下，添加全局样式的最佳方法是使用公共的布局组件。[查看文档](/docs/global-css/) 了解有关该方法的更多信息。

## 使用组件范围内 CSS

到目前为止，我们已经讨论了更传统的使用标准 CSS 样式表的方法。 现在，我们将讨论以面向组件的方式处理样式的 CSS 样式模块化的各种方法。

### CSS 模块

让我们一起了解 **CSS 模块**。 引用自
[the CSS Module homepage](https://github.com/css-modules/css-modules)：

> 一个 **CSS 模块**是一个 CSS 文件，其中包含所有样式和 CSS3 动画名称
> 默认情况下只适用于本范围内。

CSS 模块之所以受欢迎，是因为它们可以让你正常编写 CSS 的同时，安全性更高。 该工具会自动生成唯一的样式和 CSS3 动画名称，因此你不必担心选择器名称冲突。

Gatsby 可与 CSS 模块一起使用。 强烈建议 Gatsby（以及 React）新手使用此方式进行构建。

#### ✋ 使用 CSS 模块构建新页面

在本节中，你将创建一个新的页面组件，并使用 CSS 模块对该页面组件进行样式设置。

首先，新建一个 `Container` 组件。

1. 在 `src/components` 目录下创建一个新目录，然后在此目录中创建一个名为 `container.js` 的文件并粘贴以下内容：

```javascript:title=src/components/container.js
import React from "react"
import containerStyles from "./container.module.css"

export default ({ children }) => (
  <div className={containerStyles.container}>{children}</div>
)
```

你会注意到你导入了一个名为 `container.module.css` 的 CSS 模块文件。 现在创建该文件。

2. 在同一目录（`src/components`）中，创建一个 container.module.css 文件并复制 / 粘贴以下内容：

```css:title=src/components/container.module.css
.container {
  margin: 3rem auto;
  max-width: 600px;
}
```

你会注意到，文件名以 .module.css 结尾，而不是通常的 .css 结尾。 这是告诉 Gatsby 该 CSS 文件应作为 CSS 模块而不是纯 CSS 处理的方式。

3. 创建新的页面组件文件 `src/pages/about-css-modules.js`:

```javascript:title=src/pages/about-css-modules.js
import React from "react"

import Container from "../components/container"

export default () => (
  <Container>
    <h1>About CSS Modules</h1>
    <p>CSS Modules are cool</p>
  </Container>
)
```

现在，如果你访问 `http://localhost:8000/about-css-modules/`，则页面应如下所示：

![使用 CSS 模块设置页面](css-modules-basic.png)

#### ✋ 使用 CSS 模块为组件设置样式

在本部分中，你将创建一个具有姓名、头像和拉丁字母组成的简短人物简介的人员列表。 你将创建一个 `<User />` 组件，并使用 CSS 模块对该组件进行样式设置。

1. 创建 CSS 文件 `src/pages/about-css-modules.module.css`。

2. 将以下内容粘贴到文件中：

```css:title=src/pages/about-css-modules.module.css
.user {
  display: flex;
  align-items: center;
  margin: 0 auto 12px auto;
}

.user:last-child {
  margin-bottom: 0;
}

.avatar {
  flex: 0 0 96px;
  width: 96px;
  height: 96px;
  margin: 0;
}

.description {
  flex: 1;
  margin-left: 18px;
  padding: 12px;
}

.username {
  margin: 0 0 12px 0;
  padding: 0;
}

.excerpt {
  margin: 0;
}
```

3. 把文件 `src/pages/about-css-modules.module.css` 文件导入到你之前创建 `about-css-modules.js` 文件页面，编辑开始几行代码如下：

```javascript:title=src/pages/about-css-modules.js
import React from "react"
// highlight-next-line
import styles from "./about-css-modules.module.css"
import Container from "../components/container"

// highlight-next-line
console.log(styles)
```

代码 `console.log(styles)` 将在控制台打印出已处理的被导入的 `./about-css-modules.module.css` 文件的结果。 如果你在浏览器中打开开发者控制台（使用 Firefox 或 Chrome 的开发者工具），则会看到：

![在控制台中导入的 CSS 模块的结果](https://github.com/gatsbyjs/gatsby/raw/master/docs/tutorial/part-two/css-modules-console.png)

如果将其与 CSS 文件进行比较，你会发现每个格式现在都是导入对象中指向长字符串的键（key），例如 `avatar` 指向 `src-pages----about-css-modules-module---avatar---2lRF7`。 这些样式名称是 CSS 模块生成的。 保证它们在你的网站上是唯一的。 而且由于必须导入它们才能使用这些类，所以对于在任何地方使用这些 CSS 样式都没问题。

4. 在 `about-css-modules.js` 页面组件中内联新建一个的 `<User />` 组件。 修改 `about-css-modules.js` 文件如下：

```jsx:title=src/pages/about-css-modules.js
import React from "react"
import styles from "./about-css-modules.module.css"
import Container from "../components/container"

console.log(styles)

// highlight-start
const User = props => (
  <div className={styles.user}>
    <img src={props.avatar} className={styles.avatar} alt="" />
    <div className={styles.description}>
      <h2 className={styles.username}>{props.username}</h2>
      <p className={styles.excerpt}>{props.excerpt}</p>
    </div>
  </div>
)
// highlight-end

export default () => (
  <Container>
    <h1>About CSS Modules</h1>
    <p>CSS Modules are cool</p>
    {/* highlight-start */}
    <User
      username="Jane Doe"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/adellecharles/128.jpg"
      excerpt="I'm Jane Doe. Lorem ipsum dolor sit amet, consectetur adipisicing elit."
    />
    <User
      username="Bob Smith"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/vladarbatov/128.jpg"
      excerpt="I'm Bob Smith, a vertically aligned type of guy. Lorem ipsum dolor sit amet, consectetur adipisicing elit."
    />
    {/* highlight-end */}
  </Container>
)
```

> 提示：通常，如果你在站点的多个位置使用组件，则该组件应放置于 `components` 目录中其自身的模块文件中。 但是，如果仅在一个文件中使用它，则可以以内联方式创建它。

现在完成的页面应如下所示：

![带有 CSS 模块的用户列表页面](css-modules-userlist.png)

### CSS-in-JS

CSS-in-JS 是一种面向组件的编写 CSS 样式方法。 通常，这是一种模式，其中 [CSS 是使用 JavaScript 内联组成](https://reactjs.org/docs/faq-styling.html#what-is-css-in-js)。

#### 在Gatsby中使用 CSS-in-JS

有许多不同的 CSS-in-JS 库，其中许多已有Gatsby插件。 在本初始教程中，我们不会介绍 CSS-in-JS 的示例，但是我们鼓励你 [探索](/docs/styling/) 生态系统必须提供的功能。 有两个库的微教程，特别是 [Emotion](/docs/emotion/) 和 [Styled Components](/docs/styled-components/)。

#### CSS-in-JS 的阅读建议

如果你有兴趣进一步阅读，请查看 [Christopher "vjeux" Chedeau's 2014 presentation that sparked this movement](https://speakerdeck.com/vjeux/react-css-in-js) 和 [Mark Dalgleish's more recent post "A Unified Styling Language"](https://medium.com/seek-blog/a-unified-styling-language-d0c208de2660)。

### 其他 CSS 处理选项

Gatsby 支持几乎所有可能的 CSS 样式处理选项（如果你最喜欢的 CSS 选项还没有 Gatsby 插件，请 [请贡献一个！](/contributing/how-to-contribute/)）

- [Typography.js](/packages/gatsby-plugin-typography/)
- [Sass](/packages/gatsby-plugin-sass/)
- [JSS](/packages/gatsby-plugin-jss/)
- [Stylus](/packages/gatsby-plugin-stylus/)
- [PostCSS](/packages/gatsby-plugin-postcss/)

和更多！

## 下一步

现在继续 [教程的第三部分](/tutorial/part-three/)，你将在其中学习 Gatsby 插件和布局组件。
