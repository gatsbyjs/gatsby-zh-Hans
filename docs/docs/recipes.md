---
title: 手册
tableOfContentsDepth: 2
---

<!-- 创建一个 Gatsby 配方的基本模板:

## 要完成的任务。
用一到两句话描述。越简洁越聚焦于主题越好！

### 前置条件
- 系统或版本要求
- 开始任务所需要的一切
- 包括在其它网站上设置账号，比如 Netlify
- 查看 [文档模板](/docs/docs-templates/) 以了解格式规范

### 操作步骤
一步一步地指导。每个步骤都应该是可重复的并且切中要点。对完成任务来说任何不重要的事物都应省略。

#### 在线演示 (可选)
由于配方的性质，可能无法提供在线演示。这种情况下可以省略。

### 补充资源
- 教程
- 文档
- 插件的 README 文档
- 等等。

要获得更多信息，请查看在 contributing 文档目录下的 [文档模板](/docs/docs-templates/) 。
-->

想要在阅读 [完整教程](/tutorial/) 和搜索 [文档](/docs/) 之间找到一个折衷方案? 可以参考这本 Gatsby 风格的指导手册。我们将告诉你构建各种东西的 “配方”。

## 1. 页面和布局

### 项目结构

在 Gatsby 项目中，你会看到这些文件和文件夹，可能是一些，可能是全部：

```
|-- /.cache
|-- /plugins
|-- /public
|-- /src
    |-- /pages
    |-- /templates
    |-- html.js
|-- /static
|-- gatsby-config.js
|-- gatsby-node.js
|-- gatsby-ssr.js
|-- gatsby-browser.js
```

一些值得注意的文件和它们的定义：

- `gatsby-config.js`——配置 Gatsby 站点的选项，比如项目标题和项目描述的元数据，插件等等
- `gatsby-node.js`——实现了 Gatsby 的 Node.js API，来自定义和扩展影响构建过程的设置
- `gatsby-browser.js`——使用 Gatsby 的浏览器 API 自定义和扩展影响浏览器的的设置
- `gatsby-ssr.js`——使用 Gatsby 的服务端渲染 API 自定义影响服务端渲染的默认设置

#### 补充资源

- 要了解所有常用的文件和文件夹，请查看 [Gatsby 的项目结构](/docs/gatsby-project-structure/)
- 要了解命令，请查看 [Gatsby CLI 文档](/docs/gatsby-cli)
- 要获得及时查阅的信息，请查看可下载的 [Gatsby 备忘录](/docs/cheat-sheet/)

### 自动创建页面

Gatsby 的核心自动把 `src/pages` 中的 React 组件转变为页面和 URL。比如：在 `src/pages/index.js` 和 `src/pages/about.js` 中的组件，会为网站的索引页（`/`）和 关于页（`/about`）自动创建基于文件名的页面。

#### 前置条件

- 一个 [Gatsby 网站](/docs/quick-start)
- 已经安装好 [Gatsby CLI](/docs/gatsby-cli) 

#### 操作步骤

1. 如果你没有 `src/pages` 这个目录的话，创建它。
2. 添加一个组件文件在这个 pages 目录里:

```jsx:title=src/pages/about.js
import React from "react"

const AboutPage = () => (
  <main>
    <h1>About the Author</h1>
    <p>Welcome to my Gatsby site.</p>
  </main>
)

export default AboutPage
```

3. 运行 `gatsby develop` 以启动开发服务器。
4. 在浏览器中访问你的新页面：<http://localhost:8000/about>

#### 补充资源

- [创建和修改页面](/docs/creating-and-modifying-pages/)

### 连接不同页面

Gatsby 中的页面路由依靠 `<Link />` 这个组件。

#### 前置条件

- 一个 Gatsby 站点，包含这两个组件：`index.js` 和 `contact.js`
- Gatsby 的 `<Link />` 组件
- [Gatsby CLI](/docs/gatsby-cli/)，用来运行 `gatsby develop`

#### 操作步骤

1. 打开索引页面（index）组件 (`src/pages/index.js`), 从 Gatsby 中引入 `<Link />` 组件，在标题上面添加一个 `<Link />` 组件， 并给它一个值为 `"/contact/"` 的 `to` 属性，来指定路径名：

```jsx:title=src/pages/index.js
import React from "react"
import { Link } from "gatsby"

export default () => (
  <div style={{ color: `purple` }}>
    <Link to="/contact/">Contact</Link>
    <p>What a world.</p>
  </div>
)
```

2. 运行 `gatsby develop` 并导航到索引页面。你应该看到一个点击后转向 contact 页面的链接!

> **注意**：Gatsby 的 `<Link />` 组件是 [`@reach/router` 的 Link 组件](https://reach.tech/router/api/Link) 的一个包装。更多信息请参考 Gatsby 的 `<Link />` 组件， 查阅 [API 参考中的 `<Link />`](/docs/gatsby-link/)。

### 添加一个布局组件

用 React 布局组件来封装页面是非常常见的。这使我们可以在不同页面中分享标记、样式和功能。

#### 前置条件

- 一个 Gatsby 站点

#### 操作步骤

1. 在 `src/components` 目录中添加一个布局组件, 其中子组件作为 props 传入：

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `0 auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children}
  </div>
)
```

2. 在一个页面中引入并使用这个布局组件：

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <Link to="/contact/">Contact</Link>
    <p>What a world.</p>
  </Layout>
)
```

#### 补充资源

- 通过阅读 [第 3 章教程](/tutorial/part-three/#your-first-layout-component) 来创建一个布局组件
- 为 [布局组件](/docs/layout-components/) 创建样式

### 以编程的方式使用 createPage 创建页面

你可以使用 Gatsby 提供的辅助方法，在 `gatsby-node.js` 文件中以编程方式创建页面。

#### 前置条件

- 一个 [Gatsby 站点](/docs/quick-start)
- 一个 `gatsby-node.js` 文件

#### 操作步骤

1. 在 `gatsby-node.js` 文件中，为 `createPages` 添加一个输出（export）

```javascript:title=gatsby-node.js
// highlight-start
exports.createPages = ({ actions }) => {
  // ...
}
// highlight-end
```

2. 从可用操作中解构 `createPage` 操作，以便可以单独调用它，并添加或获取数据

```javascript:title=gatsby-node.js
exports.createPages = ({ actions }) => {
  // highlight-start
  const { createPage } = actions
  // pull in or use whatever data
  const dogData = [
    {
      name: "Fido",
      breed: "Sheltie",
    },
    {
      name: "Sparky",
      breed: "Corgi",
    },
  ]
  // highlight-end
}
```

3. 遍历 `gatsby-node.js` 中的数据，并为每次调用将路径，模板和上下文（在 props 的 pageContext 中传入的数据）提供给 `createPage`

```javascript:title=gatsby-node.js
exports.createPages = ({ actions }) => {
  const { createPage } = actions

  const dogData = [
    {
      name: "Fido",
      breed: "Sheltie",
    },
    {
      name: "Sparky",
      breed: "Corgi",
    },
  ]
  // highlight-start
  dogData.forEach(dog => {
    createPage({
      path: `/${dog.name}`,
      component: require.resolve(`./src/templates/dog-template.js`),
      context: { dog },
    })
  })
  // highlight-end
}
```

4. 创建一个 React 组件作为你的页面模板，这个组件在 `createPage` 中已经被使用了

```jsx:title=src/templates/dog-template.js
import React from "react"

export default ({ pageContext: { dog } }) => (
  <section>
    {dog.name} - {dog.breed}
  </section>
)
```

5. 运行 `gatsby develop` 并导航到一个你创建的页面路径（例如 <http://localhost:8000/Fido>）来查看传给它的数据是否成功显示在页面上

#### 补充资源

- 教程部分中的 [以编程的方式利用数据创建页面](/tutorial/part-seven/)
- 参考指导中的 [在不用 GraphQL 的情况下使用 Gatsby](/docs/using-gatsby-without-graphql/)
- 这个配方的 [示例仓库](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-createPage)

## 2. 使用 CSS 样式

有很多方法可以为你的网站添加样式。Gatsby 通过官方和社区插件，能够支持几乎所有的选项。

### 在不使用布局组件的情况下使用全局 CSS 文件

#### 前置条件

- 有一个索引页面组件的 [Gatsby 网站](/docs/quick-start/)
- 一个 `gatsby-browser.js` 文件

#### 操作步骤

1. 添加一个全局 CSS 文件 `src/styles/global.css` 并输入以下内容：

```css:title=src/styles/styles/global.css
html {
  background-color: lavenderblush;
}

p {
  color: maroon;
}
```

2. 在 `gatsby-browser.js` 文件中引入这个全局 CSS 文件，像这样：

```javascript:gatsby-browser.js
import "./src/styles/global.css"
```

> **注意：** 你也可以使用 `require('./src/styles/global.css')` 来向你的 `gatsby-config.js` 文件引入全局 CSS 文件。

3. 运行 `gatsby-develop` 来观察全局样式是否应用到你的整个网站了。

> **注意：** 如果你使用 CSS-in-JS 对网站进行样式设置，这种方法不是最合适的选择。在这种情况下，应当使用一个包含所有共享组件的布局页面。下一个配方将对此进行介绍。

#### 补充资源

- 更多信息请参考 [在不使用布局组件的情况下添加全局样式](/docs/global-css/#adding-global-styles-without-a-layout-component)

### 在一个布局组件中使用全局样式

#### 前置条件

- 有一个索引页面组件的 [Gatsby 网站](/docs/quick-start/)

#### 操作步骤

你可以添加全局样式到一个 [共享的布局组件](/tutorial/part-three/#your-first-layout-component)。这个组件用于整个站点通用的内容，例如页眉或页脚。

1. 如果你没有这个目录 `/src/components` 的话，创建它。

2. 在组件目录中创建两个文件：`layout.css` 和 `layout.js`。

3. 添加以下内容到 `layout.css`：

```css:title=/src/components/layout.css
body {
  background: red;
}
```

4. 编辑 `layout.js` 文件使其引入 CSS 文件并输出 layout 标记：

```jsx:title=/src/components/layout.js
import React from "react"
import "./layout.css"

