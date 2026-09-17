# Java IO 常见应用（附带对应的异常）

> IO 异常一般是 `IOException`，`FileNotFoundException` 是它的子类，属于**受检异常**，必须 try-catch 或者 throws。

## 1. 读取本地文本文件

读取 txt 里的内容（日志、配置文件）

```
import java.io.FileReader;
import java.io.IOException;

public class IoDemo {
    public static void main(String[] args) {
        try (FileReader fr = new FileReader("test.txt")) {
            int ch;
            while ((ch = fr.read()) != -1) {
                System.out.print((char) ch);
            }
        } catch (IOException e) {
            System.out.println("文件读取失败");
            e.printStackTrace();
        }
    }
}
```

可能异常：文件不存在、文件权限不足、文件损坏。

## 2. 写入文本文件

程序输出内容保存到本地文件，比如保存日志、导出简单文本

```
import java.io.FileWriter;
import java.io.IOException;

public class WriteFileDemo {
    public static void main(String[] args) {
        try (FileWriter fw = new FileWriter("out.txt")) {
            fw.write("你好，Java IO");
        } catch (IOException e) {
            System.out.println("写入文件出错");
        }
    }
}
```

## 3. 文件复制（图片 / 文档 / 视频拷贝）

把 A 文件复制一份到 B 路径，上传下载底层都是这个逻辑
```
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

// 文件复制Demo，使用字节流复制图片
public class CopyDemo {
    public static void main(String[] args) {
        // try-with-resources：括号内的流会自动关闭，无需手动close()
        try(FileInputStream in = new FileInputStream("a.jpg"); // 字节输入流，读取a.jpg
            FileOutputStream out = new FileOutputStream("b.jpg")){ // 字节输出流，写入b.jpg
            byte[] buf = new byte[1024]; // 创建字节缓冲区，一次读取1024字节(1KB)
            int len; // 保存每次读取到的实际字节长度
            // 循环读取：将数据读到buf数组，返回读到的字节数；读到文件末尾返回-1
            while((len = in.read(buf)) != -1){
                // 写出数据：从buf数组0位置开始，写出len个有效字节
                out.write(buf,0,len);
            }
        }catch (IOException e){ // 捕获IO相关异常：文件不存在、权限不足等
            System.out.println("文件复制失败");
        }
    }
}
```
## 4. 网络 IO（Socket，简单了解）

客户端和服务器互相收发消息，网络读写，比如聊天程序。 网络断开、端口不通都会抛出`IOException`。

## 5. 读取配置文件

项目里的 `config.properties`，读取数据库账号、接口地址。

---

### 重点小结

IO 操作容易失败的原因不是代码写错，是**外部环境问题**：

- 文件不存在
- 文件被别的软件占用
- 没有读写权限
- 硬盘满了
- 网络中断

所以 Java 强制要求：IO 代码**必须处理 IOException**。