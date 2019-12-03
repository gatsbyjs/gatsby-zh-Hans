---
title: Gatsby Link API
---

对于内部导航，Gatsby 包含了一个内置的 `<Link>` 组件和一个 用于在代码中导航的 `navigate` 函数。

Gatsby 的 `<Link>` 组件可以链接到内部页面。它有着称为预加载的显著提升性能的功能。 预加载用于预获取资源，以便在用户使用此组件导航时取回资源。 当 `Link` 在视图中时，我们使用 `IntersectionObserver` 来获取低优先级的请求，然后当用户可能导航到所请求的资源时，使用 `onMouseOver` 事件来触发高优先级的请求。

该组件是 [@reach/router's Link 组件](https://reach.tech/router/api/Link) 的一个包装，它添加了针对 Gatsby 的功能加强。所有的属性都传递到 @reach/router 的 `Link` 组件中。

## 如何使用 Gatsby Link

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-why-and-how-to-use-gatsby-s-link-component"
  lessonTitle="Why and How to Use Gatsby’s Link Component"
/>

### 为本地链接将 `a` 标签替换为 `Link` 标签

如果你想在同一站点的页面之间进行链接，请使用 `Link` 组件而不是 `a` 标签。

```jsx
import React from "react"
// highlight-next-line
import { Link } from "gatsby"

const Page = () => (
  <div>
    <p>
      {/* highlight-next-line */}
      Check out my <Link to="/blog">blog</Link>!
    </p>
    <p>
      {/* Note that external links still use `a` tags. */}
      Follow me on <a href="https://twitter.com/gatsbyjs">Twitter</a>!
    </p>
  </div>
)
```

### 为当前活动链接添加自定义样式

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-add-custom-styles-for-the-active-link-using-gatsby-s-link-component"
  lessonTitle="Add Custom Styles for the Active Link Using Gatsby’s Link Component"
/>

通常，最好从视觉上更改与当前页面匹配的链接，来显示当前正在查看哪个页面。

`Link` 提供了两个向活动链接添加样式的选项：

- `activeStyle`——仅当当前项目处于活动状态时才会应用的样式对象
- `activeClassName` — 一个仅在当前项目处于活动状态时才会添加到 `Link` 的类名

如果要将活动链接变为红色，有这样两种有效的方法：

```jsx
import React from "react"
import { Link } from "gatsby"

const SiteNavigation = () => (
  <nav>
    <Link
      to="/"
      {/* highlight-start */}
      {/* This assumes the `active` class is defined in your CSS */}
      activeClassName="active"
      {/* highlight-end */}
    >
      Home
    </Link>
    <Link
      to="/about/"
      {/* highlight-next-line */}
      activeStyle={{ color: "red" }}
    >
      About
    </Link>
  </nav>
)
```

### 为高级链接样式使用 `getProps`

Gatsby 的 `<Link>` 组件带有 `getProps` 这个属性（prop），这对于高级样式很有用。 它提供了一个具有以下属性的对象：

- `isCurrent`——当 `location.pathname` 和 `<Link>` 组件的 `to` 属性相同时，值为 true
- `isPartiallyCurrent` — 当 `location.pathname` 以 `<Link>` 组件的 `to` 属性开头时，值为 true
- `href` — `to` 属性的值
- `location` — 页面的 `location` 对象

更多信息请参考 [`@reach/router` 的文档](https://reach.tech/router/api/Link)。

### 为部分匹配的链接和父链接显示活动样式

默认情况下，仅在当前 URL 与其  `to` 属性（prop）_完全_ 匹配时，才在 `<Link>` 组件上设置 `activeStyle` 和 `activeClassName` 属性。 有时，你可能希望将 `<Link>` 设置为活动样式，即使它只部分匹配当前 URL。 例如：

- 你可能想要将 `/blog/hello-world` 匹配到 `<Link to="/blog">`
- 或者将 `/gatsby-link/#passing-state-through-link-and-navigate` 匹配到 `<Link to="/gatsby-link">`

在这种情况下，只需将 `partiallyActive` 属性添加到你的 `<Link>` 组件中，即使 `to` 属性只是部分匹配，样式也会被应用：

```jsx
import React from "react"
import { Link } from "gatsby"

const Header = <>
  <Link
    to="/articles/"
    activeStyle={{ color: "red" }}
    {/* highlight-next-line */}
    partiallyActive={true}
  >
    Articles
  </Link>
</>;
```

_**注意：**此功能仅在 Gatsby V2.1.3 及以后的版本中可用。如果你遇到问题请检查版本并更新。_

### 将状态（state）作为属性（prop）传递到连接的页面中

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-include-information-about-state-in-navigation-with-gatsby-s-link-component"
  lessonTitle="Include Information About State in Navigation With Gatsby’s Link Component"
/>

有时您会想要将数据从源页面传递到连接的页面中。 您可以通过将 `state` 属性（prop）传递给  `Link` 组件或调用 `navigate` 函数来实现。 连接的页面中将有一个 `location` 属性，其中包含一个嵌套的 `state` 对象结构，该对象包含传递的数据。

```jsx
const PhotoFeedItem = ({ id }) => (
  <div>
    {/* (skip the feed item markup for brevity) */}
    <Link
      to={`/photos/${id}`}
      {/* highlight-next-line */}
      state={{ fromFeed: true }}
    >
      View Photo
    </Link>
  </div>
)

// highlight-start
const Photo = ({ location, photoId }) => {
  if (location.state.fromFeed) {
    // highlight-end
    return <FromFeedPhoto id={photoId} />
  } else {
    return <Photo id={photoId} />
  }
}
```

### 替换历史记录来改变 “后退” 按钮的行为

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-replace-navigation-history-items-with-gatsby-s-link-component"
  lessonTitle="Replace Navigation History Items with Gatsby’s Link Component"
/>

在某些情况下我们需要修改“后退”按钮的行为。 例如，如果你构建了一个页面，在页面上选择了一个东西，然后 “确定吗？” 页面出现以确保它是你真正想要的，最后看到成功确认页面。于是你可能希望在点击 “后退” 按钮后跳过 “确定吗？”页面。

这时就可以使用 `replace` 属性将历史记录中的当前 URL 替换为 `Link` 的目标。

```jsx
import React from "react"
import { Link } from "gatsby"

const AreYouSureLink = () => (
  <Link
    to="/confirmation/"
    {/* highlight-next-line */}
    replace
  >
    Yes, I’m sure
  </Link>
)
```

## 如何使用 `navigate` 辅助函数

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-navigate-to-a-new-page-programmatically-in-gatsby"
  lessonTitle="Navigate to a New Page Programmatically in Gatsby"
/>

有时，您需要以编程方式导航到页面，例如在表单提交期间。 在这种情况下，`Link` 将不起作用。

_**注意：** `navigate` 之前被命名为 `navigateTo`。 `navigateTo` 在 Gatsby v2 中不赞成使用，会在下一个重大更新时被移除。_

作为另一种方案，Gatsby 导出了一个 `navigate` 帮助函数，该函数接受 `to` 和 `options` 参数。

| 参数              |  是否必要 | 描述                                                                                            |
| ----------------- | -------- | ----------------------------------------------------------------------------------------------- |
| `to`              | 是       | 导航到的页面（例如 `/blog/`）。                                                                   |
| `options.state`   | 否       | 一个对象。传入的值会在目标页面属性的 `location.state` 中可用。                                       |
| `options.replace` | 否       | 一个布尔值。 值为 true 时在历史记录中替换当前 URL。                                                  |

默认情况下，`navigate` 的操作方式与点击 `Link` 组件的方式相同。

```jsx
import React from "react"
import { navigate } from "gatsby" // highlight-line

const Form = () => (
  <form
    onSubmit={event => {
      event.preventDefault()

      // TODO: do something with form values
      // highlight-next-line
      navigate("/form-submitted/")
    }}
  >
    {/* (skip form inputs for brevity) */}
  </form>
)
```

### 将状态添加到代码式的导航

要包含状态信息，添加一个 `options` 对象，并包含具有所需状态的 `state` 属性。

```jsx
import React from "react"
import { navigate } from "gatsby"

const Form = () => (
  <form
    onSubmit={event => {
      event.preventDefault()

      // Implementation of this function is an exercise for the reader.
      const formValues = getFormValues()

      navigate(
        "/form-submitted/",
        // highlight-start
        {
          state: { formValues },
        }
        // highlight-end
      )
    }}
  >
    {/* (skip form inputs for brevity) */}
  </form>
)
```

### 在代码式的导航中替换历史记录

如果导航行为应该替换历史记录而不是在导航历史记录中添加新条目，则将值为 `true` 的 `replace` 属性添加到 `navigate` 的 `options` 参数中。

```jsx
import React from "react"
import { navigate } from "gatsby"

const Form = () => (
  <form
    onSubmit={event => {
      event.preventDefault()

      // TODO: do something with form values
      navigate(
        "/form-submitted/",
        // highlight-next-line
        { replace: true }
      )
    }}
  >
    {/* (skip form inputs for brevity) */}
  </form>
)
```

## 使用 `withPrefix` 给路径添加路径前缀

站点通常被托管在站点的子目录中。 通过 Gatsby，您可以[设置网站的路径前缀](/docs/path-prefix/)。 之后 Gatsby 的 `<Link>` 组件将自动在开发和生产环境中构造正确的 URL。

对于你手动构建的路径名，一个辅助函数 `withPrefix` 可以帮你在生产环境中添加路径前缀（在开发环境中不需要路径前缀）。

```jsx
import { withPrefix } from "gatsby"

const IndexLayout = ({ children, location }) => {
  const isHomepage = location.pathname === withPrefix("/")

  return (
    <div>
      <h1>Welcome {isHomepage ? "home" : "aboard"}!</h1>
      {children}
    </div>
  )
}
```

## 提醒：仅将 `<Link>` 用于内部链接！

该组件 _仅_ 用于链接到 Gatsby 处理的页面。 对于链接到其他域上页面的链接，或同一域上未由 Gatsby 处理的页面的链接，请使用常规的 `<a>` 元素。

有时，您可能无法提前知道链接是否在内部，例如数据来自 CMS 的时候。 在这些情况下，您可能会发现制作一个组件来检查链接，并使用 Gatsby 的 `<Link>` 或常规的 `<a>` 标签进行渲染很有用。

由于确定链接是否为内部链接取决于站点本身，你可能需要针对您的环境自己构思新方案，但是以下内容可能是一个不错的起点：

```jsx
import { Link as GatsbyLink } from "gatsby"

// Since DOM elements <a> cannot receive activeClassName
// and partiallyActive, destructure the prop here and
// pass it only to GatsbyLink
const Link = ({ children, to, activeClassName, partiallyActive, ...other }) => {
  // Tailor the following test to your environment.
  // This example assumes that any internal link (intended for Gatsby)
  // will start with exactly one slash, and that anything else is external.
  const internal = /^\/(?!\/)/.test(to)

  // Use Gatsby Link for internal links, and <a> for others
  if (internal) {
    return (
      <GatsbyLink
        to={to}
        activeClassName={activeClassName}
        partiallyActive={partiallyActive}
        {...other}
      >
        {children}
      </GatsbyLink>
    )
  }
  return (
    <a href={to} {...other}>
      {children}
    </a>
  )
}

export default Link
```

### 文件下载

您可以类似地检查文件下载：

```jsx
  const file = /\.[0-9a-z]+$/i.test(to)

  ...

  if (internal) {
    if (file) {
        return (
          <a href={to} {...other}>
            {children}
          </a>
      )
    }
    return (
      <GatsbyLink to={to} {...other}>
        {children}
      </GatsbyLink>
    )
  }
```

## 代码式的应用内部导航推荐

带有哈希值或查询参数的 `<Link>` 和 `navigate` 都不能用于一个路由内部的导航。 如果你需要这么做，则应该使用 `<a>` 标签或导入 Gatsby 已经依赖的 `@reach/router` 包，以利用其 `navgate` 函数，如下所示：

```jsx
import { navigate } from '@reach/router';

...

onClick = () => {
  navigate('#some-link');
  // OR
  navigate('?foo=bar');
}
```

## 补充资源

- [仅在客户端中路由的身份验证教程](/tutorial/authentication-tutorial/)
