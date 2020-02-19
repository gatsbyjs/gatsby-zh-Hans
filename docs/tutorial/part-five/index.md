---
title: 数据源插件
typora-copy-images-to: ./
disableTableOfContents: true
---

> 本章教程是 Gatsby 数据层系列的一部分。在继续之前，确保你已经读过一遍 [第 4 章](/tutorial/part-four/) 教程。

## 这章教程里有什么?

在本章教程中，你将学习如何使用 GraphQL 和数据源插件将数据提取到 Gatsby 站点中。但是在学习这些插件之前，你需要了解如何使用一个叫做 GraphiQL 的工具，该工具可帮助你正确构建查询语句。

## 介绍 GraphiQL

GraphiQL 是 GraphQL 的集成开发环境（IDE）。它功能强大，且各方面都很棒，在你构建 Gatsby 网站时会经常使用。

<<<<<<< HEAD
你可以在站点的开发服务器正在运行的时候访问它——地址通常是：
<http://localhost:8000/___graphql>。
=======
You can access it when your site's development server is running—normally at
`http://localhost:8000/___graphql`.
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="/graphiql-explore.mp4"></source>
  <p>Your browser does not support the video element.</p>
</video>

检查一下内置的 `Site` "类型"，看看里面有哪些字段可用——它包括了你先前查询过的 `siteMetadata` 对象。尝试打开 GraphiQL 并玩玩看这些数据！按下 <kbd>Ctrl + Space</kbd>（或 <kbd>Shift + Space</kbd>）来调出自动完成窗口，然后按下 <kbd>Ctrl + Enter</kbd> 来运行 GraphQL 的查询。在本教程的其余部分中，你将使用到 GraphiQL 的更多功能。

## 使用 GraphiQL Explorer

GraphiQL Explorer 使你可以通过点击可用的字段和在输入框里输入的方式，来交互式地构建完整的查询，而无需重复手工输入的过程来完成查询。

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-build-a-graphql-query-using-gatsby-s-graphiql-explorer"
  lessonTitle="Build a GraphQL Query using Gatsby’s GraphiQL Explorer"
/>

## 数据源插件

Gatsby 网站中的数据可以来自任何地方：API、数据库、CMS、本地文件等等。

数据源插件可以从其数据源中获取数据。例如文件系统源插件知道如何从文件系统中获取数据。 WordPress 插件知道如何从 WordPress API 获取数据。

添加 [`gatsby-source-filesystem`](/packages/gatsby-source-filesystem/) 这个插件并探索一下它是如何工作的。

首先，将插件安装在项目的根目录：

```shell
npm install --save gatsby-source-filesystem
```

然后把这些代码添加到 `gatsby-config.js`:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
  },
  plugins: [
    // highlight-start
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    // highlight-end
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

保存这个文件然后重启 Gatsby 开发服务器。然后重新打开 GraphiQL。

在 explorer 面板里，你会看见 `allFile` 和 `file` 这两个可选项：

![GraphiQL 文件系统](graphiql-filesystem.png)

点击 `allFile` 下拉按钮，在 query 输入框中，把光标放在 `allFile` 之后，然后按下 <kbd>Ctrl + Enter</kbd>。它会为你自动填好查询所有文件的语句。按下 “Play” 按钮来运行查询语句：

![文件系统查询](filesystem-query.png)

在 explorer 面板中，`id` 字段已经被自动选择了。通过选中字段的相应复选框来选择更多字段。按下 “Play” 按钮以使用新字段再次运行查询：

![文件系统的 explorer 选项](filesystem-explorer-options.png)

或者，你可以使用自动完成的快捷键（<kbd>Ctrl + Space</kbd>）。这将在 `File` 节点上显示可查询的字段。

![文件系统的自动完成](filesystem-autocomplete.png)

<<<<<<< HEAD
尝试在查询中添加许多字段，每次要重新运行查询的时候按下 <kbd>Ctrl + Enter</kbd>。你会看到更新后的查询结果：
=======
Try adding a number of fields to your query, press <kbd>Ctrl + Enter</kbd>
each time to re-run the query. You'll see the updated query results:
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

![所有文件的查询](allfile-query.png)

结果是一个 `File` “节点” 的数组（节点（node）是图（graph）中一个对象的特殊称呼）。每个 `File` 节点对象都包含了你要查询的字段。

## 通过 GraphQL 查询语句来构建一个页面

用 Gastby 构建新页面的过程通常开始于 GraphiQL。你先在 GraphiQL 测试出数据查询语句，然后把 query 输入框里的语句复制到你的 React 页面组件里，接下来才开始构建 UI。

让我们试试看这个。

用你刚刚创建好的 `allFile` GraphQL 查询语句来创建一个新文件 `src/pages/my-files.js`

```jsx:title=src/pages/my-files.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data) // highlight-line
  return (
    <Layout>
      <div>Hello world</div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

`console.log(data)` 这一行在上面高亮显示出来了。当要创建一个新组件时，在控制台打印出从 GraphQL 查询到的数据通常会很有帮助。你在构建 UI 时就可以在浏览器控制台中浏览数据。

如果你访问了一个新页面 `/my-files/` 并打开浏览器控制台，你会看到类似于：

![控制台中显示的数据](data-in-console.png)

图片里的数据和 GraphQL 查询语句是相匹配的。

让我们继续添加一些代码来显示出文件数据。

```jsx:title=src/pages/my-files.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data)
  return (
    <Layout>
      {/* highlight-start */}
      <div>
        <h1>My Site's Files</h1>
        <table>
          <thead>
            <tr>
              <th>relativePath</th>
              <th>prettySize</th>
              <th>extension</th>
              <th>birthTime</th>
            </tr>
          </thead>
          <tbody>
            {data.allFile.edges.map(({ node }, index) => (
              <tr key={index}>
                <td>{node.relativePath}</td>
                <td>{node.prettySize}</td>
                <td>{node.extension}</td>
                <td>{node.birthTime}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
      {/* highlight-end */}
    </Layout>
  )
}

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

<<<<<<< HEAD
然后访问 [http://localhost:8000/my-files](http://localhost:8000/my-files)… 😲
=======
And now visit `http://localhost:8000/my-files`… 😲
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

![我的文件页面](my-files-page.png)

## 接下来？

现在，你已经了解了数据源插件如何将数据 _引入_ Gatsby 的数据系统。在下一章教程中，你将学习数据转换插件如何 _转换_ 数据源插件给出的原始数据。通过数据源插件和数据转换插件两者的协同合作，所有在构建 Gatsby 网站时可能需要的数据源和数据转换都能够处理了。让我们在  [第 6 章教程](/tutorial/part-six/) 中了解有关数据转换插件的信息。