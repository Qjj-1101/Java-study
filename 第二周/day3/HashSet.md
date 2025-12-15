# Java HashSet 知识点全面总结

`HashSet` 是 Java 集合框架中 `Set` 接口的典型实现类。它基于 **HashMap** 实现，具有**元素唯一性**和**无序性**的特点，是解决去重问题的首选工具。

---

## 📦 1. 核心特性

| 特性          | 说明                                                                 |
|:----------- |:------------------------------------------------------------------ |
| **底层结构**    | 基于 `HashMap` 实现，利用 HashMap 的 Key 存储元素，Value 固定为一个静态对象 (`PRESENT`)。 |
| **元素唯一**    | 不允许重复元素。通过 `hashCode()` 和 `equals()` 方法保证唯一性。                      |
| **无序性**     | 不保证元素的迭代顺序（存储顺序与插入顺序无关）。                                           |
| **允许 null** | 允许插入一个 `null` 元素。                                                  |
| **线程不安全**   | `HashSet` 本身不是线程安全的。多线程环境下需手动同步。                                   |
| **性能**      | 基于哈希表，`add`, `remove`, `contains` 操作的平均时间复杂度为 **O(1)**。            |

---

## ⚙️ 2. 底层实现原理

### 2.1 数据结构

* **JDK 7**：数组 + 链表
* **JDK 8+**：数组 + 链表 + 红黑树
  * 当链表长度 $\ge$ 8 **且** 数组长度 $\ge$ 64 时，链表转为红黑树，以提高查询效率。
  * 当红黑树节点数 $\le$ 6 时，退化回链表。

### 2.2 去重机制 (关键)

`HashSet` 判断元素是否重复的逻辑分为两步：

1. **哈希值检查**：调用 `hashCode()` 方法计算哈希值，确定元素在数组中的存储位置（桶）。
2. **内容比对**：如果哈希值相同（发生冲突），则调用 `equals()` 方法进行比对。
   * 若 `equals()` 返回 `true`，视为**重复元素**，添加失败。
   * 若 `equals()` 返回 `false`，视为**不同元素**，添加成功（链表尾插）。

> **💡 核心口诀**：**同哈同值即重复**（哈希码相同且 equals 为 true）。

---

## 🛠️ 3. 构造方法

| 构造方法                                             | 说明                                            |
|:------------------------------------------------ |:--------------------------------------------- |
| `HashSet()`                                      | 构造一个空的 HashSet。默认初始容量为 **16**，加载因子为 **0.75**。 |
| `HashSet(int initialCapacity)`                   | 构造指定初始容量的 HashSet，加载因子默认 0.75。                |
| `HashSet(int initialCapacity, float loadFactor)` | 构造指定初始容量和加载因子的 HashSet。                       |
| `HashSet(Collection<? extends E> c)`             | 构造一个包含指定集合元素的 HashSet。                        |

---

## 📝 4. 常用方法速查

| 方法                           | 说明                                         |
|:---------------------------- |:------------------------------------------ |
| `boolean add(E e)`           | 添加元素。如果集合中不存在该元素，则添加成功并返回 true；否则返回 false。 |
| `boolean remove(Object o)`   | 如果存在该元素，则将其移除并返回 true。                     |
| `boolean contains(Object o)` | 判断集合是否包含指定元素。                              |
| `int size()`                 | 返回集合中元素的数量。                                |
| `boolean isEmpty()`          | 判断集合是否为空。                                  |
| `void clear()`               | 清空集合中所有元素。                                 |
| `Iterator<E> iterator()`     | 返回一个迭代器用于遍历集合。                             |

---

## 🔄 5. 遍历方式

```java
HashSet<String> set = new HashSet<>();
set.add("A"); set.add("B");

// 方式1：增强 for 循环 (最常用)
for (String s : set) {
    System.out.println(s);
}

// 方式2：迭代器 (Iterator)
Iterator<String> it = set.iterator();
while (it.hasNext()) {
    System.out.println(it.next());
}

// 方式3：Lambda 表达式 (JDK 8+)
set.forEach(System.out::println);
```

## ⚠️ 6. 自定义对象去重 (重难点)

如果你存储的是自定义对象（如 `Student` 类），**必须重写 `hashCode()` 和 `equals()` 方法**，否则去重会失效。

- **原因**：默认的 `hashCode()` 基于内存地址生成，`equals()` 基于引用比较。两个内容相同的对象，如果地址不同，HashSet 会认为它们是不同的。
- **做法**：根据对象的属性（如 id, name）生成哈希码和进行比对。

public class Student {
    private String id;
    private String name;

    // getter/setter...
    
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student student = (Student) o;
        return Objects.equals(id, student.id) && Objects.equals(name, student.name);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }

}

---

## 🧩 7. 线程安全解决方案

`HashSet` 是非线程安全的。在多线程环境下，可以通过以下方式解决：

1. **使用 Collections 包装**：
   
   java
   
   ![](https://cdn.sm.cn/static/25/04/08/6dde6cccfb252115393d782994df6a63.svg)亮色
   
   复制
   
   `Set<String> syncSet = Collections.synchronizedSet(new HashSet<>());`

2. **使用 ConcurrentHashMap** (JDK 8+)：
   
   java
   
   ![](https://cdn.sm.cn/static/25/04/08/6dde6cccfb252115393d782994df6a63.svg)亮色
   
   复制
   
   `Set<String> concurrentSet = ConcurrentHashMap.newKeySet();`

---

## 🆚 8. Set 集合选型对比

表格

| 集合                | 底层结构          | 顺序性         | 特点                                  |
| ----------------- | ------------- | ----------- | ----------------------------------- |
| **HashSet**       | 哈希表 (HashMap) | **无序**      | 查询、增删最快 (O(1))，不保证顺序。               |
| **LinkedHashSet** | 哈希表 + 链表      | **插入顺序**    | 维护插入顺序，性能略低于 HashSet，但遍历较快。         |
| **TreeSet**       | 红黑树 (TreeMap) | **自然/定制排序** | 元素自动排序，查询/增删较慢 (O(log n))，不支持 null。 |

---

## 💡 9. 性能优化建议

1. **合理设置初始容量**：
   - 如果预估数据量很大（如 1000 条），建议初始化时指定容量，避免频繁扩容带来的性能损耗。
   - 计算公式：`初始容量 = 预计元素数量 / 加载因子` (例如：`new HashSet<>(1000 / 0.75)` ≈ `1333`)。
2. **避免哈希冲突**：
   - 重写 `hashCode()` 时，尽量让哈希值分布均匀。
3. **慎用 null**：
   - 虽然允许 null，但在多线程或复杂逻辑中容易引发 `NullPointerException`。
