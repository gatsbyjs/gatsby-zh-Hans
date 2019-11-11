---
title: 使用多个 Gatsby 主题
---

Gatsby 主题的宗旨就是可组合。这意味着你可以同时安装多个主题。

`gatsby-starter-theme` 由两个主题组合而成：`gatsby-theme-blog` 和 `gatsby-theme-notes`

```shell
gatsby new my-notes-blog https://github.com/gatsbyjs/gatsby-starter-theme
```

这个 Starter 包含了2个主题包（`gatsby-theme-blog` 和 `gatsby-theme-notes`），在 Starter 的 `gatsby-config.js` 文件中你可以找到它们。

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-theme-notes`,
      options: {
        mdx: true,
        basePath: `/notes`,
      },
    },
    // with gatsby-plugin-theme-ui, the last theme in the config
    // will override the theme-ui context from other themes
    { resolve: `gatsby-theme-blog` },
  ],
  siteMetadata: {
    title: `Shadowed Site Title`,
  },
}
```

默认配置中，博客内容将会存在于根目录下(`/`)，笔记内容将会存在于 `/notes` 路径下。

运行 `gatsby develop` 启动开发服务器，然后便可查看自己的站点：

![The homepage of the site created by gatsby-theme-starter](../images/gatsby-theme-starter-home.png)

![The `notes` route of a site created by gatsby-theme starter](../images/gatsby-theme-starter-notes.png)

## 教程

关于更加详细的分步教程，你可以在[教程：“同时使用多个主题"](/tutorial/using-multiple-themes-together)中查阅。
