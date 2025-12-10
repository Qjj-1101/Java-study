把“静态方法”与“其他方法”（即实例方法）拉到一起，用一张“对打表” + 代码示例，一眼就能记住区别。

---

### ① 一张表秒懂所有差异

| 对比维度                      | 静态方法 (`static`) | 实例方法（普通方法） |
| ------------------------- | --------------- | ---------- |
| **隶属**                    | 类本身             | 类的某个对象     |
| **调用方式**                  | `类名.方法()`       | `对象.方法()`  |
| **能不能用 `this` / `super`** | ❌ 没有 this       | ✅ 有 this   |
| **能不能直接访问实例字段**           | ❌ 必须先 new 对象    | ✅ 直接访问     |
| **能不能直接访问静态字段**           | ✅ 可以            | ✅ 可以       |
| **加载时机**                  | 类第一次加载就存在       | 创建对象后才出现   |
| **线程安全**                  | 与线程无关，自己写锁      | 同对象字段需考虑锁  |
| **典型用途**                  | 工具函数、工厂、全局计数    | 对象行为、业务逻辑  |

---

### ② 一段代码看差异

```java
class Counter {
    static int total = 0;   // 静态字段（类共享）
    int num;                // 实例字段（每对象一份）

    /* 静态方法：无 this，只能直接碰静态成员 */
    static void resetTotal() {
        total = 0;          // ✅ 可以
        // num = 0;         // ❌ 编译失败，没有 this.num
    }

    /* 实例方法：有 this，两种字段都能碰 */
    void inc() {
        num++;              // ✅ 自己的实例字段
        total++;            // ✅ 类字段
    }
}

public class Demo {
    public static void main(String[] args) {
        Counter.resetTotal();      // 静态方法：类名直接调

        Counter c1 = new Counter();
        c1.inc();                  // 实例方法：必须 new 对象
        System.out.println(Counter.total); // 1
    }
}
```

---

### ③ 一句话背下来

> **“静态方法归类管，没有 this 不碰实例；实例方法归对象管， this 在手啥都能摸。”**

把这句口诀 + 表格贴在屏幕边，再也不用 googling “static vs instance method” 了。

一句话先记住：  
**静态方法属于“类”；普通方法属于“对象”。**

把 6 条核心区别做成“一眼表”，随时能背：

| 维度               | 静态方法 (`static`) | 普通方法 (实例方法) |
| ---------------- | --------------- | ----------- |
| 1. 所属            | 类               | 对象          |
| 2. 调用方式          | `类名.方法()`       | `对象.方法()`   |
| 3. 能否访问实例变量/实例方法 | ❌ 没有 `this`     | ✅ 有 `this`  |
| 4. 能否访问静态变量/静态方法 | ✅               | ✅           |
| 5. 加载时机          | 类加载时即存在         | 创建对象后才存在    |
| 6. 典型用途          | 工具函数、工厂方法、全局计数器 | 对象行为、业务逻辑   |

代码对比例子：

```java
class Counter {
    static int total = 0;          // 类变量
    int num;                       // 实例变量

    static void showTotal(){       // 静态方法
        System.out.println(total);
        // System.out.println(num); // 编译错误！
    }

    void inc(){                    // 普通方法
        num++;
        total++;                   // 可以改静态变量
        showTotal();               // 可以调静态方法
    }
}

class Demo {
    public static void main(String[] args){
        Counter.showTotal();       // 无需 new 对象

        Counter c1 = new Counter();
        c1.inc();                  // 普通方法必须 new 对象
    }
}
```

口诀：  
“**静态无 this，类名就能点；实例有 this，先 new 再调用。**”
