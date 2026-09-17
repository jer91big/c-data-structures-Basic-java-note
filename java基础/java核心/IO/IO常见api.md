# Java IO 常用 API（分两大类：**字符流**、**字节流**）

> 后缀 `Reader/Writer` → **字符流**：专门处理文本（txt、properties），操作 char 后缀 `InputStream/OutputStream` → **字节流**：一切文件（图片、视频、pdf、文本都行），操作 byte

## 一、字符流（文本文件）

### 读文本 Reader

1. `FileReader`：从文件读取字符
    - 方法：`read()` 读单个字符；`read(char[] buf)` 读字符数组
2. `BufferedReader`：带缓冲区，**读文本最常用**，效率更高
    - 方法：`readLine()` 👉 **按一整行读取（超级常用！）**

### 写文本 Writer

1. `FileWriter`：往文件写入字符
    - 方法：`write(字符串)`、`write(单个字符)`；`flush()` 刷新缓冲区
2. `BufferedWriter`：缓冲字符输出流
    - 方法：`write()`；`newLine()` 换行

## 二、字节流（所有类型文件：图片、视频、音频、文档）

### 读字节 InputStream

1. `FileInputStream`：文件字节输入流
    - `read()` 读 1 字节；`read(byte[] buf)` 批量读入字节数组
2. `BufferedInputStream`：缓冲字节输入流，提速

### 写字节 OutputStream

1. `FileOutputStream`：文件字节输出流
    - `write(byte[])` 写字节数组
2. `BufferedOutputStream`：缓冲字节输出流

## 三、文件操作辅助类（不读写内容，操作文件本身）

`java.io.File` 类（不是流！）

- `exists()` 判断文件 / 文件夹是否存在
- `createNewFile()` 创建空文件
- `delete()` 删除文件
- `isFile()` 判断是不是文件
- `isDirectory()` 判断是不是文件夹
- `listFiles()` 获取文件夹里面所有子文件

## ✅ 高频 API 小例子（只看方法）

```
BufferedReader br = new BufferedReader(new FileReader("test.txt"));
String line = br.readLine(); //读一行，最常用
```

```
BufferedWriter bw = new BufferedWriter(new FileWriter("out.txt"));
bw.write("hello");
bw.newLine(); //换行
bw.flush(); //把缓冲区数据刷入文件
```

```
FileInputStream fis = new FileInputStream("1.jpg");
byte[] buffer = new byte[1024];
int len = fis.read(buffer); //批量读取字节
```

## 📌 一句话区分

- `readLine()` 只有**BufferedReader**才有，读文本行神器
- 字节流没有`readLine`，只能读 byte，适合图片视频
- 所有流用完都要关闭；推荐 `try-with-resources` 自动 close

## 面试常记重点

- 缓冲流 `BufferedXXX`：加缓冲区，减少磁盘 IO 次数，**提升性能**
- `flush()`：强制把缓冲区的数据写入磁盘；close 的时候会自动调用 flush