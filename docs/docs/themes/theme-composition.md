---
title: 主题结构
---

当你构建主题时，如何与其它主题组合是值得思考的事情。
在某些情况下， 你可能希望将主题分解为模块化的部分，就像 `gatsby-theme-blog` 和 `gatsby-theme-ecommerce`。

但是在构建主题的早期阶段，我们推荐你尽可能构建完整的主题（将功能集合在一个主题内）。 这样他人可以快速地开始，使用起来也比较方便。一段时间后，如果你想将主题拆分为独立的子主题，其实也不会花太多功夫。

## 布局

在 Gatsby 主题中你可以通过 [`wrapRootElement`](/docs/browser-apis/#wrapRootElement)
或者 [`wrapPageElement`](/docs/browser-apis/#wrapPageElement) 使用全局布局。 为了主题能更好的与其它主题组合，我们不建议你使用这些 API。 这两个 API 适合设置一些必要的
[React 上下文](https://reactjs.org/docs/context.html) 配置。通常情况下，你不应该使用任何布局组件，包括页眉或者页脚。因为布局组件将会在全局范围内应用，这可能和其它主题造成冲突。

[阅读更多关于 Gatsby 中布局和主题相关的知识](https://www.christopherbiscardi.com/post/layouts-in-gatsby-themes)
