# HashMap 常用API（Java）

> 基于 `java.util.HashMap`，JDK8+

## 1. 增 / 改

```
V put(K key, V value)
```

放入键值对；**key存在则覆盖旧value，返回旧值；不存在返回null**

```
V putIfAbsent(K key, V value)
```

key不存在才放入；存在不覆盖，返回旧value

## 2. 查

```
V get(Object key)
```

获取key对应value；找不到返回`null`

```
V getOrDefault(Object key, V defaultValue)
```

找不到key时，返回默认值（JDK8）

```
boolean containsKey(Object key)
```

是否包含指定key

```
boolean containsValue(Object value)
```

是否包含指定value

## 3. 删

```
V remove(Object key)
```

删除key，返回被删的value；不存在返回null

```
boolean remove(Object key, Object value)
```

**key和value同时匹配才删除**，返回是否删除成功（JDK8）

## 4. 大小 / 判断空

```
int size()
```

键值对数量

```
boolean isEmpty()
```

是否为空

## 5. 批量操作

```
void putAll(Map<? extends K, ? extends V> m)
```

把另一个map全部合并进来

```
void clear()
```

清空所有键值对

## 6. 获取视图（遍历用）

```
Set<K> keySet()
```

返回所有key的Set集合

```
Collection<V> values()
```

返回所有value的集合

```
Set<Map.Entry<K,V>> entrySet()
```

返回键值对Entry集合，**效率最高的遍历方式**

## 7. JDK8 新增函数式API

```
void forEach(BiConsumer<? super K, ? super V> action)
```

遍历map

```
V compute(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)
```

根据key+旧value计算新值存入

```
V computeIfAbsent(K key, Function<? super K, ? extends V> mappingFunction)
```

key不存在，执行函数生成value存入（缓存常用）

```
V computeIfPresent(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)
```

key存在才执行函数更新value

```
V merge(K key, V value, BiFunction<? super V, ? super V, ? extends V> remappingFunction)
```

合并：不存在就put；存在则用旧值+新值函数计算覆盖

## 简单示例

```
HashMap<String,Integer> map = new HashMap<>();
map.put("a",1);//添加
map.putIfAbsent("b",2);//// 键不存在才添加：key="b"不存在，存入2；若key已存在则不操作
map.get("a");//查找
map.getOrDefault("c",0);//查找不到返回默认值0
map.containsKey("a");// 判断map中是否包含key="a"，返回布尔值
map.remove("a");// 删除key="a"对应的键值对，返回被删除的value
map.forEach((k,v)-> System.out.println(k+":"+v));// JDK8函数式遍历，依次拿到key和value并打印
```

## 高频考点提醒

1. HashMap 线程不安全；key允许`null`（只能一个null key），value允许多个null
2. JDK8底层：数组+链表+红黑树；链表长度>8转红黑树，<6退链表
3. `entrySet()`遍历性能 > `keySet()`二次get

要不要我顺便给你写**HashMap 4种遍历方式**对比代码？