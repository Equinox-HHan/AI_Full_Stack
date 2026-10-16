# HTML5 Syntax & Semantic Guide | HTML5 完整语法与语义化规范指南

> 从标签基础到语义化布局、表单校验与多媒体交互，覆盖现代 Web 页面骨架核心标准。

---

## 目录

1. [什么是 HTML](#1-什么是-html)
2. [HTML 文档基本结构](#2-html-文档基本结构)
3. [HTML 语法规则](#3-html-语法规则)
4. [全局属性](#4-全局属性)
5. [文档结构元素](#5-文档结构元素)
6. [文本内容元素](#6-文本内容元素)
7. [列表元素](#7-列表元素)
8. [链接与导航](#8-链接与导航)
9. [图片与多媒体](#9-图片与多媒体)
10. [表格元素](#10-表格元素)
11. [表单元素](#11-表单元素)
12. [语义化容器元素](#12-语义化容器元素)
13. [脚本与样式](#13-脚本与样式)
14. [交互与可折叠元素](#14-交互与可折叠元素)
15. [HTML 实体字符](#15-html-实体字符)

---

## 1. 什么是 HTML

**HTML**（HyperText Markup Language，超文本标记语言）是构建网页的标准语言。它用**标签（Tag）**来"标记"内容，告诉浏览器这段内容是什么——是标题、段落、图片还是链接。

**核心概念：**
- HTML 不是编程语言，是**标记语言**——它描述内容的结构和含义，不包含逻辑
- HTML 文件的后缀名通常是 `.html` 或 `.htm`
- 浏览器读取 HTML 文件后，将其渲染成可视化的网页

---

## 2. HTML 文档基本结构

每个 HTML 文件都遵循一个标准骨架：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>页面标题</title>
</head>
<body>
    <!-- 页面可见内容写在这里 -->
    <h1>你好，世界！</h1>
    <p>这是我的第一个网页。</p>
</body>
</html>
```

### 逐行解释

| 代码 | 含义 |
|------|------|
| `<!DOCTYPE html>` | 文档声明，告诉浏览器"这是一个 HTML5 文档"。必须写在第一行。 |
| `<html lang="zh-CN">` | 根元素，包裹整个页面。`lang` 属性指定页面语言。 |
| `<head>` | 头部区域，存放页面的**元信息**（标题、编码、样式等），不直接显示。 |
| `<meta charset="UTF-8">` | 设置字符编码为 UTF-8，确保中文正常显示。 |
| `<meta name="viewport" ...>` | 视口设置，让页面在手机上也能正常缩放和显示。 |
| `<title>` | 浏览器标签页上显示的标题。 |
| `<body>` | 主体区域，所有**可见内容**都写在这里。 |
| `<!-- ... -->` | 注释，不会被浏览器显示。 |

---

## 3. HTML 语法规则

### 3.1 标签的基本形式

```html
<标签名 属性="值">内容</标签名>
```

- **开始标签**：`<标签名>`
- **结束标签**：`</标签名>`
- **自闭合标签**：某些标签没有内容和结束标签，如 `<img>`、`<br>`、`<hr>`、`<input>`

### 3.2 属性的写法

属性写在开始标签的标签名后面，格式为 `属性名="属性值"`：

```html
<a href="https://example.com" target="_blank">链接文字</a>
```

- 属性值建议用双引号 `""` 包裹（单引号 `''` 也可以，但双引号是惯例）
- 多个属性之间用空格分隔
- 布尔属性可以只写属性名，如 `<input disabled>` 等价于 `<input disabled="disabled">`

### 3.3 嵌套规则

- 标签可以嵌套，但必须**正确闭合**
- 错误：`<p><strong>文字</p></strong>`
- 正确：`<p><strong>文字</strong></p>`
- **块级元素**（`<div>`、`<p>`、`<h1>` 等）可以包含内联元素，但**内联元素不应包含块级元素**

### 3.4 大小写

HTML 标签和属性**不区分大小写**（`<P>` 和 `<p>` 一样），但**推荐全部小写**。

---

## 4. 全局属性

全局属性是**几乎所有 HTML 元素都可以使用**的属性。

### 4.1 `id` — 唯一标识

```html
<h1 id="main-title">标题</h1>
```

- 给元素一个**页面内唯一**的标识名
- 同一个 `id` 值在一个页面中只能出现一次
- 作用：CSS 选择器（`#main-title`）、JavaScript 定位（`document.getElementById`）、锚点跳转

### 4.2 `class` — 类名

```html
<p class="intro highlight">段落文字</p>
```

- 给元素分类，**同一个 class 可以用在多个元素上**
- 一个元素可以有多个 class，用空格分隔（上面的例子有两个类：`intro` 和 `highlight`）
- 作用：CSS 选择器（`.intro`）、JavaScript 批量操作

### 4.3 `style` — 行内样式

```html
<p style="color: red; font-size: 16px;">红色文字</p>
```

- 直接在元素上写 CSS 样式
- **缺点**：样式和结构混在一起，难以维护。推荐优先使用外部 CSS

### 4.4 `title` — 提示文本

```html
<abbr title="HyperText Markup Language">HTML</abbr>
```

- 鼠标悬停时会显示提示文字（tooltip）
- 常用于缩写解释、图片补充说明

### 4.5 `lang` — 语言

```html
<p lang="en">Hello World</p>
<p lang="zh-CN">你好世界</p>
```

- 指定元素内容的语言
- 帮助屏幕阅读器选择正确的发音规则

### 4.6 `dir` — 文字方向

```html
<p dir="ltr">从左到右</p>
<p dir="rtl">من اليمين إلى اليسار</p>
```

- `ltr`：从左到右（默认），`rtl`：从右到左（如阿拉伯语）
- `auto`：让浏览器自动判断

### 4.7 `hidden` — 隐藏元素

```html
<div hidden>这段内容不会显示</div>
```

- 布尔属性，有此属性则该元素不显示
- 等价于 CSS 的 `display: none`

### 4.8 `tabindex` — Tab 键导航顺序

```html
<button tabindex="1">第一个</button>
<button tabindex="2">第二个</button>
```

- `0`：按 DOM 顺序参与 Tab 导航（默认）
- `-1`：不参与 Tab 导航，但可用 JavaScript 聚焦
- 正数：按数值从小到大排序（不推荐使用正数值，维护困难）

### 4.9 `contenteditable` — 内容可编辑

```html
<div contenteditable="true">这段文字可以直接点击编辑</div>
```

- `true`：内容可编辑
- `false`：不可编辑（默认）
- `plaintext-only`：只能输入纯文本（不支持富文本）

### 4.10 `draggable` — 可拖拽

```html
<div draggable="true">可以拖拽的元素</div>
```

- `true`：可拖拽
- `false`：不可拖拽（默认）
- `auto`：浏览器默认行为（如 `<img>` 默认可拖拽）

### 4.11 `data-*` — 自定义数据属性

```html
<div data-user-id="123" data-role="admin">用户信息</div>
```

- `*` 可以替换为任意自定义名称
- 用于在 HTML 中存储自定义数据，JavaScript 可通过 `element.dataset` 访问
- 名称规则：`data-user-id` 在 JS 中变为 `dataset.userId`（短横线转驼峰）

### 4.12 `accesskey` — 快捷键

```html
<button accesskey="s">保存 (Alt+S)</button>
```

- 为元素指定一个键盘快捷键
- 不同系统触发方式不同：Windows 用 `Alt + 键`，Mac 用 `Ctrl + Option + 键`

---

## 5. 文档结构元素

### 5.1 `<!DOCTYPE html>` — 文档类型声明

- 必须是 HTML 文件的**第一行**
- 告诉浏览器使用**标准模式**渲染
- HTML5 的声明非常简单，不再像以前那样需要写一长串 DTD

### 5.2 `<html>` — 根元素

```html
<html lang="zh-CN">
```

| 常用属性 | 说明 |
|----------|------|
| `lang` | 页面主语言 |
| `dir` | 文字默认方向 |
| `manifest` | (已废弃) 离线缓存清单 |

### 5.3 `<head>` — 头部容器

`<head>` 中包含的元素都不会直接显示在页面上，它们是"幕后工作者"。

### 5.4 `<title>` — 页面标题

```html
<title>我的网站 - 首页</title>
```

- 显示在浏览器标签页上
- 被收藏网页时的默认名称
- 搜索引擎结果中显示的标题
- **每个页面必须有一个** `<title>`

### 5.5 `<meta>` — 元数据

`<meta>` 是自闭合标签，提供页面相关的元信息。

#### 字符编码
```html
<meta charset="UTF-8">
```

#### 视口设置（响应式必备）
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
- `width=device-width`：页面宽度等于设备宽度
- `initial-scale=1.0`：初始缩放比例为 1

#### SEO 相关
```html
<meta name="description" content="网站描述，搜索引擎会显示这段文字">
<meta name="keywords" content="HTML, 教程, 前端">
<meta name="author" content="作者名">
<meta name="robots" content="index, follow">
```
- `robots` 值说明：`index`（允许收录）`/ noindex`（禁止收录），`follow`（允许跟踪链接）`/ nofollow`（禁止跟踪）

#### 社交媒体分享（Open Graph）
```html
<meta property="og:title" content="分享标题">
<meta property="og:description" content="分享描述">
<meta property="og:image" content="https://example.com/image.jpg">
<meta property="og:url" content="https://example.com/page">
```

#### HTTP 等效指令
```html
<meta http-equiv="refresh" content="5; url=https://example.com">
<!-- 5秒后跳转到指定URL -->

<meta http-equiv="X-UA-Compatible" content="IE=edge">
<!-- 让IE浏览器使用最高版本渲染 -->
```

### 5.6 `<link>` — 外部资源链接

自闭合标签，常用于链接 CSS、网站图标等。

```html
<!-- 链接外部样式表（最常用） -->
<link rel="stylesheet" href="style.css">

<!-- 网站图标（favicon） -->
<link rel="icon" href="favicon.ico" type="image/x-icon">

<!-- 预加载资源 -->
<link rel="preload" href="font.woff2" as="font" type="font/woff2" crossorigin="anonymous">

<!-- 响应式CSS -->
<link rel="stylesheet" href="mobile.css" media="screen and (max-width: 600px)">
```

| 属性 | 说明 |
|------|------|
| `rel` | 关系类型：`stylesheet`、`icon`、`preload`、`prefetch`、`canonical` 等 |
| `href` | 资源路径 |
| `media` | 媒体查询，满足条件时才加载 |
| `type` | MIME 类型 |
| `sizes` | 图标尺寸，如 `16x16`、`32x32` |

### 5.7 `<base>` — 基准 URL

```html
<base href="https://example.com/" target="_blank">
```

- 必须写在 `<head>` 中，且**最多出现一次**
- `href`：所有相对路径的基础 URL
- `target`：所有链接的默认打开方式

### 5.8 `<body>` — 页面主体

```html
<body>
    <!-- 所有可见内容 -->
</body>
```

| 常用事件属性（可用 JS 替代） | 说明 |
|------------------------------|------|
| `onload` | 页面加载完成后触发 |
| `onunload` | 页面卸载时触发 |
| `onscroll` | 滚动时触发 |
| `onresize` | 窗口大小改变时触发 |

---

## 6. 文本内容元素

### 6.1 标题 `<h1>` ～ `<h6>`

```html
<h1>一级标题（最大，每个页面建议只有一个）</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<h4>四级标题</h4>
<h5>五级标题</h5>
<h6>六级标题（最小）</h6>
```

- 数字越小，字号越大，层级越高
- **语义化原则**：`<h1>` 是整个页面的主标题，`<h2>` 是章节标题，`<h3>` 是小节标题...
- 不要为了调整字号而跳级使用（字号由 CSS 控制）

### 6.2 `<p>` — 段落

```html
<p>这是一个段落。浏览器会自动在段落前后添加间距。</p>
```

- 块级元素，独占一行
- 多个空格和换行会被合并为一个空格（空白折叠）
- 不能嵌套另一个块级元素（如不能放 `<div>`）

### 6.3 `<br>` — 换行

```html
<p>第一行<br>第二行</p>
```

- 自闭合标签，在文本中强制换行
- **不要用多个 `<br>` 来制造间距**，间距应该用 CSS `margin` 控制

### 6.4 `<hr>` — 水平分隔线

```html
<p>上面一节</p>
<hr>
<p>下面一节</p>
```

- 自闭合标签，画一条水平线表示主题转换
- 语义含义是"话题的改变"，不仅仅是装饰线

### 6.5 `<pre>` — 预格式化文本

```html
<pre>
    空格  和
换行
    都会被保留
</pre>
```

- 文本按原样显示——**空格、换行、缩进全部保留**
- 默认使用等宽字体

### 6.6 `<blockquote>` — 块级引用

```html
<blockquote cite="https://source.com">
    <p>这是一个较长的引用内容。</p>
</blockquote>
```

| 属性 | 说明 |
|------|------|
| `cite` | 引用的来源 URL |

### 6.7 `<q>` — 行内短引用

```html
<p>他说：<q cite="https://source.com">学而不思则罔。</q></p>
```

- 浏览器通常会自动加引号
- 和 `<blockquote>` 的区别：`<q>` 是行内元素，用于短引用

### 6.8 文本格式标签

#### 语义强调

```html
<strong>重要内容（粗体，有语义强调）</strong>
<em>强调内容（斜体，有语义强调）</em>
<mark>高亮标记</mark>
```

#### 纯视觉样式（无语义，优先用 CSS 替代）

```html
<b>粗体文字（无额外语义）</b>
<i>斜体文字（无额外语义，常用于图标字体）</i>
<u>下划线文字</u>
<s>删除线文字（表示不再准确）</s>
<small>小号文字（如版权声明）</small>
```

#### 上下标与编辑标记

```html
<p>水分子：H<sub>2</sub>O —— 下标</p>
<p>面积：100m<sup>2</sup> —— 上标</p>
<p><del>被删除的内容</del> <ins>新插入的内容</ins></p>
```

| 标签 | 语义 | 默认样式 |
|------|------|----------|
| `<strong>` | 非常重要 | 粗体 |
| `<em>` | 强调重读 | 斜体 |
| `<mark>` | 高亮/标记 | 黄色背景 |
| `<b>` | 无特殊语义 | 粗体 |
| `<i>` | 无特殊语义 | 斜体 |
| `<u>` | 无特殊语义 | 下划线 |
| `<s>` | 不再准确/过时 | 删除线 |
| `<small>` | 附属细则 | 小字号 |
| `<sub>` | 下标 | 低于基线 |
| `<sup>` | 上标 | 高于基线 |
| `<del>` | 已删除 | 删除线 |
| `<ins>` | 新增 | 下划线 |

### 6.9 `<code>` / `<kbd>` / `<samp>` / `<var>` — 计算机相关

```html
<p>使用 <code>console.log()</code> 输出信息 —— 代码</p>
<p>按 <kbd>Ctrl</kbd> + <kbd>C</kbd> 复制 —— 键盘输入</p>
<p>程序输出：<samp>Hello World</samp> —— 样本输出</p>
<p>变量 <var>x</var> 的值为 5 —— 变量名</p>
```

### 6.10 `<abbr>` — 缩写

```html
<p><abbr title="HyperText Markup Language">HTML</abbr> 是网页的基础。</p>
```

| 属性 | 说明 |
|------|------|
| `title` | 缩写的全称，鼠标悬停显示 |

### 6.11 `<address>` — 联系信息

```html
<address>
    作者：张三<br>
    邮箱：<a href="mailto:zhangsan@example.com">zhangsan@example.com</a><br>
    地址：北京市朝阳区
</address>
```

- 通常用于页面或文章作者的联系方式
- 默认斜体显示

### 6.12 `<time>` — 时间/日期

```html
<time datetime="2025-01-15">2025年1月15日</time>
<time datetime="2025-01-15T14:30:00">1月15日下午2:30</time>
```

| 属性 | 说明 |
|------|------|
| `datetime` | 机器可读的日期时间格式（`YYYY-MM-DD` 或 `YYYY-MM-DDTHH:MM:SS`） |

### 6.13 `<span>` — 通用行内容器

```html
<p>这是一段文字，<span class="highlight">这部分需要特殊样式</span>。</p>
```

- 本身无任何默认样式和语义
- 用于"圈定一段行内文字"以便 CSS 或 JS 操作

### 6.14 `<ruby>` — 注音/拼音

```html
<ruby>
    漢字<rp>(</rp><rt>かんじ</rt><rp>)</rp>
</ruby>
```

- `<rt>`：注音文本（ruby text）
- `<rp>`：当浏览器不支持注音时显示的括号（ruby parentheses）

---

## 7. 列表元素

### 7.1 `<ul>` — 无序列表

```html
<ul>
    <li>苹果</li>
    <li>香蕉</li>
    <li>橙子</li>
</ul>
```

- 列表项前面默认显示圆点（bullet）
- 列表项 `<li>` 必须是 `<ul>` 或 `<ol>` 的直接子元素

### 7.2 `<ol>` — 有序列表

```html
<ol>
    <li>第一步：打开电脑</li>
    <li>第二步：启动浏览器</li>
    <li>第三步：输入网址</li>
</ol>
```

| 属性 | 说明 |
|------|------|
| `start` | 起始编号，如 `start="5"` 从 5 开始编号 |
| `reversed` | 布尔属性，倒序编号 |
| `type` | 编号样式：`1`（数字）、`a`（小写字母）、`A`（大写字母）、`i`（小写罗马数字）、`I`（大写罗马数字） |

### 7.3 `<li>` — 列表项

```html
<li value="10">从10开始的列表项</li>
```

| 属性 | 说明 |
|------|------|
| `value` | 仅在 `<ol>` 中使用，指定该列表项的编号（后续项顺延） |

### 7.4 `<dl>` / `<dt>` / `<dd>` — 描述列表

```html
<dl>
    <dt>HTML</dt>
    <dd>超文本标记语言，用于构建网页结构。</dd>

    <dt>CSS</dt>
    <dd>层叠样式表，用于控制网页外观。</dd>

    <dt>JavaScript</dt>
    <dd>脚本语言，用于实现网页交互。</dd>
</dl>
```

- `<dl>`：description list，描述列表容器
- `<dt>`：description term，被描述的术语
- `<dd>`：description details，术语的解释/定义
- 一个 `<dt>` 可以有多个 `<dd>`

---

## 8. 链接与导航

### 8.1 `<a>` — 超链接

```html
<a href="https://example.com">点击访问</a>
```

| 属性 | 说明 | 示例 |
|------|------|------|
| `href` | **目标 URL**，最核心的属性 | `href="https://example.com"` |
| `target` | 打开方式 | `_self`（当前窗口，默认）、`_blank`（新窗口）、`_parent`、`_top` |
| `rel` | 链接关系 | `nofollow`、`noopener`、`noreferrer` |
| `download` | 下载文件而非打开 | `download="文件名.pdf"` |
| `hreflang` | 目标页面的语言 | `hreflang="en"` |
| `type` | 目标资源的 MIME 类型 | `type="application/pdf"` |

#### 链接的几种写法

```html
<!-- 外部链接 -->
<a href="https://www.baidu.com">百度</a>

<!-- 相对路径链接 -->
<a href="about.html">关于页面</a>
<a href="../index.html">上级目录</a>

<!-- 页面内锚点跳转 -->
<a href="#section2">跳转到第2节</a>
<h2 id="section2">第2节</h2>

<!-- 跳转到页面顶部 -->
<a href="#">回到顶部</a>

<!-- 发送邮件 -->
<a href="mailto:example@example.com">发送邮件</a>

<!-- 拨打电话 -->
<a href="tel:+8613800138000">打电话</a>

<!-- 发送短信 -->
<a href="sms:+8613800138000">发短信</a>
```

#### `target="_blank"` 的安全问题

```html
<!-- 不安全的写法 -->
<a href="https://example.com" target="_blank">链接</a>

<!-- 安全的写法（防止新页面通过 window.opener 操控原页面） -->
<a href="https://example.com" target="_blank" rel="noopener noreferrer">安全链接</a>
```

| `rel` 值 | 含义 |
|-----------|------|
| `noopener` | 新页面无法通过 `window.opener` 访问原页面 |
| `noreferrer` | 不发送 HTTP Referer 头 |
| `nofollow` | 告诉搜索引擎不要追踪此链接 |
| `noopener noreferrer` | 两者组合，推荐跟 `target="_blank"` 一起使用 |

### 8.2 `<nav>` — 导航区域

```html
<nav>
    <a href="/">首页</a>
    <a href="/about">关于</a>
    <a href="/contact">联系我们</a>
</nav>
```

- 语义化标签，表示"导航链接集合"
- 页面主要导航（如顶部菜单栏、侧边栏目录）才用 `<nav>`，页脚中的版权链接通常不需要

---

## 9. 图片与多媒体

### 9.1 `<img>` — 图片

```html
<img src="photo.jpg" alt="一张风景照片">
```

| 属性 | 必须？ | 说明 |
|------|--------|------|
| `src` | **必须** | 图片的路径（URL） |
| `alt` | **强烈推荐** | 替代文本。图片无法显示时显示这段文字；屏幕阅读器会朗读它 |
| `width` | 否 | 图片宽度（像素或百分比） |
| `height` | 否 | 图片高度（像素或百分比） |
| `loading` | 否 | `lazy`（懒加载，图片滚到视口时才加载）/ `eager`（立即加载，默认） |
| `srcset` | 否 | 响应式图片，提供多个不同尺寸的图片源 |
| `sizes` | 否 | 配合 `srcset` 使用，告诉浏览器不同视口下图片显示多大 |
| `decoding` | 否 | `sync` / `async`（异步解码，不阻塞页面渲染）/ `auto`（默认） |
| `crossorigin` | 否 | 跨域设置，配合 Canvas 使用时需要 |

#### 响应式图片

```html
<img 
    src="photo-800.jpg"
    srcset="photo-400.jpg 400w, photo-800.jpg 800w, photo-1200.jpg 1200w"
    sizes="(max-width: 600px) 400px, (max-width: 1000px) 800px, 1200px"
    alt="响应式图片示例"
>
```

- `srcset`：列出不同宽度的图片，`400w` 表示该图片宽度为 400 像素
- `sizes`：告诉浏览器在不同屏幕宽度下图片的显示尺寸
- `src`：作为 `srcset` 不支持时的后备方案

### 9.2 `<picture>` — 响应式图片容器

```html
<picture>
    <!-- 屏幕宽度 >= 800px 时使用 -->
    <source srcset="large.jpg" media="(min-width: 800px)">
    <!-- 屏幕宽度 >= 400px 时使用 -->
    <source srcset="medium.jpg" media="(min-width: 400px)">
    <!-- 默认使用小图 -->
    <img src="small.jpg" alt="描述文字">
</picture>
```

- 根据媒体查询条件选择不同的图片
- 也支持根据不同格式加载（如优先 WebP，不支持则用 JPEG）

```html
<picture>
    <source srcset="photo.webp" type="image/webp">
    <source srcset="photo.jpg" type="image/jpeg">
    <img src="photo.jpg" alt="描述文字">
</picture>
```

### 9.3 `<figure>` / `<figcaption>` — 图文组合

```html
<figure>
    <img src="chart.png" alt="销售数据图表">
    <figcaption>图1：2024年第四季度销售数据</figcaption>
</figure>
```

- 用于包含图片、图表、代码片段等"独立内容"并附上说明
- `<figcaption>` 是图片/内容的说明文字（标题），必须是 `<figure>` 的子元素

### 9.4 `<audio>` — 音频

```html
<audio controls>
    <source src="music.mp3" type="audio/mpeg">
    <source src="music.ogg" type="audio/ogg">
    您的浏览器不支持 audio 标签。
</audio>
```

| 属性 | 说明 |
|------|------|
| `src` | 音频文件路径 |
| `controls` | 布尔属性，显示播放控件（播放/暂停/音量等） |
| `autoplay` | 自动播放（大多数浏览器会阻止，除非设为静音） |
| `muted` | 布尔属性，静音 |
| `loop` | 布尔属性，循环播放 |
| `preload` | `none`（不预加载）/ `metadata`（只预加载元数据）/ `auto`（预加载，默认） |

### 9.5 `<video>` — 视频

```html
<video controls width="640" height="360" poster="cover.jpg">
    <source src="movie.mp4" type="video/mp4">
    <source src="movie.webm" type="video/webm">
    您的浏览器不支持 video 标签。
</video>
```

| 属性 | 说明 |
|------|------|
| `src` | 视频文件路径 |
| `controls` | 显示播放控件 |
| `autoplay` | 自动播放（通常需要同时设置 `muted`） |
| `muted` | 静音 |
| `loop` | 循环播放 |
| `poster` | 视频加载前显示的封面图片 |
| `width` / `height` | 视频宽高 |
| `playsinline` | 在 iOS 设备上不自动全屏播放 |
| `preload` | `none` / `metadata` / `auto` |

### 9.6 `<source>` — 媒体资源

- 自闭合标签，必须用在 `<picture>`、`<audio>` 或 `<video>` 中
- 浏览器会从上到下找到第一个支持的格式并使用

| 属性 | 说明 |
|------|------|
| `src` | 媒体文件路径 |
| `type` | MIME 类型（如 `video/mp4`、`audio/ogg`） |
| `media` | 媒体查询条件（仅在 `<picture>` 中使用） |
| `srcset` | 响应式图片源（仅在 `<picture>` 中使用） |

### 9.7 `<track>` — 字幕/轨道路

```html
<video controls>
    <source src="movie.mp4" type="video/mp4">
    <track src="subtitles-zh.vtt" kind="subtitles" srclang="zh" label="中文字幕" default>
    <track src="subtitles-en.vtt" kind="subtitles" srclang="en" label="English">
</video>
```

| 属性 | 说明 |
|------|------|
| `src` | 字幕文件路径（.vtt 格式） |
| `kind` | 轨道类型：`subtitles`（字幕）、`captions`（说明字幕，含音效描述）、`descriptions`（视频描述）、`chapters`（章节）、`metadata`（元数据） |
| `srclang` | 轨道语言（如 `zh`、`en`） |
| `label` | 显示给用户的名称 |
| `default` | 布尔属性，设为默认轨道 |

### 9.8 `<map>` / `<area>` — 图片热区

```html
<img src="china-map.jpg" alt="中国地图" usemap="#china">

<map name="china">
    <area shape="rect" coords="0,0,100,100" href="beijing.html" alt="北京" title="北京市">
    <area shape="circle" coords="200,150,50" href="shanghai.html" alt="上海" title="上海市">
    <area shape="poly" coords="300,100,350,80,400,120,380,180,320,160" href="guangdong.html" alt="广东">
</map>
```

- `<map>` 的 `name` 属性对应 `<img>` 的 `usemap`（要加 `#`）
- `<area>` 的 `shape`：`rect`（矩形）、`circle`（圆形）、`poly`（多边形）、`default`（整个图片）
- `<area>` 的 `coords`：坐标，格式依 shape 不同
  - `rect`：`x1,y1,x2,y2`（左上角和右下角）
  - `circle`：`x,y,radius`（圆心和半径）
  - `poly`：`x1,y1,x2,y2,x3,y3,...`（每个顶点的坐标）

---

## 10. 表格元素

### 10.1 基本表格结构

```html
<table>
    <tr>
        <td>第1行第1列</td>
        <td>第1行第2列</td>
    </tr>
    <tr>
        <td>第2行第1列</td>
        <td>第2行第2列</td>
    </tr>
</table>
```

- `<table>`：表格容器
- `<tr>`：table row，一行
- `<td>`：table data，一个单元格

### 10.2 `<th>` — 表头单元格

```html
<table>
    <tr>
        <th>姓名</th>
        <th>年龄</th>
        <th>城市</th>
    </tr>
    <tr>
        <td>张三</td>
        <td>25</td>
        <td>北京</td>
    </tr>
</table>
```

- 默认粗体居中
- `scope` 属性（增强无障碍）：`row` / `col` / `rowgroup` / `colgroup` 表明表头作用于哪些数据

### 10.3 表格语义分区

```html
<table>
    <caption>2024年度销售报表</caption>
    <thead>
        <tr>
            <th>月份</th>
            <th>销售额</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1月</td>
            <td>¥50,000</td>
        </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>合计</td>
            <td>¥600,000</td>
        </tr>
    </tfoot>
</table>
```

- `<caption>`：表格标题（显示在表格上方）
- `<thead>`：表头区域
- `<tbody>`：表格主体（一个表格可以有多个 `<tbody>`）
- `<tfoot>`：表尾汇总（在 `<tbody>` 之前或之后都可以，浏览器会渲染在底部）

### 10.4 `<colgroup>` / `<col>` — 列分组

```html
<table>
    <colgroup>
        <col style="background-color: #f0f0f0">
        <col span="2" style="background-color: #e0e0e0">
    </colgroup>
    <tr><td>列1</td><td>列2</td><td>列3</td></tr>
</table>
```

| `<col>` 属性 | 说明 |
|--------------|------|
| `span` | 该样式应用到几列上（默认 1） |

### 10.5 `colspan` 与 `rowspan` — 合并单元格

```html
<table>
    <tr>
        <!-- 跨两列 -->
        <td colspan="2">合并列</td>
    </tr>
    <tr>
        <!-- 跨两行 -->
        <td rowspan="2">合并行</td>
        <td>单元格B</td>
    </tr>
    <tr>
        <td>单元格C</td>
    </tr>
</table>
```

- `colspan`：水平合并（跨列）
- `rowspan`：垂直合并（跨行）

> **注意**：合并后，相邻的 `<td>` 要减少——如果 `colspan="2"` 则本行少写一个 `<td>`。

---

## 11. 表单元素

### 11.1 `<form>` — 表单容器

```html
<form action="/submit" method="POST" enctype="multipart/form-data">
    <!-- 表单控件写在这里 -->
</form>
```

| 属性 | 说明 |
|------|------|
| `action` | 数据提交到的 URL。省略则提交到当前页面 |
| `method` | HTTP 请求方法：`GET`（数据在 URL 中可见，有长度限制）或 `POST`（数据在请求体中，更适合大量/敏感数据） |
| `enctype` | 编码类型。`application/x-www-form-urlencoded`（默认）、`multipart/form-data`（有文件上传时必须用这个）、`text/plain` |
| `target` | 提交后在哪里显示响应（同 `<a>` 的 target） |
| `novalidate` | 布尔属性，跳过浏览器的表单验证 |
| `autocomplete` | `on`（默认，允许浏览器记住输入）/ `off` |
| `name` | 表单名称 |

### 11.2 `<input>` — 输入控件

`<input>` 是表单中最重要的元素，通过 `type` 属性变化出多种形态。

```html
<input type="类型" name="字段名" value="默认值" placeholder="提示文字" required>
```

#### `type` 属性一览

| type | 外观/用途 | 特别说明 |
|------|-----------|----------|
| `text` | 单行文本框（默认） | — |
| `password` | 密码框（输入显示为 ●） | 只是视觉遮蔽，不加密传输 |
| `email` | 邮箱输入框 | 浏览器会自动验证邮箱格式 |
| `tel` | 电话号码输入框 | 手机上弹出数字键盘 |
| `url` | 网址输入框 | 浏览器验证 URL 格式 |
| `number` | 数字输入框 | 配合 `min`、`max`、`step` |
| `search` | 搜索框 | 有些浏览器会在输入时显示清除按钮 |
| `date` | 日期选择器 | `min`、`max` 限定范围 |
| `time` | 时间选择器 | — |
| `datetime-local` | 本地日期时间选择器 | 不包含时区 |
| `month` | 月份选择器 | — |
| `week` | 周选择器 | — |
| `color` | 颜色选择器 | 点击弹出取色面板 |
| `range` | 滑块 | 配合 `min`、`max`、`step` |
| `file` | 文件上传 | 配合 `accept`、`multiple` |
| `checkbox` | 复选框（多选） | `checked` 表示默认选中 |
| `radio` | 单选框 | 同一组 `name` 相同 |
| `hidden` | 隐藏字段 | 用户看不到，但会随表单提交 |
| `submit` | 提交按钮 | 点击提交表单 |
| `reset` | 重置按钮 | 点击恢复表单默认值 |
| `button` | 普通按钮 | 默认无行为，配合 JS 使用 |
| `image` | 图片提交按钮 | 用图片代替 submit 按钮 |

#### `<input>` 通用属性

| 属性 | 适用类型 | 说明 |
|------|----------|------|
| `name` | 所有 | 字段名，提交时的键名 |
| `value` | 大部分 | 默认值 |
| `placeholder` | text 类 | 占位提示文字，用户输入后消失 |
| `required` | 大部分 | 布尔属性，必填项 |
| `disabled` | 所有 | 布尔属性，禁用（灰色，不提交） |
| `readonly` | text 类 | 只读（外观正常，不能修改，会提交） |
| `maxlength` | text 类 | 最大字符数 |
| `minlength` | text 类 | 最小字符数 |
| `size` | text 类 | 输入框可视宽度（字符数） |
| `autocomplete` | text 类 | 允许/禁止浏览器自动填充 |
| `autofocus` | 所有 | 布尔属性，页面加载时自动聚焦 |
| `pattern` | text 类 | 正则表达式验证 |
| `min` | number/date/range | 最小值 |
| `max` | number/date/range | 最大值 |
| `step` | number/range | 步进值 |
| `multiple` | file/email | 布尔属性，允许多选/多文件 |
| `accept` | file | 限定文件类型，如 `image/*`、`.pdf`、`image/png` |
| `checked` | checkbox/radio | 布尔属性，默认选中 |
| `list` | text/date/color 等 | 关联一个 `<datalist>` 的 `id`，提供自动补全建议 |

#### 常见示例

```html
<!-- 文本输入 -->
<input type="text" name="username" placeholder="请输入用户名" required>

<!-- 密码 -->
<input type="password" name="password" minlength="6" maxlength="20">

<!-- 数字 -->
<input type="number" name="age" min="1" max="120" step="1">

<!-- 滑块 -->
<input type="range" name="volume" min="0" max="100" value="50">

<!-- 单选框（name 相同为一组） -->
<input type="radio" name="gender" value="male" id="male"><label for="male">男</label>
<input type="radio" name="gender" value="female" id="female"><label for="female">女</label>

<!-- 复选框 -->
<input type="checkbox" name="hobby" value="reading" id="reading"><label for="reading">阅读</label>

<!-- 日期 -->
<input type="date" name="birthday" min="1900-01-01" max="2025-12-31">

<!-- 颜色 -->
<input type="color" name="theme" value="#ff0000">

<!-- 文件上传 -->
<input type="file" name="avatar" accept="image/*" multiple>
```

### 11.3 `<label>` — 标签

```html
<!-- 方式一：用 for 关联 input 的 id -->
<label for="username">用户名</label>
<input type="text" id="username" name="username">

<!-- 方式二：直接包裹 input -->
<label>
    <input type="checkbox" name="agree"> 我同意协议
</label>
```

- 点击 `<label>` 会自动聚焦/选中关联的输入控件
- `for` 属性的值必须匹配控件的 `id`（不是 `name`）
- 方式二（包裹）不需要 `for` 和 `id`，但方式一更灵活

### 11.4 `<textarea>` — 多行文本

```html
<textarea name="comment" rows="5" cols="40" placeholder="请输入评论">
默认文本
</textarea>
```

| 属性 | 说明 |
|------|------|
| `rows` | 可见行数 |
| `cols` | 可见列数（字符宽） |
| `placeholder` | 占位提示 |
| `required` | 必填 |
| `maxlength` | 最大字符数 |
| `readonly` | 只读 |
| `disabled` | 禁用 |
| `wrap` | `soft`（提交时不换行）/ `hard`（提交时自动换行，需配合 `cols`） |
| `resize` | 非 HTML 属性，通过 CSS `resize: none` 控制是否可拖拽大小 |

### 11.5 `<select>` / `<optgroup>` / `<option>` — 下拉菜单

```html
<label for="city">城市：</label>
<select name="city" id="city" required>
    <option value="">请选择城市</option>
    <optgroup label="华北地区">
        <option value="beijing">北京</option>
        <option value="tianjin">天津</option>
    </optgroup>
    <optgroup label="华东地区">
        <option value="shanghai" selected>上海</option>
        <option value="hangzhou">杭州</option>
    </optgroup>
</select>
```

| `<select>` 属性 | 说明 |
|-----------------|------|
| `name` | 字段名 |
| `multiple` | 布尔属性，允许多选（显示为列表而非下拉） |
| `size` | 配合 `multiple`，设置可见选项数 |
| `required` | 必选 |

| `<option>` 属性 | 说明 |
|-----------------|------|
| `value` | 提交的值。省略则提交 `<option>` 的文本内容 |
| `selected` | 布尔属性，默认选中 |
| `disabled` | 禁用该选项 |

| `<optgroup>` 属性 | 说明 |
|-------------------|------|
| `label` | 选项组的标签（不可选） |
| `disabled` | 禁用整组选项 |

### 11.6 `<datalist>` — 自动补全建议

```html
<label for="browser">浏览器：</label>
<input type="text" id="browser" name="browser" list="browsers">
<datalist id="browsers">
    <option value="Chrome">
    <option value="Firefox">
    <option value="Safari">
    <option value="Edge">
</datalist>
```

- `<input>` 的 `list` 属性值必须匹配 `<datalist>` 的 `id`
- 用户可以输入不在列表中的值（只是"建议"）
- 与 `<select>` 不同，它始终是可以自由输入的

### 11.7 `<button>` — 按钮

```html
<!-- 三种写法，功能相同 -->
<button type="submit">提交</button>
<button type="reset">重置</button>
<button type="button">普通按钮</button>

<!-- 也可以用 input 创建按钮 -->
<input type="submit" value="提交">
<input type="reset" value="重置">
<input type="button" value="普通按钮">
```

| `type` 属性 | 行为 |
|-------------|------|
| `submit` | 提交表单（在 `<form>` 中是**默认值**） |
| `reset` | 重置表单 |
| `button` | 无默认行为，完全由 JS 控制 |

> **推荐**：用 `<button>` 而非 `<input type="button">`，因为 `<button>` 内部可以放其他 HTML（如 `<img>`、`<span>`），更方便样式定制。

其他属性：`disabled`、`name`、`value`、`autofocus`、`form`（将按钮关联到页面中某个表单）

### 11.8 `<fieldset>` / `<legend>` — 表单分组

```html
<form>
    <fieldset>
        <legend>基本信息</legend>
        <label>姓名：<input type="text" name="name"></label>
        <label>年龄：<input type="number" name="age"></label>
    </fieldset>

    <fieldset>
        <legend>联系方式</legend>
        <label>邮箱：<input type="email" name="email"></label>
        <label>电话：<input type="tel" name="phone"></label>
    </fieldset>
</form>
```

- `<fieldset>`：将表单控件分组，默认有边框
- `<legend>`：分组的标题
- `<fieldset>` 有 `disabled` 和 `name` 属性

### 11.9 `<output>` — 计算结果展示

```html
<form oninput="result.value = parseInt(a.value) + parseInt(b.value)">
    <input type="range" name="a" value="50"> +
    <input type="number" name="b" value="50"> =
    <output name="result">100</output>
</form>
```

| 属性 | 说明 |
|------|------|
| `for` | 空格分隔的一组元素 `id`，标识参与计算的元素 |
| `name` | 输出字段名 |
| `form` | 关联的表单 `id` |

### 11.10 `<progress>` — 进度条

```html
<progress value="70" max="100">70%</progress>
```

| 属性 | 说明 |
|------|------|
| `value` | 当前值 |
| `max` | 最大值（默认 1） |
| 省略 `value` | 表示"不确定状态"（进度不可知，显示为动画） |

### 11.11 `<meter>` — 度量值

```html
<meter value="0.6" min="0" max="1" low="0.3" high="0.7" optimum="0.5">
    60%
</meter>
```

| 属性 | 说明 |
|------|------|
| `value` | 当前值（必须） |
| `min` | 最小值（默认 0） |
| `max` | 最大值（默认 1） |
| `low` | 低于此值为"低"级别（显示警告色） |
| `high` | 高于此值为"高"级别（显示警告色） |
| `optimum` | 最优值（影响颜色显示） |

> `<progress>` vs `<meter>`：`<progress>` 表示"任务完成了多少"，`<meter>` 表示"一个值在某个范围中的位置"（如磁盘使用量、分数）。

---

## 12. 语义化容器元素

HTML5 引入了一批语义化标签，用有意义的名字取代大量 `<div>`。它们的外观和 `<div>` 一样，但**表达了结构含义**，让搜索引擎和屏幕阅读器更好地理解页面。

```html
<body>
    <header>
        <nav>导航菜单</nav>
    </header>

    <main>
        <article>
            <section>
                <h2>章节标题</h2>
                <p>内容...</p>
            </section>
        </article>
        <aside>侧边栏（相关但非核心内容）</aside>
    </main>

    <footer>页脚</footer>
</body>
```

### 各标签含义

| 标签 | 含义 | 使用场景 |
|------|------|----------|
| `<header>` | 页眉/头部 | 顶部横幅、Logo、导航栏 |
| `<footer>` | 页脚 | 版权信息、联系方式、底部链接 |
| `<main>` | 页面主体内容 | **每页只能用一次**，包裹页面的核心内容 |
| `<nav>` | 导航 | 主要导航链接集合 |
| `<article>` | 独立的完整内容 | 一篇博客、一条新闻、一条评论（即使脱离页面也能独立存在） |
| `<section>` | 内容分区 | 文章中的章节、页面中的主题区块。通常包含标题 |
| `<aside>` | 侧边内容 | 和主体相关但非核心的内容（侧边栏、广告、相关链接） |
| `<div>` | 通用容器 | 无特定语义，仅用于 CSS 布局或 JS 操作。**当没有合适的语义标签时才用** |

### `<article>` vs `<section>` 怎么区分？

- 问自己：**这个内容能否独立于页面被分享/引用？** 能 → `<article>`，不能 → `<section>`
- `<article>` 可以包含多个 `<section>`；反过来 `<section>` 也可以包含多个 `<article>`

---

## 13. 脚本与样式

### 13.1 `<script>` — 脚本

```html
<!-- 内联脚本 -->
<script>
    console.log("Hello World");
</script>

<!-- 外部脚本（推荐） -->
<script src="app.js"></script>
```

| 属性 | 说明 |
|------|------|
| `src` | 外部脚本文件路径。指定后，标签内部不能写代码 |
| `type` | 脚本类型。`text/javascript`（默认，可省略）、`module`（ES 模块） |
| `async` | 异步下载，下载完**立即执行**（不保证执行顺序），适合独立脚本如统计代码 |
| `defer` | 异步下载，等 HTML 解析完再**按顺序执行**（推荐），适合依赖 DOM 的脚本 |
| `nomodule` | 布尔属性，不支持 ES 模块的浏览器才执行此脚本 |
| `crossorigin` | 跨域设置（配合 `src`） |
| `integrity` | SRI（子资源完整性）哈希值，校验文件是否被篡改 |

#### `async` vs `defer` 的区别

| | 下载 | 执行时机 | 执行顺序 |
|------|------|------|------|
| 无属性 | 阻塞 HTML 解析 | 下载完立即执行 | 文档顺序 |
| `async` | 异步（不阻塞） | 下载完立即执行 | **不保证**顺序 |
| `defer` | 异步（不阻塞） | HTML 解析完后 | **保证**文档顺序 |

```html
<!-- 推荐：大多数情况用 defer -->
<script src="app.js" defer></script>

<!-- 独立的第三方脚本用 async -->
<script src="analytics.js" async></script>

<!-- ES 模块 -->
<script type="module" src="app.js"></script>
```

### 13.2 `<noscript>` — 无脚本时的备用内容

```html
<noscript>
    <p>您的浏览器不支持 JavaScript 或已禁用 JavaScript。
        <a href="/nojs.html">请查看无脚本版本</a>
    </p>
</noscript>
```

### 13.3 `<style>` — 内嵌样式

```html
<style>
    body {
        background-color: #f5f5f5;
    }
    .highlight {
        color: red;
    }
</style>
```

| 属性 | 说明 |
|------|------|
| `media` | 媒体查询，如 `media="print"` 只在打印时生效 |
| `type` | `text/css`（默认，可省略） |

> **推荐**：优先使用外部 CSS（`<link rel="stylesheet">`），`<style>` 只用于小范围或特殊场景。

### 13.4 `<canvas>` — 画布

```html
<canvas id="myCanvas" width="400" height="300">
    您的浏览器不支持 Canvas。
</canvas>

<script>
    const canvas = document.getElementById('myCanvas');
    const ctx = canvas.getContext('2d');
    ctx.fillStyle = 'red';
    ctx.fillRect(10, 10, 100, 50);
</script>
```

| 属性 | 说明 |
|------|------|
| `width` | 画布宽度（默认 300px） |
| `height` | 画布高度（默认 150px） |

> 不要用 CSS 设置宽高——CSS 只是缩放画布（会模糊），要在标签属性上设置。

---

## 14. 交互与可折叠元素

### 14.1 `<details>` / `<summary>` — 可折叠内容

```html
<details>
    <summary>点击展开查看更多</summary>
    <p>这里是隐藏的详细内容，点击 summary 后显示。</p>
</details>

<!-- 默认展开 -->
<details open>
    <summary>已展开的内容</summary>
    <p>这段内容默认是可见的。</p>
</details>
```

| `<details>` 属性 | 说明 |
|------------------|------|
| `open` | 布尔属性，默认展开（不写则默认折叠） |
| `name` | 给多个 `<details>` 相同的 `name`，可实现"手风琴"效果（打开一个自动关闭其他，Chrome 120+ 支持） |

- `<summary>` 始终可见，点击后展开/折叠其余内容
- `<summary>` 必须是 `<details>` 的第一个子元素

### 14.2 `<dialog>` — 对话框

```html
<dialog id="myDialog">
    <h2>提示</h2>
    <p>这是对话框内容。</p>
    <button onclick="document.getElementById('myDialog').close()">关闭</button>
</dialog>

<button onclick="document.getElementById('myDialog').showModal()">打开对话框</button>
```

| 属性 | 说明 |
|------|------|
| `open` | 布尔属性，显示对话框（不推荐直接加，用 JS 的 `show()` 或 `showModal()`） |

- `showModal()`：以模态方式打开（有遮罩层，阻止页面上其他操作）
- `show()`：以非模态方式打开

---

## 15. HTML 实体字符

某些字符在 HTML 中有特殊含义（如 `<`、`>`、`&`），要显示它们自身，需要用**字符实体（Entity）**。

### 常用实体

| 实体 | 显示 | 说明 |
|------|------|------|
| `&lt;` | `<` | 小于号（less than） |
| `&gt;` | `>` | 大于号（greater than） |
| `&amp;` | `&` | 和号（ampersand） |
| `&quot;` | `"` | 双引号 |
| `&apos;` | `'` | 单引号（HTML5 支持直接写 `'`） |
| `&nbsp;` | (不换行空格) | non-breaking space，两个词不会在此处换行 |
| `&copy;` | © | 版权符号 |
| `&reg;` | ® | 注册商标 |
| `&trade;` | ™ | 商标 |
| `&mdash;` | — | 长破折号（em dash） |
| `&ndash;` | – | 短破折号（en dash） |
| `&hellip;` | … | 省略号 |
| `&laquo;` | « | 左双角引号 |
| `&raquo;` | » | 右双角引号 |

```html
<!-- 示例 -->
<p>在HTML中，标签用 &lt; 和 &gt; 包裹。</p>
<p>AT&amp;T 公司</p>
<p>价格：&copy; 2025 版权所有</p>
<p>北京&mdash;上海 高铁</p>
```

### 使用 Unicode 码点

也可以用 `&#数字;` 或 `&#x十六进制;` 的形式：

```html
<p>&#x2764; &#x1F600;</p>  <!-- ❤ 😀 -->
```

---

## 附录：快速参考总表

### 自闭合标签（不需要写结束标签）

`<area>`、`<base>`、`<br>`、`<col>`、`<embed>`、`<hr>`、`<img>`、`<input>`、`<link>`、`<meta>`、`<param>`、`<source>`、`<track>`、`<wbr>`

### 块级元素（默认独占一行）

`<div>`、`<p>`、`<h1>`~`<h6>`、`<ul>`、`<ol>`、`<li>`、`<dl>`、`<dt>`、`<dd>`、`<table>`、`<form>`、`<header>`、`<footer>`、`<main>`、`<nav>`、`<article>`、`<section>`、`<aside>`、`<blockquote>`、`<pre>`、`<hr>`、`<figure>`、`<figcaption>`、`<fieldset>`、`<details>`、`<address>`

### 内联元素（默认不换行）

`<a>`、`<span>`、`<strong>`、`<em>`、`<b>`、`<i>`、`<u>`、`<s>`、`<small>`、`<mark>`、`<del>`、`<ins>`、`<sub>`、`<sup>`、`<code>`、`<kbd>`、`<samp>`、`<var>`、`<abbr>`、`<time>`、`<q>`、`<label>`、`<img>`、`<br>`、`<input>`、`<button>`、`<select>`、`<textarea>`

---

> **学习建议**：不要试图一次性记住所有标签和属性。先掌握常用的 20%，在实际项目中反复使用，剩下的随用随查。最重要的是理解 HTML 的**语义化思想**——用正确的标签描述正确的内容。
