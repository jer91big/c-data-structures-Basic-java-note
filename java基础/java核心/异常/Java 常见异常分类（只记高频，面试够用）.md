

异常分两大类：**运行时异常（RuntimeException，非受检异常）**、**受检异常（Exception 直接子类，必须 try-catch / throws）**

> 所有异常都继承 `Exception`；`RuntimeException` 是 Exception 的子类。

## ✅ 运行时异常（最常用！代码写错才抛，不用提前声明）

|异常类名|什么时候出现|
|---|---|
|`NullPointerException`|NPE 空指针：对象是null，调用方法/属性|
|`ArrayIndexOutOfBoundsException`|数组越界，你前面例子的|
|`ArithmeticException`|算术异常，比如 `1/0` 除零|
|`ClassCastException`|类型转换异常，强转类型不对|
|`NumberFormatException`|字符串转数字失败：`Integer.parseInt("abc")`|
|`IndexOutOfBoundsException`|集合List下标越界|

示例：

```
catch (NullPointerException e){
    System.out.println("空指针！");
}
```

## ✅ 受检异常（编译强制要求处理，必须 try-catch 或者 throws抛出）

|异常类名|什么时候出现|
|---|---|
|`IOException`|IO异常：读文件、网络读写|
|`FileNotFoundException`|文件找不到，属于IOException子类|
|`SQLException`|数据库操作异常|

示例：

```
catch (IOException e){
    System.out.println("文件读取失败");
}
```

## ✅ 万能捕获（父类）

`Exception`：所有上面异常的父类，能捕获**所有异常**

> 注意：多个catch的时候，**Exception要写在最后！**

```
catch (Exception e){
    System.out.println("捕获所有未知异常");
}
```

## 极简代码演示，一次性看多个

```
public class Test {
    public static void main(String[] args) {
        try {
            String s = null;
            s.length(); //空指针
            //int a = 1/0; //除零
            //int[] arr = {1,2};
            //System.out.println(arr[5]);//数组越界
        } catch (NullPointerException e) {
            System.out.println("空指针异常");
        } catch (ArithmeticException e) {
            System.out.println("除零异常");
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("数组越界");
        } catch (Exception e) {
            System.out.println("其他异常");
        }
    }
}
```

## 小口诀

1. 运行时异常：代码bug，**不用写throws**，最常遇到：空指针、数组越界、除零
2. 受检异常：外部问题（文件、网络），**编译器强制你处理**
3. `Exception` 是万能兜底，放catch最后面

要不要我出个小练习：try里面写 `Integer.parseInt("abc")`，你说catch该填什么异常类？**