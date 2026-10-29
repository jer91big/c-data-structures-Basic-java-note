# HTML + CSS 阶段复习笔记

## 一、先搞懂 CSS 究竟是什么

还记得我们制作的新闻网页吗？

HTML：搭建骨架

## 科技创新赋能美好生活

这是新闻正文的第一段。

这是新闻正文的第二段。

CSS：设计外观

# 科技创新赋能美好生活

通过颜色、字号、间距和对齐方式，让新闻更清晰、更易阅读。

你可以这样理解：

- HTML 决定网页里有什么内容。
    
- CSS 决定这些内容呈现成什么样子。
    

比如，HTML 创建一个标题，CSS 就能让这个标题变红、变大、变粗或者居中。

## 二、我们已经学过的 CSS 属性

这是前面课程的核心内容，建议你重点掌握。

| CSS 属性             | 作用               | 示例                               |
| ------------------ | ---------------- | -------------------------------- |
| `color`            | 设置文字颜色           | `color: red;`                    |
| `font-size`        | 设置字体大小           | `font-size: 18px;`               |
| `font-weight`      | 设置文字粗细           | `font-weight: bold;`             |
| `text-align`       | 设置文字水平对齐         | `text-align: center;`            |
| `line-height`      | 设置行高             | `line-height: 1.8;`              |
| `margin-bottom`    | 设置元素下方的外边距       | `margin-bottom: 20px;`           |
| `padding`          | 设置内容与元素边缘之间的内部空间 | `padding: 20px;`                 |
| `background-color` | 设置背景颜色           | `background-color: lightyellow;` |
| `border-bottom`    | 设置元素的底部边框        | `border-bottom: 2px solid #ddd;` |
| max-width          | 控制新闻正文的最大宽度      | max-width: 760px;                |

我们逐个回忆一下。

### 1. 控制文字的外观

这三个属性很容易混淆，记住它们各自负责什么就行。

`color`：文字颜色

## 科技创新赋能美好生活

`font-size`：文字大小

# 科技创新赋能美好生活

`font-weight`：文字粗细

科技创新赋能美好生活

它们分别对应颜色、大小和粗细，不能混为一谈。

### 2. 控制文字的排列

`text-align` 控制文字在元素内部的水平对齐方式。

```
<h1 style="text-align: left;">左对齐标题</h1>

<h1 style="text-align: center;">居中标题</h1>

<h1 style="text-align: right;">右对齐标题</h1>
```

新闻标题可以居中显示，正文通常使用左对齐，这样读者更容易跟随文章阅读。

`line-height` 则控制行高。对于新闻正文，适当的行高能让多行文字看起来更加舒展。

```
<p style="font-size: 18px; line-height: 1.8;">
    科技创新正在改变我们的生活方式。
    越来越多的新技术走进日常生活。
</p>
```

### 3. 控制元素周围的空间

这里是初学者最容易弄混的地方。

想象一个新闻提示框：文字是里面的家具，提示框是房间，而家具与墙之间的距离就是内部空间。

外部空间：margin

新闻内容

内容与边框之间是 padding

边框外部与其他元素之间的距离属于 margin。

记住这个对照：

- `margin-bottom: 20px;` 给元素下方留出外部空间。
    
- `padding: 20px;` 给元素内容周围增加内部空间。
    

这两个属性不是一回事。`padding: 20px` 默认会给上、下、左、右四个方向都增加 20px 的内部空间。

### 4. 控制背景和边框

新闻正文中，我们有时需要使用背景色突出重点，或者用分隔线区分不同区域。

```
<p style="background-color: lightyellow; padding: 12px;">
    重要消息：我国科技创新取得新进展。
</p>
```

这段代码同时使用了两个属性：

- `background-color`：设置背景颜色。
    
- `padding`：让文字不要紧贴背景区域的边缘。
    

再看底部分隔线：

```
<h1 style="border-bottom: 2px solid #ddd;">
    科技创新赋能美好生活
</h1>
```

`border-bottom` 是底部边框的简写属性。这里的 `2px` 表示边框粗细，`solid` 表示实线，`#ddd` 表示浅灰色。
## 三、我们已经学会两种 CSS 写法

这是你目前学习进度中很重要的一个变化。

### 第一种：内联 CSS

之前我们一直使用这种写法：

```
<h1 style="color: red; font-size: 32px;">
    科技创新赋能美好生活
</h1>
```

样式直接写在 HTML 标签的 `style` 属性里。

优点是直观、容易练习；缺点是页面一复杂，就会出现大量重复样式，不方便统一维护。

### 第二种：CSS 类选择器

第 13 课，我们开始学习让 HTML 和 CSS 分工合作。

HTML 中：

```
<h1 class="news-title">
    科技创新赋能美好生活
</h1>
```

CSS 中：

```
<style>
    .news-title {
        color: red;
        font-size: 32px;
    }
</style>
```

这里需要牢牢记住：

==🔴HTML==

`class="news-title"`

给元素贴上一个名字叫 news-title 的标签，前面不加点。

==🔴CSS==

`.news-title`

用英文句点 `.` 开头的类选择器，找到具有这个 class 的元素并应用样式。

而且，一个类可以被多个元素使用。例如，多个新闻标题都添加 `class="news-title"`，就可以通过同一条 CSS 规则统一修改它们的外观。

目前你只需要先掌握类选择器的基本语法。 后面我们还会学习其他选择器，以及如何把 CSS 写进单独的 `.css` 文件。

## 四、把知识组合成一篇新闻网页

我们把之前学过的内容放到一起，看看它们怎样配合工作。下面这段代码可以直接放进 `news.html` 运行。

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>我的新闻网页</title>

    <style>
        .news-title {
            color: #222;
            font-size: 32px;
            font-weight: bold;
            text-align: center;
            margin-bottom: 20px;
            border-bottom: 2px solid #ddd;
        }

        .news-meta {
            color: #666;
            font-size: 14px;
            text-align: center;
        }

        .news-content {
            font-size: 18px;
            line-height: 1.8;
        }

        .news-notice {
            background-color: lightyellow;
            padding: 12px;
        }

        .highlight {
            color: red;
            font-weight: bold;
        }
    </style>
</head>

<body>
    <h1 class="news-title">科技创新赋能美好生活</h1>

    <p class="news-meta">
        来源：
        <a href="https://news.cctv.com/">央视新闻</a>
        发布时间：2026年10月28日
    </p>

    <p class="news-content">
        科技创新正在改变我们的生活方式。
        越来越多的新技术走进日常生活。
    </p>

    <p class="news-notice">
        重要消息：
        <span class="highlight">科技创新取得新进展</span>
    </p>
</body>
</html>
```
