# Gatsby 简中文档翻译风格指南

尝试翻译的小伙伴，请先阅读此文档，以保证翻译风格一致。

## 规则

### 注意事项

1. 提交前检查增加和删除的行数是否一致。
2. 遵循以下关于空格的排版规范：
   + 英文或数字前后都是中文时，前后各加一个空格。
   + 英文或数字若紧邻中文全角标点，则其与标点之间不加空格。
   + 行内代码的两端各添加一个空格。若行内代码紧邻标点符号，则其与标点之间不加空格。


### 代码块中的文案

除了注释以外尽量不要翻译代码块中的文本。 你可以选择性的翻译一些字符串，但是一定给要注意，不要将代码中的字符翻译。

例如

```js
// Example
import React from "react"
export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Hello Gatsby!</div>
)
```

✅ 正确:

```js
// 案例
import React from "react"
export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Hello Gatsby!</div>
)
```

✅ 这样也行:

```js
// 案例
import React from "react"
export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>你好 Gatsby</div>
)
```

❌ 千万不要这样:

```js
// 案例
import React from "react"
export default () => (
  // 'purple' 是 CSS 中的关键字
  <div style={{ color: `紫色`, fontSize: `72px` }}>¡Hola Gatsby!</div>
)
```

❌ 绝对不要这样:

```js
导入 "反应" 从 "反应"
导出默认（）=>（
   <div style = {{color：`purple`，fontSize：`72px`}}>你好盖茨比</ div>
）
```

### 扩展链接

指向 [MDN] 或者 [Wikipedia]的扩展链接，如果其中文版本的翻译质量不错，则可以链接到中文版本的地址，否则还是保留原来的英文链接。

[mdn]: https://developer.mozilla.org/en-US/
[wikipedia]: https://en.wikipedia.org/wiki/Main_Page

例如:

```md
React elements are [immutable](https://en.wikipedia.org/wiki/Immutable_object).
```

✅ OK:

```md
React 元素是 [不可变的](https://zh.wikipedia.org/zh-cn/%E4%B8%8D%E5%8F%AF%E8%AE%8A%E7%89%A9%E4%BB%B6)。
```

对于那些没有对应翻译内容的链接(Stack Overflow, YouTube 等)，保留英文链接即可。

## 词汇表

文档中提到下表中出现的词汇时，应该按照表中规则翻译。对于不知道如何翻译的词汇，可以在 Discord 中提出并商讨，然后更新到下列表格中。

| 词汇   | 翻译 | 注释 |
| ------ | ----------- | ----------------- |
| Plugin | 插件        | |
| Theme  | 主题        ||
| Query  | ??          ||
| Site   | 网站/站点     |二者均可|
| Starter  | ??     |暂不翻译|
| tutorial/part-x | 第 <数字> 章 | 为了与 `section` 区分，这里译为 `章` 而不是 `部分` |
