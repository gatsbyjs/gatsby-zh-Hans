---
title: 以编程的方式利用数据创建页面
typora-copy-images-to: ./
disableTableOfContents: true
---

> 本章教程是 Gatsby 数据层系列的一部分。在继续之前，确保你已经读过一遍 [第 4 章](/tutorial/part-four/)、[第 5 章](/tutorial/part-five/) 和 [第 6 章](/tutorial/part-six/) 教程。

## 这章教程里有什么?

在前一章教程中，你创建了一个漂亮的索引页面。这个页面能够展示出一个博文标题和摘要列表。但你不只是想要看到摘要，你想看到你的 Markdown 文件的真正博文页面。

你可以通过将 React 组件放置在 `src/pages` 目录中来继续创建页面。但是接下来，你将学习如何通过 _以编程的方式_ 利用 _数据_ 创建页面。Gatsby _并不_ 仅限于使用文件（例如许多静态网站生成器）制作页面。Gatsby 同样可以在构建时使用 GraphQL 查询 _数据_ 并将查询结果 _映射_ 到 _页面_ 中。这是一个非常好的想法。你将探索这个想法的具体含义和使用方法。

让我们开始吧。

## 为页面创建 slug

创建一个新页面，一共有两步:

1.  生成页面的 “路径”（path）或 slug。
2.  创建页面。

_**注意**: 通常，数据源会直接为内容提供一个 slug 或路径名——使用这些系统之一（例如CMS）时，你不需要像创建 Markdown 文件那样自己创建 slug。_

为了创建你的 Markdown 页面，你需要学习如何使用这两个 Gatsby API：[`onCreateNode`](/docs/node-apis/#onCreateNode) 和 [`createPages`](/docs/node-apis/#createPages)。你将在许多站点和插件中看到这两个主要的API

我们已经尽力使 Gatsby API 易于实现。要实现 API，请在 `gatsby-node.js` 文件中导出（export）以 API 为名称的函数。

所以你要这么做：在站点的根目录下，创建一个名为 `gatsby-node.js` 的文件并添加以下代码。

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  console.log(node.internal.type)
}
```

每当创建新节点（或更新）时，Gatsby 都会调用 `onCreateNode` 函数。

停止并重启开发服务器，你会看到终端控制台显示出条很多新创建的节点记录。

在下一部分中，你将会使用这个 API 来把 slug 添加到你的 Markdown页面。 

修改你的函数使其仅仅记录 `MarkdownRemark` 节点。

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  // highlight-start
  if (node.internal.type === `MarkdownRemark`) {
    console.log(node.internal.type)
  }
  // highlight-end
}
```

你需要使用每一个 Markdown 文件的名称来创建页面 slug。`pandas-and-bananas.md` 就变成 `/pandas-and-bananas/`。但要如何从 `MarkdownRemark` 节点中获取文件名称呢？你需要 _遍历_ 一遍它的 _父节点_ `File` 的 “节点图”。因为 `File` 节点包含了磁盘中你所需要的文件数据。再次修改函数来实现它：

```javascript:title=gatsby-node.js
// highlight-next-line
exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    // highlight-start
    const fileNode = getNode(node.parent)
    console.log(`\n`, fileNode.relativePath)
    // highlight-end
  }
}
```

在重启开发服务器之后，你应该能在终端看到你的两个 Markdown 文件的相对路径。

![Markdown 相对路径](markdown-relative-path.png)

现在你需要创建 slug。由于通过文件名创建 slug 的逻辑可能会很棘手，因此 `gatsby-source-filesystem` 插件附带了创建 slug 的功能。让我们来使用它。

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`) // highlight-line

exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    console.log(createFilePath({ node, getNode, basePath: `pages` })) // highlight-line
  }
}
```

这个函数负责查找父“文件”节点以及创建 slug。再次运行开发服务器，你应该看到终端记录了两个 slug，每个 Markdown 文件各一个。

现在，你可以将新的 slug 直接添加到 `MarkdownRemark` 节点里。这非常有用，因为你添加到节点的任何数据都可以在以后使用 GraphQL 查询到。因此创建页面时很容易得到 slug。

为此，你将向 API 的实现传递一个函数，该函数称为 [`createNodeField`](/docs/actions/#createNodeField)。此功能允许你在其他插件创建的节点里创建其他字段。只有节点的原始创建者才能直接修改该节点——所有其他插件（包括你的`gatsby-node.js`）都必须使用此函数来创建额外字段。

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`)
// highlight-next-line
exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions // highlight-line
  if (node.internal.type === `MarkdownRemark`) {
    // highlight-start
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
    // highlight-end
  }
}
```

重启开发服务器，打开或刷新 GraphiQL。然后运行这条 GraphQL 查询语句来查看新的 slug。

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        fields {
          slug
        }
      }
    }
  }
}
```

现在 slug 已经创建好了。你可以创建页面了。

## 创建页面

在同一个文件  `gatsby-node.js` 中，添加以下代码。

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`)

exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
}

// highlight-start
exports.createPages = async ({ graphql, actions }) => {
  // **Note:** The graphql function call returns a Promise
  // see: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise for more info
  const result = await graphql(`
    query {
      allMarkdownRemark {
        edges {
          node {
            fields {
              slug
            }
          }
        }
      }
    }
  `)

  console.log(JSON.stringify(result, null, 4))
}
// highlight-end
```

你添加了 [`createPages`](/docs/node-apis/#createPages) 这个 API 的实现。Gatsby 会调用它，从而使得插件能添加页面。

如本章教程的导语所述，以编程的方式创建页面的步骤是：

1.  使用 GraphQL 查询数据
2.  把查询结果映射到页面上

上面的代码是从 Markdown 创建页面的第一步。你使用了我们提供的 `graphql` 函数查询你创建的 Markdown slug。然后你会在终端里看到如下查询结果：

![查询 Markdown slugs](query-markdown-slugs.png)

创建页面还需要一样东西：一个页面模板组件。与 Gatsby 中所有其他东西一样，用编程生成的页面也是依靠 React 组件的。创建页面时，需要指定所使用的组件。

新建一个目录位于 `src/templates`，然后添加以下代码到一个新文件 `src/templates/blog-post.js` 中。

```jsx:title=src/templates/blog-post.js
import React from "react"
import Layout from "../components/layout"

