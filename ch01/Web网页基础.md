## 1. 网页的组成

网页可以分为三大部分：HTML、CSS 和 JavaScript。

**HTML（超文本标记语言）** ：用来描述网页的一种语言。

**CSS（层叠样式表）**：设置样式。

**JavaScript**：是一种脚本语言，实现了一种实时、动态、交互的页面功能。

## 2. 网页的结构

一个网页的标准形式是 html 标签内嵌套 head 和 body 标签，head 内定义网页的配置和引用，body 内定义网页的正文。

## 3. 节点树及节点间的关系

在 HTML 中，所有标签定义的内容都是节点，它们构成了一个 HTML DOM 树。

文档对象模型（DOM）：允许程序和脚本动态地访问和更新文档的内容、结构和样式。

<img src="https://gitee.com/linwang0714/ImgHosting/raw/master/article_img//20200830_01.png" style="zoom:80%;" />

通过 HTML DOM，树中的所有节点均可通过 JavaScript 访问，所有 HTML 节点元素均可被修改，也可以被创建或删除。

## 4. 选择器

| 选择器            | 例子           | 例子描述                                   |
| ----------------- | -------------- | ------------------------------------------ |
| .class            | .intro         | 选择 class="intro" 的所有节点              |
| #id               | #firstname     | 选择 id="firstname" 的所有节点             |
| element           | p              | 选择所有 p 节点                            |
| element,element   | div,p          | 选择所有 div 节点和所有 p 节点             |
| element element   | div p          | 选择 div 节点内部的所有 p 节点             |
| element>element   | div>p          | 选择父节点为 div 节点的所有 p 节点         |
| element+element   | div+p          | 选择紧接在 div 节点之后的所有 p 节点       |
| [attribute]       | [target]       | 选择带有 target 属性的所有节点             |
| [attribute=value] | [target=blank] | 选择 target="blank" 的所有节点             |
| :link             | a:link         | 选择所有未被访问的链接                     |
| :visited          | a:visited      | 选择所有已被访问的链接                     |
| :first-child      | p:first-child  | 选择属于父节点的第一个子节点的所有 p 节点  |
| :before           | p:before       | 在每个 p 节点的内容之前插入内容            |
| :after            | p:after        | 在每个 p 节点的内容之后插入内容            |
| :nth-child(n)     | p:nth-child(2) | 选择属于其父节点的第2个子节点的所有 p 节点 |

