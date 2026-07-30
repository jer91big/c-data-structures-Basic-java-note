## 1. 构造器必须是私有的（Implicitly Private）

枚举的构造器默认且只能是 `private`。你不能使用 `public` 或 `protected` 来修饰枚举构造器。这是为了防止外部代码通过 `new` 关键字创建新的枚举实例，从而保证枚举常量的唯一性。

Java

```
enum Season {
    SPRING("温暖"), SUMMER("炎热");

    private final String description;

    // 正确：默认就是 private，写不写 private 都可以
    Season(String description) {
        this.description = description;
    }

    // 错误示例：如果加上 public 会编译报错
    // public Season(String description) { ... }
}
```

## 2. 无法继承其他类，但可以实现接口

因为 Java 是单继承的，而所有的枚举类在编译后都已经隐式继承了 `java.lang.Enum`，所以**枚举类不能再继承其他任何类**。不过，它们**可以实现一个或多个接口**。

Java

```
interface Behavior {
    void OnlyShow();
}

// 正确：枚举实现了 Behavior 接口
enum Light implements Behavior {
    RED, GREEN, YELLOW;

    @Override
    public void OnlyShow() {
        System.out.println("显示灯光");
    }
}
```

## 3. 比较时优先使用 `==` 而不是 `equals()`

虽然 `equals()` 也可以用，但在比较两个枚举值时，**强烈推荐使用 `==`**。

- **原因 1：** `==` 是绝对线程安全且类型安全的。
    
- **原因 2（核心）：** `==` 可以避免 `NullPointerException`（空指针异常）。如果左边的变量为 `null`，用 `equals()` 会直接崩掉，而 `==` 只会返回 `false`。
    

Java

```
Light myLight = null;

// ❌ 坏习惯：会抛出 NullPointerException
if (myLight.equals(Light.RED)) { }

//  好习惯：安全，直接返回 false
if (myLight == Light.RED) { }
```

## 4. 在 `switch` 语句中不要带上枚举类名

在 `switch` 分支中，直接写枚举常量的名称即可，**不能**写成 `ClassName.Constant` 的形式，否则编译器会报错。

Java

```
Light status = Light.RED;

switch (status) {
    case RED: // 正确：直接写 RED
        System.out.println("红灯停");
        break;
    // case Light.GREEN: // ❌ 错误：不能带上 Light.
    //     break;
}
```

## 5. 警惕 `values()` 方法的性能陷阱

`MyEnum.values()` 是一个很常用的方法，它会返回包含所有枚举常量的数组。但需要注意：**每次调用 `values()`，Java 都会在堆内存中克隆一个新的数组返回**（这是为了防止外部代码修改数组内容导致破坏枚举）。

如果在高频调用的循环体里频繁使用 `values()`，会产生大量的临时对象，加重垃圾回收（GC）的负担。

Java

```
// ❌ 糟糕的做法：在循环里反复调用 values()
for (int i = 0; i < 100000; i++) {
    for (Light light : Light.values()) { // 每次都在克隆数组！
        // 执行逻辑
    }
}

//  更好的做法：提前缓存数组
Light[] lights = Light.values();
for (int i = 0; i < 100000; i++) {
    for (Light light : lights) {
        // 执行逻辑
    }
}
```

## 6. 善用专属集合：`EnumSet` 和 `EnumMap`

如果你需要存储多个枚举值，不要盲目使用 `HashSet` 或 `HashMap`。Java 为枚举量身定制了 `EnumSet` 和 `EnumMap`，它们的内部实现是基于位运算（Bit Vector）和数组，性能极高，内存占用极小。

Java

```
import java.util.EnumSet;

public class Main {
    public static void main(String[] args) {
        // 创建一个包含特定枚举的集合，速度远超 HashSet
        EnumSet<Light> stopLights = EnumSet.of(Light.RED, Light.YELLOW);
        
        if (stopLights.contains(Light.RED)) {
            System.out.println("需要减速或停车");
        }
    }
}
```

## 总结

把枚举当成一个**自带实例限制、天生单例且安全的特殊类**去理解，记住比较用 `==`，高频循环避开 `values()`，你就能避开绝大多数的枚举坑。
