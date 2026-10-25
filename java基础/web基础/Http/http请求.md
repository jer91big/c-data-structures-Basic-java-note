HTTP 请求（HTTP Request）是客户端（如浏览器、手机 App、API 客户端等）向服务器发送的消息，用于请求服务器执行特定操作（通常是获取、创建、修改或删除资源）。

  

一个标准的 HTTP 请求主要由 **4 个部分**组成：**请求行、请求头、空行和请求体**。

  

## 1. HTTP 请求的结构

HTTP

```
[请求方法] [请求路径] [HTTP版本]   <-- 1. 请求行
[请求头部字段1]: [值]             <-- 2. 请求头 (Headers)
[请求头部字段2]: [值]
...
                                  <-- 3. 空行 (CRLF)
{ "key": "value" }                <-- 4. 请求体 (Body，可选)
```

### ① 请求行（Request Line）

位于请求的第一行，由三部分组成，中间用空格隔开：

  

- **请求方法（Method）**：告诉服务器要做什么（如 `GET`、`POST`）。
    
      
    
- **请求路径（URI / Path）**：请求的资源路径或接口地址（如 `/index.html` 或 `/api/users`）。
    
      
    
- **HTTP 版本（Version）**：使用的 HTTP 协议版本（如 `HTTP/1.1` 或 `HTTP/2`）。
    
      
    

### ② 请求头（Request Headers）

以键值对（Key-Value）的形式存在，用于向服务器传递关于客户端环境、身份认证、期望的数据格式等附加信息。常见的请求头有：

  

- **`Host`**：目标服务器的主机名和端口号（如 `api.example.com`）。
    
      
    
- **`User-Agent`**：客户端的浏览器、操作系统或设备类型信息。
    
      
    
- **`Accept`**：客户端能够接收的响应数据类型（如 `application/json`）。
    
      
    
- **`Content-Type`**：请求体中数据的媒体类型（如发送 JSON 数据时设为 `application/json`）。
    
      
    
- **`Authorization`**：用于身份验证的凭证（如 Bearer Token、Basic Auth）。
    
      
    
- **`Cookie`**：客户端存储的 Cookie 信息，用于会话跟踪。
    
      
    

### ③ 空行（Blank Line）

请求头和请求体之间必须有一个**空行**（即一个回车换行符 `\r\n`），用来告诉服务器：“请求头到这里结束，接下来是请求体”。

  

### ④ 请求体（Request Body）

用来向服务器提交数据。

  

- **`GET` 请求**通常没有请求体，参数直接拼接在 URL 后面（Query Parameters）。
    
      
    
- **`POST`、`PUT`、`PATCH` 请求**通常包含请求体，存放提交给服务器的数据（如表单数据、JSON 对象、文件等）。
    
      
    

## 2. 常用的 HTTP 请求方法

|**方法**|**描述**|**是否有请求体**|**特点**|
|---|---|---|---|
|**GET**|请求获取指定的资源|通常无|安全且**幂等**（多次请求结果相同）|
|**POST**|向指定资源提交数据（如表单提交、创建新数据）|**有**|非幂等（多次提交可能创建多条数据）|
|**PUT**|完整替换或更新指定的资源|**有**|幂等|
|**PATCH**|部分更新指定的资源|**有**|通常幂等，取决于具体实现|
|**DELETE**|请求删除指定的资源|通常无|幂等|
|**HEAD**|类似于 GET，但只请求响应头，不要响应体|无|用于检查资源是否存在或获取元数据|
|**OPTIONS**|查询服务器支持哪些请求方法|无|常用于跨域（CORS）预检请求|

## 3. 一个完整的 HTTP 请求示例

以下是一个通过 `POST` 方法向服务器提交 JSON 数据的真实 HTTP 请求报文：

  

HTTP

```
POST /api/v1/users HTTP/1.1
Host: api.example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
Accept: application/json, text/plain, */*
Content-Type: application/json
Content-Length: 42
Authorization: Bearer eyJhbGciOi...

{
  "name": "张三",
  "email": "zhangsan@example.com"
}
```

服务器收到这个请求后，会解析请求头和请求体，执行相应的业务逻辑（如在数据库中创建用户），并最终返回一个 **HTTP 响应（HTTP Response）**。