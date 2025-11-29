# 两数之和（Two Sum）速记笔记

## 1. 题目一句话

在**无序数组**里找**两个不同下标** `i, j`，使得  
`nums[i] + nums[j] == target`，返回任意一组即可。

---

## 2. 方法总览

| 方法    | 思路       | 时间复杂度 | 空间复杂度 | 适用场景        |
| ----- | -------- | ----- | ----- | ----------- |
| 暴力双循环 | 枚举所有二元组  | O(n²) | O(1)  | 数据量极小或写最快版本 |
| 哈希表   | 一边扫一边存补数 | O(n)  | O(n)  | 通用、最优、面试高频  |

---

## 3. 暴力法（新手先写这个）

### 思路

1. 外层固定第一个数 `i`  
2. 内层从 `i+1` 开始找第二个数 `j`  
3. 满足和立即返回；全部扫完没找到返回空数组

### 代码

```java

public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) { // 避免重复
            if (nums[i] + nums[j] == target) {
                return new int[]{i, j};             // 找到立刻返回
            }
        }
    }
    return new int[0];                              // 没找到
}
```

**进阶：哈希表法**

```markdown
### 代码
```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();    // 纸条
    for (int i = 0; i < nums.length; i++) {
        int need = target - nums[i];                // 补数
        if (map.containsKey(need)) {                // 查到旧下标
            return new int[]{map.get(need), i};     // 返回[旧, 新]
        }
        map.put(nums[i], i);                        // 当前值入库
    }
    return new int[0];                              // 没找到
}


```


