---
title: 什么是 Headless CMS 以及如何从同一个地方采集数据
overview: true
---

_Headless CMS_ 是一个内容管理软件，它允许作者创建和管理内容，以及提供结构化数据给开发者，让开发者能够将数据展示在网站或者应用前端的一个独立系统中。

一个传统的，完整的 CMS 是同时负责后端的内容管理以及提供内容给最终用户。但相比之下，一个 headless CMS 将前端分离出来，让开发者能够用最好的技术来建立优越的用户体验。

如今许多内容管理系统（CMS）都支持 “无头” 或者 “分离” 模式。这些模式都适用于 Gatsby 网站。

现在通过使用 [数据源插件](/plugins/?=source)，Gatsby 已经可以支持很多 headless CMS 方案。这样你的内容团队能够保持对于管理员界面的熟悉和便利。同时你的开发团队获得了更好的开发体验，以及因使用 Gatsby，GraphQL 和 React 来开发前端而带来的性能表现的提升。

这一节中的教程将详细介绍采集一些目前最流行的 headless CMS 数据的设置过程。

<GuideList slug={props.slug} />

<!--
  这一部分的排列顺序是根据 Gatsby 插件的下载量 & CMS 的供应商规模/采用程度。
-->

<<<<<<< HEAD
这里有更多你能连接上的 CMS 系统的教程，插件和 starter。

