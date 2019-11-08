---
title: 什么是 Gatsby 主题?
---

为了介绍主题，我们先回顾一下历史。这将有助于我们理解，为什么要推出主题这一功能。

如果你曾经从头到尾地创建过一个 Gatsby 站点，你肯定有这种体会，你需要做很多的决定。以博客为例，你需要决定博文数据放在哪里（是在本地以 markdown 形式存储，还是存储在远程的 CMS 服务中），怎样获取数据（是通过 transformer 转化 markdown 文件，还是从 API 中获取），展示哪些内容，主题如何配色，使用什么风格的组件等等。

## Gatsby starters

使用 "[Gatsby starters](/docs/starters/)" 可以快速搭建一个与 Starter 功能相同的站点。 Starter 本质上是一些配置好特定功能的 Gatbsy 站点。你下载的是一个完整的 Gatsby 站点，它们通常是为特定场景构建的 (例如：博客、 作品集展示站点等等) 并且是这些都是定制好的。

传统的 Starter 可以帮助新手快速构建一个 Gatsby 站点，它减少了工作量，使得入门变得容易。但是这存在 2 个主要的问题：

- 通过 Starter 创建的站点，已经和 Starter 脱离了关系。即它们已经不再受到 Starter 影响，出现了分叉。如果上游的 Starter 后续有了变更，这些通过 Starter 创建的站点，则没有很好的办法保持与上游 Starter 同步。
- 如果你使用同一个 Starter 创建了多个站点，后来你又想对这多个站点进行相同的更改，你将不得不逐个地去修改这些站点。

## Gatsby 主题

言归正传，该讲讲主题了。Gatsby 主题允许你将站点功能作为一个单独的产品打包，它有助于自己和他人轻松地复用这些功能。 使用传统的 Starter，所有的默认配置都是直接存储在你的站点中。使用主题，这些默认配置则存储在 npm 包中。

主题解决了传统 Starter 面临的一些问题：

- 使用 Gatbsy 主题创建的站点可以很方便地跟进上游主题的更新 ———— 主题是一个版本化的 npm 包，就跟其它的 npm 包一样。
- 你可以使用同一个主题创建多个站点. 要同时更新这些站点，你只需要更新主题包，发布新版。然后修改 `package.json` 文件的主题版本号即可（而不用逐个地修改站点内的功能）。
- 主题是可组合使用的。 你可以将博客主题、笔记主题和电子商务主题（等等）一起安装使用。

> Gatsby 主题实际上是可组合的 Gatsby 配置。 它们提供了一种高阶的方法，可以与 Gatsby 一起使用。通过这种方法将复杂或重复的部分抽象为可重用的 npm 包。

## 哪些情况下我应该使用/构建主题?

**以下情况考虑使用主题：**

- 你有一个 Gatsby 站点了，你想加入其它 Starter 的功能。
- 你希望保持网站上的功能为最新版本。
- 你希望你的网站拥有很多功能，但是没有一个 Starter 完全拥有你想要的功能 ————  你可以使用多个主题，在一个 Gatsby 站点中组合使用。

**以下情况考虑构建主题：**

- 你打算在多个 Gatsby 站点中，使用相同的功能。
- 你打算和 Gatsby 社区分享你的作品。

## 接下来?

- [使用 Gatsby 主题](/docs/themes/using-a-gatsby-theme)
- [同时使用多个 Gatsby 主题](/docs/themes/using-multiple-gatsby-themes)
- [遮蔽](/docs/themes/shadowing/)
- [构建主题](/docs/themes/building-themes)
- [Starter 转化为主题](/docs/themes/converting-a-starter/)
- [主题结构](/docs/themes/theme-composition/)
- [约定](/docs/themes/conventions/)

## 相关博客文章

这里有些附加的内容，它们是主题开发期间发布的博客文章：

- [Why Themes?](/blog/2019-01-31-why-themes/)
- [Themes Roadmap](/blog/2019-03-11-gatsby-themes-roadmap/)
- [Getting Started with Gatsby Themes and MDX](/blog/2019-02-26-getting-started-with-gatsby-themes/)
- [Watch Us Build a Theme Live](/blog/2019-02-11-gatsby-themes-livestream-and-example/)
- [Introducing Gatsby Themes by Chris Biscardi at Gatsby Days](https://www.gatsbyjs.com/gatsby-days-themes-chris/)
- [See all blog posts on themes](/blog/tags/themes)
