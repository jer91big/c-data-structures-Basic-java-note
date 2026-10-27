CSS 的引入方式主要有 3 种，这是 Web 前端的基础知识，考试和写网页都会用到。我们结合代码来学。

## 一、三种引入方式

### 1. 行内样式（直接写在标签里）

使用 `style` 属性，把 CSS 直接写进 HTML 标签。

```
<p style="color: red; font-size: 20px;">
    Hello World
</p>
```

- `color: red`：文字变红。
    
- `font-size: 20px`：文字大小为 20px。
    

特点： 直接、方便，但不适合给大量元素统一设置样式。

### 2. 内部样式表（写在 HTML 文件里）

使用 `<style>` 标签，通常放在 HTML 的 `<head>` 标签中。

```
<!DOCTYPE html>
<html>
<head>
    <style>
        p {
            color: blue;
            font-size: 20px;
        }
    </style>
</head>
<body>
    <p>Hello World</p>
    <p>你好，世界</p>
</body>
</html>
```

这里两个 `<p>` 标签都会变成蓝色，字体大小都是 20px。

特点： 适合单个 HTML 页面使用，样式可以统一管理。

### 3. 外部样式表（单独创建 CSS 文件）

这是实际开发中非常常用的方式。

先创建两个文件：

index.html

HTML 页面

style.css

专门存放 CSS 样式

在 `style.css` 中写：

```
p {
    color: green;
    font-size: 20px;
}
```

在 `index.html` 的 `<head>` 中引入：

```
<head>
    <link rel="stylesheet" href="style.css">
</head>
```

这样 HTML 页面就会使用 `style.css` 中的样式。

注意：`href="style.css"` 表示 CSS 文件的路径。如果两个文件不在同一个目录，需要写正确的相对路径。

特点： HTML 和 CSS 分离，方便多个页面共享样式，也是推荐的常用方式。

## 二、三种方式对比

|引入方式|写在哪里|适用场景|
|---|---|---|
|行内样式|HTML 标签的 `style` 属性|临时设置单个元素|
|内部样式表|HTML 的 `<style>` 标签中|单个页面|
|外部样式表|独立的 `.css` 文件，通过 `<link>` 引入|多页面、实际项目|

## 三、考试常考点

记住下面几个关键词：

- 行内样式：`style`
    
- 内部样式表：`<style>`
    
- 外部样式表：`<link rel="stylesheet" href="...">`
    

补充一个容易混淆的知识：CSS 样式冲突时，不是简单地认为行内样式永远优先。 优先级还会受到 `!important`、选择器优先级、层叠规则等因素影响。