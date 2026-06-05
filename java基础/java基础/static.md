

---

## 1️⃣ static 修饰变量（类变量）

```
class Person {    static int count = 0; // 所有人共享    String name;          // 每个人自己的}
```

```
Person a = new Person();Person b = new Person();a.count = 5;System.out.println(b.count); // 5 ✅
```

👉 含义：

- `count` 属于类，不属于某个对象
- 无论创建多少对象，`count` 都只有一份

📌 注意：

- 普通变量（非 static）每个对象都有自己的副本
- static 变量可以和 final 结合，变成 **类常量**

```
static final double PI = 3.14159;
```

---

## 2️⃣ static 修饰方法（类方法）

```
class MathUtils {    static int add(int a, int b) {        return a + b;    }}int sum = MathUtils.add(3, 4); // ✅ 不需要new对象
```

👉 含义：

- 方法属于类，可以直接用类名调用
- static 方法里**只能访问 static 变量和 static 方法**
- 不能用 `this`（因为没有具体对象）

---

## 3️⃣ static 修饰代码块（类初始化块）

```
class A {    static {        System.out.println("类加载时执行一次");    }}
```

- 在类被加载到 JVM 时执行一次
- 常用来初始化 static 变量

---

## 4️⃣ 总结记忆法

|修饰符|作用对象|说明|
|---|---|---|
|final|变量/方法/类|锁定值/方法/类，不能改|
|static|变量/方法/代码块|所有对象共享，属于类本身|
|static + final|变量|类常量（全局不变）|

💡 小技巧：

- **final** = “不能改”
- **static** = “大家共享同一个”