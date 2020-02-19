---
title: GraphQL 概念
---

<<<<<<< HEAD
import LayerModel from "../../www/src/components/layer-model"

把数据加载到 React 组件里有许多方式。[GraphQL](http://graphql.org/) 是一个非常受欢迎且强大的技术。
=======
There are many options for loading data into React components. One of the most
popular and powerful of these is a technology called
[GraphQL](https://graphql.org/).
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

GraphQL 是 Facebook 发明的，它能帮助项目工程师 _提取_ 所需数据到组件里。

GraphQL 是一种 **q**uery **l**anguage（查询语言）（名字中 _QL_ 的由来）。如果你熟悉 SQL，它们的使用方式很像。通过一种特殊的语法，你就能描述你想要的组件中的数据，然后它就能把数据传给你。

Gatsby 利用 GraphQL 来让 [page 和 StaticQuery 组件](/docs/building-with-components/) 能够声明它们和它们的子组件所需的数据。然后，Gatsby 在浏览器里让这些数据随时能被需要的组件使用。

数据从任何数量的数据源被查询到了一个聚合层，这个聚合层是 Gatsby 构建过程中的一个重要组成部分：

<LayerModel initialLayer="Data" />

## 为什么 GraphQL 这么棒？

想深入了解更多，请阅读 [为什么 Gatsby 使用 GraphQL](/docs/why-gatsby-uses-graphql/)。

- 淘汰前端数据模板 — 不用担心请求和等待数据。只需要在一个 Gatsby 查询里说明你想要的数据，数据就会在你需要的时候出现
- 把前端复杂的东西都放入查询里 — 你的 GraphQL 在 _构建时_ 就可以完成很多数据转换
- 对于经常要依赖复杂/嵌套数据的现代应用，它是一种完美的数据查询语言
- 通过防止数据过度膨胀来提升性能 — GraphQL 允许你只选择需要的数据，无论 API 返回的是什么

## 一个 GraphQL 查询是什么样的？

GraphQL 允许你请求必要的数据。查询格式看起来像 JSON：

```graphql
{
  site {
    siteMetadata {
      title
    }
  }
}
```

然后返回：

```json
{
  "site": {
    "siteMetadata": {
      "title": "A Gatsby site!"
    }
  }
}
```

一个带有 GraphQL 查询的基本页面组件可以是这样子的：

```jsx
import React from "react"
import { graphql } from "gatsby"

export default ({ data }) => (
  <div>
    <h1>About {data.site.siteMetadata.title}</h1>
    <p>We're a very cool website you should return to often.</p>
  </div>
)

export const query = graphql`
  query {
    site {
      siteMetadata {
        title
      }
    }
  }
`
```

查询的结果会被自动放进 React 组件的 `data` prop 里。GraphQL 和 Gatsby 能让你请求数据然后立马就能用上。

**注意：** 如果你要在一个非页面组件里运行 GraphQL 查询，你需要使用 [Gatsby 的静态查询功能](/docs/static-query/)。

### 了解查询的各个部分

下图展示了在一个 GraphQL 查询里，每个单词用颜色标出的图例名称：

![GraphQL 查询图解](./images/basic-query.png)

#### 查询操作类型（Query operation type）

图中标记了 `query` 为 “操作类型”。因为 Gatsby 唯一使用的操作类型是 `query`，所以这个操作类型是可以省略的（就像上面的例子一样）。

#### 操作名称（Operation name）

被标记为 “操作名称” 的 `SiteInformation` 是一个你赋予给这个查询的独有名称。这个名称等同于你命名一个方程或者一个变量。如果你希望这个查询是匿名的，它也可以像方程一样被省略。

#### 查询字段（Query fields）

`site`，`id`，`siteMetadata` 和 `title` 都被标记为 “字段”。任何上层字段 -- 如图中的 `site` -- 有时候被称为 **根级字段（root level fields）**。事实上，这个名字并没有暗示任何功能上的特殊性，因为在 GraphQL 查询里所有的字段都在执行同样的事请。

## 如何学习 GraphQL

可能用 Gatsby 开发是让你第一次遇上了 GraphQL！我们希望你和我们一样喜爱它，也希望你能发现它可以帮助你所有的项目。

我们推荐下面的两个教程来学习 GraphQL：

- https://www.howtographql.com/
- http://graphql.org/learn/

[官方的 Gatsby 教程](/tutorial/part-four/) 也介绍了如何在 Gatsby 里使用 GraphQL。

## GraphQL 和 Gatsby 如何一起工作？

Gatsby 的一大优点是灵活性。人们可以在 [很多不同的编程语言中](http://graphql.org/code/) 以及在网页和原生应用中使用 GraphQL。

多数人在服务器里运行 GraphQL 来响应客户端的数据请求。你定义一个模式（schema，一个描述数据形状的正规方法）给你的 GraphQL 服务器，然后你的 GraphQL 解析器会向一些数据库和/或者其它 API 检索数据。

Gatsby 是在 _构建时_ 使用 GraphQL _而不是_ 在运行中的网站使用。这种方式是独一无二的，而且你不需要再在 production 模式的网站里运行额外的服务（例如一个数据库和 node.js 服务）来使用 GraphQL。

用 Gatsby 这个框架来开发应用是非常实用的，所以结合 Gatsby 的原生构建 GraphQL 和 GraphQL 查询一起比在浏览器里运行 GraphQL 服务器更好。

## Gatsby 的 GraphQL 模式是怎么来的？

GraphQL 的常见用法需要人工创建一个 GraphQL 模式。

Gatsby 运用了可以从不同数据源提取数据的插件。数据会自动 _推断_ 出一个 GraphQL 模式。

如果你的 Gatsby 数据是这样的：

```json
{
  "title": "A long long time ago"
}
```

Gatsby 会生成如下模式：

```
title: String
```

这样从任何地方提取数据都会变得很容易，而且你可以马上开始编写 GraphQL 查询。

有时候一些数据源会允许你定义一个数据还没有被添加的模式，这样是 _会_ 引起混乱的，但 Gatsby 可能会跳过生成那部分的模式。

## 强大的数据转换

GraphQL 也为 Gatsby 带来了其它独一无二的特色 — 它允许你在查询里用参数控制数据转换。请看下面的例子。

### 格式化日期

人们通常喜欢保存像 “2018-01-05” 的日期格式，但希望用其它格式，例如 “January 5th, 2018”，来显示。一种方法是加载一个格式化日期的 JavaScript 库到浏览器里。或者，利用 Gatsby 的 GraphQL 层，你就可以在查询时就做格式化，就像这样：

```graphql
{
  date(formatString: "MMMM Do, YYYY")
}
```

请在我们的 [GraphQL 参考页](/docs/graphql-reference/#dates) 里查看全部的格式化选项。

### Markdown

Gatsby 有能把数据从一个格式变成另一个格式的 _数据转换插件_。一个常见的例子是 markdown。如果你安装了 [`gatsby-transformer-remark`](/packages/gatsby-transformer-remark/)， 那么在你的查询里，你可以指定你想要的是经转换的 HTML 版本而不是 markdown：

```graphql
markdownRemark {
  html
}
```

### 图片

Gatsby 对于图片处理有丰富的支持。响应式图片是现代网站的一大部分，每张图片通常需要创建 5 种以上不同大小的缩略图。利用 Gatsby 的 [`gatsby-transformer-sharp`](/packages/gatsby-transformer-sharp/)，你可以 _查询_ 图片的响应式版本。你的查询会自动生成全部所需的响应式缩略图，然后返回 `src` 和 `srcSet` 字段来让你添加你的图片元素。

结合 [gatsby-image](/packages/gatsby-image/) 这个特别的 Gatsby 图片组件，你在建立网站时就拥有了一个非常强大的图片处理基础。

这是一个用了 `gatsby-image` 的组件：

```jsx
import React from "react"
import Img from "gatsby-image"
import { graphql } from "gatsby"

export default ({ data }) => (
  <div>
    <h1>Hello gatsby-image</h1>
    <Img fixed={data.file.childImageSharp.fixed} />
  </div>
)

export const query = graphql`
  query {
    file(relativePath: { eq: "blog/avatars/kyle-mathews.jpeg" }) {
      childImageSharp {
        # 在查询里就可以指定图片处理的参数。
        # 这样即使你的页面有设计变化也不会对图片本身造成影响
        fixed(width: 125, height: 125) {
          ...GatsbyImageSharpFixed
        }
      }
    }
  }
`
```

你也可以查看下面的博文：

- [让建立网站变得有趣](/blog/2017-10-16-making-website-building-fun/)
- [Gatsby.js 让优化图片变得容易](https://medium.com/@kyle.robert.gill/ridiculously-easy-image-optimization-with-gatsby-js-59d48e15db6e)

## 高级

### 片段

在上面的 [图片查询](#images) 例子里，我们用了一个 Gatsby 片段 `...GatsbyImageSharpFixed`，它是一个可以重复使用的字段组合。你可以在 [这里](http://graphql.org/learn/queries/#fragments) 了解更多。

如果你希望自定义片段，你可以用一些指定的 export 来把它们导出到任何 JavaScript 文件里，然后 Gatsby 会自动在你的 GraphQL 查询里处理它们。

例如，如果我在一个 helper 组件里放一个片段，那么我就可以在任何其它查询里使用这个片段：

```jsx:title=src/components/PostItem.js
export const markdownFrontmatterFragment = graphql`
  fragment MarkdownFrontmatter on MarkdownRemark {
    frontmatter {
      path
      title
      date(formatString: "MMMM DD, YYYY")
    }
  }
`
```

然后它们能在任何 GraphQL 查询里被使用！

```graphql
query($path: String!) {
  markdownRemark(frontmatter: { path: { eq: $path } }) {
    ...MarkdownFrontmatter
  }
}
```

你的 helper 组件应当定义和导出一个数据所需的片段。例如，在你的 index 页面里，你可能想遍历所有的文章然后显示在一个列表里。

```jsx:title=src/pages/index.jsx
import React from "react"
import { graphql } from "gatsby"

export default ({ data }) => {
  return (
    <div>
      <h1>Index page</h1>
      <h4>{data.allMarkdownRemark.totalCount} Posts</h4>
      {data.allMarkdownRemark.edges.map(({ node }) => (
        <div key={node.id}>
          <h3>
            {node.frontmatter.title} <span>— {node.frontmatter.date}</span>
          </h3>
        </div>
      ))}
    </div>
  )
}

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
        }
      }
    }
  }
`
```

如果 index 组件变得非常大，你可能需要把它重构成一些小组件。

```jsx:title=src/components/IndexPost.jsx
import React from "react"
import { graphql } from "gatsby"

export default ({ frontmatter: { title, date } }) => (
  <div>
    <h3>
      {title} <span>— {date}</span>
    </h3>
  </div>
)

export const query = graphql`
  fragment IndexPostFragment on MarkdownRemark {
    frontmatter {
      title
      date(formatString: "MMMM DD, YYYY")
    }
  }
`
```

现在，你可以在 index 页面里与导出的片段一起使用。

```jsx:title=src/pages/index.jsx
import React from "react"
import IndexPost from "../components/IndexPost"
import { graphql } from "gatsby"

export default ({ data }) => {
  return (
    <div>
      <h1>Index page</h1>
      <h4>{data.allMarkdownRemark.totalCount} Posts</h4>
      {data.allMarkdownRemark.edges.map(({ node }) => (
        <div key={node.id}>
          <IndexPost frontmatter={node.frontmatter} />
        </div>
      ))}
    </div>
  )
}

export const query = graphql`
  query {
    allMarkdownRemark {
      totalCount
      edges {
        node {
          id
          ...IndexPostFragment
        }
      }
    }
  }
`
```

## 阅读更多

- [为什么 Gatsby 使用 GraphQL](/docs/why-gatsby-uses-graphql/)
- [GraphQL 查询分析](https://blog.apollographql.com/the-anatomy-of-a-graphql-query-6dffa9e9e747)

### 上手 GraphQL

- http://graphql.org/learn/
- https://www.howtographql.com/
- https://reactjs.org/blog/2015/05/01/graphql-introduction.html
- https://services.github.com/on-demand/graphql/

### GraphQL 高级阅读

- [GraphQL 规范](https://facebook.github.io/graphql/October2016/)
- [接口和联合](https://medium.com/the-graphqlhub/graphql-tour-interfaces-and-unions-7dd5be35de0d)
- [Relay 编译器（Gatsby 用它来处理查询）](https://facebook.github.io/relay/docs/en/compiler-architecture.html)
