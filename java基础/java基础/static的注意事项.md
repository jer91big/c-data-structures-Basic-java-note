# 1. static 属于类，不属于对象

这是第一原则。

```
class Student {    static String school = "清华";}
```

访问：

```
Student.school;
```

而不是依赖对象。

虽然：

```
Student s = new Student();s.school;
```

能运行，但不推荐。

因为 `school` 属于整个 `Student` 类，不属于某个 `s`。

---

# 2. static 方法只能直接访问 static 成员 ⭐

这是最常考的。

看代码：

```
class Test {    int num = 10;    static int count = 20;    public static void show() {        System.out.println(count); // √        System.out.println(num);   // ×    }}
```

为什么？

因为：

- `count` 属于类
- `show()` 也是属于类

大家在同一个“类空间”。

而：

```
num
```

属于对象。

static 方法运行时可能压根没有对象。

---

口诀：

```
静态只能直接访问静态
```

注意是**直接访问**。

创建对象后仍然可以：

```
static void show() {    Test t = new Test();    System.out.println(t.num);}
```

---

# 3. static 方法中不能使用 this、super ⭐

因为：

```
this
```

代表当前对象。

```
super
```

代表父类对象部分。

但 static 方法属于类，调用时：

```
Test.show();
```

可能根本没有对象。

所以：

```
public static void show() {    System.out.println(this.num); // ×}
```

会报错。

---

# 4. 普通方法可以访问 static 成员

很多人容易反过来搞混。

看：

```
class Test {    static int a = 10;    int b = 20;    public void show() {        System.out.println(a); // √        System.out.println(b); // √    }}
```

普通方法为什么可以？

因为：

对象已经存在。

既能访问对象成员，也能访问类成员。

---

口诀：

```
普通可以找静态静态不能直接找普通
```

---

# 5. static 变量是共享的 ⭐

这个特别重要。

代码：

```
class Student {    static int count = 0;    Student() {        count++;    }}
```

测试：

```
Student s1 = new Student();Student s2 = new Student();System.out.println(Student.count);
```

输出：

```
2
```

为什么？

因为：

`count` 只有一份。

内存里像这样：

```
Student类│└── count = 2
```

而不是：

```
s1.counts2.count
```

所以 static 常用来：

- 统计人数
- 统计对象数量
- 全局共享数据

---

# 6. static 变量会随着类加载而加载

不是等：

```
new 对象
```

才出现。

而是：

```
类加载↓static成员出现↓new对象
```

例如：

```
class Test {    static {        System.out.println("类加载了");    }}
```

即使：

```
Test t = null;
```

再：

```
Test.class
```

也可能触发类加载。

---

# 7. 静态代码块只执行一次

代码：

```
class Test {    static {        System.out.println("执行");    }}
```

即使：

```
new Test();new Test();new Test();
```

输出仍：

```
执行
```

一次。

因为：

> 类只加载一次，静态代码块也只执行一次。

常用于：

- 初始化配置
- 初始化静态变量

---

# 8. 工具类一般私有构造 + 全 static ⭐

例如：

```
class MathTool {    private MathTool() {}    public static int add(int a,int b){        return a+b;    }}
```

为什么私有构造？

因为：

工具类不需要创建对象。

禁止：

```
new MathTool();
```

只允许：

```
MathTool.add(1,2);
```

像扳手，不需要“制造一把扳手对象”才能拧螺丝 🔧。

---

## 最后浓缩成考试口诀

```
1.static 属于类2.静态只能直接访问静态3.static 中没有 this、super4.普通方法都能访问5.static 变量共享一份6.static 随类加载7.静态代码块只执行一次8.工具类常用 static
```

