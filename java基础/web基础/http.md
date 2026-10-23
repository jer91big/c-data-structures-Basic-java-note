# Java Web 基础笔记：HTTP 协议与请求协议

这份笔记按照「概念 → 请求结构 → 请求方法 → 请求头 → 请求体 → 完整示例」来整理，你可以直接复制到自己的 Java Web 学习笔记中。

## 一、HTTP 协议是什么？

HTTP（HyperText Transfer Protocol），超文本传输协议。

它规定了浏览器与服务器之间如何进行通信，包括客户端如何发送请求，以及服务器如何返回响应。

例如，你在浏览器中访问一个网站：

客户端（浏览器）

发出 HTTP 请求

HTTP 请求 Request

HTTP 响应 Response

服务端（Web 服务器）

接收请求、处理业务、返回结果

例如，在浏览器中访问图书管理系统：

1. 浏览器向服务器发送请求：我要获取图书列表。
    
2. 服务器查询图书数据。
    
3. 服务器返回图书列表。
    
4. 浏览器将数据展示给用户。
    

在 Java Web 开发中，你写的后端程序，就是要接收这些请求、处理业务，并返回响应。

### HTTP 的几个基本特点

- 基于请求与响应： 客户端发起请求，服务器返回响应。
    
- 无状态： HTTP 本身不会自动记住上一次请求的业务状态。登录状态等信息通常需要通过 Cookie、Session 或令牌等机制维护。
    
- 基于 TCP 或其他传输机制： HTTP/1.1 和 HTTP/2 通常使用 TCP；HTTP/3 使用 QUIC。
    
- 常见端口： HTTP 通常使用 80 端口，HTTPS 通常使用 443 端口。
    

## 二、HTTP 请求协议是什么？

HTTP 请求协议，就是客户端按照规定的格式，把请求信息发送给服务器。

一条典型的 HTTP/1.1 请求由以下部分组成：

① 请求行（Request Line）

请求方法、请求资源路径、HTTP 版本

② 请求头（Request Headers）

描述客户端信息、数据格式、身份凭证等

③ 空行

分隔请求头与请求体

④ 请求体（Request Body）

可选，用于携带提交的数据

记忆口诀：请求行、请求头、空行、请求体。

注意：上面的结构是 HTTP/1.1 的文本格式。HTTP/2 和 HTTP/3 在底层传输时使用不同的编码方式，但这些核心概念依然适用。

## 三、请求行详解

请求行的格式：

```
请求方法 请求资源路径 HTTP版本
```

例如：

```
GET /books HTTP/1.1
```

|内容|含义|
|---|---|
|`GET`|请求方法，表示获取资源|
|`/books`|请求的资源路径，表示图书相关资源|
|`HTTP/1.1`|使用的 HTTP 协议版本|

可以把它理解为给服务器写了一张任务单：

- 方法： 我要做什么？
    
- 路径： 我要操作什么资源？
    
- 版本： 按照什么协议格式通信？
    

注意，`/books` 是资源路径，不一定对应服务器硬盘上的某个 `books` 文件夹，它也可能对应 Java 后端中的一个接口。

## 四、HTTP 请求方法

请求方法用来表达客户端的意图。Java Web 开发中最常见的是 GET 和 POST，另外还需要了解 PUT、DELETE 等方法。

