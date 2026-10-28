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

|CSS 属性|作用|示例|
|---|---|---|
|`color`|设置文字颜色|`color: red;`|
|`font-size`|设置字体大小|`font-size: 18px;`|
|`font-weight`|设置文字粗细|`font-weight: bold;`|
|`text-align`|设置文字水平对齐|`text-align: center;`|
|`line-height`|设置行高|`line-height: 1.8;`|
|`margin-bottom`|设置元素下方的外边距|`margin-bottom: 20px;`|
|`padding`|设置内容与元素边缘之间的内部空间|`padding: 20px;`|
|`background-color`|设置背景颜色|`background-color: lightyellow;`|
|`border-bottom`|设置元素的底部边框|`border-bottom: 2px solid #ddd;`|

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