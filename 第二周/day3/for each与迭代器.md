###### 🔄 Java 迭代器 (Iterator) 与 增强 for 循环

#### 1. Iterator 迭代器（唯一在遍历中删除元素的方法）

**核心作用**：提供一种统一的方式来遍历集合（List, Set 等），并在遍历过程中安全地删除元素。

**语法格式**：

java

![](https://cdn.sm.cn/static/25/04/08/6dde6cccfb252115393d782994df6a63.svg)亮色

复制

`Iterator<E> iterator = collection.iterator(); // 1. 获取迭代器 while (iterator.hasNext()) {                   // 2. 判断是否有下一个元素     E element = iterator.next();               // 3. 获取下一个元素     // ... 处理元素     // iterator.remove();                     // 4. (可选) 删除当前元素 }`

**常用方法**：

表格

| 方法          | 说明                                           |
| ----------- | -------------------------------------------- |
| `hasNext()` | 判断是否还有下一个元素，返回 `boolean`。                    |
| `next()`    | 返回下一个元素，并将游标指针移动一位。                          |
| `remove()`  | **删除** `lastRet` 指向的元素（即上一次 `next()` 返回的元素）。 |

---

#### 2. 增强 for 循环 (for-each)

**核心作用**：简化集合和数组的遍历代码，底层实际上是基于 Iterator 实现的语法糖。

**语法格式**：

java

![](https://cdn.sm.cn/static/25/04/08/6dde6cccfb252115393d782994df6a63.svg)亮色

复制

`// 遍历集合 for (ElementType element : collection) {     // ... 处理 element }  // 遍历数组 for (ElementType element : array) {     // ... 处理 element }`

---

#### 3. 核心区别对比表

表格

| 特性        | 迭代器 (Iterator)            | 增强 for 循环 (for-each) |
| --------- | ------------------------- | -------------------- |
| **代码简洁度** | 代码较长，相对繁琐                 | **代码简洁**，可读性强        |
| **删除元素**  | **支持** (使用 `it.remove()`) | **不支持** (会抛异常)       |
| **获取索引**  | 不支持 (需额外定义变量)             | **不支持**              |
| **底层实现**  | 显式调用迭代器方法                 | 底层编译后就是 Iterator     |
| **适用场景**  | 需要删除元素、复杂控制               | 仅用于遍历读取数据            |

---

#### 4. 注意事项与避坑指南

**1. 遍历时删除元素的陷阱**

- **错误做法**：在增强 for 循环中直接调用 `list.remove()`。
  - **后果**：抛出 `ConcurrentModificationException`（并发修改异常）。
- **正确做法**：
  - 使用 **Iterator** 的 `remove()` 方法。
  - 或者使用 `removeIf()` 方法（Java 8+）。

**2. Iterator.remove() 的调用规则**

- **必须**在调用 `next()` 之后调用 `remove()`。
- 不能连续调用两次 `remove()` 而中间没有调用 `next()`。
  - **后果**：抛出 `IllegalStateException`。

**3. 无法修改集合结构**

- 在使用这两种方式遍历时，都**不能**通过集合对象本身（如 `list.add()`）来修改集合的结构（增删），只能通过 Iterator 自身的方法（仅限删除）来操作。

**4. 无法获取索引**

- 如果你需要知道当前遍历到了第几个元素（索引），这两种方式都不适用，请改用普通的 `for (int i = 0; ...)` 循环。

**5. 空指针异常**

- 如果遍历的集合对象本身是 `null`，无论是 Iterator 还是 for-each，都会抛出 `NullPointerException`。建议在遍历前进行判空处理。



**核心作用**：提供一种统一的方式来遍历集合（List, Set 等），并在遍历过程中安全地删除元素。

**语法格式**：

java

![](https://cdn.sm.cn/static/25/04/08/6dde6cccfb252115393d782994df6a63.svg)亮色

复制

`Iterator<E> iterator = collection.iterator(); // 1. 获取迭代器 while (iterator.hasNext()) {                   // 2. 判断是否有下一个元素     E element = iterator.next();               // 3. 获取下一个元素     // ... 处理元素     // iterator.remove();                     // 4. (可选) 删除当前元素 }`

**常用方法**：

表格

| 方法          | 说明                                           |
| ----------- | -------------------------------------------- |
| `hasNext()` | 判断是否还有下一个元素，返回 `boolean`。                    |
| `next()`    | 返回下一个元素，并将游标指针移动一位。                          |
| `remove()`  | **删除** `lastRet` 指向的元素（即上一次 `next()` 返回的元素）。 |

---

#### 2. 增强 for 循环 (for-each)

**核心作用**：简化集合和数组的遍历代码，底层实际上是基于 Iterator 实现的语法糖。

**语法格式**：

java

![](https://cdn.sm.cn/static/25/04/08/6dde6cccfb252115393d782994df6a63.svg)亮色

复制

`// 遍历集合 for (ElementType element : collection) {     // ... 处理 element }  // 遍历数组 for (ElementType element : array) {     // ... 处理 element }`

---

#### 3. 核心区别对比表

表格

| 特性        | 迭代器 (Iterator)            | 增强 for 循环 (for-each) |
| --------- | ------------------------- | -------------------- |
| **代码简洁度** | 代码较长，相对繁琐                 | **代码简洁**，可读性强        |
| **删除元素**  | **支持** (使用 `it.remove()`) | **不支持** (会抛异常)       |
| **获取索引**  | 不支持 (需额外定义变量)             | **不支持**              |
| **底层实现**  | 显式调用迭代器方法                 | 底层编译后就是 Iterator     |
| **适用场景**  | 需要删除元素、复杂控制               | 仅用于遍历读取数据            |

---

#### 4. 注意事项与避坑指南

**1. 遍历时删除元素的陷阱**

- **错误做法**：在增强 for 循环中直接调用 `list.remove()`。
  - **后果**：抛出 `ConcurrentModificationException`（并发修改异常）。
- **正确做法**：
  - 使用 **Iterator** 的 `remove()` 方法。
  - 或者使用 `removeIf()` 方法（Java 8+）。

**2. Iterator.remove() 的调用规则**

- **必须**在调用 `next()` 之后调用 `remove()`。
- 不能连续调用两次 `remove()` 而中间没有调用 `next()`。
  - **后果**：抛出 `IllegalStateException`。

**3. 无法修改集合结构**

- 在使用这两种方式遍历时，都**不能**通过集合对象本身（如 `list.add()`）来修改集合的结构（增删），只能通过 Iterator 自身的方法（仅限删除）来操作。

**4. 无法获取索引**

- 如果你需要知道当前遍历到了第几个元素（索引），这两种方式都不适用，请改用普通的 `for (int i = 0; ...)` 循环。

**5. 空指针异常**

- 如果遍历的集合对象本身是 `null`，无论是 Iterator 还是 for-each，都会抛出 `NullPointerException`。建议在遍历前进行判空处理。



**核心作用**：提供一种统一的方式来遍历集合（List, Set 等），并在遍历过程中安全地删除元素。

**语法格式**：


// 1. 获取迭代器
Iterator<String> it = list.iterator();

// 2. 循环判断是否有下一个
while (it.hasNext()) {

    // 3. 获取下一个元素
    String item = it.next();
    System.out.println(item);
    
    // 4. 删除元素 (安全)
    if ("张三".equals(item)) {
        it.remove();
    }

}### Java 集合遍历：Iterator 与 For-Each 完全指南

**1. Iterator 迭代器**  
**核心用途**：在遍历过程中删除元素（这是唯一安全的方式）。

**代码格式**：  
// 1. 获取迭代器  
Iterator`<String>` it = list.iterator();

// 2. 循环判断  
while (it.hasNext()) {

// 3. 获取元素  
String item = it.next();  
System.out.println(item);

// 4. 条件删除 (安全操作)  
if ("要删除的内容".equals(item)) {  
it.remove(); // 只有迭代器能这样删  
}  
}

---

**2. 增强 For 循环 (For-Each)**  
**核心用途**：仅用于读取数据，代码最简洁。

**代码格式**：  
// 遍历 List  
for (String item : list) {  
System.out.println(item);  
}

// 遍历 Map (推荐方式)  
for (Map.Entry<String, Integer> entry : map.entrySet()) {  
System.out.println("Key=" + entry.getKey() + ", Value=" + entry.getValue());  
}

---

**3. 核心区别对比 (MarkText 安全版)**

**【Iterator 迭代器】**

- **优点**：功能强大。
- **缺点**：代码冗长。
- **关键能力**：**支持删除元素**。

**【增强 For 循环】**

- **优点**：代码极其简洁，可读性好。
- **缺点**：功能受限。
- **致命限制**：**严禁在循环体内调用集合的 remove() 方法**。

---

**4. 必须注意的“坑”**

**❌ 绝对禁止的做法**  
// 这样写代码会崩溃！  
for (String s : list) {  
if ("B".equals(s)) {  
list.remove(s); // 抛出 ConcurrentModificationException  
}  
}

**✅ 正确的解决方案**  
// 方案 1：使用 Iterator (最标准)  
Iterator`<String>` it = list.iterator();  
while (it.hasNext()) {  
String s = it.next();  
if ("B".equals(s)) {  
it.remove(); // 只有 it.remove() 是安全的  
}  
}

// 方案 2：使用 Java 8 的 removeIf (最简洁)  
list.removeIf(s -> "B".equals(s));

---

**5. 总结：怎么选？**

- **情况**：我只是想把数据打印出来，或者计算总和。
  - **选择**：**增强 For 循环** (for-each)。
- **情况**：我需要根据条件，把集合里的某些数据删掉。
  - **选择**：**Iterator**。
- **情况**：我需要知道当前遍历到了第几个（索引）。
  - **选择**：**普通 for 循环** (for i)。


