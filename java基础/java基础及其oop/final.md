在 Java 里，`final` 就像给代码“贴封条”的魔法胶带 🧷——一旦封上，就不允许再随便改动。

它主要有 **3 种用法**，分别作用在：变量、方法、类。

---

## 1️⃣ final 修饰变量（常量化）

```java
final int a = 10;
a = 20; // ❌ 错误
```

含义：  
👉 一旦赋值，就不能再修改

📌 注意：

- 基本数据类型：值不能变
    
- 引用数据类型：地址不能变，但“里面的内容”可以变
    

```java
final int[] arr = {1,2,3};
arr[0] = 99;   // ✅ 可以
arr = new int[5]; // ❌ 不可以
```

---

## 2️⃣ final 修饰方法（禁止重写）

```java
class A {
    final void show() {
        System.out.println("A");
    }
}
```

子类不能改这个方法：

```java
class B extends A {
    void show() { } // ❌ 错误：不能重写
}
```

👉 含义：  
这个方法“定稿”，子类只能用，不能改剧情 📜

---

## 3️⃣ final 修饰类（禁止继承）

```java
final class A {
}
```

```java
class B extends A { } // ❌ 错误
```

👉 含义：  
这个类是“终极版本”，不能再被继承扩展

比如 Java 里的：

- `String` 就是 final 类（很经典）
    

---

## 🌟 一句话总结

`final` 就是 Java 里的“锁定键”：

- 变量 👉 锁值
    
- 方法 👉 锁重写
    
- 类 👉 锁继承
    

---

如果你愿意，我可以顺手帮你把 **final vs static vs const（C++）** 做一张对比图，让你一次性不再混。