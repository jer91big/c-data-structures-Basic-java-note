
### StringBuilder 常见方法速查表

|**操作类型**|**方法声明**|**功能描述**|**示例/特点**|
|---|---|---|---|
|**追加**|`append(任意数据类型)`|将数据**追加**到当前字符串的末尾|`sb.append("hello")`|
|**插入**|`insert(int offset, 任意数据类型)`|将数据**插入**到指定的索引位置|`sb.insert(0, "前缀")`|
|**删除**|`delete(int start, int end)`|删除指定范围内的字符|**左闭右开**（含首不含尾）|
||`deleteCharAt(int index)`|删除指定索引位置的**单个字符**|`sb.deleteCharAt(2)`|
|**修改/替换**|`replace(int start, int end, String str)`|将指定范围内的内容**替换**为新字符串|`sb.replace(0, 2, "替换")`|
|**反转**|`reverse()`|将字符串的内容进行**反转**|`"abc"` $\rightarrow$ `"cba"`|
|**转换**|`toString()`|将 `StringBuilder` 转换为普通的 `String`|拼接完成后通常最后调用此方法|
|**获取**|`length()`|获取当前包含的字符数量|和 `String` 的用法一致|

> **💡 核心小贴士（与 String 的对比）：**
> 
>   
> 
> - `String` 是**不可变**的，每次拼接或修改都会在内存中创建一个**新的**字符串对象。
>     
>       
>     
> - `StringBuilder` 是**可变（Mutable）**的，所有的 `append`、`insert`、`reverse` 等操作都是直接在**原对象**上进行的，**不会产生新的对象**（方法调用后通常会返回 `this` 本身，支持链式编程，如 `sb.append("a").append("b")`）。
>     
>       
>     
> - **使用场景：** 当你需要**频繁**地拼接、修改字符串内容时（比如在循环中拼接字符串），强烈建议使用 `StringBuilder`，这能极大地提高运行效率并节省内存。
>