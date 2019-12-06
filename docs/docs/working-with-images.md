---
title: 用 Gatsby 处理图片
---

优化图片对于任何站点来说一直都是一个挑战。在不同设备下都可以提升性能的最佳方法是为每一张图片提供多种不同的尺寸和分辨率。幸运的是，Gatsby 已经有很多 [插件](/docs/plugins/) 能一起在 [图片组件](/docs/building-with-components/#page-components) 中处理图片。

推荐的做法是使用 [GraphQL 查询](/docs/querying-with-graphql/) 来获取图片的最佳尺寸或者分辨率，然后用 [`gatsby-image`](/packages/gatsby-image/) 组件来展示它们。

## 通过 GraphQL 查询图片

通过 GraphQL 查询图片允许你获取图片的数据以及利用 [Sharp](https://github.com/lovell/sharp)，一个高效的图片处理库，完成数据转换。

你需要如下几个插件：

- [`gatsby-source-filesystem`](/packages/gatsby-source-filesystem/) 插件允许你 [用 GraphQL 查询文件](/docs/querying-with-graphql/#images)
- [`gatsby-plugin-sharp`](/packages/gatsby-plugin-sharp) 连接 Sharp 和 Gatsby 插件
- [`gatsby-transformer-sharp`](/packages/gatsby-transformer-sharp/) 允许你在一个查询里创建多张尺寸和分辨率都合适的图片

如果最终的图片尺寸是固定的，那么优化是通过生成多种不同的分辨率。如果图片是响应式的，也就是说它会延伸到装满整个容器或者页面，那么优化是通过生成多种不同的尺寸。查看 [Gatsby 的图片说明文档来了解更多](/packages/gatsby-image/#two-types-of-responsive-images)。

你也可以在查询里用参数来说明准确的最小和最大尺寸。查看 [Gatsby 的图片说明文档来了解所有的选项](/packages/gatsby-image/#two-types-of-responsive-images)。

下面是一个画廊例子。当页面改变时，画廊里的图片尺寸会拉伸。它使用了 `fluid` 方法和 fluid 片段来获取正确的数据，这些数据会在 `gatsby-image` 组件中使用。例子的参数被设置为最大宽度是 400px 以及最大高度是 250px。

```js
export const query = graphql`
  query {
    fileName: file(relativePath: { eq: "images/myimage.jpg" }) {
      childImageSharp {
        fluid(maxWidth: 400, maxHeight: 250) {
          ...GatsbyImageSharpFluid
        }
      }
    }
  }
`
```

## 通过 gatsby-image 优化图片

[`gatsby-image`](/packages/gatsby-image/) 是一个插件，这个插件能自动创建一些能优化图片的 React 组件：

> - 为各种设备尺寸和屏幕分辨率加载最好的图片尺寸
> - 为加载中的图片占位，这样你的页面内容就不会因为图片加载而跳动
> - 运用 “模糊化（blur-up）” 效果，例如，当整张图片在加载时，图片会先显示一小部分
> - 或者提供一张图片的 “跟踪占位（traced placeholder）” SVG
> - 延迟加载图片，这样可以减少带宽和加快初始加载的时间
> - 如果浏览器也支持 [WebP](https://developers.google.com/speed/webp/) 格式的话就可以使用这种图片

这里是一个用了之前查询例子的图片组件：

```jsx
<Img fluid={data.fileName.childImageSharp.fluid} alt="" />
```

## 通过片段把格式标准化

假如你有很多的图片，然后你想让它们都用同样的格式，那么你应该怎么做呢？

自定义片段是一个很好的方法。这个片段很容易就可以把多张图片的格式标准化然后重复使用：

```js
export const squareImage = graphql`
  fragment squareImage on File {
    childImageSharp {
      fluid(maxWidth: 200, maxHeight: 200) {
        ...GatsbyImageSharpFluid
      }
    }
  }
`
```

这个片段然后可以在图片查询中引用：

```js
export const query = graphql`
  query {
    image1: file(relativePath: { eq: "images/image1.jpg" }) {
      ...squareImage
    }

    image2: file(relativePath: { eq: "images/image2.jpg" }) {
      ...squareImage
    }

    image3: file(relativePath: { eq: "images/image3.jpg" }) {
      ...squareImage
    }
  }
`
```

### 更多资料

- [Gatsby 图片 API 文档](/docs/gatsby-image/)
- [在 Gatsby 中使用 gatsby-image](https://egghead.io/playlists/using-gatsby-image-with-gatsby-ea85129e)，egghead.io 的免费播放列表
- [gatsby-image 插件 README 文件](/packages/gatsby-image/)
- [gatsby-plugin-sharp README 文件](/packages/gatsby-plugin-sharp/)
- [gatsby-transformer-sharp README 文件](/packages/gatsby-transformer-sharp/)