export default ({ children }) => <div>{children}</div>
```

5. 现在编辑你的站点主页 `/src/pages/index.js` 并使用新的布局组件：

```jsx:title=/src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => <Layout>Hello world!</Layout>
```

#### 补充资源

- [使用全局 CSS 文件的标准](/docs/global-css/)
- [更多布局组件的信息](/tutorial/part-three)

### 使用样式化组件（Styled Components）

#### 前置条件

- 有一个索引页面组件的 [Gatsby 网站](/docs/quick-start/)
- 在 `package.json` 安装好 [gatsby-plugin-styled-components、styled-components 和 babel-plugin-styled-components](/packages/gatsby-plugin-styled-components/)

#### 操作步骤

1. 在你的 `gatsby-config.js` 文件中添加 `gatsby-plugin-styled-components`

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-styled-components`],
}
```

2. 打开索引页面组件 (`src/pages/index.js`) 并引入 `styled-components` 包

3. 为每种元素添加样式代码块来给组件添加样式

4. 通过在 JSX 中添加样式组件来应用到页面

```jsx:title=src/pages/index.js
import React from "react"
import styled from "styled-components" //highlight-line

const Container = styled.div`
  margin: 3rem auto;
  max-width: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
`

const Avatar = styled.img`
  flex: 0 0 96px;
  width: 96px;
  height: 96px;
  margin: 0;
`

const Username = styled.h2`
  margin: 0 0 12px 0;
  padding: 0;
`

const User = props => (
  <>
    <Avatar src={props.avatar} alt={props.username} />
    <Username>{props.username}</Username>
  </>
)

export default () => (
  <Container>
    <h1>About Styled Components</h1>
    <p>Styled Components is cool</p>
    <User
      username="Jane Doe"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/adellecharles/128.jpg"
    />
    <User
      username="Bob Smith"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/vladarbatov/128.jpg"
    />
  </Container>
)
```

4. 运行 `gatsby develop`，看看有什么改变

#### 补充资源

- [更多关于使用样式化组件的信息](/docs/styled-components/)
- [Egghead 的教学](https://egghead.io/lessons/gatsby-style-gatsby-sites-with-styled-components)

### 使用 CSS 模块

#### 前置条件

- 有一个索引页面组件的 [Gatsby 网站](/docs/quick-start/)

#### 操作步骤

1. 创建一个 CSS 模块 `src/pages/index.module.css` 并粘贴以下代码到模块中：

```css:title=src/components/index.module.css
.feature {
  margin: 2rem auto;
  max-width: 500px;
}
```

2. 修改 `index.js` 文件内容如下，以 JSX 对象 `style` 的形式引入 CSS 模块：

```jsx:title=src/pages/index.js
import React from "react"

// highlight-start
import style from "./index.module.css"

export default () => (
  <section className={style.feature}>
    <h1>Using CSS Modules</h1>
  </section>
)
// highlight-end
```

3. 运行 `gatsby develop` 命令以查看变化。

**注意：**
请注意文件扩展名是 `.module.css` 而不是 `.css`。它告诉 Gatsby 这是一个 CSS 模块。

#### 补充资源

- 更多 [使用 CSS 模块](/tutorial/part-two/#css-modules) 的信息
- [使用 CSS 模块的在线演示](https://github.com/gatsbyjs/gatsby/blob/master/examples/using-css-modules)

### 使用 Sass 或 SCSS

Sass 是一个 CSS 的扩展。它提供了更多诸如嵌套规则，变量和 mixin 等等高级功能。

Sass具有两种语法。最常用的语法是 “SCSS”，它是 CSS 的超集。这意味着所有有效的 CSS 语法都是有效的 SCSS 语法。SCSS 文件使用扩展名.scss。

Sass 会为你把 .scss 和 .sass 文件编译成 .css 文件。因此你可以为样式表编写更多高级功能。

#### 前置条件

- 一个 [Gatsby 站点](/docs/quick-start/)。

#### 操作步骤

1. 安装 Gatsby 插件 [gatsby-plugin-sass](https://www.gatsbyjs.org/packages/gatsby-plugin-sass/) 和 `node-sass`。

`npm install --save node-sass gatsby-plugin-sass`

2. 把插件添加到你的 `gatsby-config.js` 文件中。

```javascript:title=gatsby-config.js
plugins: [`gatsby-plugin-sass`],
```

3.  在 `.sass` 或 `.scss` 文件中编写你的样式表并引入到 JavaScript 文件中。如果你不知道如何引入样式，请看这部分 [使用 CSS 编写样式](/docs/recipes/#2-styling-with-css)

```css:title=styles.scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

```css:title=styles.sass
$font-stack:    Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
```

```javascript
import "./styles.scss"
import "./styles.sass"
```

_注意: 你也可以用模块的方式使用 Sass/SCSS 文件。 像前一个关于 CSS 模块的配方所提到的一样。唯一的区别就是 .css 文件扩展名要变成 .scss 或 .sass_

#### 补充资源

