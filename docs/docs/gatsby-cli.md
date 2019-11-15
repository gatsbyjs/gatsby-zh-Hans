---
title: 命令（Gatsby CLI）
tableOfContentsDepth: 2
---

Gatsby 命令行工具（CLI） 是启动和运行 Gatsby 应用程序以及使用诸如运行开发服务器和构建 Gatsby 应用程序进行部署等功能的主要入口点。

_我们为 gatsby-cli 准备了一份 [README](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-cli/README.md) 文档， 这份 [备忘录](/docs/cheat-sheet/) 中包含了一些常见的命令以方便你打印_

## 如何使用 gatsby-cli

Gatsby CLI (`gatsby-cli`) 是一个全局可执行的 npm 包， Gatsby CLI 发布在 [npm](https://www.npmjs.com/) 上，你应该运行 `npm install -g gatsby-cli` 全局安装它以便在本地使用。

运行 `gatsby --help` 查看帮助。

你也可以使用 `package.json` script 中自行定义这些命令，通常大多数[starters](/docs/starters/)都会_为你_暴露一些命令。例如，如果你想在你的应用中使用 [`gatsby develop`](#develop) 命令，打开`package.json` 像这样添加一份 script:

```json:title=package.json
{
  "scripts": {
    "develop": "gatsby develop"
  }
}
```

## API 命令

### `new`

```
gatsby new [<site-name> [<starter-url>]]
```

#### 参数

| 参数    | 说明                                                                                                                                                                                                     |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| site-name   | 你的 Gatsby 站点的名称，会创建同名目录。                                                                                                                                    |
| starter-url | Gatsby starter URL 或者本地的文件路径。 默认使用 [gatsby-starter-default](https://github.com/gatsbyjs/gatsby-starter-default)，参见：[Gatsby starters](/docs/gatsby-starters/) |

> 注: `site-name` 应该只包含字母和数字。 如果名称中使用了 `.`, `./` 或者 `<空格>`， `gatsby new` 会抛出一个错误。

#### 示例

- 使用默认的 Starter 创建一个名为 `my-awesome-site` 的 Gatsby 站点。

```shell
gatsby new my-awesome-site
```

- 使用 Starter:[gatsby-starter-blog](https://www.gatsbyjs.org/starters/gatsbyjs/gatsby-starter-blog/) 创建一个名为 `my-awesome-blog-site` 的站点:

```shell
gatsby new my-awesome-blog-site https://github.com/gatsbyjs/gatsby-starter-blog
```

- 如果你两个参数都留空不填，CLI 会运行一个可交互的 shell，向你询问一些输入：

```shell
gatsby new
? What is your project called? › my-gatsby-project
? What starter would you like to use? › - Use arrow-keys. Return to submit.
❯  gatsby-starter-default
   gatsby-starter-hello-world
   gatsby-starter-blog
   (Use a different starter)
```

阅读 [Gatsby starters docs](https://www.gatsbyjs.org/docs/gatsby-starters/) 获取更新信息。

### `develop`

一旦你安装了一个 Gatsby 站点，切换到项目根目录下开启开发服务器：

`gatsby develop`

#### 选项

|     选项      | 说明                                     |
| :-------------: | ----------------------------------------------- |
| `-H`, `--host`  | 设置主机地址。默认是 localhost                 |
| `-p`, `--port`  | 设置端口。默认是 8000                      |
| `-o`, `--open`  | 在浏览器中打开站点 |
| `-S`, `--https` | 使用 HTTPS                                      |

参照[本地 HTTPS 指南](/docs/local-https/)，了解如何使用 Gatsby 配置一个本地 HTTPS 的开发服务器。


#### 在其它设备上预览修改

你可以使用带 host 选项的 Gatsby 开发命令运行你的开发服务器，这样你可以通过同一网络下的其它设别访问服务，运行：

```shell
gatsby develop -H 0.0.0.0
```

终端会照常打印日志。但是会附加一个 URL，你可以通过同一网络下的其它客户端访问这个 URL，以便查看站点在其它设备上如何展示（适用于手机真机调试）。

```
You can now view gatsbyjs.org in the browser.
⠀
  Local:            http://0.0.0.0:8000/
  On Your Network:  http://192.168.0.212:8000/ // highlight-line
```

**注**：在 Windows 上你无法访问 0.0.0.0:8000 （但是在 Windows 上可以通过 localhost:8000 或者 "On Your Network" 中给出的 URL 访问）

### `build`

在 Gatsby 站点的根目录下，编译你的应用为部署做好准备：

`gatsby build`

#### 选项

|            选项            | 说明                                                                                               |
| :--------------------------: | --------------------------------------------------------------------------------------------------------- |
|       `--prefix-paths`       | 构建站点时，使用 link paths 前缀（在你的配置中设置的 pathPrefix）                                       |
|        `--no-uglify`         | 构建站点时，不混淆 JS 包（方便调试）
| `--open-tracing-config-file` | 追踪器配置文件（和 OpenTracing 兼容）。参见 [性能追踪](/docs/performance-tracing/) |
| `--no-color`, `--no-colors`  | 禁用终端打印彩色输出                                                                          |

出了上面这些构建选线，这里有更多的选项[构建环境变量](/docs/environment-variables/#build-variables) 用于更加高级的配置，去调整构建的运行。例如，设置 `CI=true` 环境变量将会为[哑终端
](https://en.wikipedia.org/wiki/Computer_terminal#Dumb_terminals)定制输出。

### `serve`

在 Gatsby 站点的根目录下，启动一个服务器用于测试你站点的生产环境版本：

`gatsby serve`

#### 选项

|      选项      | 说明                                                                              |
| :--------------: | ---------------------------------------------------------------------------------------- |
|  `-H`, `--host`  | 设置主机地址。默认是 localhost                                                          |
|  `-p`, `--port`  | 设置端口。 默认是 9000                                                               |
|  `-o`, `--open`  | 在浏览器中打开站点                                         |
| `--prefix-paths` | 站点服务带有路径前缀（如果构建时你的 gatsby-config.js 使用了 pathPrefix) |

### `info`

在 Gatsby 站点根目录下，获取有用的环境信息，当你报告 bug 时，通常需要提供这些信息：

`gatsby info`

#### 选项

|       选项        | 说明                                             |
| :-----------------: | ------------------------------------------------------- |
| `-C`, `--clipboard` | 自动复制环境信息到剪切板 |

### `clean`

在 Gatsby 站点根目录下， 清空缓存（`.cache` 目录）和 `public 目录：

`gatsby clean`

当您的本地项目似乎有问题或内容似乎没有刷新时，此方法非常有用。 这通常可以解决下面这些问题：

- 数据过时，例如：某个文件/资源等没有出现。
- GraphQ 错误，例如： GraphQL 资源应该存在，但是找不到。
- 依赖问题，例如：无效的版本，控制台中的隐秘错误等等。
- 插件问题，例如：开发一个本地插件，但是变更似乎没有生效。

### `plugin`

运行与 Gatsby 插件有关的命令。

#### `docs`

`gatsby plugin docs`

指导你使用和创建插件的文档。

### Repl

使用你的 Gatsby 环境的上下文获取一个 Node.js 的 REPL（交互式 shell）：

`gatsby repl`

Gatsby 将提示你键入命令并进行浏览。 当显示以下内容时：`gatsby >`

你可以键入下面这些命令：

`babelrc`

`components`

`dataPaths`

`getNodes()`

`nodes`

`pages`

`schema`

`siteConfig`

`staticQueries`

当和[GraphQL explorer](/docs/introducing-graphiql/)一起使用时，这些 REPL 命令会非常有用，它有助于你理解 Gatsby 站点的数据。

想了解更多信息，查阅[Gatsby REPL 文档](/docs/gatsby-repl/)。

### 禁用彩色输出

出了显式的 `--no-color` 选项，CLI 还考虑到了 `NO_COLOR` 环境变量的存在（参见 [no-color.org](https://no-color.org/)）。

## 如何为你的下一个项目更改默认的包管理器？

当你使用 `gatsby new` 首次创建一个新的项目时，你会被询问是使用 yarn 还是 npm 作为包管理器。

```shell
Which package manager would you like to use ? › - Use arrow-keys. Return to submit.
❯  yarn
   npm
```

一旦你做出了选择，在后续的项目中 CLI 不会再询问你的偏好。

如果要为下一个项目更改此设置，则必须编辑由 CLI 自动创建的配置文件。
这个文件位于你系统中的这个位置： `~/.config/gatsby/config.json`

在里面你将看到这样的内容。

```json:title=config.json
{
  "cli": {
    "packageManager": "yarn"
  }
}
```

编辑你的 `packageManager` 的值，保存。然后你就可以愉快地使用 `gatsby new` 创建下一个项目了。
