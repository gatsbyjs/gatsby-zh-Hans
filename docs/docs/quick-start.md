---
title: 快速开始
---

快速开始适用于中高级开发人员。有关 Gatsby 的入门介绍，[请查看我们的教程](/tutorial/)！

## 使用 Gatsby CLI

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-quick-start-with-gatsby-create-develop-and-build-gatsby-sites-from-the-command-line"
  lessonTitle="Quick Start with Gatsby: Create, Develop, and Build Gatsby Sites From the Command Line"
/>

**注**: 视频中使用的是 `npx`，它是一个工具，允许你执行一个 npm 包命令而不需要事先安装这个包。 在你的电脑安装 gatsby-cli 之后，运行命令 `npx gatsby new` 和运行命令 `gatsby new` 的效果相同。

<<<<<<< HEAD
### 安装 Gatsby CLI.
=======
### Install the Gatsby CLI
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

```shell
npm install -g gatsby-cli
```

<<<<<<< HEAD
### 创建站点
=======
> The above command installs Gatsby CLI globally on your machine.

### Create a new site
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

```shell
gatsby new gatsby-site
```

<<<<<<< HEAD
### 切换到站点目录
=======
### Change directories into site folder
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

```shell
cd gatsby-site
```

<<<<<<< HEAD
### 启动开发服务器
=======
### Start development server
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

```shell
gatsby develop
```

<<<<<<< HEAD
Gatsby 将会启动一个热更新的开发环境，你可以通过 `localhost:8000` 访问。
=======
Gatsby will start a hot-reloading development environment accessible by default at `http://localhost:8000`.
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

尝试修改位于 `src/pages` 下的页面中的 JavaScript。 保存变更，浏览器中的内容会自动热更新。

<<<<<<< HEAD
### 构建生产版本
=======
### Create a production build
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

```shell
gatsby build
```

Gatsby 将会为你站点的生产版本执行一些优化工作，生产静态 HTML 和预加载的 JavaScript 代码包。

<<<<<<< HEAD
### 在本地启动生产版本服务器
=======
### Serve the production build locally
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

```shell
gatsby serve
```

Gatsby 会启动一个本地 HTML 服务器，用于测试你构建的站点。在执行这个命令前，记得先使用 `gatsby build` 构建你的站点。

### 查看 CLI 指令文档

在终端中运行 `gatsby --help` 指令，可查看 CLI 指令的详细文档。

对于特殊的指令，运行 `gatsby COMMAND_NAME --help` ，例如 `gatsby new --help`。

想要了解更多关于 Gatsby CLI 的内容，访问文档中的 [CLI 参考](/docs/gatsby-cli/) 章节。
