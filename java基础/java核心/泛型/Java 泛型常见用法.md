# Java 泛型常见用法

> 核心：泛型 = 参数化类型，把**类型当作参数**，编译期做类型检查，避免强转、ClassCastException。

## 1. 泛型类（最常用，容器类典型）

类名后面定义 `<T>`，T 是类型形参，实例化时指定真实类型。

```
// 泛型类
public class Box<T> {
    private T data;
    public void setData(T data) { this.data = data; }
    public T getData() { return data; }
}

// 使用
Box<String> strBox = new Box<>();
Box<Integer> intBox = new Box<>();
```

- 约定命名：`T` Type，`E` Element，`K` Key，`V` Value
- 适用：工具容器、实体包装类

## 2. 泛型方法（方法独立拥有泛型，和类泛型无关）

泛型符号写在**返回值前面**。

```
public static <T> T getFirst(T[] arr) {
    if(arr == null || arr.length == 0) return null;
    return arr[0];
}

//调用，自动类型推断
String s = getFirst(new String[]{"a","b"});
```

> 重点：静态方法如果要用泛型，**必须自己声明`<T>`**，不能使用类上的泛型。

## 3. 泛型接口

接口上定义泛型，实现类两种写法：

```
//泛型接口
interface Handler<T> {
    void handle(T t);
}
```

写法 1：实现类指定具体类型

```
class StringHandler implements Handler<String>{
    @Override
    public void handle(String s) {}
}
```

写法 2：实现类继续保留泛型

```
class MyHandler<T> implements Handler<T>{
    @Override
    public void handle(T t) {}
}
```

## 4. 有界类型（限定泛型范围 `extends`）

`<T extends 类/接口>`：T 只能是该类或者它的子类，**上界限定**

```
// T只能是Number及其子类 Integer、Double
public <T extends Number> double add(T a, T b){
    return a.doubleValue() + b.doubleValue();
}
```

> ❗注意：`extends` 对类是继承，对接口是实现；**不能用 `super` 做有界类型声明！** `super` 只用在通配符。

## 5. 通配符 `?`（用于参数接收，不定义新泛型）

> `?` 代表**未知类型**，多用于方法参数，读取已有泛型对象。

### ① `? extends T` 上界通配符

`List<? extends Number>`：元素是 Number 或 Number 子类 ✅ 可以读，取出是 Number ❌ **不能 add**（除 null），不知道真实子类型

```
public void printNum(List<? extends Number> list){
    for(Number n : list) System.out.println(n);
}
```

### ② `? super T` 下界通配符

`List<? super Integer>`：元素是 Integer 或者 Integer 父类 ✅ **可以 add Integer** ❌ 取出来只能当成 Object

```
public void addInt(List<? super Integer> list){
    list.add(100);
}
```

### ③ `?` 无界通配符 `List<?>`

任意类型都可以传入；只能读成 Object，不能 add（null 除外） 适合：只遍历、不修改元素的场景

> PECS 原则（背诵）：**Producer Extends，Consumer Super** 生产者（读数据）→ `? extends` 消费者（写数据）→ `? super`

## 6. 泛型集合（开发最常用）

```
List<String> list = new ArrayList<>();
Map<String, Integer> map = new HashMap<>();
Set<Long> set = new HashSet<>();
```

作用：存入时编译校验，取出无需强转。

## 7. 泛型数组（慎用，有 unchecked 警告）

不能直接 `new T[10]`，只能 Object 数组强转

```
T[] arr = (T[]) new Object[10];
```

---

# ✅ 什么时候该用泛型？

1. 类 / 方法要支持多种类型，但逻辑完全一样（容器、工具类）
2. 想避免强制类型转换，提前在编译期捕获类型错误
3. 需要一套代码复用，同时保证类型安全

# ❌ 什么时候不要用泛型？

1. 类型固定不变
2. 静态变量不能用类泛型（静态属于类，T 属于实例）