![](https://www.google.com/s2/favicons?domain=https://www.rfc-editor.org&sz=32)

RFC Editor

+1

|请求方法|主要用途|图书管理系统示例|
|---|---|---|
|`GET`|获取资源|查询图书列表|
|`POST`|提交数据，执行处理|新增一本图书|
|`PUT`|整体替换资源|更新一本图书的完整信息|
|`PATCH`|局部修改资源|只修改图书价格|
|`DELETE`|删除资源|删除一本图书|
|`HEAD`|获取响应头，不返回响应正文|检查资源相关信息|

这里有两个容易混淆的地方：

1. GET 和 POST 不等于有参数与没参数。 GET 参数经常放在 URL 查询字符串中，POST 常把提交数据放在请求体中，但不能简单理解为 GET 不能携带请求体。
    
2. PUT 和 POST 不是绝对的修改与新增之分。 实际行为取决于接口设计，应该优先理解它们的标准语义，而不是只记某个项目的用法。
    

对于你现在的 Java Web 入门阶段，先牢牢记住 GET 用于查询、POST 常用于提交数据，就可以继续往下学习。

## 五、请求头（Request Headers）

请求头位于请求行之后，用来补充说明请求的相关信息。

例如：

```
GET /books HTTP/1.1
Host: localhost:8080
User-Agent: Mozilla/5.0
Accept: application/json
```

其中：

|请求头|作用|
|---|---|
|`Host`|指定目标主机及可选端口|
|`User-Agent`|描述发起请求的客户端软件|
|`Accept`|告诉服务器客户端能够接受哪些响应媒体类型|
|`Content-Type`|说明请求体中数据的媒体类型|
|`Content-Length`|表示请求体的字节长度（适用时）|
|`Authorization`|携带用于身份验证的凭证信息|
|`Cookie`|携带客户端保存的 Cookie 信息|

### 重点理解：Content-Type

这个请求头在 Java Web 开发中非常重要。

它告诉服务器：我发送过来的请求体，究竟是什么格式的数据？

常见的值有：

|Content-Type|含义|常见场景|
|---|---|---|
|`application/json`|JSON 格式|前端向后端提交 JSON 数据|
|`application/x-www-form-urlencoded`|表单键值对格式|普通 HTML 表单|
|`multipart/form-data`|多部分表单格式|上传图片、文件|

例如：

```
Content-Type: application/json
```

意思就是：请求体中的数据采用 JSON 格式。

注意，`Content-Type` 描述的是实际发送的数据格式，而 `Accept` 描述的是客户端希望接收的数据格式。两者不要记反。

## 六、请求体（Request Body）

请求体是请求中携带实际数据的部分，常见于 POST、PUT、PATCH 请求。

假设我们正在开发图书管理系统，想新增一本图书。

请求可以写成这样：

```
POST /books HTTP/1.1
Host: localhost:8080
Content-Type: application/json

{
  "title": "Java核心技术",
  "author": "Cay Horstmann",
  "price": 89.00
}
```

逐行理解：

- `POST /books HTTP/1.1`：向 `/books` 提交请求。
    
- `Host: localhost:8080`：目标主机是本机的 8080 端口。
    
- `Content-Type: application/json`：请求体采用 JSON 格式。
    
- 空行：请求头结束，请求体开始。
    
- `{...}`：本次请求实际提交的图书数据。
    

服务器接收到数据后，可以解析 JSON，获取书名、作者和价格，再执行业务逻辑。

关键点： JSON 是一种数据格式，HTTP 是通信协议。两者不是同一种东西，JSON 可以作为 HTTP 请求体中的内容发送。

## 七、GET 请求与 POST 请求的区别

这是 Java Web 基础中非常重要的知识点。

### 1. GET 请求示例

假设我们要查询 ID 为 1 的图书：

```
GET /books?id=1 HTTP/1.1
Host: localhost:8080
```

这里的 `id=1` 是 URL 中的查询参数。

URL 的组成可以简单理解为：

```
/books?id=1
       └── 查询参数
```

多个查询参数通常使用 `&` 连接，例如：

```
/books?id=1&author=Tom
```

### 2. POST 请求示例

假设我们要新增一本图书：

```
POST /books HTTP/1.1
Host: localhost:8080
Content-Type: application/json

{
  "title": "Java入门",
  "price": 59.9
}
```

这次，图书数据放在请求体中，而不是直接写在 URL 的查询字符串里。

### 3. 对比总结

|对比项|GET|POST|
|---|---|---|
|典型用途|查询数据|提交数据|
|参数常见位置|URL 查询字符串|请求体|
|是否一定有请求体|不一定，通常没有|不一定，但通常用于携带数据|
|数据是否对用户可见|URL 参数通常可见|请求体不会直接显示在 URL 中|
|是否适合敏感数据|不应把敏感信息放在 URL 中|仍需 HTTPS 等安全措施|

特别注意：POST 并不天然比 GET 安全。 请求体也可能被抓包或记录，传输敏感信息应使用 HTTPS，并做好身份验证和访问控制。

![](https://www.google.com/s2/favicons?domain=https://www.rfc-editor.org&sz=32)

RFC Editor

+1

## 八、HTTP 请求与 Java Web 的关系

你已经完成了 Java 核心，接下来学习 Java Web 时，可以把 HTTP 理解成前后端沟通的共同语言。

前端：发送请求

例如，用户点击“新增图书”，前端发送 POST 请求，并携带图书信息。

HTTP 请求

Java 后端：接收并处理

接口接收数据，完成参数校验、业务处理，必要时操作数据库。

HTTP 响应

前端：接收结果

例如，收到新增成功的消息，更新页面上的图书列表。

以后学习 Servlet、Spring Boot 和前后端分离项目时，你会看到类似的代码：

```
@GetMapping("/books")
public String getBooks() {
    return "图书列表";
}
```

这里的 `@GetMapping("/books")` 是 Spring 提供的注解，用来将 GET 请求映射到这个方法。

它不是 HTTP 协议本身，而是 Java 框架帮助你处理 HTTP 请求的工具。

## 九、本节知识点速记

- HTTP 是一种应用层的请求与响应协议。
    
- HTTP/1.1 请求通常由请求行、请求头、空行和可选的请求体组成。
    
- 请求行包含请求方法、资源路径和协议版本。
    
- GET 常用于查询，POST 常用于提交数据。
    
- 请求头用于描述请求的附加信息。
    
- `Content-Type` 表示请求体的数据类型。
    
- 请求体可以携带 JSON、表单数据或文件等内容。
    
- 前端发送 HTTP 请求，Java 后端接收并处理，再通过 HTTP 响应返回结果。