| CMS                                           | 教程                                                                            | 插件文档                                             | 官方 Starter                                                        |
| --------------------------------------------- | ------------------------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------- |
| [Contentful](https://www.contentful.com/)     | [教程](/docs/sourcing-from-contentful/)                                         | [文档](/packages/gatsby-source-contentful)           | [starter](/starters/contentful-userland/gatsby-contentful-starter/) |
| [NetlifyCMS](https://www.netlifycms.org/)     | [教程](/docs/sourcing-from-netlify-cms/)                                        | [文档](/packages/gatsby-plugin-netlify-cms)          | [starter](/starters/netlify-templates/gatsby-starter-netlify-cms/)  |
| [WordPress](https://www.wordpress.com/)       | [教程](/docs/sourcing-from-wordpress/)                                          | [文档](/packages/gatsby-source-wordpress)            |                                                                     |
| [Prismic](https://www.prismic.io/)            | [教程](/docs/sourcing-from-prismic/)                                            | [文档](/packages/gatsby-source-prismic)              |                                                                     |
| [Strapi](https://strapi.io/)                  | [教程](/blog/2018-1-18-strapi-and-gatsby/)                                      | [文档](/packages/gatsby-source-strapi)               |
| [DatoCMS](https://www.datocms.com/)           | [教程](https://www.gatsbyjs.com/guides/datocms/)                                | [文档](/packages/gatsby-source-datocms)              | [starter](/starters/datocms/gatsby-portfolio/)                      |
| [Sanity](https://www.sanity.io/)              | [教程](/docs/sourcing-from-sanity)                                              | [文档](/packages/gatsby-source-sanity/)              |
| [Drupal](https://www.drupal.com/)             | [教程](/docs/sourcing-from-drupal/)                                             | [文档](/packages/gatsby-source-drupal)               |                                                                     |
| [Shopify](https://www.shopify.com/)           |                                                                                 | [文档](/packages/gatsby-source-shopify)              |                                                                     |
| [CosmicJS](https://cosmicjs.com/)             | [教程](/blog/2018-06-07-build-a-gatsby-blog-using-the-cosmic-js-source-plugin/) | [文档](/packages/gatsby-source-cosmicjs)             | [starters](/starters/?s=cosmicjs&v=2)                               |
| [Contentstack](https://www.contentstack.com/) | [教程](/docs/sourcing-from-contentstack)                                        | [文档](/packages/gatsby-source-contentstack)         | [starter](/starters/contentstack/gatsby-starter-contentstack/)      |
| [ButterCMS](https://buttercms.com/)           | [教程](/docs/sourcing-from-buttercms/)                                          | [文档](/packages/gatsby-source-buttercms)            | [starter](/starters/ButterCMS/gatsby-starter-buttercms/)            |
| [Ghost](https://ghost.org/)                   | [教程](/docs/sourcing-from-ghost/)                                              | [文档](/packages/gatsby-source-ghost/)               | [starter](/starters/TryGhost/gatsby-starter-ghost/)                 |
| [Kentico Cloud](https://kenticocloud.com/)    | [教程](/docs/sourcing-from-kentico-cloud)                                       | [文档](/packages/gatsby-source-kentico-cloud)        | [starter](/starters/Kentico/gatsby-starter-kentico-cloud/)          |
| [Directus](https://directus.io/)              |                                                                                 | [文档](/packages/gatsby-source-directus)             |
| [GraphCMS](https://graphcms.com/)             | [教程](/docs/sourcing-from-graphcms)                                            | [文档](/packages/gatsby-source-graphql)              | [starter](/starters/GraphCMS/gatsby-graphcms-tailwindcss-example/)  |
| [Storyblok](https://www.storyblok.com/)       |                                                                                 | [文档](/packages/gatsby-source-storyblok)            |
| [Cockpit](https://getcockpit.com/)            |                                                                                 | [文档](/packages/gatsby-plugin-cockpit)              |
| [CraftCMS](https://craftcms.com/)             |                                                                                 | [文档](/packages/gatsby-source-craftcms)             |
| [AgilityCMS](https://agilitycms.com/)         | [教程](/docs/sourcing-from-agilitycms/)                                         | [文档](/packages/@agility/gatsby-source-agilitycms/) | [starter](/starters/agility/agility-gatsby-starter/)                |
| [Gentics Mesh](https://getmesh.io)            | [教程](/docs/sourcing-from-gentics-mesh)                                        |                                                      |                                                                     |

## 如何添加新的教程到这一章节中

如果你在列表中没有看到你喜爱的 CMS，你可以 [自己写一份新的教程](/contributing/how-to-contribute/) 或者 [提交一个 issue 来获取它](https://github.com/gatsbyjs/gatsby/issues/new/choose).

你也可以 [写自己的数据源插件](/docs/creating-a-source-plugin/) 来结合列表中没有的 CMS 到 Gatsby。
=======
Here are more resources for guides, plugins, and starters for CMS systems you can connect to:

| CMS                                           | Guides                                                                           | Plugin Docs                                          | Official Starter                                                    |
| --------------------------------------------- | -------------------------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------- |
| [Contentful](https://www.contentful.com/)     | [guide](/docs/sourcing-from-contentful/)                                         | [docs](/packages/gatsby-source-contentful)           | [starter](/starters/contentful-userland/gatsby-contentful-starter/) |
| [NetlifyCMS](https://www.netlifycms.org/)     | [guide](/docs/sourcing-from-netlify-cms/)                                        | [docs](/packages/gatsby-plugin-netlify-cms)          | [starter](/starters/netlify-templates/gatsby-starter-netlify-cms/)  |
| [WordPress](https://www.wordpress.com/)       | [guide](/docs/sourcing-from-wordpress/)                                          | [docs](/packages/gatsby-source-wordpress)            |                                                                     |
| [Prismic](https://www.prismic.io/)            | [guide](/docs/sourcing-from-prismic/)                                            | [docs](/packages/gatsby-source-prismic)              |                                                                     |
| [Strapi](https://strapi.io/)                  | [guide](/blog/2018-1-18-strapi-and-gatsby/)                                      | [docs](/packages/gatsby-source-strapi)               |
| [DatoCMS](https://www.datocms.com/)           | [guide](https://www.gatsbyjs.com/guides/datocms/)                                | [docs](/packages/gatsby-source-datocms)              | [starter](/starters/datocms/gatsby-portfolio/)                      |
| [Sanity](https://www.sanity.io/)              | [guide](/docs/sourcing-from-sanity)                                              | [docs](/packages/gatsby-source-sanity/)              |
| [Drupal](https://www.drupal.com/)             | [guide](/docs/sourcing-from-drupal/)                                             | [docs](/packages/gatsby-source-drupal)               |                                                                     |
| [Shopify](https://www.shopify.com/)           |                                                                                  | [docs](/packages/gatsby-source-shopify)              |                                                                     |
| [CosmicJS](https://cosmicjs.com/)             | [guide](/blog/2018-06-07-build-a-gatsby-blog-using-the-cosmic-js-source-plugin/) | [docs](/packages/gatsby-source-cosmicjs)             | [starters](/starters/?s=cosmicjs&v=2)                               |
| [Contentstack](https://www.contentstack.com/) | [guide](/docs/sourcing-from-contentstack)                                        | [docs](/packages/gatsby-source-contentstack)         | [starter](/starters/contentstack/gatsby-starter-contentstack/)      |
| [ButterCMS](https://buttercms.com/)           | [guide](/docs/sourcing-from-buttercms/)                                          | [docs](/packages/gatsby-source-buttercms)            | [starter](/starters/ButterCMS/gatsby-starter-buttercms/)            |
| [Ghost](https://ghost.org/)                   | [guide](/docs/sourcing-from-ghost/)                                              | [docs](/packages/gatsby-source-ghost/)               | [starter](/starters/TryGhost/gatsby-starter-ghost/)                 |
| [Kentico Kontent](https://kontent.ai/)        | [guide](/docs/sourcing-from-kentico-kontent)                                     | [docs](/packages/@kentico/gatsby-source-kontent)     | [starter](/starters/Kentico/gatsby-starter-kontent/)                |
| [Directus](https://directus.io/)              |                                                                                  | [docs](/packages/gatsby-source-directus)             |
| [GraphCMS](https://graphcms.com/)             | [guide](/docs/sourcing-from-graphcms)                                            | [docs](/packages/gatsby-source-graphql)              | [starter](/starters/GraphCMS/gatsby-graphcms-tailwindcss-example/)  |
| [Storyblok](https://www.storyblok.com/)       |                                                                                  | [docs](/packages/gatsby-source-storyblok)            |
| [Cockpit](https://getcockpit.com/)            |                                                                                  | [docs](/packages/gatsby-plugin-cockpit)              |
| [CraftCMS](https://craftcms.com/)             |                                                                                  | [docs](/packages/gatsby-source-craftcms)             |
| [AgilityCMS](https://agilitycms.com/)         | [guide](/docs/sourcing-from-agilitycms/)                                         | [docs](/packages/@agility/gatsby-source-agilitycms/) | [starter](/starters/agility/agility-gatsby-starter/)                |
| [Forestry](https://forestry.io/)              | [guide](/docs/sourcing-from-forestry/)                                           |                                                      |                                                                     |
| [Gentics Mesh](https://getmesh.io)            | [guide](/docs/sourcing-from-gentics-mesh)                                        |                                                      |                                                                     |
| [Seams-CMS](https://seams-cms.com/)           | [guide](/docs/sourcing-from-seams-cms)                                           |                                                      |                                                                     |

## How to add new guides to this section

If you don’t see your preferred CMS in this list, you can [write a new guide yourself](/contributing/how-to-contribute/) or [open an issue to request it](https://github.com/gatsbyjs/gatsby/issues/new/choose).

You can also [write your own source plugin](/docs/creating-a-source-plugin/) to integrate Gatsby with a CMS that is not in the list.
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc
