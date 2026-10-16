# CSS3 Syntax & Modern Responsive Layout Guide | CSS3 完整语法与现代响应式布局指南

> 涵盖盒模型、Flexbox 弹性盒、Grid 网格布局、CSS 变量、媒体查询与现代动效实战。

---

## 目录

1. [什么是 CSS](#1-什么是-css)
2. [CSS 引入方式](#2-css-引入方式)
3. [CSS 语法规则](#3-css-语法规则)
4. [基础选择器](#4-基础选择器)
5. [组合选择器](#5-组合选择器)
6. [属性选择器与伪类](#6-属性选择器与伪类)
7. [伪元素](#7-伪元素)
8. [选择器优先级](#8-选择器优先级)
9. [继承与层叠](#9-继承与层叠)
10. [颜色表示法](#10-颜色表示法)
11. [长度单位](#11-长度单位)
12. [CSS 盒模型](#12-css-盒模型)
13. [背景](#13-背景)
14. [文本样式](#14-文本样式)
15. [字体样式](#15-字体样式)
16. [显示模式](#16-显示模式)
17. [定位](#17-定位)
18. [Flexbox 弹性布局](#18-flexbox-弹性布局)
19. [Grid 网格布局](#19-grid-网格布局)
20. [过渡](#20-过渡)
21. [动画](#21-动画)
22. [变换](#22-变换)
23. [响应式设计](#23-响应式设计)
24. [CSS 变量](#24-css-变量)
25. [CSS 常用函数](#25-css-常用函数)
26. [CSS 最佳实践](#26-css-最佳实践)

---

## 1. 什么是 CSS

**CSS**（Cascading Style Sheets，层叠样式表）是一种用来控制网页外观的**样式语言**。如果说 HTML 是房子的骨架（结构），那么 CSS 就是房子的装修（样式）。

**核心概念：**

- CSS 不是编程语言，是**样式表语言**——它描述元素该如何显示
- "层叠"意味着多条样式规则可以叠加作用于同一个元素
- CSS 文件的后缀名是 `.css`
- 浏览器读取 CSS 规则后，将其应用到对应的 HTML 元素上

**CSS 版本简史：**

| 版本 | 年代 | 关键特性 |
|------|------|----------|
| CSS 1 | 1996 | 基础选择器、字体、颜色、对齐 |
| CSS 2 / 2.1 | 1998 / 2011 | 定位、z-index、media types |
| CSS 3 | 2000s+ | 按**模块**发布：选择器、盒模型、背景、动画、Flexbox、Grid 等 |
| CSS 4+ | 持续演进 | `:has()`、`@layer`、容器查询、视图过渡等新特性 |

> **重要认知：** CSS 3 之后不再有统一的版本号。浏览器厂商按模块逐步实现新特性，因此你需要关注的不是"CSS 几"，而是"这个属性能不能在目标浏览器上用"。

---

## 2. CSS 引入方式

CSS 有三种方式应用到 HTML 页面。

### 2.1 内联样式（Inline Style）

直接在 HTML 标签的 `style` 属性中写 CSS：

```html
<p style="color: red; font-size: 18px;">这是一段红色文字</p>
```

- **优点：** 直接、优先级最高
- **缺点：** 样式与结构混杂，无法复用，难以维护
- **适用场景：** 偶尔的一次性覆盖；动态样式（JavaScript 生成）

### 2.2 内部样式表（Internal Style Sheet）

在 `<head>` 中通过 `<style>` 标签书写：

```html
<head>
    <style>
        p {
            color: blue;
            font-size: 16px;
        }
        .highlight {
            background-color: yellow;
        }
    </style>
</head>
```

- **优点：** 一个页面内可复用，无需额外文件
- **缺点：** 仅限当前页面，多页面共享仍需复制
- **适用场景：** 单页 demo、非常小的项目

### 2.3 外部样式表（External Style Sheet）⭐ 推荐

将 CSS 写入独立的 `.css` 文件，通过 `<link>` 标签引入：

**HTML 中：**

```html
<head>
    <link rel="stylesheet" href="styles/main.css">
</head>
```

**`styles/main.css` 中：**

```css
p {
    color: #333;
    line-height: 1.6;
}
```

- **优点：** 样式与结构分离，一份 CSS 可被多个页面共享，浏览器可缓存
- **缺点：** 多一次 HTTP 请求
- **适用场景：** 所有正式项目

**`@import` 方式（不推荐）：**

```css
@import url("reset.css");
@import "theme.css";
```

> ⚠️ `@import` 会阻塞渲染，性能不如 `<link>`，一般不推荐使用。

---

## 3. CSS 语法规则

### 3.1 基本结构

一条 CSS 规则由**选择器**和**声明块**组成：

```css
选择器 {
    属性: 值;
    属性: 值;
}
```

**图解：**

```
   h1    {  color: red;   font-size: 24px;  }
   ─┬─     ─────────────┬─────────────────
   选择器              声明块（包含两条声明）
                        ├─ 属性: 值;
                        └─ 属性: 值;
```

- **选择器：** 指定这条规则"选中"哪些 HTML 元素
- **声明块：** 被 `{}` 包裹，内部是一条或多条声明
- **声明：** 由 `属性: 值;` 组成，分号 `;` 不可省略（最后一条分号可选但建议保留）

### 3.2 注释

```css
/* 这是单行注释 */

/*
 * 这是
 * 多行注释
 */

p {
    color: red; /* 也可以写在声明后面 */
}
```

> 注意：CSS 不支持 `//` 这种单行注释方式（那是预处理器如 Sass/Less 的语法）。

### 3.3 @规则（At-rules）

以 `@` 开头的特殊规则，用于定义条件、导入、动画等：

```css
@import url("other.css");         /* 导入其他样式表 */
@media (max-width: 768px) {  }   /* 媒体查询 */
@keyframes slideIn {  }          /* 定义动画 */
@font-face {  }                  /* 定义自定义字体 */
@layer base {  }                 /* 定义层叠层 */
@supports (display: grid) {  }   /* 特性检测 */
```

### 3.4 简写属性（Shorthand）

很多属性支持简写，一条声明同时设置多个子属性：

```css
/* 分开写 */
margin-top: 10px;
margin-right: 20px;
margin-bottom: 10px;
margin-left: 20px;

/* 简写（上 右 下 左） */
margin: 10px 20px 10px 20px;

/* 更短的简写（上下 左右） */
margin: 10px 20px;
```

---

## 4. 基础选择器

选择器决定了 CSS 规则应用到哪些元素上。

### 4.1 类型选择器（Type Selector）

按 HTML 标签名选中元素：

```css
p { color: #333; }        /* 所有 <p> 元素 */
h1 { font-size: 2em; }    /* 所有 <h1> 元素 */
a { text-decoration: none; } /* 所有 <a> 元素 */
```

### 4.2 类选择器（Class Selector）⭐ 最常用

按元素的 `class` 属性选中，以 `.` 开头：

```css
.card { border: 1px solid #ddd; }
.highlight { background-color: yellow; }
```

```html
<div class="card highlight">我同时拥有两个类</div>
```

- 一个元素可以有多个 class，用空格分隔
- 同一个 class 可以给多个元素复用
- **这是最推荐的选择器**——灵活、可复用、优先级适中

### 4.3 ID 选择器（ID Selector）

按元素的 `id` 属性选中，以 `#` 开头：

```css
#header { background: black; }
#main-title { font-size: 2em; }
```

```html
<h1 id="main-title">页面标题</h1>
```

> ⚠️ 一个页面中，同一个 `id` 只能出现一次。CSS 中尽量使用 class 而非 id——id 优先级过高，难以覆盖。

### 4.4 通配选择器（Universal Selector）

选中**所有**元素：

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
```

- 常用于全局重置
- 性能略低（匹配所有元素），但实际可忽略

### 4.5 分组选择器（Grouping）

用逗号 `,` 将多个选择器合并，共用同一组声明：

```css
h1, h2, h3 {
    font-family: "Microsoft YaHei", sans-serif;
    color: #222;
}
```

---

## 5. 组合选择器

通过元素之间的关系来精确定位。

### 5.1 后代选择器（Descendant Combinator）

选中某元素内部**所有**匹配的后代（无论层级深浅），用空格连接：

```css
/* 选中 .article 内部所有的 <p>，不管是直接子元素还是更深嵌套 */
.article p {
    line-height: 1.8;
}
```

```html
<div class="article">
    <p>被选中 ✓</p>
    <blockquote>
        <p>也被选中 ✓（嵌套了两层）</p>
    </blockquote>
</div>
```

### 5.2 子元素选择器（Child Combinator）

只选中**直接子元素**，用 `>` 连接：

```css
/* 只选中 .nav 的直接 <li>，不选更深层的 <li> */
.nav > li {
    display: inline-block;
}
```

```html
<ul class="nav">
    <li>被选中 ✓（直接子元素）</li>
    <li>
        被选中 ✓
        <ul>
            <li>不被选中 ✗（孙子元素）</li>
        </ul>
    </li>
</ul>
```

### 5.3 相邻兄弟选择器（Adjacent Sibling Combinator）

选中**紧接在后面的第一个兄弟元素**，用 `+` 连接：

```css
/* 选中紧跟在 <h2> 后面的第一个 <p> */
h2 + p {
    margin-top: 0;
    color: #666;
}
```

```html
<h2>标题</h2>
<p>被选中 ✓（紧跟在 h2 后面的第一个 p）</p>
<p>不被选中 ✗（不是第一个）</p>
```

### 5.4 通用兄弟选择器（General Sibling Combinator）

选中在后面的**所有兄弟元素**，用 `~` 连接：

```css
/* 选中 <h2> 后面所有的 <p> 兄弟 */
h2 ~ p {
    color: #666;
}
```

```html
<h2>标题</h2>
<p>被选中 ✓</p>
<div>一个分隔 div</div>
<p>也被选中 ✓（虽然在 div 后面，但仍是 h2 的兄弟）</p>
```

### 5.5 组合使用

选择器可以任意组合，写出高精度的定位：

```css
/* 选中 .sidebar 内 .menu 的直接 <a> 子元素 */
.sidebar .menu > a { }

/* 选中 h2 后面紧跟着的、带 .subtitle 类的 p 元素 */
h2 + p.subtitle { }
```

---

## 6. 属性选择器与伪类

### 6.1 属性选择器（Attribute Selectors）

按元素的属性及属性值来选中：

| 选择器 | 含义 | 示例 |
|--------|------|------|
| `[attr]` | 有该属性的元素 | `[disabled]` |
| `[attr="val"]` | 属性值**精确等于** val | `[type="text"]` |
| `[attr^="val"]` | 属性值以 val **开头** | `[href^="https"]` |
| `[attr$="val"]` | 属性值以 val **结尾** | `[src$=".png"]` |
| `[attr*="val"]` | 属性值**包含** val | `[class*="btn"]` |
| `[attr~="val"]` | 属性值包含**完整单词** val | `[class~="active"]` |
| `[attr\|="val"]` | 属性值等于 val 或以 `val-` 开头 | `[lang\|="zh"]` |

```css
/* 选中所有 type="email" 的输入框 */
input[type="email"] {
    border-color: blue;
}

/* 选中所有链接到外部 https 的链接 */
a[href^="https"]::after {
    content: " 🔗";
}

/* 选中 class 中包含 "btn" 的元素（如 class="btn-primary btn-large"） */
[class*="btn"] {
    display: inline-block;
}
```

### 6.2 伪类（Pseudo-classes）

伪类表示元素的**特定状态**，以 `:` 开头。

**（A）超链接相关：**

```css
a:link { color: blue; }       /* 未访问的链接 */
a:visited { color: purple; }  /* 已访问的链接 */
a:hover { color: red; }       /* 鼠标悬停 */
a:active { color: orange; }   /* 鼠标按下时 */
```

> 记忆口诀：**L**o**V**e **HA**te → LVHA 顺序，按此顺序写才能正确层叠。

**（B）表单状态相关：**

```css
input:focus { outline: 2px solid blue; }       /* 获得焦点 */
input:disabled { background: #eee; }             /* 禁用状态 */
input:checked + label { color: green; }          /* 选中状态（复选框/单选框） */
input:required { border-left: 3px solid red; }   /* 必填字段 */
input:valid { border-color: green; }             /* 验证通过 */
input:invalid { border-color: red; }             /* 验证失败 */
```

**（C）结构化伪类（按位置选择）：**

```css
li:first-child { }      /* 第一个子元素 */
li:last-child { }       /* 最后一个子元素 */
li:nth-child(2) { }     /* 第 2 个子元素 */
li:nth-child(odd) { }   /* 奇数个子元素 (1, 3, 5, ...) */
li:nth-child(even) { }  /* 偶数个子元素 (2, 4, 6, ...) */
li:nth-child(3n+1) { }  /* 第 1, 4, 7, 10, ... 个 */
li:nth-last-child(1) { }/* 倒数第 1 个 */
li:only-child { }       /* 唯一的子元素 */

/* of-type 系列：在同类型兄弟中定位 */
p:first-of-type { }     /* 同类型中的第一个 */
p:nth-of-type(2) { }    /* 同类型中的第 2 个 */
p:last-of-type { }      /* 同类型中的最后一个 */
```

**（D）其他常用伪类：**

```css
:not(.excluded) { }      /* 排除匹配选择器的元素 */
:is(h1, h2, h3) { }      /* 匹配列表中任意一个（简化分组） */
:where(h1, h2, h3) { }   /* 同 :is() 但优先级为 0 */
:has(> img) { }           /* 父选择器：包含直接子元素 <img> 的元素 */
:empty { }                /* 没有子元素和文本的元素 */
:root { }                 /* 文档根元素（即 <html>） */
```

> `:has()` 被称为"父选择器"，是近年最重要的 CSS 新特性之一，让很多以前需要 JavaScript 的判断可以直接用 CSS 实现。

---

## 7. 伪元素

伪元素用于选中元素的**某个部分**或**创建虚拟元素**，以 `::` 开头。

| 伪元素 | 作用 |
|--------|------|
| `::before` | 在元素内容**前**插入虚拟内容 |
| `::after` | 在元素内容**后**插入虚拟内容 |
| `::first-line` | 文本的**第一行** |
| `::first-letter` | 文本的**第一个字** |
| `::selection` | 用户**选中**的文本部分 |
| `::placeholder` | 输入框的**占位文字** |
| `::marker` | 列表项的**标记符号**（圆点、数字等） |

```css
/* 在引用前添加装饰引号 */
blockquote::before {
    content: "❝";
    font-size: 2em;
    color: #ccc;
}

/* 在链接后添加外链图标 */
a[href^="https"]::after {
    content: " ↗";
    font-size: 0.8em;
}

/* 首字下沉效果 */
.article p:first-of-type::first-letter {
    float: left;
    font-size: 3em;
    line-height: 1;
    margin-right: 0.1em;
    color: #c00;
}

/* 选中文字的颜色 */
::selection {
    background: #007acc;
    color: white;
}

/* 输入框占位文字样式 */
input::placeholder {
    color: #aaa;
    font-style: italic;
}
```

> **`::before` / `::after` 的关键规则：**
> - 必须设置 `content` 属性（即使为空字符串 `content: "";`）
> - 它们创建的虚拟元素默认是**行内**元素
> - 它们是元素的"子元素"——`::before` 在内容前，`::after` 在内容后

---

## 8. 选择器优先级

当多条规则作用于同一个元素时，浏览器按**优先级（Specificity）**决定谁生效。

### 8.1 优先级计算规则

把优先级看作一个四位数字 `(a, b, c, d)`：

| 等级 | 选择器类型 | 示例 | 权重 |
|------|-----------|------|------|
| a | 内联样式 | `style="color: red"` | (1,0,0,0) |
| b | ID 选择器 | `#header` | (0,1,0,0) |
| c | 类/属性/伪类 | `.card`, `[type]`, `:hover` | (0,0,1,0) |
| d | 类型/伪元素 | `div`, `::before` | (0,0,0,1) |

> **注意：** 通配符 `*`、组合符（` `、`>`、`+`、`~`）、`:where()` 不增加优先级。

**例子：**

```css
/* 优先级 (0,0,0,1) */
p { color: red; }

/* 优先级 (0,0,1,1) */
.article p { color: blue; }

/* 优先级 (0,0,2,0) */
.article .highlight { color: green; }

/* 优先级 (0,1,1,0) */
#main .highlight { color: orange; }
```

最终 `#main .highlight` 的 `color: orange` 会生效。

### 8.2 `!important`

在声明后加 `!important` 会将其提升到最高优先级，甚至超过内联样式：

```css
p {
    color: red !important;
}
```

> ⚠️ **慎用 `!important`！** 滥用会导致样式难以调试和维护。只有在以下场景才使用：
> - 覆盖第三方库/插件的样式
> - 工具类（如 `.hidden { display: none !important; }`）

### 8.3 优先级实战建议

1. **优先使用 class 选择器**（优先级适中，易复用）
2. **避免使用 ID 选择器**做样式（优先级过高，难以覆盖）
3. **尽量不用内联样式**
4. **尽量不用 `!important`**
5. 覆盖样式时，使用**更具体的选择器**而非提高优先级层级

---

## 9. 继承与层叠

### 9.1 继承（Inheritance）

某些 CSS 属性会由父元素传递给子元素。

**会自动继承的属性：**

| 类别 | 会继承的属性 |
|------|-------------|
| 字体 | `font-family`, `font-size`, `font-weight`, `font-style` |
| 文本 | `color`, `text-align`, `line-height`, `letter-spacing` |
| 列表 | `list-style` |
| 可见性 | `visibility` |
| 光标 | `cursor` |

**不会自动继承的属性：**

| 类别 | 不继承的属性 |
|------|-------------|
| 盒模型 | `margin`, `padding`, `border`, `width`, `height` |
| 背景 | `background`, `background-color` |
| 定位 | `position`, `top`, `left` |
| 显示 | `display` |

**手动控制继承：**

```css
.child {
    color: inherit;        /* 强制继承父元素的 color */
    color: initial;        /* 恢复为属性的默认值 */
    color: unset;          /* 如果属性可继承则 inherit，否则 initial */
    color: revert;         /* 回退到浏览器默认样式 */
    all: unset;            /* 重置所有属性 */
}
```

### 9.2 层叠（Cascade）

当多条规则作用于同一元素、且优先级相同时，按以下顺序决定：

1. **来源** — 浏览器默认样式 < 用户自定义样式 < 作者样式（你写的 CSS）
2. **`!important`** — 加 `!important` 的声明优先
3. **优先级** — 高优先级覆盖低优先级
4. **书写顺序** — 后写的覆盖先写的

### 9.3 `@layer` 层叠层

CSS 的新特性，可以显式管理样式的层叠顺序：

```css
/* 定义层（后面的优先级更高） */
@layer reset, base, components, utilities;

/* 在具体层中写样式 */
@layer reset {
    * { margin: 0; padding: 0; }
}

@layer components {
    .btn { padding: 8px 16px; }
}

/* utilities 层优先级最高，它的样式最不容易被覆盖 */
@layer utilities {
    .mt-0 { margin-top: 0 !important; }
}
```

> `@layer` 让你无需 `!important` 就能优雅地管理样式优先级，特别适合大型项目。

---

## 10. 颜色表示法

### 10.1 颜色关键字

CSS 内置了 140+ 种命名颜色：

```css
color: red;
color: tomato;
color: dodgerblue;
background: transparent;   /* 透明 */
background: currentColor;  /* 使用当前元素的 color 值 */
```

### 10.2 HEX 十六进制 ⭐ 最常用

```css
color: #ff0000;      /* 红色（6 位，RRGGBB）*/
color: #f00;         /* 红色（3 位简写，等同于 #ff0000）*/
color: #333;         /* 深灰 */
color: #00000080;    /* 半透明黑色（8 位，RRGGBBAA）*/
```

### 10.3 RGB / RGBA

```css
color: rgb(255, 0, 0);        /* 红色 */
color: rgba(255, 0, 0, 0.5);  /* 半透明红色 */
color: rgb(255 0 0 / 50%);    /* 现代写法（不需要逗号）*/
```

### 10.4 HSL / HSLA

用**色相、饱和度、亮度**来描述颜色，更符合人类直觉：

```css
color: hsl(0, 100%, 50%);          /* 红色 */
color: hsl(0, 100%, 50%, 0.5);     /* 半透明红色 */
color: hsl(0 100% 50% / 50%);      /* 现代写法 */
```

- **H（Hue / 色相）：** 0–360°，0=红、120=绿、240=蓝
- **S（Saturation / 饱和度）：** 0% 灰色 → 100% 纯色
- **L（Lightness / 亮度）：** 0% 黑 → 50% 正常 → 100% 白

### 10.5 现代颜色函数

```css
/* HWB：色相 + 白 + 黑 */
color: hwb(0 0% 0%); /* 纯红 */

/* Lab / LCH / OKLCH：感知均匀的色彩空间 */
color: oklch(62.8% 0.25 29.2);  /* 亮红色 */

/* color()：广色域 */
color: color(display-p3 1 0 0);
```

---

## 11. 长度单位

### 11.1 绝对单位

| 单位 | 含义 | 换算 |
|------|------|------|
| `px` | 像素 | 屏幕上的一个物理/逻辑点 |
| `pt` | 磅 | 1pt = 1/72 英寸（常用于打印） |
| `cm` | 厘米 | 1cm = 37.8px（约） |
| `mm` | 毫米 | 1mm = 3.78px（约） |
| `in` | 英寸 | 1in = 96px |

**日常开发中，`px` 是最常用的绝对单位。**

### 11.2 相对单位

| 单位 | 相对于 | 说明 |
|------|--------|------|
| `%` | 父元素对应属性 | 百分比 |
| `em` | 父元素的 `font-size` | 用于 `font-size` 时相对父元素；用于其他属性时相对自身 `font-size` |
| `rem` | 根元素（`<html>`）的 `font-size` | ⭐ **最推荐的字号单位** |
| `vw` | 视口宽度的 1% | 100vw = 整个视口宽度 |
| `vh` | 视口高度的 1% | 100vh = 整个视口高度 |
| `vmin` | 视口宽高中较小的 1% | 适合保证内容不超出屏幕 |
| `vmax` | 视口宽高中较大的 1% | 适合覆盖整个视口 |
| `ch` | 数字 `0` 的宽度 | 常用于限制文本行宽 |
| `ex` | 字母 `x` 的高度 | 较少使用 |

**实战示例：**

```css
html {
    font-size: 16px;  /* 根字号，作为所有 rem 的基准 */
}

h1 {
    font-size: 2rem;  /* = 32px */
}

p {
    font-size: 1rem;          /* = 16px */
    line-height: 1.5;         /* 无单位数字，相对于自身 font-size */
    max-width: 60ch;          /* 约 60 个字符宽，适合阅读 */
}

.hero {
    min-height: 100vh;        /* 至少占满整个屏幕高度 */
    font-size: clamp(1rem, 2vw + 0.5rem, 2rem); /* 响应式字号 */
}
```

### 11.3 `em` vs `rem` 关键区别

```css
html { font-size: 16px; }

.parent {
    font-size: 20px;              /* 父元素字号 20px */
}

.parent .child-em {
    font-size: 1.5em;             /* 1.5 × 20px = 30px（相对父元素）*/
    padding: 1em;                 /* 1 × 30px = 30px（相对自身 font-size！）*/
}

.parent .child-rem {
    font-size: 1.5rem;            /* 1.5 × 16px = 24px（始终相对根元素）*/
    padding: 1rem;                /* 1 × 16px = 16px（始终相对根元素）*/
}
```

> **推荐策略：** 字号用 `rem`，间距/边距也用 `rem`——全局一致，不受嵌套影响。

---

## 12. CSS 盒模型

在 CSS 中，每个元素都是一个**矩形盒子**。

### 12.1 盒模型的四个层次

```
┌─────────────────────────────┐
│         margin              │  ← 外边距（透明，元素与外部的距离）
│   ┌─────────────────────┐   │
│   │      border         │   │  ← 边框
│   │   ┌─────────────┐   │   │
│   │   │   padding   │   │   │  ← 内边距（元素内容到边框的距离）
│   │   │ ┌─────────┐ │   │   │
│   │   │ │ content │ │   │   │  ← 内容区域（文字/图片等）
│   │   │ └─────────┘ │   │   │
│   │   └─────────────┘   │   │
│   └─────────────────────┘   │
└─────────────────────────────┘
```

**逐个解释：**

| 层次 | 说明 | 会受背景色影响？ |
|------|------|:---:|
| `content` | 内容区域，摆放文字和子元素 | ✅ |
| `padding` | 内边距，内容到边框的留白 | ✅ |
| `border` | 边框 | ✅（可单独设置颜色） |
| `margin` | 外边距，元素与元素之间的距离 | ❌ 始终透明 |

### 12.2 `box-sizing`：决定宽度包含什么

这是必须理解的最重要属性之一。

```css
/* 默认值：width 只算 content */
* {
    box-sizing: content-box;
}

/* ⭐ 推荐：width 包含 content + padding + border */
* {
    box-sizing: border-box;
}
```

**对比：**

```css
div {
    width: 300px;
    padding: 20px;
    border: 5px solid black;
}
```

| box-sizing | 实际占用宽度 | 内容宽度 |
|:---:|:---:|:---:|
| `content-box` | 300 + 2×20 + 2×5 = **350px** | 300px |
| `border-box` | **300px** | 300 − 2×20 − 2×5 = 250px |

> **几乎所有现代项目都会在开头写：**
> ```css
> *, *::before, *::after {
>     box-sizing: border-box;
> }
> ```

### 12.3 Margin 的特殊行为

**（A）垂直外边距合并（Margin Collapse）**

两个块级元素的上下 margin 相遇时，会合并为一个（取较大值）：

```css
.top { margin-bottom: 30px; }
.bottom { margin-top: 20px; }
/* 实际间距：max(30, 20) = 30px，而非 50px */
```

**（B）父子间的 margin 传递**

子元素的 `margin-top` 可能"穿透"父元素，贴在父元素外面：

```html
<div class="parent">
    <p class="child" style="margin-top: 50px;">文字</p>
</div>
<!-- 结果是 parent 上面多了 50px，而不是 child 在里面下移 -->
```

**解决方案：** 给父元素加 `overflow: hidden`、`border-top` 或 `padding-top`。

---

## 13. 背景

### 13.1 `background-color`

```css
.box {
    background-color: #f0f0f0;
    background-color: rgba(0, 0, 0, 0.5);  /* 半透明 */
    background-color: transparent;          /* 透明（默认值）*/
}
```

### 13.2 `background-image`

```css
/* 普通图片 */
background-image: url("bg.jpg");

/* 渐变（CSS 生成的图像） */
background-image: linear-gradient(to right, red, blue);
background-image: radial-gradient(circle, yellow, green);
background-image: conic-gradient(from 45deg, red, blue, green, red);

/* 多背景叠加 */
background-image:
    url("overlay.png"),
    linear-gradient(to bottom, #000, transparent),
    url("hero.jpg");
```

### 13.3 `background-repeat`

```css
background-repeat: repeat;      /* 默认：平铺 */
background-repeat: no-repeat;   /* 不平铺 */
background-repeat: repeat-x;    /* 只水平平铺 */
background-repeat: repeat-y;    /* 只垂直平铺 */
background-repeat: space;       /* 平铺但不裁剪，均匀分布 */
background-repeat: round;       /* 平铺并拉伸以填满 */
```

### 13.4 `background-position`

```css
background-position: center;           /* 居中（水平和垂直都居中）*/
background-position: top right;        /* 右上角 */
background-position: 50% 50%;          /* 居中 */
background-position: 20px 40px;        /* 距左 20px，距上 40px */
background-position: left 10px top 20px; /* 距左边缘 10px，距上 20px */
```

### 13.5 `background-size`

```css
background-size: cover;    /* 覆盖整个容器，可能裁剪（⭐ 常用）*/
background-size: contain;  /* 完整显示图片，可能留白 */
background-size: 100% auto; /* 宽 100%，高自适应 */
background-size: 200px 150px; /* 固定宽高 */
```

### 13.6 其他背景属性

```css
background-attachment: fixed;   /* 背景固定不动（视差效果）*/
background-attachment: scroll;  /* 随元素滚动（默认）*/
background-clip: border-box;    /* 背景延伸到边框后 */
background-clip: padding-box;   /* 背景只到内边距 */
background-clip: text;          /* 背景裁剪到文字形状 🔥 */
background-origin: padding-box; /* 背景定位原点 */
```

### 13.7 简写 `background`

```css
/* 顺序不重要，但建议按此顺序 */
background: #fff url("bg.jpg") no-repeat center / cover fixed;
/*          颜色  图片         重复方式   位置   / 尺寸   附着 */
```

**文字渐变效果（利用 `background-clip: text`）：**

```css
.gradient-text {
    background: linear-gradient(135deg, #667eea, #764ba2);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
}
```

---

## 14. 文本样式

### 14.1 颜色与对齐

```css
color: #333;                       /* 文字颜色 */
text-align: left;                  /* 左对齐（默认）*/
text-align: center;                /* 居中 */
text-align: right;                 /* 右对齐 */
text-align: justify;               /* 两端对齐 */
text-align-last: center;           /* 最后一行对齐方式 */
vertical-align: middle;            /* 行内元素/表格单元格的垂直对齐 */
```

### 14.2 行高与间距

```css
line-height: 1.6;                  /* ⭐ 推荐无单位值，相对于 font-size */
letter-spacing: 0.05em;            /* 字间距 */
word-spacing: 0.1em;               /* 词间距 */
text-indent: 2em;                  /* 首行缩进 */
```

### 14.3 文本修饰

```css
text-decoration: underline;        /* 下划线 */
text-decoration: line-through;     /* 删除线 */
text-decoration: overline;         /* 上划线 */
text-decoration: none;             /* 无修饰（常用于去掉链接下划线）*/
text-decoration-color: red;        /* 装饰线颜色 */
text-decoration-thickness: 2px;    /* 装饰线粗细 */
text-underline-offset: 4px;        /* 下划线偏移距离 */
```

### 14.4 文本溢出处理 ⭐ 常用

```css
/* 单行省略 */
.single-line-ellipsis {
    white-space: nowrap;           /* 不换行 */
    overflow: hidden;              /* 隐藏溢出 */
    text-overflow: ellipsis;       /* 用省略号表示 */
}

/* 多行省略（限定 3 行） */
.multi-line-ellipsis {
    display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
}
```

### 14.5 其他文本属性

```css
text-transform: uppercase;         /* 全部大写 */
text-transform: lowercase;         /* 全部小写 */
text-transform: capitalize;        /* 首字母大写 */
word-break: break-all;             /* 允许在任何字符处换行 */
overflow-wrap: break-word;         /* 长单词/URL 折行 */
white-space: pre-wrap;             /* 保留空格和换行，但自动换行 */
white-space: nowrap;               /* 强制不换行 */
writing-mode: vertical-rl;         /* 竖排文字（从右到左）*/
direction: rtl;                    /* 文字从右到左（阿拉伯语等）*/
tab-size: 4;                       /* Tab 缩进宽度 */
```

---

## 15. 字体样式

### 15.1 `font-family`

```css
body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI",
                 "Microsoft YaHei", sans-serif;
}
```

**规则：**
- 字体名含空格、数字或中文时要用引号包裹
- 多个字体用逗号分隔，浏览器从左到右查找，使用第一个可用的
- 最后设一个**通用字体系列**作为兜底：

| 通用系列 | 含义 |
|----------|------|
| `serif` | 衬线体（笔画有装饰，如宋体、Times New Roman） |
| `sans-serif` | 无衬线体（笔画无装饰，如黑体、Arial） |
| `monospace` | 等宽字体（每个字符宽度相同，如 Consolas, Courier New） |
| `cursive` | 手写体 |
| `fantasy` | 装饰体 |

### 15.2 字号与粗细

```css
font-size: 16px;                   /* 像素 */
font-size: 1rem;                   /* 相对根元素 */
font-size: 1.2em;                  /* 相对父元素 */
font-size: clamp(1rem, 2vw, 2rem); /* 响应式字号 */

font-weight: normal;               /* 正常 = 400 */
font-weight: bold;                 /* 加粗 = 700 */
font-weight: 300;                  /* 细体（需字体支持）*/
font-weight: 900;                  /* 最粗（需字体支持）*/
```

### 15.3 其他字体属性

```css
font-style: italic;                /* 斜体 */
font-style: oblique;               /* 倾斜（模拟斜体）*/
font-style: normal;                /* 正常 */

font-variant: small-caps;          /* 小型大写字母 */

/* 简写（必须按此顺序：style weight size/line-height family） */
font: italic bold 1rem/1.5 "Microsoft YaHei", sans-serif;
```

### 15.4 自定义字体 `@font-face`

```css
@font-face {
    font-family: "MyCustomFont";
    src: url("fonts/MyFont.woff2") format("woff2"),
         url("fonts/MyFont.woff") format("woff");
    font-weight: 400;
    font-style: normal;
    font-display: swap;            /* ⭐ 避免 FOIT（文字不可见闪烁）*/
}

/* 使用 */
body {
    font-family: "MyCustomFont", sans-serif;
}
```

**`font-display` 选项：**

| 值 | 行为 |
|----|------|
| `swap` | 先用后备字体，加载完再替换（⭐ 推荐） |
| `block` | 短暂空白后使用后备字体，加载完替换 |
| `fallback` | 极短空白后用后备字体，短时间内不替换 |
| `optional` | 极短空白后用后备字体，可能不替换 |
| `auto` | 浏览器默认行为 |

---

## 16. 显示模式

### 16.1 `display` 属性

| 值 | 行为 |
|----|------|
| `none` | 元素不显示，不占空间 |
| `block` | 块级：独占一行，可设宽高 |
| `inline` | 行内：不独占一行，不能设宽高 |
| `inline-block` | 行内块：不独占一行，可设宽高 |
| `flex` | 弹性容器（详见第 18 节） |
| `inline-flex` | 行内弹性容器 |
| `grid` | 网格容器（详见第 19 节） |
| `inline-grid` | 行内网格容器 |
| `table` / `table-row` / `table-cell` | 模拟表格行为 |

```css
/* 经典居中方案（行内块 + 文本居中） */
.parent {
    text-align: center;
}
.child {
    display: inline-block;
}
```

### 16.2 `visibility` 与 `opacity`

```css
visibility: visible;       /* 可见 */
visibility: hidden;        /* 隐藏但保留空间（布局不变）*/

opacity: 1;                /* 完全不透明 */
opacity: 0.5;              /* 半透明 */
opacity: 0;                /* 完全透明但保留空间且可交互 */

/* display: none vs visibility: hidden vs opacity: 0 */
```

| 属性 | 看不见 | 占空间 | 可交互 | 触发回流 |
|------|:---:|:---:|:---:|:---:|
| `display: none` | ✅ | ❌ | ❌ | ✅ |
| `visibility: hidden` | ✅ | ✅ | ❌ | ❌ |
| `opacity: 0` | ✅ | ✅ | ✅ | ❌ |

---

## 17. 定位

### 17.1 `position` 属性

| 值 | 参照物 | 是否脱离文档流 |
|----|--------|:---:|
| `static` | 默认值，正常文档流 | ❌ |
| `relative` | 元素**自身原本的位置** | ❌（保留原位） |
| `absolute` | 最近的**非 static 祖先**；无则相对于 `<body>` | ✅ |
| `fixed` | **视口**（浏览器窗口） | ✅ |
| `sticky` | 滚动到阈值后固定在视口 | ❌（保留原位） |

### 17.2 偏移属性

只有 `position` 为 `relative` / `absolute` / `fixed` / `sticky` 时生效：

```css
top: 10px;
right: 20px;
bottom: 30px;
left: 40px;
```

### 17.3 实战示例

```css
/* 🅐 相对定位：微调位置，不影响其他元素 */
.icon {
    position: relative;
    top: -2px; /* 向上偏移 2px */
}

/* 🅑 绝对定位居中（经典技巧） */
.modal {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

/* 🅒 固定导航栏 */
.navbar {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1000;
}

/* 🅓 粘性标题 */
.sticky-header {
    position: sticky;
    top: 0;
    background: white;
}
```

### 17.4 `z-index`

控制定位元素的堆叠顺序，值越大越靠上：

```css
.modal {
    position: fixed;
    z-index: 9999;     /* 弹窗在最上面 */
}

.tooltip {
    position: absolute;
    z-index: 1000;
}
```

**规则：**
- 只对定位元素（非 `static`）有效
- `z-index` 受**层叠上下文**限制——子元素再高的 z-index 也无法穿透父元素的层叠上下文
- 创建新层叠上下文的常见方式：`position` 非 static + `z-index` 非 auto、`opacity < 1`、`transform`、`filter` 等

---

## 18. Flexbox 弹性布局

**Flexbox 是一维布局方案**——它在一个方向（行或列）上排列元素。

### 18.1 基础概念

```
弹性容器（flex container）：设 display: flex 的元素
弹性项目（flex items）：容器内的直接子元素
主轴（main axis）：排列的方向（默认水平）
交叉轴（cross axis）：与主轴垂直的方向
```

### 18.2 容器属性

```css
.container {
    display: flex;                          /* 或 inline-flex */

    /* 主轴方向 */
    flex-direction: row;                    /* 默认：水平从左到右 */
    flex-direction: row-reverse;            /* 水平从右到左 */
    flex-direction: column;                 /* 垂直从上到下 */
    flex-direction: column-reverse;         /* 垂直从下到上 */

    /* 主轴对齐 */
    justify-content: flex-start;            /* 默认：起点对齐 */
    justify-content: flex-end;              /* 终点对齐 */
    justify-content: center;                /* 居中 */
    justify-content: space-between;         /* 两端对齐，间距均匀 */
    justify-content: space-around;          /* 每个项目两侧间距相等 */
    justify-content: space-evenly;          /* 所有间距完全相等 */

    /* 交叉轴对齐 */
    align-items: stretch;                   /* 默认：拉伸填满 */
    align-items: flex-start;                /* 起点对齐 */
    align-items: flex-end;                  /* 终点对齐 */
    align-items: center;                    /* 居中 */
    align-items: baseline;                  /* 基线对齐 */

    /* 换行 */
    flex-wrap: nowrap;                      /* 默认：不换行 */
    flex-wrap: wrap;                        /* 换行 */
    flex-wrap: wrap-reverse;                /* 反向换行 */

    /* 多行时的交叉轴对齐 */
    align-content: stretch;                 /* 默认：拉伸 */
    align-content: center;                  /* 居中 */
    align-content: space-between;           /* 两端对齐 */

    /* 行/列间距（简写） */
    gap: 20px;                              /* 统一间距 */
    gap: 16px 24px;                         /* 行间距 列间距 */
    row-gap: 16px;                          /* 只设行间距 */
    column-gap: 24px;                       /* 只设列间距 */
}
```

### 18.3 项目属性

```css
.item {
    /* 排列顺序（默认 0，值越小越靠前） */
    order: 1;

    /* 放大比例（默认 0，不放大） */
    flex-grow: 1;

    /* 缩小比例（默认 1，空间不足时缩小） */
    flex-shrink: 0;                         /* 设为 0 表示不缩小 */

    /* 初始大小（分配剩余空间前的基准） */
    flex-basis: auto;                       /* 默认：按内容 */
    flex-basis: 200px;                      /* 基准为 200px */
    flex-basis: 0;                          /* 完全按 flex-grow 比例分配 */

    /* 简写：flex: grow shrink basis */
    flex: 1;                                /* = flex: 1 1 0 */
    flex: 0 0 200px;                        /* 不伸缩，固定 200px */

    /* 单个项目的交叉轴对齐（覆盖 align-items） */
    align-self: auto;                       /* 默认：继承容器 */
    align-self: center;                     /* 居中 */
    align-self: flex-end;                   /* 终点对齐 */
}
```

### 18.4 Flexbox 经典布局

```css
/* ① 水平垂直居中 */
.center {
    display: flex;
    justify-content: center;
    align-items: center;
}

/* ② 圣杯布局（header + 三栏 + footer） */
body {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}
main {
    display: flex;
    flex: 1;
}
.sidebar { flex: 0 0 250px; }              /* 固定宽 250px */
.content { flex: 1; }                       /* 自适应 */

/* ③ 等宽多列（自动换行） */
.card-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
}
.card-grid > * {
    flex: 1 1 300px;  /* 最小 300px，等分剩余空间，自动换行 */
}

/* ④ 导航栏两端对齐 */
.nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
```

---

## 19. Grid 网格布局

**Grid 是二维布局方案**——它同时在行和列上排列元素，是 CSS 最强大的布局系统。

### 19.1 基础概念

```
网格容器（grid container）：设 display: grid 的元素
网格项目（grid items）：容器内的直接子元素
网格线（grid lines）：行与列的边界线
网格轨道（grid tracks）：两条线之间的空间（行或列）
网格区域（grid area）：由四条线围成的矩形空间
```

### 19.2 容器属性

```css
.container {
    display: grid;                              /* 或 inline-grid */

    /* 定义列轨道 */
    grid-template-columns: 200px 1fr 200px;     /* 三列：固定-自适应-固定 */
    grid-template-columns: repeat(3, 1fr);       /* 三等分 */
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); /* 自适应列数 */

    /* 定义行轨道 */
    grid-template-rows: auto 1fr auto;           /* header-content-footer */
    grid-template-rows: 100px repeat(3, 1fr);

    /* 间距 */
    gap: 20px;
    gap: 16px 24px;                              /* 行间距 列间距 */

    /* 项目排列方向（默认先填行） */
    grid-auto-flow: row;                         /* 默认：按行排列 */
    grid-auto-flow: column;                      /* 按列排列 */
    grid-auto-flow: dense;                       /* 紧密填充（不留空单元格）*/

    /* 隐式轨道大小（自动生成的额外行/列） */
    grid-auto-rows: 200px;                       /* 新增行高度 */
    grid-auto-columns: 150px;

    /* 项目整体对齐（在容器内的位置，当内容小于容器时有效） */
    justify-content: center;                     /* 水平居中所有列 */
    align-content: center;                       /* 垂直居中所有行 */
    place-content: center;                       /* 上述二者的简写 */

    /* 单元格内项目的默认对齐 */
    justify-items: stretch;                      /* 默认：水平拉伸 */
    align-items: stretch;                        /* 默认：垂直拉伸 */
    place-items: center;                         /* 水平和垂直居中 */
}
```

### 19.3 项目属性

```css
.item {
    /* 指定项目的位置（基于网格线） */
    grid-column-start: 1;                        /* 从第 1 条列线开始 */
    grid-column-end: 3;                          /* 到第 3 条列线结束 */
    grid-column: 1 / 3;                          /* 简写：跨 2 列 */
    grid-column: 1 / span 2;                     /* 从第 1 条线开始，跨 2 列 */

    grid-row-start: 2;
    grid-row-end: 4;
    grid-row: 2 / 4;                             /* 跨第 2-4 行 */

    /* 简写：row-start / col-start / row-end / col-end */
    grid-area: 2 / 1 / 4 / 3;                    /* 占据 2-4 行、1-3 列 */

    /* 单个项目的对齐 */
    justify-self: center;                        /* 水平居中该单元格内容 */
    align-self: center;                          /* 垂直居中该单元格内容 */
    place-self: center;                          /* 简写 */
}
```

### 19.4 `fr` 单位

`fr`（fraction）是 Grid 专用单位，表示**按比例分配剩余空间**：

```css
/* 三等分 */
grid-template-columns: 1fr 1fr 1fr;

/* 中间列占两倍宽度 */
grid-template-columns: 1fr 2fr 1fr;

/* 混合使用 */
grid-template-columns: 200px 1fr 2fr 100px;
/* 第 1 列 200px，第 4 列 100px，剩余空间按 1:2 分配给第 2 和第 3 列 */
```

### 19.5 Grid 经典布局

```css
/* ① 圣杯布局（仅需 4 行 CSS） */
body {
    display: grid;
    grid-template: "header  header  header" auto
                   "sidebar content aside"  1fr
                   "footer  footer  footer" auto
                   / 200px   1fr    200px;
    min-height: 100vh;
}

/* ② 自动响应式卡片网格（无需媒体查询！） */
.card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 24px;
}

/* ③ 完美居中 */
.center {
    display: grid;
    place-items: center;
}
```

### 19.6 Flexbox vs Grid 选择指南

| 场景 | 推荐方案 |
|------|----------|
| 一维排列（一行或一列） | Flexbox |
| 二维排列（行 + 列） | Grid |
| 导航栏、工具栏 | Flexbox |
| 页面整体布局 | Grid |
| 内容驱动（不确定数量） | Flexbox |
| 精确的"设计稿"布局 | Grid |
| 两者都能用 | 选择更简单的那一个 |

---

## 20. 过渡

`transition` 让属性值的变化平滑过渡，而不是瞬间跳变。

### 20.1 基本语法

```css
.element {
    /* 简写：属性 时长 缓动函数 延迟 */
    transition: all 0.3s ease 0s;

    /* 分开写 */
    transition-property: all;              /* 哪些属性需要过渡 */
    transition-duration: 0.3s;             /* 过渡时长 */
    transition-timing-function: ease;      /* 缓动函数 */
    transition-delay: 0s;                  /* 延迟多久开始 */
}
```

### 20.2 缓动函数（Easing）

| 值 | 效果 |
|----|------|
| `ease` | 默认：慢→快→慢 |
| `linear` | 匀速 |
| `ease-in` | 慢→快 |
| `ease-out` | 快→慢 |
| `ease-in-out` | 慢→快→慢（比 ease 更平滑） |
| `cubic-bezier(n,n,n,n)` | 自定义贝塞尔曲线 |
| `steps(n)` | 分步过渡（帧动画） |

### 20.3 实战示例

```css
/* 按钮悬浮效果 */
.btn {
    background: #007acc;
    color: white;
    padding: 10px 24px;
    border-radius: 4px;
    transition: background 0.2s ease, transform 0.2s ease;
}
.btn:hover {
    background: #005a9e;
    transform: translateY(-2px);
}

/* 卡片悬浮上浮 */
.card {
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.card:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
}
```

> **性能提示：** 尽量只过渡 `transform` 和 `opacity`——它们由 GPU 加速，不会触发重排。避免过渡 `width`、`height`、`top`、`left` 等属性。

---

## 21. 动画

`animation` 比 `transition` 更强大——可以定义**多阶段**的复杂动画，且无需触发器（hover 等）就能自动播放。

### 21.1 定义关键帧 `@keyframes`

```css
@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

/* 使用百分比定义多阶段 */
@keyframes bounce {
    0%, 100% { transform: translateY(0); }
    50%      { transform: translateY(-30px); }
}

@keyframes pulse {
    0%   { transform: scale(1); opacity: 1; }
    50%  { transform: scale(1.1); opacity: 0.7; }
    100% { transform: scale(1); opacity: 1; }
}
```

### 21.2 应用动画

```css
.element {
    /* 简写：名称 时长 缓动 延迟 次数 方向 填充模式 播放状态 */
    animation: fadeIn 0.5s ease-out;

    /* 分开写 */
    animation-name: fadeIn;
    animation-duration: 0.5s;
    animation-timing-function: ease-out;
    animation-delay: 0s;
    animation-iteration-count: 1;              /* 播放次数，infinite = 无限 */
    animation-direction: normal;               /* normal / reverse / alternate */
    animation-fill-mode: forwards;             /* 动画结束后保留最终状态 */
    animation-play-state: running;             /* running / paused */
}
```

### 21.3 `animation-fill-mode` 详解

| 值 | 行为 |
|----|------|
| `none` | 默认：动画只在播放期间生效 |
| `forwards` | 动画结束后保留最后一帧的样式 |
| `backwards` | 在延迟期间就应用第一帧的样式 |
| `both` | 同时具有 forwards 和 backwards 的效果 |

### 21.4 实战示例

```css
/* 加载旋转动画 */
@keyframes spin {
    to { transform: rotate(360deg); }
}
.spinner {
    width: 40px;
    height: 40px;
    border: 3px solid #e0e0e0;
    border-top-color: #007acc;
    border-radius: 50%;
    animation: spin 0.8s linear infinite;
}

/* 骨架屏闪烁 */
@keyframes shimmer {
    0%   { background-position: -200% 0; }
    100% { background-position: 200% 0; }
}
.skeleton {
    background: linear-gradient(90deg, #eee 25%, #f5f5f5 50%, #eee 75%);
    background-size: 200% 100%;
    animation: shimmer 1.5s ease-in-out infinite;
}

/* 淡入上移（元素出现时） */
@keyframes fadeInUp {
    from {
        opacity: 0;
        transform: translateY(30px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}
.animate-in {
    animation: fadeInUp 0.6s ease-out both;
}

/* 逐项延迟出现的列表 */
.list-item {
    opacity: 0;
    animation: fadeInUp 0.4s ease-out forwards;
}
.list-item:nth-child(1) { animation-delay: 0.1s; }
.list-item:nth-child(2) { animation-delay: 0.2s; }
.list-item:nth-child(3) { animation-delay: 0.3s; }
```

---

## 22. 变换

`transform` 对元素进行旋转、缩放、平移、倾斜等变换，**不影响文档流**。

### 22.1 变换函数

```css
/* 平移（移动） */
transform: translateX(20px);             /* 水平移动 */
transform: translateY(-10px);            /* 垂直移动 */
transform: translate(50px, -20px);       /* 水平和垂直同时 */
transform: translate(-50%, -50%);        /* 百分比相对于自身尺寸（居中神技）*/

/* 缩放 */
transform: scale(1.2);                   /* 整体放大 20% */
transform: scale(1, 0.5);                /* 宽不变，高缩小一半 */
transform: scaleX(1.5);                  /* 只水平缩放 */

/* 旋转 */
transform: rotate(45deg);                /* 顺时针旋转 45° */
transform: rotate(-90deg);               /* 逆时针旋转 90° */
transform: rotate(0.5turn);              /* 旋转半圈（180°）*/

/* 倾斜 */
transform: skewX(10deg);                 /* 水平倾斜 */
transform: skewY(-5deg);                 /* 垂直倾斜 */
transform: skew(10deg, -5deg);

/* 组合变换（空格分隔，从右到左依次应用） */
transform: translate(-50%, -50%) rotate(45deg) scale(1.2);

/* 3D 变换 */
transform: perspective(500px) rotateY(30deg);
transform: rotateX(45deg);
transform: translateZ(100px);
transform: translate3d(10px, 20px, 30px);
```

### 22.2 变换原点 `transform-origin`

```css
transform-origin: center;          /* 默认：中心 */
transform-origin: top left;        /* 左上角 */
transform-origin: 50% 50%;         /* 居中 */
transform-origin: 0 100%;          /* 左下角 */
transform-origin: 100px 50px;      /* 具体位置 */
```

### 22.3 实战示例

```css
/* 居中对齐（绝对定位 + transform） */
.center {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

/* 悬浮放大（不影响周围元素） */
.zoom-on-hover {
    transition: transform 0.3s;
}
.zoom-on-hover:hover {
    transform: scale(1.05);
}

/* 倾斜标签 */
.tag {
    display: inline-block;
    transform: skewX(-10deg);
}
.tag span {
    display: inline-block;
    transform: skewX(10deg);       /* 反向倾斜恢复文字 */
}
```

---

## 23. 响应式设计

### 23.1 视口设置

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

> 这是响应式的**第一个前提**——没有这行，移动端会按桌面宽度渲染并缩小。

### 23.2 媒体查询 `@media`

```css
/* 基础语法 */
@media (条件) {
    /* 满足条件时应用的样式 */
}

/* 按宽度 */
@media (max-width: 768px) {
    /* 屏幕宽度 ≤ 768px 时（移动端） */
    .sidebar { display: none; }
}

@media (min-width: 769px) and (max-width: 1024px) {
    /* 769px ≤ 屏幕宽度 ≤ 1024px（平板） */
}

/* 按设备类型 */
@media print {
    /* 打印时 */
    .no-print { display: none; }
}

/* 按屏幕方向 */
@media (orientation: landscape) {
    /* 横屏 */
}

/* 按像素比（高清屏） */
@media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
    /* Retina 屏幕高清图 */
}

/* 按用户偏好 */
@media (prefers-color-scheme: dark) {
    /* 用户系统主题为深色模式 */
    body { background: #1a1a1a; color: #eee; }
}

@media (prefers-reduced-motion: reduce) {
    /* 用户偏好减少动画 */
    * { animation-duration: 0.01ms !important; }
}
```

### 23.3 响应式断点策略

**移动优先（Mobile First）⭐ 推荐：**

```css
/* 基础样式：移动端（最小屏幕） */
.container { padding: 16px; }
.card { width: 100%; }

/* 平板 */
@media (min-width: 768px) {
    .container { padding: 24px; }
    .card { width: calc(50% - 12px); }
}

/* 桌面 */
@media (min-width: 1024px) {
    .container { max-width: 1200px; margin: 0 auto; }
    .card { width: calc(33.333% - 16px); }
}
```

### 23.4 响应式工具

```css
/* 响应式字号（clamp 无敌） */
h1 { font-size: clamp(1.5rem, 4vw, 3rem); }

/* 自适应的容器内边距 */
.container {
    padding: clamp(16px, 5vw, 80px);
}

/* 自适应网格（无需媒体查询） */
.card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 24px;
}

/* 响应式图片 */
img {
    max-width: 100%;
    height: auto;
}

/* 宽高比容器 */
.video-wrapper {
    aspect-ratio: 16 / 9;
}
```

---

## 24. CSS 变量

CSS 变量（Custom Properties）让你定义可复用的值，并在整个样式表中引用。

### 24.1 基本用法

```css
/* 通常在 :root 中定义全局变量 */
:root {
    --primary-color: #007acc;
    --secondary-color: #6c757d;
    --text-color: #333;
    --border-radius: 8px;
    --spacing-unit: 8px;
    --max-width: 1200px;
    --shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.1);
    --shadow-md: 0 4px 12px rgba(0, 0, 0, 0.15);
}

/* 使用 var() 引用 */
.btn-primary {
    background: var(--primary-color);
    border-radius: var(--border-radius);
    padding: var(--spacing-unit) calc(var(--spacing-unit) * 3);
    box-shadow: var(--shadow-sm);
}

/* var() 的第二个参数是默认值 */
.card {
    background: var(--card-bg, #fff);  /* 如果 --card-bg 未定义，用 #fff */
}
```

### 24.2 局部变量与作用域

```css
/* 变量在定义它的选择器及其子元素中可用 */
.card {
    --card-padding: 24px;
    padding: var(--card-padding);
}

.card .title {
    padding: var(--card-padding);  /* 可以访问父级定义的变量 */
}

.outside {
    padding: var(--card-padding);  /* ❌ 不可用，未在作用域内 */
}
```

### 24.3 动态修改变量（配合 JavaScript）

```javascript
// 读取
getComputedStyle(document.documentElement).getPropertyValue('--primary-color');

// 设置（一行实现主题切换！）
document.documentElement.style.setProperty('--primary-color', '#ff6600');
```

### 24.4 实战：深色模式

```css
:root {
    --bg: #ffffff;
    --text: #333333;
    --card-bg: #f5f5f5;
}

@media (prefers-color-scheme: dark) {
    :root {
        --bg: #1a1a2e;
        --text: #e0e0e0;
        --card-bg: #16213e;
    }
}

/* 也可以手动切换（给 html 加 class） */
html[data-theme="dark"] {
    --bg: #1a1a2e;
    --text: #e0e0e0;
    --card-bg: #16213e;
}

body {
    background: var(--bg);
    color: var(--text);
}

.card {
    background: var(--card-bg);
}
```

---

## 25. CSS 常用函数

| 函数 | 作用 | 示例 |
|------|------|------|
| `calc()` | 数学计算 | `width: calc(100% - 40px)` |
| `min()` | 取最小值 | `width: min(100%, 600px)` |
| `max()` | 取最大值 | `width: max(300px, 50%)` |
| `clamp()` | 限定在区间内 | `font-size: clamp(1rem, 2vw, 2rem)` |
| `var()` | 引用 CSS 变量 | `color: var(--primary)` |
| `url()` | 引用资源 | `background: url("bg.jpg")` |
| `rgb()` / `hsl()` | 颜色函数 | `color: rgb(255 0 0 / 50%)` |
| `attr()` | 读取 HTML 属性值 | `content: attr(data-tooltip)` |
| `repeat()` | Grid 中重复轨道 | `grid-template-columns: repeat(3, 1fr)` |
| `minmax()` | Grid 轨道最小/最大 | `grid-template-columns: minmax(200px, 1fr)` |

### `calc()` 实战

```css
/* 侧边栏 + 内容区 */
.sidebar { width: 250px; }
.content { width: calc(100% - 250px - 40px); /* 减去侧边栏和间距 */ }

/* 混合单位 */
.indent { margin-left: calc(2rem + 10px); }
```

### `min()` / `max()` / `clamp()` 实战

```css
/* 容器宽度不超过 1200px，但小屏幕时自适应 */
.container { width: min(100%, 1200px); }

/* 按钮宽度至少 200px */
.btn { width: max(200px, 50%); }

/* 响应式字号：最小 1rem，最大 2rem，理想值 2vw */
h1 { font-size: clamp(1rem, 2vw, 2rem); }
```

---

## 26. CSS 最佳实践

### 26.1 命名规范：BEM

**BEM**（Block Element Modifier）是业界最流行的 CSS 命名方法之一：

```css
/* Block（块） */
.card { }

/* Block__Element（元素） */
.card__title { }
.card__image { }
.card__footer { }

/* Block--Modifier（修饰符） */
.card--featured { }
.card--large { }

/* Element + Modifier 组合 */
.card__title--highlighted { }
```

```html
<div class="card card--featured">
    <h2 class="card__title card__title--highlighted">标题</h2>
    <div class="card__body">内容</div>
    <div class="card__footer">底部</div>
</div>
```

**BEM 的好处：**
- 一眼看出选择器之间的关系
- 避免嵌套导致的优先级问题
- 团队协作时代码意图清晰

### 26.2 文件组织建议

```
styles/
├── reset.css          # 浏览器默认样式重置
├── variables.css      # CSS 变量（颜色、间距、字号等）
├── base.css           # 基础元素样式（body, h1-h6, a, p 等）
├── layout.css         # 布局（header, footer, grid, flex 容器）
├── components/        # 组件样式
│   ├── button.css
│   ├── card.css
│   └── modal.css
└── utilities.css      # 工具类（.text-center, .hidden, .mt-0 等）
```

### 26.3 编写原则

1. **用 class 而非 id** —— class 可复用、优先级适中
2. **避免深层嵌套** —— 选择器嵌套不超过 3 层，减少对 DOM 结构的强依赖
3. **使用 CSS 变量** —— 颜色、间距等统一管理，方便全局修改和主题切换
4. **移动优先** —— 先写移动端样式，再用 `min-width` 媒体查询向上扩展
5. **短选择器优于长选择器** —— `.card-title` 优于 `.card .header h2 span.title`
6. **使用 `rem` / `em` 而非固定 `px`** —— 更灵活，适配不同字号偏好
7. **善用现代布局** —— 能用 Flexbox/Grid 就不用 float/position 布局
8. **性能优先属性** —— 动画/过渡尽量只用 `transform` 和 `opacity`

### 26.4 浏览器兼容性

```css
/* 优雅降级：先写 fallback，再写新特性 */
.btn {
    background: #007acc;                       /* fallback */
    background: color(display-p3 0 0.2 0.8);  /* 广色域浏览器才用 */

    display: flex;                             /* fallback */
    display: grid;                             /* 支持 grid 的浏览器用这个 */
}

/* 特性检测 */
@supports (display: grid) {
    .layout {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr;
    }
}

@supports not (aspect-ratio: 16 / 9) {
    .video-wrapper {
        padding-bottom: 56.25%;  /* 传统 hack */
        position: relative;
    }
}
```

**常用检查工具：**
- [Can I Use](https://caniuse.com) — 查询浏览器对某特性的支持情况
- [Autoprefixer](https://autoprefixer.github.io) — 自动添加厂商前缀

### 26.5 推荐学习资源

| 资源 | 说明 |
|------|------|
| **MDN Web Docs** | CSS 最权威的参考文档 |
| **CSS-Tricks** | Flexbox / Grid 图文指南 |
| **web.dev** | Google 的现代 Web 开发教程 |
| **Codepen** | 在线实验 CSS 效果 |
| **CSS Battle** | 用 CSS 解题的游戏化练习 |

---

> **写在最后：** CSS 看起来简单，但它拥有极大的深度和表现力。从盒模型到 Grid，从过渡到动画，掌握这些核心概念后，你就能把设计稿精准地变成网页。最好的学习方式永远是**动手写**——每学一个属性，打开浏览器开发者工具试一试，看看它到底怎么生效的。
>
> 祝你写得一手好 CSS 🎨
