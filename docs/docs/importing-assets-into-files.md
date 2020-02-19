---
title: 将资源直接引入到文件里
---

把图片，字体和文件等资源引入到一个 Gatsby 网站主要有两种方法。常用方法是直接把文件引入到一个 Gatsby 模板，页面或者组件中。另一种方法是利用 [static 文件夹](/docs/static-folder)，这种方法对于一些边缘案例是有帮助的。

## 通过 Webpack 引入资源

通过 Webpack，你可以 **把一个文件 `import` 成一个 JavaScript 模块**。这样 Webpack 最终就会把那个文件组合到 bundle 里。与 CSS 的引入不同的是，引入一个文件会给你一个 string 值。这个值其实是一个你可以在代码中引用的最终路径，类似于一张图片的 `src` 属性或者一条 PDF 连接的 `href` 属性。

为了降低请求服务器的次数，引入小于 10,000 字节大小的图片会返回一条
[data URI](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs) 而不是一条路径。这种情况适用于以下文件后缀：`svg`，`jpg`，`jpeg`，`png`，`gif`，`mp4`，`webm`，`wav`，`mp3`，`m4a`，`aac` 和 `oga`。

请看下面的例子：

```jsx
import React from "react"
import logo from "./logo.png" // 这里告诉 Webpack 这个 JS 文件用了这张图片

console.log(logo) // /logo.84287d09.png

function Header() {
  // 引入得到的是你的图片 URL
  return <img src={logo} alt="Logo" />
}

export default Header
```

当 Webpack 打包的时候，Webpack 会准确地把这些图片放入 public 文件夹里，然后帮我们提供正确的路径。

你甚至可以在 CSS 里引用文件来引入它们：

```css
.Logo {
  background-image: url(./logo.png);
}
```

Webpack 会在 CSS 里查找所有的相对模块引用（它们都是以 `./` 开头），然后把它们替换成在已经编译好的 bundle 里的最终路径。如果你拼写错误或者无意地删除了一个重要的文件，你会看到一个类似于引入不存在的 JavaScript 模块的编译错误。在 Webpack 已编译好的 bundle 里，这些最终文件名是由 Webpack 通过 content hashes 生成的。如果日后文件的内容有改动，Webpack 会在 production 模式里重命名文件。所以这样你不需要担心资源的持久化缓存。

如果你用的是 SCSS，引入的是一个入口 SCSS 文件。

但请注意，这事实上是一个可以自行定制的 Webpack 功能。

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-import-a-local-image-into-a-gatsby-component-with-webpack"
  lessonTitle="Import a Local Image into a Gatsby Component with webpack"
/>

### 更多资料

<<<<<<< HEAD
- 更多关于 [使用引入的字体](https://www.gatsbyjs.org/docs/recipes/#adding-a-local-font)。
=======
- More on [using an imported font](/docs/recipes/styling-css#adding-a-local-font).
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

## 通过 gatsby-source-filesystem 用 GraphQL 查询 `File`

你也可以在 GraphQL 的数据层里查询来引入文件，GraphQL 会复制那些文件到 public 目录里。通过查询 `File` 节点（node）的 `publicURL` 字段（field），你可以获得能用在 Javascript 组件，页面和模板的 URLs。

### 例子

1. 复制你所有在数据层里的 `.pdf` 文件到你的 build 目录然后返回它们的 URLs：

```graphql
{
  allFile(filter: { extension: { eq: "pdf" } }) {
    edges {
      node {
        publicURL
      }
    }
  }
}
```

在页面里用 useStaticQuery 来引用这些 PDF 文件：

```jsx
import React from "react"
import { useStaticQuery, graphql } from "gatsby"

import Layout from "../components/layout"

const DownloadsPage = () => {
  const data = useStaticQuery(graphql`
    {
      allFile(filter: { extension: { eq: "pdf" } }) {
        edges {
          node {
            publicURL
            name
          }
        }
      }
    }
  `)
  return (
    <Layout>
      <h1>All PDF Downloads</h1>
      <ul>
        {data.allFile.edges.map((file, index) => {
          return (
            <li key={`pdf-${index}`}>
              <a href={file.node.publicURL} download>
                {file.node.name}
              </a>
            </li>
          )
        })}
      </ul>
    </Layout>
  )
}
export default DownloadsPage
```

2. 在 Markdown 的头（frontmatter）处连接附件：

```markdown
---
title: "Title of article"
attachments:
  - "./assets.zip"
  - "./presentation.pdf"
---

Hi, this is a great article.
```

接着在一个文章模板组件里，你可以查询那些附件：

```graphql
query($slug: String!) {
  markdownRemark(fields: { slug: { eq: $slug } }) {
    html
    frontmatter {
      title
      attachments {
        publicURL
      }
    }
  }
}
```

[`gatsby-source-filesystem`](/packages/gatsby-source-filesystem/) 文件可以阅读到更多的相关信息。