- [.sass 和 .scss 之间的区别](https://responsivedesign.is/articles/difference-between-sass-and-scss/)
- [Sass 官网指导 ](https://sass-lang.com/guide)
- [一个更全面的 Sass 安装教程。包含更多解释和资源](https://www.gatsbyjs.org/docs/sass/)

### 添加一个本地字体

#### 前置条件

- 一个 [Gatsby 网站](/docs/quick-start/)
- 一个字体文件: `.woff2`、`.ttf` 等文件类型。

#### 操作步骤

1. 把一个字体文件复制到你的项目当中，例如 `src/fonts/fontname.woff2`。

```
src/fonts/fontname.woff2
```

2. 把这个字体资源引入到一个 CSS 文件中，来把它集成到你的 Gatsby 网站当中：

```css:title=src/css/typography.css
@font-face {
  font-family: "Font Name";
  src: url("../fonts/fontname.woff2");
}
```

**注意：** 确保你的字体名称在相关 CSS 文件中被引用，例如：

```css:title=src/components/layout.css
body {
  font-family: "Font Name", sans-serif;
}
```

通过为 HTML 的 `body` 元素设置字体，你的字体就会引用到大多数页面文本中。你需要一些其他 CSS 来设置其他元素，例如 `button` 或 `textarea`。

如果在这些操作以后字体依然没有更新，请确保你在相关的 CSS 中替换了已有的 font-family。

#### 补充资源

- 更多信息请参考 [向文件引入资源](/docs/importing-assets-into-files/)

### 使用 Emotion

[Emotion](https://emotion.sh) 是一个强大的 CSS-in-JS 库。它同时支持内联 CSS 样式和样式组件。你可以单独使用每一个样式功能或在同一个文件中一起用。

#### 前置条件

- 一个 [Gatsby 网站](/docs/quick-start/)

#### 操作步骤

1. 安装 [Gatsby Emotion 插件](/packages/gatsby-plugin-emotion/) 和 Emotion 的相关包。

```shell
npm install --save gatsby-plugin-emotion @emotion/core @emotion/styled
```

2. 添加 `gatsby-plugin-emotion` 插件到你的 `gatsby-config.js` 文件中：

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-emotion`],
}
```

3. 在你的 Gatsby 站点中创建一个页面 `src/pages/emotion-sample.js`，如果你还没有它的话。

引入 Emotion 的 `css` 核心包。 之后你就可以使用  `css` prop 来添加 [Emotion 对象样式](https://emotion.sh/docs/object-styles) 到任何一个组件中的元素中了：

```jsx:title=src/pages/emotion-sample.js
import React from "react"
import { css } from "@emotion/core"

export default () => (
  <div>
    <p
      css={{
        background: "pink",
        color: "blue",
      }}
    >
      This page is using Emotion.
    </p>
  </div>
)
```

4. 要使用 Emotion 的 [样式化组件](https://emotion.sh/docs/styled)，引入这个包并用  `styled` 函数定义组件。

```jsx:title=src/pages/emotion-sample.js
import React from "react"
import styled from "@emotion/styled"

const Content = styled.div`
  text-align: center;
  margin-top: 10px;
  p {
    font-weight: bold;
  }
`

export default () => (
  <Content>
    <p>This page is using Emotion.</p>
  </Content>
)
```

#### 补充资源

- [在 Gatsby 中使用 Emotion](/docs/emotion/)
- [Emotion 网站](https://emotion.sh)
- [开始使用 Emotion 和 Gatsby](https://egghead.io/lessons/gatsby-getting-started-with-emotion-and-gatsby)

### 使用 Google Fonts

在项目中本地托管你自己的 [Google 字体](https://fonts.google.com/)，使你在网站加载时不必通过网络获取它们，从而使网站的速度指数在电脑上提高约 300 毫秒 ，在 3G 下提高约为 1 秒多。 我们还建议你限制自定义字体的使用，只应用于重要的部分，这样能提高网站性能。

#### 前置条件

- 一个 [Gatsby 网站](/docs/quick-start/)
- 安装好 [Gatsby CLI](/docs/gatsby-cli/)
- 从 [Typefaces 项目](https://github.com/KyleAMathews/typefaces) 中挑选字体包

#### 操作步骤

1. 运行 `npm install --save typeface-your-chosen-font`，替换 `your-chosen-font` 为你在 [Typefaces 项目](https://github.com/KyleAMathews/typefaces) 中想要的字体。

比如要加载现在很流行的 “Source Sans Pro” 字体，就运行 `npm install --save typeface-source-sans-pro`。

2. 添加 `import "typeface-your-chosen-font"` 这一行代码到一个布局模板，页面组件，或 `gatsby-browser.js`中。

```jsx:title=src/components/layout.js
import "typeface-your-chosen-font"
```

3. 引入之后，你就可以在一个 CSS 样式表，CSS 模块或者 CSS-in-JS 中引用字体名字了。

```css:title=src/components/layout.css
body {
  font-family: "Your Chosen Font";
}
```

_注意： 对于上面这个例子，CSS 字体引用应该是 `font-family: 'Source Sans Pro';`_

#### 补充资源

- [Typography.js](/docs/typography-js/)——另一个在 Gatsby 站点中使用 Google 字体的选项
- [Typefaces 项目文档](https://github.com/KyleAMathews/typefaces/blob/master/README.md)
- [Kyle Mathews 博客上的在线演示](https://www.bricolage.io/typefaces-easiest-way-to-self-host-fonts/)

## 3. 使用 starters

[Starters](/docs/starters/) 是官方或社区维护的 Gatsby 站点样板。

### 使用一个 starter

#### 前置条件

- 安装好 [Gatsby CLI](/docs/gatsby-cli)

#### 操作步骤

1. 找到你想使用的 starter (_[Starter 库](/starters/?v=2)里应有尽有！_)

2. 基于 starter 生成一个新站点。 在终端中运行：

```shell
gatsby new {your-project-name} {link-to-starter}
```

> _不要一字不改地运行上面的命令——记得把 {your-project-name} 和 {link-to-starter} 替换成你自己的！_

3. 运行你的新站点：

```shell
cd {your-project-name}
gatsby develop
```

#### 补充资源

- 跟着 [更详细的指导](/docs/starters/) 来学习使用 Gatsby starters
- 学习如何使用 [Gatsby CLI](/docs/gatsby-cli) 工具，在 [教程第 1 章](/tutorial/part-one/#using-gatsby-starters) 中使用 starter
- 浏览 [Starter 库](/starters/?v=2)
- 看看我们 Gatsby 的 [官方默认 starter](https://github.com/gatsbyjs/gatsby-starter-default)

## 4. 处理主题

Gatsby 主题将 Gatsby 的配置（共享功能，数据源，设计）抽象为可安装的程序包。 这意味着配置和功能不是直接写到你的项目中，而是通过版本化，集中管理等手段，并作为依赖项安装。 你可以无缝更新主题、将主题组合在一起，甚至可以将一个主题换成另一个兼容的主题。

- 更多内容请参考 什么是 Gatsby 主题？](/docs/themes/what-are-gatsby-themes)

### 使用主题 starter 创建新站点

基于有主题配置的 starter 创建网站的过程，与基于**没有主题配置**的启动器创建网站的过程相同。 在此示例中，你可以使用 [使用官方 Gatsby 博客主题的新站点 starter](https://github.com/gatsbyjs/gatsby-starter-blog-theme)。

#### 前置条件

- 安装好 [Gatsby CLI](/docs/gatsby-cli)

#### 操作步骤

1. 基于博客主题 starter 生成一个新站点：

```shell
gatsby new {your-project-name} https://github.com/gatsbyjs/gatsby-starter-blog-theme
```

2. 运行你的新站点：

```shell
cd {your-project-name}
gatsby develop
```

#### 补充资源

- 在 [简短的概念指南](/docs/themes/using-a-gatsby-theme) 中学习如何使用一个已有的 Gatsby 主题，或参考更详细的 [分步教程](/tutorial/using-a-theme)。

### 构建一个新主题

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-the-gatsby-theme-workspace-starter-to-begin-building-a-new-theme"
  lessonTitle="Use the Gatsby Theme Workspace Starter to Begin Building a New Theme"
/>

#### 前置条件

- 安装好 [Gatsby CLI](/docs/gatsby-cli)

* 安装好 [Yarn](https://yarnpkg.com/lang/en/docs/install/#mac-stable)

#### 操作步骤

1. 使用 [Gatsby 主题工作区 starter](https://github.com/gatsbyjs/gatsby-starter-theme-workspace) 生成一个新的主题工作区：

```shell
gatsby new {your-project-name} https://github.com/gatsbyjs/gatsby-starter-theme-workspace
```

2. 在工作区中运行示例站点：

```shell
yarn workspace example develop
```

#### 补充资源

- 阅读关于如何使用 Gatsby 主题工作区 starter 的 [更详细的指南](/docs/themes/building-themes/)。
- 通过 [Egghead 上的 Gatsby Theme Authoring 视频](https://egghead.io/courses/gatsby-theme-authoring) 学习如何构建你自己的主题，或者通过 [视频课程的补充书面教程](/tutorial/building-a-theme)。

## 5. 处理数据源

在 Gatsby 中处理数据源是基于插件的：数据源插件从数据源中提取数据（例如 `gatsby-source-filesystem` 插件从文件系统中提取数据，`gatsby-source-wordpress` 插件从 WordPress API 提取数据，等等）。你也可以自行处理数据源。

### 向 GraphQL 添加数据

Gatsby 的 [GraphQL 数据层](/docs/querying-with-graphql/) 使用节点来描述数据块。Gatsby 的数据源插件添加了你可以查询到的数据源节点，你也可以自己添加数据源节点。要自己添加自定义的数据到 GraphQL 数据层，Gatsby 提供了你可以使用的方法。

这个配方将展示如何用 `createNode()` 添加自定义数据。

#### 操作步骤

1. 在 `gatsby-node.js` 中，使用 `sourceNodes()` 和 `actions.createNode()` 创建并导出可供数据查询的节点。

```javascript:title=gatsby-node.js
exports.sourceNodes = ({ actions, createNodeId, createContentDigest }) => {
  const pokemons = [
    { name: "Pikachu", type: "electric" },
    { name: "Squirtle", type: "water" },
  ]

  pokemons.forEach(pokemon => {
    const node = {
      name: pokemon.name,
      type: pokemon.type,
      id: createNodeId(`Pokemon-${pokemon.name}`),
      internal: {
        type: "Pokemon",
        contentDigest: createContentDigest(pokemon),
      },
    }
    actions.createNode(node)
  })
}
```

2. 运行 `gatsby develop`。

   > _注意：在你修改了 `gatsby-node.js` 之后，你需要重新运行 `gatsby develop` 使改变生效。_

3. 查询数据（在 GraphiQL 中或在组件中）。

```graphql
query MyPokemonQuery {
  allPokemon {
    nodes {
      name
      type
      id
    }
  }
}
```

#### 补充资源

- 使用 `gatsby-source-filesystem` 插件过一遍 [第 5 章教程](/tutorial/part-five/#source-plugins) 中的例子
- 在 [Gatsby 插件库](/plugins/?=source) 中搜索可用的数据源插件
- 在 [Pixabay 数据源教程](/docs/pixabay-source-plugin-tutorial/) 中通过构建一个数据源插件来理解数据源插件
- createNode 方法的 [文档](/docs/actions/#createNode)

### 使用 GraphQL 为博文和其他页面获取 Markdown 数据

你可以获取 Markdown 数据并使用 Gatsby 的 [`createPages` API](/docs/actions/#createPage) 来动态生成页面。

这个配方告诉你如何在你本地文件系统中，通过 Gatsby 的 GraphQL 数据层使用 Markdown 文件创建页面。

#### 前置条件

- 一个拥有 `gatsby-config.js` 文件的 [Gatsby 站点](/docs/quick-start)
- 安装好 [Gatsby CLI](/docs/gatsby-cli)
- 安装好 [gatsby-source-filesystem 插件](/packages/gatsby-source-filesystem)
- 安装好 [gatsby-transformer-remark 插件](/packages/gatsby-transformer-remark)
- 一个 `gatsby-node.js` 文件

#### 操作步骤

1. 在 `gatsby-config.js` 文件中, 配置 `gatsby-transformer-remark` 和 `gatsby-source-filesystem` 文件，让它们从一个源文件夹中获取 Markdown 文件。 如果你已经有其他 `gatsby-source-filesystem` 项目，比如图片，你应该添加到它们一起:

```js:title=gatsby-config.js
module.exports = {
  plugins: [
    `gatsby-transformer-remark`,
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `content`,
        path: `${__dirname}/src/content`,
      },
    },
  ]
```

2. 在 `src/content` 添加一篇 Markdown 博文，包含 title，date 和 path 信息作为 frontmatter，和一些文章主体的初步内容：

```markdown:title=src/content/my-first-post.md
---
title: My First Post
date: 2019-07-10
path: /my-first-post
---

This is my first Gatsby post written in Markdown!
```

3. 通过运行 `gatsby develop` 启动开发服务器，导航到 GraphiQL 浏览器 <http://localhost:8000/___graphql>，使用下面的查询语句来获取所有 Makrdown 数据：

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        frontmatter {
          path
        }
      }
    }
  }
}
```

<iframe
  title="Query for all markdown"
  src="https://q4xpb.sse.codesandbox.io/___graphql?explorerIsOpen=false&query=%7B%0A%20%20allMarkdownRemark%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20frontmatter%20%7B%0A%20%20%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D"
  width="600"
  height="300"
/>

4. 添加 JavaScript 代码以在构建时为 Makrdown 博文生成页面，通过复制 GraphQL 查询命令到 `gatsby-node.js` 中，并在循环内使用结果：

```js:title=gatsby-node.js
const path = require(`path`)

exports.createPages = async ({ actions, graphql }) => {
  const { createPage } = actions

  const result = await graphql(`
    {
      allMarkdownRemark {
        edges {
          node {
            frontmatter {
              path
            }
          }
        }
      }
    }
  `)
  if (result.errors) {
    console.error(result.errors)
  }

  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.frontmatter.path,
      component: path.resolve(`src/templates/post.js`),
    })
  })
}
```

5. 添加一个博文模版到 `src/templates`。这个模版要包含一个 GraphQL 查询语句，在构建时为 Markdown 内容动态生成页面：

```jsx:title=src/templates/post.js
import React from "react"
import { graphql } from "gatsby"

export default function Template({ data }) {
  const { markdownRemark } = data // data.markdownRemark holds your post data
  const { frontmatter, html } = markdownRemark
  return (
    <div className="blog-post">
      <h1>{frontmatter.title}</h1>
      <h2>{frontmatter.date}</h2>
      <div
        className="blog-post-content"
        dangerouslySetInnerHTML={{ __html: html }}
      />
    </div>
  )
}

export const pageQuery = graphql`
  query($path: String!) {
    markdownRemark(frontmatter: { path: { eq: $path } }) {
      html
      frontmatter {
        date(formatString: "MMMM DD, YYYY")
        path
        title
      }
    }
  }
`
```

6. 运行 `gatsby develop` 来重启开发服务器。 在你的浏览器里查看博文：<http://localhost:8000/my-first-post>

#### 补充资源

- [教程：以编程的方式通过数据创建页面](/tutorial/part-seven/)
- [创建和修改页面](/docs/creating-and-modifying-pages/)
- [添加 Markdown 页面](/docs/adding-markdown-pages/)
- [以编程的方式通过数据创建页面的指南](/docs/programmatically-create-pages-from-data/)
- [示例仓库](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-sourcing-markdown) for this recipe

### 从 WordPress 中获取数据

#### 前置条件

- 一个拥有 `gatsby-config.js` 和 `gatsby-node.js` 文件的 [Gatsby 站点](/docs/quick-start/)。
- 一个 WordPress 实例。可以是自己托管的，也可以是 Wordpress.com 托管的

#### 操作步骤

1. 通过运行以下命令安装 `gatsby-source-wordpress` 插件：

```shell
npm install gatsby-source-wordpress --save
```

2. 通过修改 `gatsby-config.js` 文件来配置插件，修改内容如下：

```javascript:title=gatsby-config.js
module.exports = {
  ...
  plugins: [
    {
      resolve: `gatsby-source-wordpress`,
      options: {
        // baseUrl will need to be updated with your WordPress source
        baseUrl: `wpexample.com`,
        protocol: `https`,
        // is it hosted on wordpress.com, or self-hosted?
        hostingWPCOM: false,
        // does your site use the Advanced Custom Fields Plugin?
        useACF: false
      }
    },
  ]
}
```

> **注意：** 请参考 [`gatsby-source-wordpress` 插件文档](/packages/gatsby-source-wordpress/?=wordpre#how-to-use) 来了解更多配置插件的信息。

3. 创建一个模版组件，例如包含以下代码的 `src/templates/post.js`：

```javascript:title=post.js
import React, { Component } from "react"
import { graphql } from "gatsby"
import PropTypes from "prop-types"

class Post extends Component {
  render() {
    const post = this.props.data.wordpressPost

    return (
      <>
        <h1>{post.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.content }} />
      </>
    )
  }
}

Post.propTypes = {
  data: PropTypes.object.isRequired,
  edges: PropTypes.array,
}

export default Post

export const pageQuery = graphql`
  query($id: String!) {
    wordpressPost(id: { eq: $id }) {
      title
      content
    }
  }
`
```

4. 为你的 WordPress 博文创建动态页面，通过粘贴以下样本代码到 `gatsby-node.js`：

```javascript:title=gatsby-node.js
const path = require(`path`)
const slash = require(`slash`)

exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions

  // query content for WordPress posts
  const result = await graphql(`
    query {
      allWordpressPost {
        edges {
          node {
            id
            slug
          }
        }
      }
    }
  `)

  const postTemplate = path.resolve(`./src/templates/post.js`)
  result.data.allWordpressPost.edges.forEach(edge => {
    createPage({
      // `path` will be the url for the page
      path: edge.node.slug,
      // specify the component template of your choice
      component: slash(postTemplate),
      // In the ^template's GraphQL query, 'id' will be available
      // as a GraphQL variable to query for this posts's data.
      context: {
        id: edge.node.id,
      },
    })
  })
}
```

5. 运行 `gatsby-develop` ，导航到新生成的页面查看它们。

6. 通过 `localhost:8000/__graphql` 打开 `GraphiQL IDE` 并且打开 Docs 或 Explorer 来为 `allWordpressPosts` 查看可查询的字段。

之前在 `gatsby-node.js` 里创建的动态页面，有导航到具体博文的唯一路径，生成博文的模版组件，和一个示例 GraphQL 命令来获取 WordPress 博文内容。

#### 补充资源

- [开始使用 WordPress 和 Gatsby](/blog/2019-04-26-how-to-build-a-blog-with-wordpress-and-gatsby-part-1/)
- 更多 [从 WordPress 获取数据](/docs/sourcing-from-wordpress/) 的信息
- [从 WordPress 获取数据的在线演示](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-wordpress)

### 从 Contentful 中获取数据

#### 前置条件

- 一个 [Gatsby 站点](/docs/quick-start/)
- 一个 [Contentful 账号](https://www.contentful.com/)
- 安装好 [Contentful CLI](https://www.npmjs.com/package/contentful-cli)

#### 操作步骤

1. 使用 CLI 和以下步骤登录到 Contentful。如果你还没有账号的话，它会为你创建一个账号。

```shell
contentful login
```

2. 创建一个新 space 如果你还没创建的话。确保你保存了命令最后给出的 space ID。如果你已经有一个 Contentful space 和 space ID，你可以跳过第 2 步和第 3 步。

注意：对于新账户，你可以覆盖默认的引导 space。详情请查看 [账户中的 space](https://app.contentful.com/account/profile/space_memberships)。

```shell
contentful space create --name 'Gatsby example'
```

3. 新建一个种子 space 来包含示例博文内容。使用在上一个命令中返回的新 space ID 替换掉 `<space ID>`。

```shell
contentful space seed -s '<space ID>' -t blog
```

这是一个包含真实 space ID 的例子：`contentful space seed -s '22fzx88spbp7' -t blog`

4. 为你的 space 创建一个新访问令牌（access token）。记住这个令牌，你会在第 6 步使用它。

```shell
contentful space accesstoken create -s '<space ID>' --name 'Example token'
```

5. 在你的 Gatsby 站点中安装 `gatsby-source-contentful` 插件：

```shell
npm install --save gatsby-source-contentful
```

6. 编辑文件 `gatsby-config.js` ，添加 `gatsby-source-contentful` 到 `plugins` 数组中来启用插件。出于安全考虑，我们强烈建议你使用 [环境变量](/docs/environment-variables/) 来存储你的 space ID 和令牌。

```javascript:title=gatsby-config.js
plugins: [
   // add to array along with any other installed plugins
   // highlight-start
   {


    resolve: `gatsby-source-contentful`,
    options: {
      spaceId: `<space ID>`, // or process.env.CONTENTFUL_SPACE_ID
      accessToken: `<access token>`, // or process.env.CONTENTFUL_TOKEN
    },
  },
  // highlight-end
],
```

7. 运行 `gatsby develop` 并确保站点成功编译。

8. 在 <https://localhost:8000/___graphql> 使用 [GraphiQL editor](/docs/introducing-graphiql/) 查询数据。 Contentful 插件在你的站点中添加了一些新节点类型，包括你的的 Contentful 网站的每一个内容类型。你的示例 space包含了一个 “Blog Post” 内容类型，它为你在 GraphQL 中生成了一个 `allContentfulBlogPost` 节点类型。 

![使用了下述查询的 GraphQL 界面](./images/recipe-sourcing-contentful-graphql.png)

要从 Contentful 中查询博文标题，请使用以下 GraphQL 命令：

```graphql
{
  allContentfulBlogPost {
    edges {
      node {
        title
      }
    }
  }
}
```

Contentful 节点还包括了一些元数据字段，比如 `createdAt` 和 `node_locale`。

9. 要显示一个博文链接列表，添加文件 `/src/pages/blog.js`。这个页面会显示所有博文，按照更新日期排序。

```jsx:title=src/pages/blog.js
import React from "react"
import { graphql, Link } from "gatsby"

const BlogPage = ({ data }) => (
  <div>
    <h1>Blog</h1>
    <ul>
      {data.allContentfulBlogPost.edges.map(({ node, index }) => (
        <li key={index}>
          <Link to={`/blog/${node.slug}`}>{node.title}</Link>
        </li>
      ))}
    </ul>
  </div>
)

export default BlogPage

export const query = graphql`
  {
    allContentfulBlogPost(sort: { fields: [updatedAt] }) {
      edges {
        node {
          title
          slug
        }
      }
    }
  }
`
```

要继续构建你的 Contentful 站点使其包含详情页面，请查看余下的 [Gatsby 文档](/docs/sourcing-from-contentful/) 和下面列出的补充资源。

#### 补充资源

- [使用 React 和 Contentful 构建网站](/blog/2018-1-25-building-a-site-with-react-and-contentful/)
- [更多从 Contentful 中获取数据的信息](/docs/sourcing-from-contentful/)
- [Contentful 数据源插件](/packages/gatsby-source-contentful/)
- [长文本字段类型作为对象返回](/packages/gatsby-source-contentful/#a-note-about-longtext-fields)
- [这个配方的示例仓库](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-sourcing-contentful)

### 从外部数据源中获取数据并且不使用 GraphQL 创建页面

虽然 [一些理由告诉你应该考虑使用 GraphQL](/docs/why-gatsby-uses-graphql/)，但是你不是一定要使用 GraphQL 数据层来为页面引入数据。你可以使用节点 `createPages` API 来直接把非结构化数据导入到你的 Gatsby 站点中，以替代 GraphQL 加数据源插件的方案。

在这个配方中，你将会使用从 [PokéAPI 的 REST 端点](https://www.pokeapi.co/) 获取的数据创建动态页面。[完整示例](https://github.com/jlengstorf/gatsby-with-unstructured-data/) 在 GitHub 上。

#### 前置条件

- 有一个 `gatsby-node.js` 文件的 Gatsby 站点。
- 安装好 [Gatsby CLI](/docs/gatsby-cli)
- 通过 npm 安装好 [axios](https://www.npmjs.com/package/axios) 包

#### 操作步骤

1. 在 `gatsby-node.js` 中添加 JavaScript 代码来从 PokéAPI 获取数据，并以编程的方式创建一个索引页面：

```js:title=gatsby-node.js
const axios = require("axios")

const get = endpoint => axios.get(`https://pokeapi.co/api/v2${endpoint}`)

const getPokemonData = names =>
  Promise.all(
    names.map(async name => {
      const { data: pokemon } = await get(`/pokemon/${name}`)
      return { ...pokemon }
    })
  )
exports.createPages = async ({ actions: { createPage } }) => {
  const allPokemon = await getPokemonData(["pikachu", "charizard", "squirtle"])

  // Create a page that lists Pokémon.
  createPage({
    path: `/`,
    component: require.resolve("./src/templates/all-pokemon.js"),
    context: { allPokemon },
  })
}
```

2. 创建一个模版来在主页显示 Pokémon：

```js:title=src/templates/all-pokemon.js
import React from "react"

export default ({ pageContext: { allPokemon } }) => (
  <div>
    <h1>Behold, the Pokémon!</h1>
    <ul>
      {allPokemon.map(pokemon => (
        <li key={pokemon.id}>
          <img src={pokemon.sprites.front_default} alt={pokemon.name} />
          <p>{pokemon.name}</p>
        </li>
      ))}
    </ul>
  </div>
)
```

3. 运行 `gatsby develop` 来获取数据，构建页面，并且启动开发服务器。

4. 在你的浏览器中查看主页：<http://localhost:8000>

#### 补充资源

- [完整的 Pokemon 数据仓库](https://github.com/jlengstorf/gatsby-with-unstructured-data/)
- 更多使用非结构化数据的信息 [在不使用 GraphQL 的情况下使用 Gatsby](/docs/using-gatsby-without-graphql/)
- 对更复杂的 Gatsby 站点，何时以及如何 [使用 GraphQL 查询数据](/docs/querying-with-graphql/)

### 从 Drupal 中获取内容

#### 前置条件

- 一个 [Gatsby 站点](/docs/quick-start)
- 一个 [Drupal](http://drupal.org) 站点
- 在 Drupal 站点中安装并启用了 [JSON:API 模块](https://www.drupal.org/project/jsonapi)

#### 操作步骤

1. 安装 `gatsby-source-drupal` 插件。

```
npm install --save gatsby-source-drupal
```

2. 编辑你的 `gatsby-config.js` 文件，启用并配置插件：

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-source-drupal`,
      options: {
        baseUrl: `https://your-website/`,
        apiBase: `api`, // optional, defaults to `jsonapi`
      },
    },
  ],
}
```

3. 运行 `gatsby develop` 以启动开发服务器。在 <http://localhost:8000/___graphql> 中打开 GraphiQL 浏览器。在 Explorer 标签下，你应该看到新节点类型，例如显示 Drupal block 信息的 `allBlockBlock` ，和一个显示 Drupal 站点中每一个内容类型的节点。例如假设你有一个 “Page” 内容类型，它能在 `allNodePage` 中显示。要查询所有 “Page” 节点的标题和内容，使用如下查询命令：

```graphql
{
  allNodePage {
    edges {
      node {
        title
        body {
          value
        }
      }
    }
  }
}
```

4. 要使用你的 Drupal 数据，在 Gatsby 中创建一个新页面 `src/pages/drupal.js`。 这个页面会列出所有 Drupal "Page" 节点。

_**注意：** 确切的 GraphQL 模式将取决于你的 Drupal 实例的结构。_

```jsx:title=src/pages/drupal.js
import React from "react"
import { graphql } from "gatsby"

const DrupalPage = ({ data }) => (
  <div>
    <h1>Drupal pages</h1>
    <ul>
    {data.allNodePage.edges.map(({ node, index }) => (
      <li key={index}>
        <h2>{node.title}</h2>
        <div>
          {node.body.value}
        </div>
      </li>
    ))}
   </ul>
  </div>
)

export default DrupalPage

export const query = graphql`
  {
  allNodePage {
    edges {
      node {
        title
        body {
          value
        }
      }
    }
  }
}
```

5. 运行开发服务器后，你可以通过访问以下页面来查看新页面 <http://localhost:8000/drupal>。

#### 补充资源

- [在 Gatsby 中使用 Decoupled Drupal ](/blog/2018-08-13-using-decoupled-drupal-with-gatsby/)
- [更多从 Drupal 中获取数据的信息](/docs/sourcing-from-drupal)
- [教程第 7 章：以编程的方式通过数据创建页面](/tutorial/part-seven/)

## 6. 查询数据

### 使用页面查询获取数据

你可以使用 `graphql` 标签在你的 Gatsby 站点页面中查询数据。这样你就可以访问 Gatsby 数据层中包含的所有内容，例如网站元数据，数据源插件，图像等。

#### 操作步骤

1. 从 `gatsby` 中引入 `graphql`。

2. 导出一个名为 `query` 的变量，并设置它的值为 `graphql` 模版加上用两个反引号括起来的查询命令。

3. 把 `data` 作为 prop 传入到组件中。

4. `data` 变量保存了被查询的数据，并且可以被 JSX 引用来生成 HTML。

```jsx:title=src/pages/index.js
import React from "react"
// highlight-next-line
import { graphql } from "gatsby"

import Layout from "../components/layout"

// highlight-start
export const query = graphql`
  query HomePageQuery {
    site {
      siteMetadata {
        title
      }
    }
  }
`
// highlight-end

// highlight-next-line
const IndexPage = ({ data }) => (
  <Layout>
    // highlight-next-line
    <h1>{data.site.siteMetadata.title}</h1>
  </Layout>
)

export default IndexPage
```

#### 补充资源

- [GraphQL 和 Gatsby](/docs/graphql/)：了解数据的预期格式
- [更多使用 GraphQL 获取页面数据的信息](/docs/page-query/)
- [模板字符串的 MDN 页面](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) 就像我们在 GraphQL 中使用的那样

### 使用 StaticQuery 组件查询数据

`StaticQuery` 是一个从 Gatsby 数据层获取 [非页面组件](/docs/static-query/) 数据的组件。非页面组件包括页眉，导航栏，或者其他子组件。

#### 操作步骤

1. `StaticQuery` 组件需要两个用于渲染的 props: `query` 和 `render`。

```jsx:title=src/components/NonPageComponent.js
import React from "react"
import { StaticQuery, graphql } from "gatsby"

const NonPageComponent = () => (
  <StaticQuery
    query={graphql` // highlight-line
      query NonPageQuery {
        site {
          siteMetadata {
            title
          }
        }
      }
    `}
    render={(
      data // highlight-line
    ) => (
      <h1>
        Querying title from NonPageComponent with StaticQuery:
        {data.site.siteMetadata.title}
      </h1>
    )}
  />
)

export default NonPageComponent
```

2. 你现在可以在 [任何其他组件](/docs/building-with-components#non-page-components) 中使用这个组件，通过把它引入到一个更大的包含 JSX 组件和 HTML 标记的页面。

### 使用 useStaticQuery hook 查询数据

从 Gatsby v2.1.0 版本起，你可以使用 `useStaticQuery` hook 来查询数据。它是一个 JavaScript 函数而不是一个组件。这个函数语法不再需要一个 `<StaticQuery>` 来包含所有东西。对于一些人来说更容易编写。 

`useStaticQuery` hook 接收一个 GraphQL 查询命令并返回所查询的数据。它可以被存储到一个变量里，在之后的 JSX 模版中使用。

#### 前置条件

- 你将需要 React 和 ReactDOM 16.8.0 或更高版本 (持续更新 Gatsby 的话就没问题了)
- 推荐阅读：[React Hooks 的使用规则](https://reactjs.org/docs/hooks-rules.html)

#### 操作步骤

1. 从 `gatsby` 中引入 `useStaticQuery` 和 `graphql` 以使用 hook 来查询数据。

2. 在一个无状态功能性组件的一开始，使用 `useStaticQuery` 和 `graphql` 查询命令作为其参数，给一个变量赋值。

3. 在组件返回的 JSX 代码中，你可以引用这个变量来处理返回的数据。

```jsx:title=src/components/NonPageComponent.js
import React from "react"
import { useStaticQuery, graphql } from "gatsby" //highlight-line

const NonPageComponent = () => {
  // highlight-start
  const data = useStaticQuery(graphql`
    query NonPageQuery {
      site {
        siteMetadata {
          title
        }
      }
    }
  `)
  // highlight-end
  return (
    <h1>
      Querying title from NonPageComponent: {data.site.siteMetadata.title}{" "}
      //highlight-line
    </h1>
  )
}

export default NonPageComponent
```

#### 补充资源

- [更多在组件中使用 Static Query 查询数据的信息](/docs/static-query/)
- [Static query 和 page query 的区别](/docs/static-query/#how-staticquery-differs-from-page-query)
- [更多 useStaticQuery hook 的信息](/docs/use-static-query/)
- [使用 GraphiQL 可视化数据](/docs/introducing-graphiql/)

### 给 GraphQL 添加限制

使用 GraphQL 查询数据时，可以使用具体数字限制返回的结果数。 如果你只需要少量数据或需要 [给数据分页](/docs/adding-pagination/)，这将很有帮助。

为了限制数据，你需要一个Gatsby站点，该站点在 GraphQL 数据层中有一些节点。 所有站点都有一些自动创建的节点，例如  `allSitePage` 和 `sitePage`：可以通过在 `gatsby-config.js` 中安装数据源插件（如 `gatsby-source-filesystem`）来添加更多节点。

#### 前置条件

- 一个 [Gatsby 站点](/docs/quick-start/)

#### 操作步骤

1. 运行 `gatsby develop` 来启动开发服务器。
2. 在你的浏览器中打开页面：<http://localhost:8000/___graphql>。
3. 在编辑器中添加一个具有以下字段的 `allSitePage` 查询命令作为开始：

```graphql
{
  allSitePage {
    edges {
      node {
        id
        path
      }
    }
  }
}
```

4. 在 `allSitePage` 字段中添加一个 `limit` 参数并给它一个整数值 `3`。

```graphql
{
  allSitePage(limit: 3) { // highlight-line
    edges {
      node {
        id
        path
      }
    }
  }
}
```

5. 单击 GraphiQL 页面中的播放按钮，`edges` 字段中的数据将限制为指定的数量。

#### 补充资源

- 学习 [Gatsby 的 GraphQL 数据 API 中的节点](/docs/node-interface/)
- [Gatsby GraphQL 添加限制的参考](/docs/graphql-reference/#limit)
- 在线演示：

<iframe
  title="Limiting returned data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allSitePage(limit%3A%203)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### 使用 GraphQL 的结果排序

结果的顺序可以用 GraphQL 的 `sort` 参数来控制。你可以指定使用哪个字段来排序，也可以设置升序还是降序。

对于这个配方，你将需要一个 Gatsby 站点和一系列节点以在 GraphQL 数据层中排序。所有的站点都有一些自动生成的节点，例如 `allSitePage`。安装数据源插件以后会有更多节点。

#### 前置条件

- 一个 [Gatsby 站点](/docs/quick-start)
- 以 `all` 为前缀的可查询字段，例如 `allSitePage`

#### 操作步骤

1. 运行 `gatsby develop` 以启动开发服务器
2. 在浏览器标签中打开 GraphiQL 浏览器：<http://localhost:8000/___graphql>
3. 在编辑器中添加一个具有以下字段的 `allSitePage` 查询命令作为开始：

```graphql
{
  allSitePage {
    edges {
      node {
        id
        path
      }
    }
  }
}
```

4. 在 `allSitePage` 字段中添加一个 `sort` 参数并且给它一个包含 `fields` 和 `order` 属性的对象。`fields` 的值可以是用于排序的一个字段或一个字段数组（这个例子中我们使用了 `path` 字段），`order` 可以是递增 `ASC` 或递减 `DESC`。

```graphql
{
  allSitePage(sort: {fields: path, order: ASC}) { // highlight-line
    edges {
      node {
        id
        path
      }
    }
  }
}

```

5. 点击 GraphiQL 页面中的播放按钮，返回的数据就会以 `path` 字段递增的顺序显示出来了。

#### 补充资源

- [Gatsby GraphQL 排序参考文档](/docs/graphql-reference/#sort)
- 学习 [Gatsby's GraphQL 数据 API 中的节点](/docs/node-interface/)
- 在线演示：

<iframe
  title="Sorting data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allSitePage(sort%3A%20%7Bfields%3A%20path%2C%20order%3A%20ASC%7D)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### 使用 GraphQL 的结果过滤

被查询到的数据可以将 `eq`（等于）、`ne`（不等于）、`in`（位于）和 `regex`（正则匹配）等操作符引用到指定字段上来进行过滤。

对于这个配方，你将需要一个拥有一系列节点的 Gatsby 站点来在 GraphQL 数据层中进行过滤。所有的站点都有一些自动生成的节点，例如 `allSitePage`。安装数据转换插件以后会有更多节点，例如可以在 `gatsby-config.js` 中安装 `gatsby-source-filesystem` 和 `gatsby-transformer-remark` 来生成 `allMarkdownRemark`。

#### 前置条件

- 一个 [Gatsby 站点](/docs/quick-start)
- 以 `all` 为前缀的可查询字段，例如 `allSitePage` 和 `allMarkdownRemark`

#### 操作步骤

1. 运行 `gatsby develop` 以启动开发服务器
2. 在浏览器标签中打开 GraphiQL 浏览器：<http://localhost:8000/___graphql>
3. 在编辑器中添加一个以 “all” 为前缀的字段，例如 `allMarkdownRemark`（意味着会返回一个节点列表）

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        frontmatter {
          title
          categories
        }
      }
    }
  }
}
```

4. 添加一个 `filter` 参数到  `allMarkdownRemark` 字段中，给它一个包含你想要过滤的字段的对象。在这个例子中，Markdown 内容被在 frontmatter 元数据中的 `categories` 属性过滤了。下一个值是操作符：这个例子中是 `eq`，或者 equals，值为 “magical creatures”。

```graphql
{
  allMarkdownRemark(filter: {frontmatter: {categories: {eq: "magical creatures"}}}) { // highlight-line
    edges {
      node {
        frontmatter {
          title
          categories
        }
      }
    }
  }
}
```

5. 点击 GraphiQL 页面中的播放按钮，只有和过滤器参数匹配的数据会返回。在这个例子中，只有类别为 “magical creatures” 的能成功获取的 Markdown 文件会被返回出来。

#### 补充资源

- [Gatsby GraphQL 过滤的参考文档](/docs/graphql-reference/#filter)
- [所有操作符的完整列表](/docs/graphql-reference/#complete-list-of-possible-operators)
- 学习 [Gatsby's GraphQL 数据 API 中的节点](/docs/node-interface/)
- 在线演示：

<iframe
  title="Filtering data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allMarkdownRemark(filter%3A%20%7Bfrontmatter%3A%20%7Bcategories%3A%20%7Beq%3A%20%22magical%20creatures%22%7D%7D%7D)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20frontmatter%20%7B%0A%20%20%20%20%20%20%20%20%20%20title%0A%20%20%20%20%20%20%20%20%20%20categories%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### GraphQL 查询命令别名（Query Alias）

你可以重命名 GraphQL 查询命令中的任意字段为一个别名（alias）。

如果你想要在同一个数据源中运行两条查询命令，你可以使用一个别名来避免两条查询命令中的命名冲突。

#### 操作步骤

1. 运行 `gatsby develop` 以启动开发服务器。
2. 在浏览器标签中打开 GraphiQL 浏览器：<http://localhost:8000/___graphql>
3. 在编辑器中添一个有两个相同名字 `allFile` 字段的查询指令

```graphql
{
  allFile {
    totalCount
  }
  allFile {
    pageInfo {
      currentPage
    }
  }
}
```

4. 在你的 GraphQL 模式中，在任何你想要设置别名的字段前面添加一个名字，用冒号分隔（例如 `[alias-name]: [original-name]`）

```graphql
{
  fileCount: allFile { // highlight-line
    totalCount
  }
  filePageInfo: allFile { // highlight-line
    pageInfo {
      currentPage
    }
  }
}
```

5. 在 GraphiQL 页面中点击播放按钮，两个带有你提供的别名的对象就会显示出来。

#### 补充资源

- [Gatsby GraphQL 别名参考文档](/docs/graphql-reference/#aliasing)
- 在线演示：

<iframe
  title="Using aliases"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20fileCount%3A%20allFile%20%7B%20%0A%20%20%20%20totalCount%0A%20%20%7D%0A%20%20filePageInfo%3A%20allFile%20%7B%0A%20%20%20%20pageInfo%20%7B%0A%20%20%20%20%20%20currentPage%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### GraphQL 查询指令片段（Fragments）

GraphQL 片段（fragments）是可分享的可重用的一段查询指令。

你可能想要使用它来在查询语句之间分享多个字段，或者在一个组件中使数据共存。

#### 操作步骤

1. 声明一个 `graphql` 模版字符串，包含一个片段。这个片应该由关键字 `fragment`、一个名字、它所使用的 GraphQL 类型（在这个例子中是 `Site`，用 `on Site` 来表示）、和组成片段的字段：

```jsx
export const query = graphql`
  // highlight-start
  fragment SiteInformation on Site {
    title
    description
  }
  // highlight-end
`
```

2. 现在，在查询语句内和片段指定的类型相同的字段中，使用这个片段。这样就能包含那些字段，而不用去逐一声明它们：

```diff
export const pageQuery = graphql`
  query SiteQuery {
    site {
-     title
-     description
+   ...SiteInformation
    }
  }
`
```

**注意**：片段不需要在 Gatsby 中被引入。导出一个包含片段的查询命令，片段就能在项目里 _所有_ 查询命令中被使用。

片段可以被嵌套在其他片段中。一个查询指令中可以使用片段多次。

#### 补充资源

- [使用片段的简单示例仓库](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-fragments)
- [Gatsby GraphQL 片段的参考文档](/docs/graphql-reference/#fragments)
- [Gatsby 图像片段](/docs/gatsby-image/#image-query-fragments)
- [使数据共存的示例仓库](https://github.com/gatsbyjs/gatsby/tree/master/examples/gatsbygram)

## 7. 使用图片

### 使用 webpack 将一个图片引入到组件中

Webpack 可以将图片引入到一个 JavaScript 模块中。这个过程中会自动最小化压缩图片并复制图片到你的站点的 `public` 目录中，并提供一个动态的图片 URL，使你可以像平常一样，将其传入到一个 HTML `<img>` 元素的文件路径中。

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-import-a-local-image-into-a-gatsby-component-with-webpack"
  lessonTitle="Import a Local Image into a Gatsby Component with webpack"
/>

#### 前置条件

- 一个 [Gatsby 站点](/docs/quick-start)，其中包含一个导出 React 组件的 `.js` 文件
- `src` 文件夹中有一个图片（`.jpg`、`.png`、`.gif`、`.svg` 等文件类型）

#### 操作步骤

1. 从文件的 `src` 文件夹的路径引入文件

```jsx:title=src/pages/index.js
import React from "react"
// Tell webpack this JS file uses this image
import FiestaImg from "../assets/fiesta.jpg" // highlight-line
```

2. 在 `index.js` 中，添加一个 `<img>` 标签，它的  `src` 属性设置为你从 webpack 中引入的名字（这个例子中是 `FiestaImg`），再添加一个 `alt` 属性来 [描述图片](https://webaim.org/techniques/alttext/)：

```jsx:title=src/pages/index.js
import React from "react"
import FiestaImg from "../assets/fiesta.jpg"

export default () => (
  // The import result is the URL of your image
  <img src={FiestaImg} alt="A dog smiling in a party hat" /> // highlight-line
)
```

3. 运行 `gatsby develop` 以启动开发服务器。

4. 在浏览器中查看图片：<http://localhost:8000/>

#### 补充资源

- [使用 webpack 引入图片的示例仓库](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-webpack-image)
- [Gatsby 中的所有图片技巧](/docs/images-and-files/)

### 从 `static` 文件夹中引用图片

作为一个使用 webpack 引入资源的替代选项，`static` 文件夹能在构建时自动复制到 `public` 文件夹，使图片能够访问。

这是一个在 [特定情况](/docs/static-folder/#when-to-use-the-static-folder) 下的 **迂回方案**。我们更推荐能够利用 Gatsby 优化的其他方案，例如 [使用 webpack 引入](#import-an-image-into-a-component-with-webpack)。

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-a-local-image-from-the-static-folder-in-a-gatsby-component"
  lessonTitle="Use a local image from the static folder in a Gatsby component"
/>

#### 前置条件

- 一个 [Gatsby 站点](/docs/quick-start)，其中包含一个导出 React 组件的 `.js` 文件
- `static` 文件夹中有一个图片（`.jpg`、`.png`、`.gif`、`.svg` 等文件类型）

#### 操作步骤

1. 确保项目根目录下的 `static` 文件夹中有你所需要的图片。你的项目结构看起来应该是这样：

```
├── gatsby-config.js
├── src
│   └── pages
│       └── index.js
├── static
│       └── fiesta.jpg
```

2. 在 `index.js` 中，添加一个 `<img>` 标签，它的  `src` 属性设置为 `static` 文件夹的相对路径，再添加一个 `alt` 属性来 [描述图片](https://webaim.org/techniques/alttext/)：

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <img src={`fiesta.jpg`} alt="A dog smiling in a party hat" /> // highlight-line
)
```

3. 运行 `gatsby develop` 以启动开发服务器。

4. 在浏览器中查看图片：<http://localhost:8000/>

#### 补充资源

- [从 static 文件夹中引用图片的示例仓库](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-static-image)
- [使用 static 文件夹](/docs/static-folder/)
- [Gatsby 中的所有图片技巧](/docs/images-and-files/)

### 使用 gatsby-image 优化和查询本地图片

`gatsby-image` 插件可以减轻站点图片优化的许多痛苦。

Gatsby 会生成优化好的资源，可以在 GraphQL 中被查询，并传入到 Gatsby 的图片组件中。这可以解决繁重的工作，包括创建多个图像尺寸并在正确的时间加载它们。

#### 前置条件

- 安装好 `gatsby-image`、`gatsby-transformer-sharp`，和 `gatsby-plugin-sharp` 包并添加到 `gatsby-config` 里的插件数组中
- 在 `gatsby-config` 中 [引用图片文件](/packages/gatsby-image/#install)，使用诸如 `gatsby-source-filesystem` 之类的插件

#### 操作步骤

1. 首先从 `gatsby-image` 中引入 `Img`，从 `gatsby` 中引入 `graphql` 和 `useStaticQuery`

```jsx
import { useStaticQuery, graphql } from "gatsby" // to query for image data
import Img from "gatsby-image" // to take image data and render it
```

2. 编写一个查询命令来获取图片数据，并传入数据到 `<Img />` 组件当中：

选择以下任意一个选项或者它们的组合：

a. 一个通过文件 [路径](/docs/content-and-data/) 查询的单一图片文件（例如：`images/corgi.jpg`）

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) { // highlight-line
      childImageSharp {
        fluid {
          base64
          aspectRatio
          src
          srcSet
          sizes
        }
      }
    }
  }
`)

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
)
```

b. 使用一个 [GraphQL 片段](/docs/using-graphql-fragments/) 来更简洁地查询所需要的字段

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fluid {
          ...GatsbyImageSharpFluid // highlight-line
        }
      }
    }
  }
`)

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
)
```

c. 使用 `extension` 和 `relativeDirectory` 字段 [过滤](/docs/graphql-reference/#filter) 后的一个目录中的多个图片文件（例如 `images/dogs`），然后映射到 `Img` 组件中

```jsx
const data = useStaticQuery(graphql`
  query {
    allFile(
      // highlight-start
      filter: {
        extension: { regex: "/(jpg)|(png)|(jpeg)/" }
        relativeDirectory: { eq: "dogs" }
      }
      // highlight-end
    ) {
      edges {
        node {
          base
          childImageSharp {
            fluid {
              ...GatsbyImageSharpFluid
            }
          }
        }
      }
    }
  }
`)

return (
  <div>
    // highlight-start
    {data.allFile.edges.map(image => (
      <Img
        fluid={image.node.childImageSharp.fluid}
        alt={image.node.base.split(".")[0]} // only use section of the file extension with the filename
      />
    ))}
    // highlight-end
  </div>
)
```

**注意**：这种方法可能会使匹配带有 `alt` 文本的图像变得困难。这个例子使用文件名中包含 `alt` 文本的图像，例如 `dog in a party hat.jpg`。

d. 一个固定大小的图片，使用 `fixed` 字段而不是 `fluid` 字段

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fixed(width: 250, height: 250) { // highlight-line
          ...GatsbyImageSharpFixed
        }
      }
    }
  }
`)
return (
  <Img fixed={data.file.childImageSharp.fixed} alt="A corgi smiling happily" />
)
```

e. 一个有 `maxWidth` 的固定大小图片

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fixed(maxWidth: 250) { // highlight-line
          ...GatsbyImageSharpFixed
        }
      }
    }
  }
`)
return (
  <Img fixed={data.file.childImageSharp.fixed} alt="A corgi smiling happily" /> // highlight-line
)
```

f. 一个使用最大宽度（以像素 pixel 为单位）或更高质量（默认值为 50 也就是 50%）填满 fluid 容器的图片

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fluid(maxWidth: 800, quality: 75) { // highlight-line
          ...GatsbyImageSharpFluid
        }
      }
    }
  }
