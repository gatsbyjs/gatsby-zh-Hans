---
title: 词汇表
disableTableOfContents: true
---

import HorizontalNavList from "../../www/src/components/horizontal-nav-list.js"

当你不熟悉 Gatsby 时，可能会不了解很多词汇。本词汇表旨在为你提供一篇足够长的的常用术语概述，以及这些术语在 Gatsby 站点中意味着什么。

<HorizontalNavList
items={"ABCDEFGHIJKLMNOPQRSTUVWXYZ".split("")}
slug={props.slug}
/>

## A

### AST（抽象语法树）

Abstract Syntax Tree（抽象语法树）：在两种语言之间的 [编译](#compiler) 阶段，源代码的树结构表示。例如 [gatsby-transformer-remark](/packages/gatsby-transformer-remark/) 会从 [Markdown](#markdown) 文件创建一个 AST，通过 [Remark](#remark) 解析器来描述一个树结构 Markdown 文档

### API（应用程序编程接口）

应用程序编程接口：一种应用程序与另一应用程序进行通信的方法。例如 [数据源插件](#source-plugin) 通常会使用API来获取其数据。

### Accessibility（可访问性）

为了消除阻碍残疾人与网站互动或访问网站障碍的一种包容性做法。如果为了实现可访问性正确设计、开发和编辑了站点，则通常所有用户对信息和功能都有同等的访问权。阅读 [Gatsby 对可访问性的承诺](/blog/2019-04-18-gatsby-commitment-to-accessibility/)。

## B

### Babel

一个让你编写最现代的 [JavaScript](#javascript) 代码的工具，并在 [构建](#build) 时将代码 [编译](#compiler) 为大多数浏览器可以理解的代码。

### Backend（后端）

[公众（public）](#public) 看不到的幕后。通常指你的 [CMS](#cms) 的控制面板。这些通常由服务器端编程语言提供支持，例如Node.js，PHP，Go，ASP.net，Ruby 或 Java。

### Build（构建）

在 Gatsby 中，这是获取代码和内容并将其打包到可以托管和访问的网站中的过程。我们通常说 _构建时……_。另请参见：[后端](#backend) and [服务器端](#server-side)。

## C

### Cache（缓存）

一个可以再次使用的本地信息存储方式，使我们可以从某一位置更快地计算和查找。Gatsby 使用缓存来存储信息，因此在你进行开发时，它可以更快地构建你的网站，而无需重复相同的工作。

### CLI（命令行界面）

命令行界面：通过 [命令行](#command-line) 在计算机上运行并通过键盘交互的应用程序。

Gatsby 有两个命令行界面。一个是[`gatsby`](/docs/gatsby-cli/)，用于 Gatsby 日常开发；另一个是 [`gatsby-dev`](/contributing/setting-up-your-local-dev-environment/#gatsby-repo-install-instructions)，供那些参与贡献 Gatsby 项目的人使用。

### Client-side（客户端）

客户端是指由用户浏览器在计算机网络中以 [客户与服务关系](https://en.wikipedia.org/wiki/Client%E2%80%93server_model) 执行的操作。在 Gatsby 中，当使用依赖于 [浏览器 DOM](#dom) 中的对象（例如 `window` 或 `navigator`）的 [客户端包](/docs/using-client-side-only-packages/)时很重要。另请参见：[服务器端](#server-side)，[前端](#frontend) 和 [后端](#backend)。

### CMS（内容管理系统）

内容管理系统：你可以在其中管理内容并将其保存到数据库或文件中以供日后访问。内容管理系统包括 WordPress，Drupal，Contentful 和 Netlify CMS 等等。

### Command Line（命令行）

一个基于文本的界面，用于在计算机上运行命令。Mac 和 Windows 的默认命令行应用程序分别是 `Terminal` 和 `Command Prompt`。

### Compiler（编译器）

编译器是将一种语言编写的代码转换为另一种语言的程序。例如，[Gatsby](#gatsby) 可以将 [React](#react) 应用程序编译成静态 [HTML](#html) 文件。

### Component（组件）

组件是由 [React](#react) 提供支持的独立且可重复使用的代码块，这些代码结合起来构成你的网站或应用。

组件中可以包含组件。[pages](#page) 和 [templates](#template) 是两个很好的实际例子。

### Config（配置）

配置文件。`gatsby-config.js` 告诉 Gatsby 有关你的网站的信息。一个常见的配置选项是你的网站元数据（meatadata），它可以为你的 SEO 元标记（meta tag）提供支持。

### CSS（级联样式表）

[CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) 代表级联样式表，它是 [HTML](#html) 和 [JavaScript](#javascript) 在网络平台（Web Platform）中的重要组成部分。CSS 是一种使网页样式化的语言，旨在使网页高度向后兼容。随着向终端用户推出新功能，[CSS 解析器](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/#CSS_parsing) 可以安全地忽略不受支持的功能，并增强其支持的属性。CSS通过 _级联_ 设计来完成任务，这是使用 [CSS Grid](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/CSS_Grid_and_Progressive_Enhancement) 等新技术进行样式设置的基础。Gatsby支持多种 [样式设计方法](/docs/styling/)，包括常规 CSS 文件，CSS 模块和 CSS-in-JS。

## D

### Data Source（数据源）

内容和数据的起点。通常通过 [数据源插件](#source-plugin) 集成到 Gatsby 中。数据源通常是 [无头 CMS](#headless-cms)，但它也可以包含 Markdown，JSON 或 YAML 文件。

### Database（数据库）

数据库是数据或内容的结构化集合。通常 [CMS](#cms) 使用 [后端技术](#backend) 将数据保存到数据库。通常可以通过 [数据源插件](#source-plugin) 在 Gatsby 中访问它们。

### Decoupled（解耦）

解耦描述了关注点的分离。对于 [Gatsby](#gatsby)，这通常意味着将 [前端](#frontend) 与 [后端](#backend) 分离，比如 [Drupal 解耦](https://dri.es/how-to-decouple-drupal-in-2019) 或 [无头 WordPress](https://www.smashingmagazine.com/2018/10/headless-wordpress-decoupled/)。

### Deploy（部署）

[构建](#build) 你的网站或应用并上传到 [托管服务提供商](#hosting) 的过程。

### Development Environment（开发环境）

开发代码时的[环境](#environment)。它可以通过使用 [CLI](#cli) `gatsby develop` 来访问，并提供额外的错误报告和帮助你在为 [生产环境](#production-environment) 构建之前进行调试的功能。

### DOM（文档对象模型）

文档对象模型（通常称为 “the DOM”）是一种标准的浏览器 API，它通过表示内存中 HTML 文档的结构，将网页连接到脚本或编程语言。开发人员通常通过 [HTML](#html) 标记（在Gatsby 中以 [JSX](#jsx) 编写）以及 [React](https://reactjs.org/docs/react-dom.html) 和 [原生 JavaScript](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction#DOM_and_JavaScript) 与 DOM 进行交互。它充分利用 DOM 的另一个重要方面是编写 [可访问的](#accessibility) HTML 标记，以将页面结构暴露给残疾人辅助技术。

## E

### ECMAScript

ECMAScript（通常称为ES）是脚本语言的规范。[JavaScript](#javascript) 是 ECMAScript 的一种实现。通常，开发人员会使用 [Babel](#babel) 将最新的 ECMAScript 代码 [编译](#compiler) 成更广泛支持的 JavaScript。

### Environment（环境）

Gatsby 所运行的环境。例如在编写代码时，你可能希望尽可能多地进行调试，但这在上线的网站或应用程序中是不可取的。因此，Gatsby 可以根据所处的环境来更改其行为。

Gatsby 默认情况下支持两种环境，即 [开发环境](#development-environment) 和 [生产环境](#production-environment)。

### Environment Variables（环境变量）

[环境变量](/docs/environment-variables/) 允许你根据应用程序的 [环境](#environment) 自定义其行为。例如你可能希望在开发过程中从测试 CMS 获取内容，并在 [构建](#build) 网站时连接到生产 CMS。使用环境变量，可以为每个环境设置不同的 URL。

## F

### Filesystem（文件系统）

文件的组织方式。在 Gatsby 中，这意味着将文件与您的网站或应用程序的代码放在同一位置，而不是从外部 [数据源](#data-source) 中提取数据。Gatsby 中常见的文件系统用法包括 Markdown 内容，图像，数据文件和其他资源。

### Frontend（前端）

您的网站或应用程序面向 [公众](#public) 的界面，使用 Web 技术（HTML，CSS 和 JavaScript）提供。有关网络平台（Web Platform）如何将这些技术整合在一起的更多信息，请查看 [浏览器的工作原理](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)。

## G

### Gatsby

Gatsby是一个现代的网站框架，它利用[React](#react)，[GraphQL](#graphql) 和现代的 [JavaScript](#javascript) 等最新网络技术，为每个网站或应用程序构建卓越的性能。通过 Gatsby，无需成为网站性能专家，你就可以轻松创建非常快速的引人入胜的 Web 体验。

### [GraphQL](/docs/glossary/graphql)

一种可让您将数据提取到您的网站或应用中的 [查询](#query) 语言。这是 [Gatsby 用于管理网站数据的界面](/docs/graphql/)。

## H

### HTML（超文本标记语言）

每个 Web 浏览器都能理解的标记语言。它表示 Hypertext Markup Language（超文本标记语言）。[HTML](https://developer.mozilla.org/en-US/docs/Web/HTML) 为您的 Web 内容提供了一种通用的信息结构，定义了标题，段落等内容。这也是提供可访问网站的关键。

### Headless CMS（无头 CMS）

一个仅处理 [后端](#backend) 而不同时处理后端和 [前端](#frontend) 的 [CMS](#cms) 内容管理方式。这种类型的设置也称为 [解耦](#decoupled)。

### [Headless WordPress](/docs/glossary/headless-wordpress)

The practice of using JSON returned from the WordPress REST API as a [headless CMS](#headless-cms). It allows you to use WordPress to write and edit content that can be consumed by any client capable of parsing JSON.

### Hosting

保留您的网站或应用程序副本的托管服务提供商，并允许 [公众](#public) 访问。[Gatsby的常见托管服务提供商](/docs/deploying-and-hosting/) 包括了 Netlify，AWS，S3，Surge，Heroku 等。

### Hot module replacement（模块热替换）

运行 `gatsby develop` 时使用的一项功能。在文本编辑器中实时保存代码后，可在打开的浏览器窗口中自动替换模块或代码块，从而实时更新网站。

### Hydration（补水）

一旦网站由 Gatsby [构建](#build) 并加载到网络浏览器中，[客户端](#client-side) JavaScript 资源就会被下载并将该站点转变为可以操纵的 [DOM](#dom)。这个过程通常称为 “补水”，因为它运行了一些用于生成 Gatsby 页面的 JavaScript 相同代码，但是这个过程中诸如 `window` 之类的浏览器 DOM API 可用。

## I

### Inference

As part of its data layer and [build](#build) process, Gatsby will automatically **infer** a [schema](#schema), or type-based structure, based on available data sources (e.g. Markdown file nodes, WordPress posts, etc.). More control can be gained over this structure by using Gatsby's [Schema Customization API](/docs/schema-customization/).

## J

### [JAMStack](/docs/glossary/jamstack)

JAMStack 是指使用 [JavaScript](#javascript)，[API](#api) 和（[HTML](#html)）标记的现代 Web 体系架构。引用自 [JAMStack.org](https://jamstack.org)：“这是一种构建网站和应用程序的新方法，可提供更强的性能，更高的安全性，更低的扩展成本以及更好的开发人员体验。”

### JavaScript

一种编程语言，可以帮助我们使网页内容动态可交互。[JavaScript](https://developer.mozilla.org/en-US/docs/Web/Javascript) 是在浏览器中广泛使用的 Web 技术。它也可以与 [Node.js](#node) 在服务器端一起使用。它是 [ECMAScript](#ECMAScript) 规范的一种实现。

### JSX

JSX 是 JavaScript 的扩展，允许开发人员在同一代码段中编写 HTML 和自定义组件。[React团队推荐](https://reactjs.org/docs/introducing-jsx.html) 使用它来描述 [UI](#UI) 如何呈现。JSX 可能会让您想起模板语言，但是它具有 JavaScript 的全部功能。需要注意的重要细节：由于 JSX 使用 JavaScript，因此必须交换标记中的某些 HTML 属性，以避免使用 JavaScript 中的保留字（例如 `htmlFor` 和 `className`）。

## K

## L

### Linting

Linting 是运行程序时分析代码是否存在潜在错误的过程。Gatsby 项目使用 [prettier](https://prettier.io/) 来识别和修复常见样式问题。React 项目中常用的另一个 linter 的例子是 [eslint-jsx-plugin-a11y](https://github.com/evcohen/eslint-plugin-jsx-a11y)，它检查开发中常见的 [可访问性](#accessibility) 问题。

## M

### MDX

一种用来支持在内容中显示 [React](#react) [组件](#component) 的 [Markdown](#markdown) 扩展。

### Markdown

一种使用纯文本编写 HTML 内容的方法，使用特殊字符来表示内容类型，比如表示的 [标题](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements) 的井号、下划线强调文本的星号。

## N

### NPM

一个 [Node](#node) 的 [包](#package) 管理器。允许您安装和更新项目所依赖的其他软件包。[Gatsby](#gatsby) 和 [React](#react) 是两个项目依赖项例子。另请参见：[Yarn](#yarn)。

### Node（节点）

Gatsby 使用 [数据节点](/docs/node-interface/) 来表示一条数据。一个 [数据源](#data-source) 会创建多个数据节点。

### [Node.js](/docs/glossary/node)

一个让你在计算机上运行 [JavaScript](#javascript) 的程序。Gatsby 由 Node 驱动。

## O

## P

### Package（包）

包通常描述一个 [JavaScript](#javascript) 程序，该程序具有有关应如何分发和使用它的附加信息，例如其版本号。[NPM](#npm) 和 [Yarn](#yarn) 管理和安装您的项目使用的软件包。[Gatsby](#gatsby) 本身也是是一个包。

### Page（页面）

一个 [HTML](#html) 页面。

通常也指存在于 `/src/pages/` 的 [组件](#component)，它们通过 [Gatsby](#gatsby) 转换为页面。也可以指 `gatsby-node.js` 文件中 [动态生成的页面](/docs/creating-and-modifying-pages/#creating-pages-in-gatsby-nodejs)。

### Plugin（插件）

为 Gatsby 添加功能的没有被囊括其中的附加代码。常见的 [Gatsby 插件](/plugins/) 包括分别负责拉取和处理数据的 [数据源](#source-plugins) 和 [数据转换](#transformer) 插件。

### Production Environment（生产环境）

[部署](#deploy) 后用户将要体验到的 [构建好](#build) 的站点或应用所在的 [环境](#environment)。它可以通过在 [CLI](#cli) 中使用 `gatsby build` 或 `gatsby serve` 访问到。

### Programmatically（以编程的方式）

某些事件会根据你的代码和配置自动执行。例如，您可以 [配置](#config) 项目来为每个撰写的博客文章创建一个 [页面](#page)，或者读取并显示当前年份，在站点页脚中作为版权的一部分。

### Progressive enhancement（渐进式增强）

渐进增强是一种强调首先从服务器加载核心页面内容，然后再加载其他内容的 Web 策略，而无需加载[JavaScript](#javascript)。然后，此策略会在终端用户的浏览器或网络连接允许的情况下，在当前内容上逐步添加更复杂的表示层和功能层。Gatsby 的默认方法是提前 [构建](#build) 页面，这意味着内容将首先加载出来，然后和随着脚本的下载和执行渐渐增强。

### Public（公众）

通常是指公众成员（而不是你的团队成员），也可以是指你 [构建好](#build) 的站点或应用中的 `/public` 文件夹。

## Q

### Query（查询）

从某处请求特定数据的过程。Gatsby 中通常使用 [GraphQL](#graphql) 进行查询。

## R

### [React](/docs/glossary/react)

一个用于构建用户界面的代码库（使用[JavaScript](#javascript))编写）。它是 [Gatsby](#gatsby) 用于构建页面和组织内容的框架。

### Remark（备注）

一种用于将[Markdown](#markdown) 转换为其他格式的解析器，其他格式包括 [HTML](#html) 和 [React](#react)代码。

### Runtime（运行时）

运行时是指一个程序正在运行（或可执行）的时间。它可以指这么几件事：[Node.js](#nodejs) 时执行 JavaScript 代码的 [服务器端](#server-side) 运行时；另一方面，[客户端 JavaScript](#client-side) 是指执行传统 JavaScript 代码的浏览器运行时。Gatsby会在 [构建时](#build) 编译您的站点，并 [使用 React 运行时进行补水](#hydration) 来提供快速的，交互式的和动态的用户体验。

### Routing（路由）

路由是一种根据网络请求（通常是 URL ）在网站或应用中加载正确内容的机制。例如，它允许将 `/about-us` 之类的 URL 路由到适当的[page](#page)，[template](#template) 或 [component](#component)。

## S

### Schema（数据模式）

一种数据在系统中如何存储的准确表示方式，例如数据库中的表和字段，或 JSON 文件结构。在 Gatsby 中，GraphQL 模式表示所有可查询的数据，或作为 Gatsby 数据层的一部分的组件可以查询的数据。

### Server-side（服务器端）

[客户端-服务器关系](https://en.wikipedia.org/wiki/Client%E2%80%93server_model) 的服务器端部分是指：管理访问集中式资源或计算机网络中的服务的由计算机程序执行的操作。Gatsby 使用服务器端技术 [Node.js](#nodejs) 在构建时编译页面，而不是在[浏览器运行时](#runtime) 中使用 [客户端](#client-side) JavaScript 来提供页面 。另请参见：[frontend](#frontend) 和 [backend](#backend)。

### Source Code（源代码）

源代码是位于 `/src/` 目录中的代码，它们构成网站或应用程序的独特内容。它由 [JavaScript](#javascript) 文件组成，有时也包括 [CSS](#css) 和其他文件。

源代码在 [构建](#build) 站点时被编译，以使得 [公众](#public) 能看见。

### Source Plugin（数据源插件）

一个向 Gatsby 添加额外 [数据源](#data-source) 的插件，数据源可以之后被 [页面](#page) 和 [组件](#component) [查询](#query) 到。

### Starter

一个预先配置好的 Gatsby 项目，可以用作项目的起点。可以在 [Gatsby Starter 库](/starters/) 中找到它们，并使用 [Gatsby CLI](/docs/starters/) 安装它们。

### Static（静态）

Gatsby 构建页面的静态版本，可以轻松地被 [托管](#hosting)。这与动态系统不同，在动态系统中，每个页面都是即时生成的。保持静态状态可带来显著的性能提升，因为只有每次更改内容或代码时才需要工作。

它也可以指 `/static` 文件夹，该文件夹会自动复制到每个 [构建](#build) 后的文件里的 `/public` 中，用于不需要由 Gatsby 处理但确实需要存在于 [public](#public) 文件夹中的文件。

### [Static Site Generator](/docs/glossary/static-site-generator)

A software application that creates HTML pages from templates or [components](#component) and a given content source.

## T

### Template（模板）

一个 [以编程的方式](#programmatically) 由 Gatsby 转换为页面的 [组件](#component)。

### Theme（主题）

Gatsby 主题就像 WordPress 主题一样，可组合（与其他主题），可扩展（具有更多逻辑）和可替换（[组件遮蔽](/blog/2019-04-29-component-shadowing/)）。Gatsby 主题可以施加在使用它的 Gatsby 应用的任何方面，也可以提供任意数量的设置来打开或关闭功能。

### Transformer（数据转换插件）

一个可以将一种数据类型转换为另一种数据类型的 [插件](#plugin)。例如你可以将电子表格转换为 [JavaScript](#javascript) 数组。

## U

### UI（用户界面）

UI 是指用户界面。在人机交互领域，UI 是人机之间交互的空间。这种交互的目的是允许从人的角度对机器进行有效的操作和控制，机器同时反馈有助于用户决策过程的信息（例如错误消息或通知）。

## V

## W

### [webpack](/docs/glossary/webpack)

一个 Gatsby 用来打包网站代码的 [JavaScript](#javascript) 应用程序。它会自动在 [构建](#build) 是执行。

## X

## Y

### Yarn

一个 [包](#package) 管理器，有些人相对于 [NPM](#npm) 更喜欢它。它也是 [开发 Gatsby 应用](/contributing/setting-up-your-local-dev-environment/#using-yarn) 所需要的。

## Z
