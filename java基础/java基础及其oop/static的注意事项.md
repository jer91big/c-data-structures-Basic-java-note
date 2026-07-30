### 1. 静态方法不能直接访问非静态成员

> ❌ **错误：** 静态方法加载时，非静态变量还没出生。

Java

```
public class User {
    int age = 18; // 实例变量

    public static void sayHello() {
        // ❌ 编译报错！静态方法里不能直接拿非静态变量
        System.out.println("Age: " + age); 
    }
}
```

> ✅ **正确：** 必须先 `new` 出对象，通过对象去访问。

Java

```
public static void sayHello() {
    User u = new User();
    System.out.println("Age: " + u.age); // ✅ 正确
}
```

### 2. 静态方法里绝对不能写 `this` 或 `super`

> ❌ **错误：** `this` 代表当前对象，而静态方法执行时可能根本没有对象。

Java

```
public class User {
    String name;

    public static void setName(String name) {
        // ❌ 编译报错！静态方法里没有 this
        this.name = name; 
    }
}
```

### 3. 静态变量是全局共享的（警惕线程安全与数据污染）

> ⚠️ **现象：** 一个人改了静态变量，所有人手里的值都跟着变。

Java

```
public class Bank {
    public static int TotalMoney = 100; // 静态共享
}

// 张三和李四分别操作
Bank user1 = new Bank();
Bank user2 = new Bank();

user1.TotalMoney = 50; // 张三把钱改成 50
System.out.println(user2.TotalMoney); // ❌ 李四发现自己的钱也变成 50 了！

// ✅ 最佳实践：如果不希望别人改，一定要加 final 变成常量：
// public static final int MAX_LIMIT = 100;
```

### 4. 静态方法不能被“重写”（多态会失效）

> ❌ **误区：** 以为父子类静态方法同名就能实现多态。

Java

```
class Parent {
    public static void print() { System.out.println("父类静态"); }
}
class Child extends Parent {
    public static void print() { System.out.println("子类静态"); } // 这叫隐藏，不叫重写
}

// 测试调用：
Parent obj = new Child(); // 左边父类，右边子类
obj.print(); 
// ❌ 输出的是 "父类静态"！
// 静态方法的调用只看左边的“声明类型”（Parent），多态在这里失效了。
```