`)

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
)
```

3. （可选）将内联样式添加到 `<Img />` 中，就像添加到其他组件一样

```jsx
<Img
  fluid={data.file.childImageSharp.fluid}
  alt="A corgi smiling happily"
  style={{ border: "2px solid rebeccapurple", borderRadius: 5, height: 250 }} // highlight-line
/>
```

4.（可选）在传递给 `<Img />` 组件之前，通过覆盖 GraphQL 查询返回的 `aspectRatio` 字段，将图像强制设置为所需的宽高比

```jsx
<Img
  fluid={{
    ...data.file.childImageSharp.fluid,
    aspectRatio: 1.6, // 1280 / 800 = 1.6
  }}
  alt="A corgi smiling happily"
/>
```

5. 运行 `gatsby develop`，从文件系统中的文件生成图像（如果尚未完成）并缓存它们

#### 补充资源

- [说明这些示例的示例仓库](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [Gatsby Image API](/docs/gatsby-image/)
- [使用 Gatsby Image](/docs/using-gatsby-image)
- [有关在 Gatsby 中处理图像的更多信息](/docs/working-with-images/)
- [解释这些步骤的 egghead.io 免费视频](https://egghead.io/playlists/using-gatsby-image-with-gatsby-ea85129e)

### 使用 gatsby-image 在博文 frontmatter 中优化和查询图像

对于博文中的特色图片这类情况，你 _依旧_ 可以使用 `gatsby-image`。`Img` 组件需要处理后的图像数据，这些图像数据可以来自本地（或远程）文件，包括来自 `.md` 或 `.mdx` 的 frontmatter 中的 URL。

要在 Makrdown 文中使用图片（使用 `![]()` 语法），请考虑使用诸如 [`gatsby-remark-images`](/packages/gatsby-remark-images/) 之类的插件

#### 前置条件

- 安装好 `gatsby-image`、`gatsby-transformer-sharp`，和 `gatsby-plugin-sharp` 包并添加到 `gatsby-config` 里的插件数组中
- 在 `gatsby-config` 中 [引用图片文件](/packages/gatsby-image/#install)，使用诸如 `gatsby-source-filesystem` 之类的插件
- 在 `gatsby-config` 中引用 Markdown 文件，其 frontmatter 中包含图片 URL
- 使用 [`createPages`](https://www.gatsbyjs.org/docs/node-apis/#createPages) 从 Markdown 文件 [创建的页面](/docs/creating-and-modifying-pages/)

#### 操作步骤

1. 验证 Markdown 文件包含了一个指向有效图片文件路径的图片 URL

```mdx:title=post.mdx
---
title: My First Post
featuredImage: ./corgi.png // highlight-line
---

Post content...
```

2. 验证在 `gatsby-node.js` 中调用 `createPages` 时，是否在上下文中传递了唯一标识符（在本示例中为 slug），该标识符随后将传递到布局组件中的 GraphQL 查询语句中

```js:title=gatsby-node.js
exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions

  // query for all markdown

  result.data.allMdx.edges.forEach(({ node }) => {
    createPage({
      path: node.fields.slug,
      component: path.resolve(`./src/components/markdown-layout.js`),
      // highlight-start
      context: {
        slug: node.fields.slug,
      },
      // highlight-end
    })
  })
}
```

3. 现在，从 `gatsby-image` 中引入 `Img`，从 `gatsby` 中引入 `graphql` 到模版组件中。编写一个 [pageQuery](/docs/page-query/) 来根据 `slug` 中传入的数据来获取图片数据，并将数据传入到 `<Img />` 组件当中：

```jsx:title=markdown-layout.jsx
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Img from "gatsby-image" // highlight-line

export default ({ children, data }) => (
  <main>
    // highlight-start
    <Img
      fluid={data.markdown.frontmatter.image.childImageSharp.fluid}
      alt="A corgi smiling happily"
    />
    // highlight-end
    {children}
  </main>
)

// highlight-start
export const pageQuery = graphql`
  query PostQuery($slug: String) {
    markdown: mdx(fields: { slug: { eq: $slug } }) {
      id
      frontmatter {
        image {
          childImageSharp {
            fluid {
              ...GatsbyImageSharpFluid
            }
          }
        }
      }
    }
  }
`
// highlight-end
```

4. 运行 `gatsby develop`，会依据文件系统中的文件生成图像

#### 补充资源

- [使用这个配方的示例仓库](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [frontmatter 中的特色图片](/docs/working-with-images-in-markdown/#featured-images-with-frontmatter-metadata)
- [Gatsby Image API](/docs/gatsby-image/)
- [使用 Gatsby Image](/docs/using-gatsby-image)
- [有关在 Gatsby 中处理图像的更多信息](/docs/working-with-images/)

## 8. 转换数据

Gatsby中的数据转换是由插件驱动的。 数据转换插件会使用数据源插件获取的数据，并将其处理为更有用的内容（例如，将 JSON 转换为 JavaScript 对象等）。

### 将 Markdown 转换为 HTML

`gatsby-transformer-remark` 插件可以转换 Markdown 文件为 HTML。

#### 前置条件

- 一个有 `gatsby-config.js` 和一个 `index.js` 页面的 Gatsby 站点
- 一个保存在你 Gatsby 站点 `src` 目录的 Markdown 文件 
- 安装好一个数据源插件，例如 `gatsby-source-filesystem`
- 安装好 `gatsby-transformer-remark` 插件

#### 操作步骤

1. 在你的 `gatsby-config.js` 文件中添加数据转换插件：

```js:title=gatsby-config.js
plugins: [
  // not shown: gatsby-source-filesystem for creating nodes to transform
  `gatsby-transformer-remark`
],
```

2. 添加一个 GraphQL 查询指令到你的 Gatsby 站点的 `index.js` 文件中，以获取 `MarkdownRemark` 节点：

```jsx:title=src/pages/index.js
export const query = graphql`
  query {
    allMarkdownRemark {
      totalCount
      edges {
        node {
          id
          frontmatter {
            title
            date(formatString: "DD MMMM, YYYY")
          }
          excerpt
        }
      }
    }
  }
`
```

3. 重启开发服务器并打开 GraphiQL <http://localhost:8000/___graphql>。探索 `MarkdownRemark` 节点上的可用字段。

#### 补充资源

- 使用 `gatsby-transformer-remark` [转换 Markdown 为 HTML 的教程](/tutorial/part-six/#transformer-plugins)
- 在 [Gatsby 插件库](/plugins/?=transformer) 中浏览可用的数据转换插件

## 9. 部署你的站点

请开始你的表演！当你对你的站点满意的时候，你可以准备将它部署上线了！

### 准备部署

#### 前置条件

- 一个 [Gatsby 站点](/docs/quick-start)
- 安装好 [Gatsby CLI](/docs/gatsby-cli)

#### 操作步骤

1. 停止你的开发服务器，如果它正在运行的话（大多数情况下在命令行中按下 `Ctrl + C` 停止）

2. 在标准的站点路径根目录（`/`）下，在命令行中使用 Gatsby CLI 运行 `gatsby build`。构件好的文件就会在 `public` 文件夹中了。

```shell
gatsby build
```

3. 要包含一个非 `/` 的站点路径（例如 `/site-name/`），通过添加以下内容到 `gatsby-config.js` 来设置一个路径前缀，替换 `yourpathprefix` 为你想要的路径前缀：

```js:title=gatsby-config.js
module.exports = {
  pathPrefix: `/yourpathprefix`,
}
```

有一些原因导致你需要这么做——例如在一个域上托管一个由 Gatsby 构建的博客，而另一个域不是基于 Gatsby 构建的。 主站点将指向 `example.com`，而具有路径前缀的 Gatsby 网站可以位于 `example.com/blog`。

4. 在 `gatsby-config.js` 中设置好路径前缀后，运行 `gatsby build` 并添加 `--prefix-paths` 标记来自动为所有 Gatsby 站点 URL 和 `<Link>` 标签添加前缀。

```shell
gatsby build --prefix-paths
```

5. 确保你的站点在运行 `gatsby build` 后和运行 `gatsby develop` 时看起来一样。通过构建时运行 `gatsby serve`，你可以在部署上线前测试（并在必要时调试）成品。

```shell
gatsby build && gatsby serve
```

#### 补充资源

- 在 [教程第 1 章](/tutorial/part-one/#deploying-a-gatsby-site) 中学习一遍构建和部署示例站点
- 学习 [性能优化](/docs/performance/)
- 阅读 [其他部署相关的内容](/docs/preparing-for-deployment/)
- 查看 [部署文档](/docs/deploying-and-hosting/) 来了解特定部署平台和如何部署到它们上

### 部署到 Netlify

使用 [`netlify-cli`](https://www.netlify.com/docs/cli/) 来部署你的 Gatsby 应用，而无需离开命令行界面。

#### 前置条件

- 有唯一一个 `index.js` 组件的 [Gatsby 站点](/docs/quick-start)
- 安装好 [netlify-cli](https://www.npmjs.com/package/netlify-cli) 包
- 安装好 [Gatsby CLI](/docs/gatsby-cli)

#### 操作步骤

1. 使用 `gatsby build` 构建你的 Gatsby 应用

2. 使用 `netlify login` 登录到 Netlify 

3. 运行命令 `netlify init`。选择 “Create & configure a new site” 选项

4. 选择一个自定义网站名称，或者按下回车来使用一个随机名称

5. 选择你的 [Team](https://www.netlify.com/docs/teams/)

6. 修改部署路径为 `public/`

7. 在使用 `netlify deploy --prod` 部署上线前，确保整个站点看起来没问题

#### 补充资源

- [使用 Netlify 托管](/docs/hosting-on-netlify)
- [gatsby-plugin-netlify](/packages/gatsby-plugin-netlify)

### 部署到 ZEIT Now

使用 [Now CLI](https://zeit.co/download) 来部署你的 Gatsby 应用，而无需离开命令行界面。

#### 前置条件

- 一个 [ZEIT Now](https://zeit.co/signup) 账号
- 有唯一一个 `index.js` 组件的 [Gatsby 站点](/docs/quick-start)
- 安装好 [Now CLI](https://zeit.co/download) 包
- 安装好 [Gatsby CLI](/docs/gatsby-cli)

#### 操作步骤

1. 使用 `now login` 登录到 Now CLI 

2. 在终端里切换到 Gatsby.js 应用的路径，如果不在的话

3. 运行 `now` 来部署应用

#### 补充资源

- [部署到 ZEIT Now](/docs/deploying-to-zeit-now/)
