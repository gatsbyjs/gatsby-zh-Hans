---
title: 数据转换插件
typora-copy-images-to: ./
disableTableOfContents: true
---

> 本章教程是 Gatsby 数据层系列的一部分。在继续之前，确保你已经读过一遍 [第 4 章](/tutorial/part-four/) 和 [第 5 章](/tutorial/part-five/) 教程。

## 这章教程里有什么?

上一章教程演示了数据源插件是如何将数据 _导入_ 到数据系统中。在这章教程中，你将学习到数据转换插件如何 _转换_ 数据源插件给出的原始数据。搭配使用数据源插件和数据转换插件，所有构建 Gatsby 网站时可能需要的数据源和数据转换都可以被处理。

## 数据转换插件

通常，从数据源插件获取的数据格式，和你要用来构建网站的格式并不一致。文件系统插件使你可以查询 _关于_ 文件的数据，但是如果要查询文件 _里面_ 的数据怎么办？

为了实现这一点，Gatsby 通过数据转换插件提供支持，它从数据源插件中获取原始内容，并将其内容 _转换_ 成更有用的内容。

就比如 Markdown 文件：Markdown 的写作体验非常棒。但当你需要把它构建成一个网页的时候，你需要把 Markdown 转换为 HTML 格式。

添加一个 Markdown 文件，位于 `src/pages/sweet-pandas-eating-sweets.md`（它会成为你的第一篇 Markdown 博客）。你将会学习到如何使用数据转换插件和 GraphQL 来把它 _转换_ 成 HTML 的形式。

```markdown:title=src/pages/sweet-pandas-eating-sweets.md
---
title: "Sweet Pandas Eating Sweets"
date: "2017-08-10"
---

Pandas are really sweet.

Here's a video of a panda eating sweets.

<iframe width="560" height="315" src="https://www.youtube.com/embed/4n0xNbfJLR8" frameborder="0" allowfullscreen></iframe>
```

保存文件以后，再次访问页面 `/my-files/` ——新的 Markdown 文件便在表格里了。这是 Gatsby 一个非常强大的功能。和之前的例子 `siteMetadata` 一样，数据源插件可以实时重载数据。`gatsby-source-filesystem`  一直在扫描新添加的文件，如果扫描到了，就会重新运行查询。

添加一个可以转换 Markdown 文件的数据转换插件：

```shell
npm install --save gatsby-transformer-remark
```

然后像往常一样把这些代码添加到 `gatsby-config.js`：

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
  },
  plugins: [
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    `gatsby-transformer-remark`, // highlight-line
    `gatsby-plugin-emotion`,
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

重启开发服务器，然后刷新（或者重新打开） GraphiQL，看一看自动完成列表的内容：

![Makrdown 自动完成](markdown-autocomplete.png)

再次选择 `allMarkdownRemark` 并且运行，和之前对 `allFile` 的操作一样。你会看到新添加的 Markdown 文件。然后再探索一下 `MarkdownRemark` 这个节点的字段。

![Markdown 查询](markdown-query.png)

很好！希望你已经掌握了一些基础知识。数据源插件把数据 _导入_ 到 Gatsby 的数据系统，然后 _数据转换_ 插件转换数据源插件导入的原始内容。这个模式能够处理构建 Gatsby 网站时可能需要的所有数据源和数据转换。

## 在 `src/pages/index.js` 创建一个站点中所有 Markdown 文件的列表

现在，你必须在首页上创建 Makrdown 文件列表。像许多博客一样，你最终需要在首页上展示一个指向每个博客文章的链接列表。使用 GraphQL，你可以 _查询_ 当前 Markdown 博文的列表，因此无需手动维护列表。

和我们对 `src/pages/my-files.js` 页面所做的一样，把 `src/pages/index.js` 替换为以下代码，来添加一个 GraphQL 查询语句、一些初步的 HTML 和样式。

```jsx:title=src/pages/index.js
import React from "react"
import { graphql } from "gatsby"
import { css } from "@emotion/core"
import { rhythm } from "../utils/typography"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data)
  return (
    <Layout>
      <div>
        <h1
          css={css`
            display: inline-block;
            border-bottom: 1px solid;
          `}
        >
          Amazing Pandas Eating Things
        </h1>
        <h4>{data.allMarkdownRemark.totalCount} Posts</h4>
        {data.allMarkdownRemark.edges.map(({ node }) => (
          <div key={node.id}>
            <h3
              css={css`
                margin-bottom: ${rhythm(1 / 4)};
              `}
            >
              {node.frontmatter.title}{" "}
              <span
                css={css`
                  color: #bbb;
                `}
              >
                — {node.frontmatter.date}
              </span>
            </h3>
            <p>{node.excerpt}</p>
          </div>
        ))}
      </div>
    </Layout>
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
          excerpt
        }
      }
    }
  }
`
```

现在首页应该是这样:

![首页](frontpage.png)

但是只有一篇博文，形单影只。所以让我们在这个地址添加另一篇 `src/pages/pandas-and-bananas.md` 。

```markdown:title=src/pages/pandas-and-bananas.md
---
title: "Pandas and Bananas"
date: "2017-08-21"
---

Do Pandas eat bananas? Check out this short video that shows that yes! pandas do
seem to really enjoy bananas!

<iframe width="560" height="315" src="https://www.youtube.com/embed/4SZl1r2O_bY" frameborder="0" allowfullscreen></iframe>
```

![两篇博文](two-posts.png)

看起来不错！除了……博文顺序不对。

但是修复起来很简单。查询某种类型的连接时，你可以传入各种参数到 GraphQL 查询语句中。你可以 `sort`（排序）和 `filter`（过滤）节点，设置要 `skip`（跳过）多少节点，并选择获得的节点的 `limit` （数量限制）。通过这些强大的操作符，你可以选择任意你想要的数据——以你需要的形式。

在你的索引（index）页面的 GraphQL 查询语句中，把 `allMarkdownRemark` 替换为 `allMarkdownRemark(sort: { fields: [frontmatter___date], order: DESC })`。_注意：在 `frontmatter` 和 `date`之间有三个下划线符号。_ 保存文件后，顺序问题应该修复了。

打开 GraphiQL 然后试试看不同的排序选项。你可以对 `allFile` 和其他各种连接进行排序。

要了解更多查询语句的操作符，请查看我们的这篇 [GraphQL 查询选项参考指南.](/docs/graphql-reference/)。

## 一个小挑战

尝试再添加一个新博文页面，看一看主页的博文列表发生了什么变化！

## 下一步

你做的很棒！你创建了一个漂亮的索引页，在这个页面里你可以查询你的 Markdown 文件，并生成一个博文标题与摘要的列表。但你不只是想要看到摘要，你想看到你的 Markdown 文件的真正博文页面。

你可以通过将 React 组件放置在 `src/pages` 目录中来继续创建页面。但是接下来，你将学习如何通过 _以编程的方式_ 利用 _数据_ 创建页面。Gatsby _并不_ 仅限于使用文件（例如许多静态网站生成器）制作页面。Gatsby 同样可以在构建时使用 GraphQL 查询 _数据_ 并将查询结果 _映射_ 到 _页面_ 中。这是一个非常好的想法。在下一章教程中，你将探索这个想法的具体含义和使用方法，并且学习如何 [以编程的方式利用数据创建页面](/tutorial/part-seven/)。