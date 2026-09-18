

  

📽**动画场景引入**

在 JDK 1.5 之前的旧时代，内存城的储物箱都是“无锁杂货箱”（普通 Collection）。

你往箱里塞了一根香蕉 `list.add("香蕉")`，因为箱子来者不拒，它把所有东西都统一打包成盲盒 `Object`。

取东西时，打工人拉出箱子大喊：“我记得这里面是香蕉！”然后手动强转 `String fruit = (String) list.get(0);`。

万一哪个糊涂程序员偷偷塞进去了一个炸弹 `list.add(new Bomb())`，编译器根本看不出来。直到程序跑起来，打工人把炸弹当成水果一强转——“轰！”`ClassCastException`（类型转换异常）直接爆炸！

为了彻底消灭这个隐患，JVM 包工头设计了一套高科技“百变防伪标签（泛型 `<T>`）”。

当你给箱子贴上 `<String>` 标签时，箱子立刻开启安检功能：谁要是想往里塞炸弹，编译器在编译阶段就当场拉响警报，拒绝编译！

  

📖**核心概念**

**泛型（Generics）**，简单来说就是“类型的参数化”。

  

- **到底是什么**：把要存储或处理的数据类型，像参数一样传递给类或方法。比如 `List<String>` 只能装字符串，`List<Integer>` 只能装整数。
    
      
    
- **解决什么问题**：
    
      
    1. **安全保障**：把类型检查提前到了**编译期**，拒绝杂货乱入，彻底告别运行时 `ClassCastException`。
        
          
        
    2. **告别强转**：取出来时自动就是你想要的类型，代码极其干净。
        
          
        

🧠**底层原理：伪泛型与“类型擦除”（高频必考点！）**

Java 的泛型其实是一个著名的“欺骗老百姓的魔术”——类型擦除（Type Erasure）！它也被称为“伪泛型”。

  

1. **编译期的严父**：在写代码和编译的时候，编译器像个严厉的父亲，死死盯着你，确保你 `List<String>` 里只放了字符串。
    
      
    
2. **运行期的无情擦除**：但是，当代码编译成 `.class` 字节码文件后，编译器突然掏出橡皮擦，把所有 `<String>`、`<Integer>` 统统擦掉！所有的 `<T>` 又变回了最古老的 `Object`。
    
      
    
3. **为什么这么做**：因为 Java 刚出泛型时（JDK 1.5），要兼容以前写的几百亿行老的没有泛型的代码。为了让老代码也能跑，只能采用“编译期检查，运行期擦擦擦”的妥协方案。
    
      
    
4. **验证小实验**：你在运行时拿 `ArrayList<String>` 和 `ArrayList<Integer>` 去比较它们的 `getClass()`，结果会发现它们一模一样，都是 `ArrayList.class`！因为在 JVM 眼里，根本没有泛型这回事！
    
      
    

💻**基础语法&常用API**

泛型不仅能给集合用，还能自己定义：

  

Java

```
import java.util.ArrayList;
import java.util.List;

// 1. 泛型类：定义一个百变宝箱
public class GenericTheater<T> { // T 代表 Type，也可以写 E(Element), K(Key), V(Value)
    private T content;

    public void put(T item) {
        this.content = item;
    }

    public T get() {
        return content;
    }

    public static void main(String[] args) {
        // 实例化时指定为水果箱
        GenericTheater<String> fruitBox = new GenericTheater<>();
        fruitBox.put("苹果");
        String fruit = fruitBox.get(); // 自动就是 String，不需要 (String) 强转

        // 2. 通配符的使用
        // List<?> 表示未知类型（只能读，不能写）
        // List<? extends Number> 上限通配符：只能装 Number 或其子类（如 Integer、Double）
        // List<? super String> 下限通配符：只能装 String 或其父类
    }
}
```

⚠️**高频坑&易错点**

  

1. **由于“类型擦除”，不能直接 `new T()`！**
    
      
    - **坑在哪里**：在泛型类里写 `T item = new T();`
        
          
        
    - **为什么错**：编译后 `T` 都被擦成 `Object` 了，JVM 跑起来时根本不知道 `T` 到底是个啥真实类，无法为其开辟具体的内存空间！
        
          
        
2. **基本数据类型不能直接当泛型参数！**
    
      
    - **坑在哪里**：`List<int> list = new ArrayList<>();`
        
          
        
    - **为什么错**：因为类型擦除后 `T` 变成 `Object`，而 `int`、`double` 这种基本类型根本不是 `Object` 的子类！
        
          
        
    - **正确姿势**：必须使用基本类型的**包装类**，如 `List<Integer>`、`List<Double>`。
        
          
        

✅**课堂小结**

  

- **核心作用**：编译期校验，消除手动类型强转，防止类型转换异常。
    
      
    
- **底层机制**：**类型擦除（伪泛型）**，只在编译期有效，运行期所有 `T` 恢复成 `Object`（或指定的上限）。
    
      
    
- **语法要点**：必须使用包装类，不能直接 `new T()`。
    
      
    
