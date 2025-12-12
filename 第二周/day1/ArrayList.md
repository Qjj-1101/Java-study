# ArrayList 初学者完整使用指南

## 一、核心概念（必懂）

ArrayList 是 Java 集合框架中`List` 接口的实现类，底层基于**动态数组**实现，支持自动扩容，核心特点：

- 初始状态：`new ArrayList<>()` 创建的集合底层是空数组（容量=0）；

- 首次扩容：第一次调用 `add()` 时，容量自动扩为 10；

- 后续扩容：元素数超当前容量时，按 1.5 倍扩容（10→15→22→...）；

- 适用场景：读多写少、频繁按索引访问（get/set）的场景。

## 二、前期准备（导包）

使用 ArrayList 需导入以下包（IDEA 可自动导入）：

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Iterator; // 遍历删除时需用
```

## 三、完整使用代码示例（可直接复制运行）

```java
public class ArrayListUsageDemo {
 public static void main(String[] args) {
 // 1. 三种初始化方式
 // 方式1：默认初始化（无预估容量时用）
 List<String> list1 = new ArrayList<>();
 // 方式2：指定初始容量（推荐！避免频繁扩容，比如已知存10个元素）
 List<Integer> list2 = new ArrayList<>(10);
 // 方式3：基于已有集合初始化（快速创建带初始元素的集合）
 List<String> list3 = new ArrayList<>(List.of("苹果", "香蕉", "橙子"));
 // 2. 核心操作：增删改查
 // --------------- 增（添加元素）---------------
 list1.add("Java"); // 尾部添加（最常用，效率高）
 list1.add(1, "Python"); // 指定索引添加（索引从0开始，需移动元素，效率低）
 list1.addAll(list3); // 批量添加另一个集合的所有元素
 list2.add(100); // 向整数集合添加元素
 list2.add(200);
 // --------------- 查（获取/判断元素）---------------
 String elem1 = list1.get(0); // 按索引获取元素（效率高，O(1)）
 int elem2 = list2.get(1); // 获取整数集合的第二个元素
 boolean hasJava = list1.contains("Java"); // 判断元素是否存在
 int size = list1.size(); // 获取集合中元素的实际个数
 boolean isEmpty = list1.isEmpty(); // 判断集合是否为空
 System.out.println("list1第0个元素：" + elem1);
 System.out.println("list1是否包含Java：" + hasJava);
 System.out.println("list1元素个数：" + size);
 // --------------- 改（修改元素）---------------
 list1.set(0, "Java8"); // 按索引修改元素（效率高，O(1)）
 list2.set(1, 250); // 修改整数集合的第二个元素
 System.out.println("修改后list1第0个元素：" + list1.get(0));
 // --------------- 删（删除元素）---------------
 list1.remove(1); // 按索引删除（需移动元素，效率低）
 list1.remove("苹果"); // 按元素删除（需先遍历查找，效率低）
 list2.clear(); // 清空集合（元素全删，容量不变）
 System.out.println("删除后list1元素个数：" + list1.size());
 System.out.println("清空后list2是否为空：" + list2.isEmpty());
 // 3. 三种常用遍历方式
 System.out.println("\n--- 遍历方式1：普通for循环（需索引时用）---");
 for (int i = 0; i < list3.size(); i++) {
 System.out.println(list3.get(i));
 }
 System.out.println("\n--- 遍历方式2：增强for循环（仅遍历，代码简洁）---");
 for (String s : list3) {
 System.out.println(s);
 }
 System.out.println("\n--- 遍历方式3：迭代器（遍历中删除元素的安全方式）---");
 Iterator<String> it = list3.iterator();
 while (it.hasNext()) { // 判断是否有下一个元素
 String s = it.next(); // 获取下一个元素
 if (s.equals("香蕉")) {
 it.remove(); // 安全删除，不会报错
 }
 System.out.println(s);
 }
 System.out.println("遍历删除后list3元素：" + list3);
 }
}
```

## 四、核心操作说明（重点）

| 操作     | 语法                                | 说明                           |
| ------ | --------------------------------- | ---------------------------- |
| 尾部添加   | list.add(元素)                      | 效率高（O(1)），无扩容时直接赋值           |
| 指定索引添加 | list.add(索引, 元素)                  | 效率低（O(n)），需移动后续元素            |
| 获取元素   | list.get(索引)                      | 效率高（O(1)），直接通过索引定位           |
| 修改元素   | list.set(索引, 新元素)                 | 效率高（O(1)），直接替换指定索引元素         |
| 删除元素   | list.remove(索引) / list.remove(元素) | 效率低（O(n)），需移动后续元素（按元素删还需先遍历） |
| 清空集合   | list.clear()                      | 仅置空元素（size=0），数组容量不变         |

## 五、初学者避坑指南（重中之重）

### 1. 遍历删除别踩坑

❌ 错误做法：增强for循环中用 `list.remove()`，会抛 `ConcurrentModificationException` 异常；

✅ 正确做法：用 `Iterator` 迭代器的 `it.remove()`（示例见上方代码“遍历方式3”）。

### 2. 初始化尽量指定容量

如果已知要存储的元素个数（比如10个），直接写 `new ArrayList<>(10)`，避免“空数组→扩容到10”的额外操作，提升性能。

### 3. 多线程环境别直接用

ArrayList 是非线程安全的，多线程同时修改（比如一边加一边删）会报错；解决方案：用 `Collections.synchronizedList(new ArrayList<>())` 包装后使用。

### 4. 区分“容量”和“元素个数”

- 容量：底层数组的长度（比如首次add后是10）；

- 元素个数：`list.size()` 的结果（比如只加了3个元素，size=3）；

容量是“最大能装多少”，元素个数是“实际装了多少”。

## 六、常见问题解答

- **Q：ArrayList 能存 null 值吗？** A：可以，支持存储多个 null 值（比如`list.add(null)`）。

- **Q：ArrayList 中的元素是有序的吗？** A：有序，按元素的插入顺序保存。

- **Q：ArrayList 能存重复元素吗？** A：可以，比如多次 `list.add("Java")`，会存储多个“Java”。

- **Q：什么时候用 ArrayList，什么时候用 LinkedList？** A：频繁按索引访问（get/set）用 ArrayList；频繁在首尾增删元素用 LinkedList。