export default () => {
  return (
    <Layout>
      <div>Hello blog post</div>
    </Layout>
  )
}
```

然后更新我们的 `gatsby-node.js`：

```javascript:title=gatsby-node.js
const path = require(`path`) // highlight-line
const { createFilePath } = require(`gatsby-source-filesystem`)

exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
}

exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions // highlight-line
  const result = await graphql(`
    query {
      allMarkdownRemark {
        edges {
          node {
            fields {
              slug
            }
          }
        }
      }
    }
  `)

  // highlight-start
  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.fields.slug,
      component: path.resolve(`./src/templates/blog-post.js`),
      context: {
        // Data passed to context is available
        // in page queries as GraphQL variables.
        slug: node.fields.slug,
      },
    })
  })
  // highlight-end
}
```

<<<<<<< HEAD
重启开发服务器后，你会看到页面已经创建好了！在开发过程中，找到新创建页面的一种简单方法是：随便转到一个不存在的路径，在该路径中 Gatsby 会帮你显示出站点的页面列表。比如转到 <http://localhost:8000/sdf>，就会看到你创建的新页面。
=======
Restart the development server and your pages will be created! An easy way to
find new pages you create while developing is to go to a random path where
Gatsby will helpfully show you a list of pages on the site. If you go to
`http://localhost:8000/sdf`, you'll see the new pages you created.
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

![新页面](new-pages.png)

访问其中一个页面，你将会看到：

![Hello World 博文](hello-world-blog-post.png)

看起来有点单调无聊，而且并不是你想要看到的。现在你可以从 Markdown 博文中提取数据。把 `src/templates/blog-post.js` 的文件内容替换为：

```jsx:title=src/templates/blog-post.js
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Layout from "../components/layout"

// highlight-start
export default ({ data }) => {
  const post = data.markdownRemark
  // highlight-end
  return (
    <Layout>
      {/* highlight-start */}
      <div>
        <h1>{post.frontmatter.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.html }} />
      </div>
      {/* highlight-end */}
    </Layout>
  )
}

// highlight-start
export const query = graphql`
  query($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
      }
    }
  }
`
// highlight-end
```

见证奇迹的时刻……

![博文](blog-post.png)

哇哦!

最后一步就是把你的新页面连接到索引页。

回到 `src/pages/index.js` 文件中，查询你的 Markdown slugs，并且创建链接。

```jsx:title=src/pages/index.js
import React from "react"
import { css } from "@emotion/core"
import { Link, graphql } from "gatsby" // highlight-line
import { rhythm } from "../utils/typography"
import Layout from "../components/layout"

export default ({ data }) => {
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
            {/* highlight-start */}
            <Link
              to={node.fields.slug}
              css={css`
                text-decoration: none;
                color: inherit;
              `}
            >
              {/* highlight-end */}
              <h3
                css={css`
                  margin-bottom: ${rhythm(1 / 4)};
                `}
              >
                {node.frontmatter.title}{" "}
                <span
                  css={css`
                    color: #555;
                  `}
                >
                  — {node.frontmatter.date}
                </span>
              </h3>
              <p>{node.excerpt}</p>
            </Link> {/* highlight-line */}
          </div>
        ))}
      </div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allMarkdownRemark(sort: { fields: [frontmatter___date], order: DESC }) {
      totalCount
      edges {
        node {
          id
          frontmatter {
            title
            date(formatString: "DD MMMM, YYYY")
          }
          // highlight-start
          fields {
            slug
          }
          // highlight-end
          excerpt
        }
      }
    }
  }
`
```

走起！一个麻雀虽小但五脏俱全的博客。

## 一个小挑战

<<<<<<< HEAD
好好把玩一下这个网站。添加更多 Markdown 文件，从 `MarkdownRemark` 节点中查询其他数据然后添加到主页或博文页面中。
=======
Try playing more with the site. Try adding some more markdown files. Explore
querying other data from the `MarkdownRemark` nodes and adding them to the
front page or blog posts pages.
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

在这一系列教程中，你学习到了构建 Gatsby 数据层的基础知识。你学会了如何使用插件从 _数据源_ 中获取数据并 _转换_ 数据，如何使用 GraphQL 将数据 _映射_ 到页面里，以及如何构建查询每个页面数据的 _页面模板组件_。

## 下一步

现在你已经构建好了一个 Gatsby 站点。那下面还能做什么呢？

- 把你的 Gatsby 站点分享到 Twitter。搜索 #gatsbytutorial 看看别人创建的网站是什么样的！确保你在推文里 @gatsbyjs 并添加了话题 #gatsbytutorial :)
- 你可以看一些 [示例网站](https://github.com/gatsbyjs/gatsby/tree/master/examples#gatsby-example-websites)
- 探索更多 [插件](/docs/plugins/)
- 看看 [别人是怎么把 Gatsby 玩出花的](/showcase/)
- 阅读文档 [Gatsby's APIs](/docs/api-specification/)，[节点](/docs/node-interface/) 或是 [GraphQL](/docs/graphql-reference/)
