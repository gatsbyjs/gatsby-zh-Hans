---
title: 让网站准备好上线
typora-copy-images-to: ./
disableTableOfContents: true
---

哇哦！你已经完成这么多了！你学到了如何：

- 创建一个新的 Gatsby 站点
- 创建页面和组件
- 为组件添加样式
- 为网站添加插件
- 使用数据源和转换数据
- 使用 GraphQL 来查询页面数据
- 以编程的方式利用数据创建页面

在这最后一章中，你将学习到一些让网站准备好上线的步骤。我们将介绍一个强大的站点诊断工具 [Lighthouse](https://developers.google.com/web/tools/lighthouse/)，并在过程中介绍一些你通常希望在 Gatsby 网站中使用的插件。

## 使用 Lighthouse 审查

引用自 [Lighthouse 官方介绍页面](https://developers.google.com/web/tools/lighthouse/)：

> Lighthouse 是一个开源的自动化工具，用于改进网络应用的质量。你可以将其作为一个 Chrome 扩展程序运行，或从命令行运行。 你为 Lighthouse 提供一个你要审查的网址，它将针对此页面运行一连串的测试，然后生成一个有关页面性能的报告。

Chrome 开发者工具中包含了 Lighthouse。运行审查功能然后解决发现的错误并执行建议的改进措施，是使网站能正常运行的好方法。它使你确保自己的网站尽可能快速，尽可能高可用。

试试看它吧！

首先，你需要使用生产环境打包。Gatsby 的开发服务器是为了快速开发而优化过的，虽然这个版本与生产版本极其相似，但是优化完全不一样。

### ✋ 创建一个生产环境版本

1.  停止开发服务器（如果它正在运行）并执行以下命令：

```shell
gatsby build
```

> 💡 和你在 [第 1 章](/tutorial/part-one/) 中学到的一样，这个命令会构建网站的生产版本，并把静态文件输出到 `public` 目录中。

2.  在本地查看你的生产环境版本。运行：

```shell
gatsby serve
```

<<<<<<< HEAD
当启动完成后，你可以个在 [`localhost:9000`](http://localhost:9000) 这个页面访问你的网站。
=======
Once this starts, you can view your site at `http://localhost:9000`.
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

### 运行一次 Lighthouse 审查

现在你将要第一次运行 Lighthouse 测试。

1.  如果你还没有这么做过，请：在 Chrome 隐身模式下打开你的网站，这样就没有浏览器扩展干扰测试。然后打开 Chrome 开发者工具。

2.  点击 “Audits” 标签，然后你会在屏幕上看到这样的内容：

![开始 Lighthouse 审查](./lighthouse-audit.png)

3.  点击 “Perform an audit...” （所有可用的审查类型应该默认被选中了）。然后点击 “Run audit”（然后就会运行一个一分钟左右的审查）。审查完成时，你应该能看到像这样的结果：

![Lighthouse 审查结果](./lighthouse-audit-results.png)

如你所见，Gatsby 网站开箱即用，性能已经非常好了。但 PWA、可访问性（Accessibility）、最佳实践（Best Practices）和 SEO 等方面还有提高的空间，分数还能更高。在提高后你的网站将对访问者和搜索引擎更加友好。

## 增加一个清单（manifest）文件

看起来你的网站在 “渐进式 Web 应用”（Progressive Web App）类别中的得分很低。让我们来解决这个问题。

首先我们要搞清楚：什么是 PWA？

它是一个利用现代浏览器功能，利用像 app 一样的功能和好处，来丰富网络体验的常规网站。查看 [Google概述](https://developers.google.com/web/progressive-web-apps/) 中定义的 PWA 网站体验。

包含 Web 应用清单文件是 [PWA 的三个公认基准要求](https://alistapart.com/article/yes-that-web-project-should-be-a-pwa#section1) 之一。

[Google](https://developers.google.com/web/fundamentals/web-app-manifest/) 提到：

> Web 应用的清单是一个简单的 JSON 文件。它告诉浏览器 Web 应用的信息，以及当用户 “安装” Web 应用时它要如何呈现。

[Gatsby 的清单插件](/packages/gatsby-plugin-manifest/) 能为每一个 Gatsby 网站的构建配置一个 `manifest.webmanifest` 文件。

### ✋ 使用 `gatsby-plugin-manifest`

1.  安装插件:

```shell
npm install --save gatsby-plugin-manifest
```

2. 添加一个网站图标（favicon） `src/images/icon.png` 到你的应用里。就本章教程所构建的应用而言，如果你手头没有可用的图标，你可以使用 [这个示例图标](https://raw.githubusercontent.com/gatsbyjs/gatsby/master/docs/tutorial/part-eight/icon.png)。这个图标是为清单文件构建所有图像所必需的。详情请查这篇文档 [`gatsby-plugin-manifest`](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-plugin-manifest/README.md)。

3. 在 `gatsby-config.js` 文件里，把插件添加到 `plugins` 数组中。

```javascript:title=gatsby-config.js
{
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png`, // This path is relative to the root of the site.
      },
    },
  ]
}
```

这就是开始向 Gatsby 网站添加网络清单所需的全部了。给出的示例包含基本的配置——请查看 [插件应用](/packages/gatsby-plugin-manifest/?=gatsby-plugin-manifest#automatic-mode) 来了解更多配置选项。

## 添加离线支持

网站要成为 PWA 的另一个要求是使用 [service workder](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)。Service worker 在后台运行，根据应用与网络的连通性来决定使用网络还是内容缓存，从而实现无缝的离线体验。

[Gatsby 的离线插件](/packages/gatsby-plugin-offline/) 使 Gatsby 站点能够离线运行，并通过创建一个 service worker 使你的站点在糟糕网络状况下受到的影响更小。

### ✋ 使用 `gatsby-plugin-offline`

1.  安装插件：

```shell
npm install --save gatsby-plugin-offline
```

2.  在 `gatsby-config.js` 文件里，把插件添加到 `plugins` 数组中。

```javascript:title=gatsby-config.js
{
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png`, // This path is relative to the root of the site.
      },
    },
    // highlight-next-line
    `gatsby-plugin-offline`,
  ]
}
```

这就是在 Gatsby 中开始使用 service worker 所需的全部了。

> 💡 离线插件应该放在清单插件 _之后_。以便离线插件可以缓存清单插件创建的 `manifest.webmanifest`。

## 添加页面元数据（metadata）

为页面添加元数据（比如 title 和 description），是帮助 Google 之类的搜索引擎理解页面内容，决定什么时候出现在搜索结果里的关键。

[React Helmet](https://github.com/nfl/react-helmet) 是一个提供了 React 组件接口的包，帮助你管理 [document head](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/head)。

Gatsby 的 [React Helmet 插件](/packages/gatsby-plugin-react-helmet/) 为 React Helmet 添加的服务端渲染的数据提供了直接支持。使用该插件，你添加到 React Helmet 的属性将被添加到 Gatsby 构建的静态 HTML 页面中。

### ✋ 使用 `React Helmet` 和 `gatsby-plugin-react-helmet`

1.  安装这两个插件：

```shell
npm install --save gatsby-plugin-react-helmet react-helmet
```

2.  确保你的 `siteMetadata` 对象中配置好了 `description` 和 `author`。 并在 `gatsby-config.js` 文件里添加 `gatsby-plugin-react-helmet` 插件到 `plugins` 数组中。

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
    // highlight-start
    description: `A simple description about pandas eating lots...`,
    author: `gatsbyjs`,
    // highlight-end
  },
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png`, // This path is relative to the root of the site.
      },
    },
    `gatsby-plugin-offline`,
    // highlight-next-line
    `gatsby-plugin-react-helmet`,
  ],
}
```

3. 在 `src/components` 目录里, 创建一个名为 `seo.js` 的文件并添加以下内容：

```jsx:title=src/components/seo.js
import React from "react"
import PropTypes from "prop-types"
import Helmet from "react-helmet"
import { useStaticQuery, graphql } from "gatsby"

function SEO({ description, lang, meta, title }) {
  const { site } = useStaticQuery(
    graphql`
      query {
        site {
          siteMetadata {
            title
            description
            author
          }
        }
      }
    `
  )

  const metaDescription = description || site.siteMetadata.description

  return (
    <Helmet
      htmlAttributes={{
        lang,
      }}
      title={title}
      titleTemplate={`%s | ${site.siteMetadata.title}`}
      meta={[
        {
          name: `description`,
          content: metaDescription,
        },
        {
          property: `og:title`,
          content: title,
        },
        {
          property: `og:description`,
          content: metaDescription,
        },
        {
          property: `og:type`,
          content: `website`,
        },
        {
          name: `twitter:card`,
          content: `summary`,
        },
        {
          name: `twitter:creator`,
          content: site.siteMetadata.author,
        },
        {
          name: `twitter:title`,
          content: title,
        },
        {
          name: `twitter:description`,
          content: metaDescription,
        },
      ].concat(meta)}
    />
  )
}

SEO.defaultProps = {
  lang: `en`,
  meta: [],
  description: ``,
}

SEO.propTypes = {
  description: PropTypes.string,
  lang: PropTypes.string,
  meta: PropTypes.arrayOf(PropTypes.object),
  title: PropTypes.string.isRequired,
}

export default SEO
```

上面的代码为你最常用的元数据标签（tag）设置了默认值，并提供了一个 `<SEO>` 组件，可在项目的其余部分中使用。很棒吧？

4.  现在你可以在模版和页面中使用 `<SEO>` 组件，并把 props 传进去。比如像这样添加到 `blog-post.js` 模版里：

```jsx:title=src/templates/blog-post.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"
// highlight-next-line
import SEO from "../components/seo"

export default ({ data }) => {
  const post = data.markdownRemark
  return (
    <Layout>
      // highlight-start
      <SEO title={post.frontmatter.title} description={post.excerpt} />
      // highlight-end
      <div>
        <h1>{post.frontmatter.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.html }} />
      </div>
    </Layout>
  )
}

export const query = graphql`
  query($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
      }
      // highlight-next-line
      excerpt
    }
  }
`
```

上面的例子基于 [Gatsby Starter 博客](/starters/gatsbyjs/gatsby-starter-blog/)。通过向`<SEO>` 组件传入 props，你可以实时更改博文的元数据。在这种情况下，将使用博客标题 `title` 和 `excerpt`（如果存在于博客帖子markdown文件中）代替 `gatsby-config.js` `文件中默认 `siteMetadata` 属性。

现在如果你再运行 Lighthouse 审查，你应该能拿到近乎完美的 100 分！

> 💡 更多文档和例子：[添加一个 SEO 组件](/docs/add-seo-component/)，以及 [React Helmet 文档](https://github.com/nfl/react-helmet#example)。

## 不断改善

在本章教程中，我们向你展示了一些 Gatsby 独有的工具，这些工具可以改善网站的性能并让网站准备好上线。

Lighthouse 是一个用于改进和学习网站的非常好的工具——继续查看它提供的详细反馈，并不断改善你的网站！

## 接下来

### 官方文档

- [官方文档](https://www.gatsbyjs.org/docs/)：查看我们的官方文档 _[快速开始](https://www.gatsbyjs.org/docs/quick-start/)_、_[详细指导](https://www.gatsbyjs.org/docs/preparing-your-environment/)_、_[API 参考](https://www.gatsbyjs.org/docs/gatsby-link/)_、等等。

### 官方插件

- [官方插件](https://github.com/gatsbyjs/gatsby/tree/master/packages): 一个包含了所有 Gatsby 自己维护的官方插件的完整列表。

### 官方 Starters

<<<<<<< HEAD
1.  [Gatsby 的默认 Starter](https://github.com/gatsbyjs/gatsby-starter-default)：使用此默认样板启动你的项目。这个入门 Starter 包含了你可能需要的主要 Gatsby 配置文件。_[效果演示](http://gatsbyjs.github.io/gatsby-starter-default/)_
2.  [Gatsby 的博客 Starter](https://github.com/gatsbyjs/gatsby-starter-blog)：能创建一个又好又快的博客的 Gatsby starter。_[效果演示](http://gatsbyjs.github.io/gatsby-starter-blog/)_
3.  [Gatsby 的 Hello-World Starter](https://github.com/gatsbyjs/gatsby-starter-hello-world): 一个最基础的能让 Gatsby 网站运行的 Gatsby starter。_[效果演示](https://gatsby-starter-hello-world-demo.netlify.com/)_
=======
1.  [Gatsby's Default Starter](https://github.com/gatsbyjs/gatsby-starter-default): Kick off your project with this default boilerplate. This barebones starter ships with the main Gatsby configuration files you might need. _[working example](https://gatsbyjs.github.io/gatsby-starter-default/)_
2.  [Gatsby's Blog Starter](https://github.com/gatsbyjs/gatsby-starter-blog): Gatsby starter for creating an awesome and blazing-fast blog. _[working example](https://gatsbyjs.github.io/gatsby-starter-blog/)_
3.  [Gatsby's Hello-World Starter](https://github.com/gatsbyjs/gatsby-starter-hello-world): Gatsby Starter with the bare essentials needed for a Gatsby site. _[working example](https://gatsby-starter-hello-world-demo.netlify.com/)_
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

## 咱们终于全整完了

呃……并不完全是。只是教程结束了。还有一些 [其他教程](/tutorial/additional-tutorials/)，它们也囊括了一些很有意义的用例。

这只是开始。让我们继续探索和使用了不起的 Gatsby！

- 你创建了一个很酷的网站？分享到 Twitter，添加话题 [#buildwithgatsby](https://twitter.com/search?q=%23buildwithgatsby)，并 [艾特我们](https://twitter.com/gatsbyjs)。
- 你为你所学的东西写了一篇博客？也同样分享出来吧！
- 你能为 Gatsby 贡献一份力？逛逛我们 Gatsby 仓库的 [open issues](https://github.com/gatsbyjs/gatsby/issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22)，并 [成为一名贡献者](/contributing/how-to-contribute/)。

看看 ["如何贡献"](/contributing/how-to-contribute/) 文档来获得更多灵感。

我们迫不及待想看到你做了什么 😄。
