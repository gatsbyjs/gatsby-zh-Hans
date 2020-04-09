---
title: 使用主题
---

在这篇指南中，你将学会如何使用 Gatsby 主题。我们将使用官方的博客主题创建一个新的站点。

## 使用博客主题 starter 创建新站点

使用主题 starter 创建新站点和使用常规 Gatsby starter 的方法一样。

```shell
gatsby new my-blog https://github.com/gatsbyjs/gatsby-starter-blog-theme
```

## 运行站点

通过上面的方式创建新站点 ，博客主题的依赖已经全部安装好了。 接下来运行站点，看看它长什么样子吧。

```shell
cd my-blog
gatsby develop
```

![Default screen when starting a project using gatsby blog starter](./images/starter-blog-theme-default.png)

## 替换你的头像

这个博客主题的 starter 使用一张纯灰度图像作为头像。选一张你自己的头像图片，覆盖 `/content/assets/avatar.png` 这个文件。

## 更新站点的 metadata

修改 `gatsby-config.js` 文件中的 `siteMetadata`，定制化自己的站点信息。

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: "gatsby-theme-blog",
      options: {},
    },
  ],
  // Customize your site metadata:
  {/* highlight-start */}
  siteMetadata: {
    title: "My Blog",
    author: "Amberley Romo",
    description: "A collection of my thoughts and writings.",
    siteUrl: "https://amberley.blog/",
    social: [
      {
        name: "twitter",
        url: "https://twitter.com/amber1ey",
      },
      {
        name: "github",
        url: "https://github.com/amberleyromo",
      },
    ],
  },
  {/* highlight-end */}
}
```

## 替换掉个人简介内容

使用 Gatsby 主题时，你可以利用称为组件遮蔽（Gatsby 会优先使用自定义的同名组件渲染）的功能。 这使你可以使用已创建的自定义组件覆盖主题中包含的默认组件。

Gatsby 博客主题包中包含一个组件，其内容为网站作者的传记内容。 该组件（在博客主题包中，而不是你的站点中）的文件路径是 `gatsby-theme-blog/src/components/bio-content.js`。 通过浏览 `node_modules/gatsby-theme-blog` 这个目录下的内容，你可以找到此路径

如果你看一下站点的文件结构，你将会看到下面这样的内容:

```text
my-blog
├── content
│   ├── assets
│   │   └── avatar.png
│   └── posts
│       ├── hello-world.mdx
│       └── my-second-post.mdx
├── src
│   └── gatsby-theme-blog
│       ├── components
│       │   └── bio-content.js
│       └── gatsby-plugin-theme-ui
│           └── colors.js
├── gatsby-config.js
└── package.json
```

`src` 目录下有一个名为 `gatsby-theme-blog` 的目录，该目录下符合名称匹配规则（与主题内的文件同名）的文件，将会遮蔽主题原有的组件。

> 💡 这个目录的名称 (`gatsby-theme-blog`) 必须与主题发布包的名称完全相同，本例中名称为：[`gatsby-theme-blog`](https://www.npmjs.com/package/gatsby-theme-blog)。

打开 `bio-content.js` 文件，然后做一些编辑：

```jsx:title=bio-content.js
import React, { Fragment } from "react"

export default () => (
  {/* highlight-start */}
  <Fragment>
    这里是我的个人介绍
    <br />
    这样将会覆盖原有主题中的个人介绍
  </Fragment>
  {/* highlight-end */}
)
```

进行到这里，你应该已经更新了头像、站点详情、个人简介。

![Screenshot of project with current tutorial edits](./images/starter-blog-theme-edited.png)

## 添加自己的博客内容

现在你可以添加自己的博客文章了，和 starter 中的演示内容说再见吧。

### 创建一篇新文章

在 `my-blog/content/posts` 目录下创建文件。 取一个自己想要的名字(以 `.md` 或者 `.mdx` 文件扩展名结尾)，添加一些内容，下面是例子。

```mdx:title=my-blog/content/posts/my-first-post.mdx
---
title: 我的第一篇文章
date: 2019-07-03
---

这里是文章内容
```

### 删除演示用的文章

删除 `/content/posts` 目录下的2篇演示文章

- `my-blog/content/posts/hello-world.mdx`
- `my-blog/content/posts/my-second-post.mdx`

重启开发服务器，你将看到新的文章内容。

![Screenshot of project with updated post content](./images/starter-blog-theme-updated-content.png)

## 修改主题颜色

这个博客主题附带默认的 Gatsby 紫色主题 ，但是你可以覆盖修改为自己喜欢的颜色。在这个指南中你会修改一些颜色。

打开 `/src/gatsby-theme-blog/gatsby-plugin-theme-ui/colors.js`，取消代码的注释。

```javascript:title=colors.js
import merge from "deepmerge"
import defaultThemeColors from "gatsby-theme-blog/src/gatsby-plugin-theme-ui/colors"

{/* highlight-start */}
const darkBlue = `#007acc`
const lightBlue = `#66E0FF`
const blueGray = `#282c35`
{/* highlight-end */}

export default merge(defaultThemeColors, {
  {/* highlight-start */}
  text: blueGray,
  primary: darkBlue,
  heading: blueGray,
  modes: {
    dark: {
      background: blueGray,
      primary: lightBlue,
      highlight: lightBlue,
    },
  },
  {/* highlight-end */}
})
```

现在主题色从紫色变成了蓝色。

![Screenshot of project with updated color theme](./images/starter-blog-theme-updated-colors.png)

在这个文件中，你会拉取默认的主题 (`defaultThemeColors`)，合并覆盖部分颜色。

想看看哪些颜色你可以修改，查看官方博客主题中的 `colors.js` 文件  (`node_modules/gatsby-theme-blog/src/gatsby-plugin-theme-ui/colors.js`)

## 总结

通过展示这些例子，我们一步一步的介绍了如何使用 Gatsby 主题。值得注意的是，不同的主题会有不同的配置选项。想深入了解，可以查看 [Gatsby 主题文档](/docs/themes/)。
