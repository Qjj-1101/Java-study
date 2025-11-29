# 两数相加（链表逆序存储）—— 带注释完整总结

## 题目回顾

给你两个**非空**链表，每个节点存一位数字，**逆序**存放。  
返回一个同样逆序的新链表，表示两数之和。

---

## 完整 Java 解答（逐行注释）

```java
/**

* Definition for singly-linked list.

* public class ListNode {

*  int val;           // 当前节点存的数字

*  ListNode next;     // 指向下一个节点

*  ListNode() {}

*  ListNode(int val) { this.val = val; }

*  ListNode(int val, ListNode next) { this.val = val; this.next = next; }

* }
  */
  class Solution {
   public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
  
       /* 1. 假头（哑结点）：避免单独处理第一个节点 */
       ListNode fakeHead = new ListNode();
       /* 2. 当前拼接待挂节点用的指针 */
       ListNode now = fakeHead;
       /* 3. 进位，初始为 0 */
       int carry = 0;
      
       /* 4. 只要还有数字或进位，就继续循环 */
       while (l1 != null || l2 != null || carry != 0) {
           /* 5. 取出两条链表当前位的值，走完就补 0 */
           int num1 = (l1 == null) ? 0 : l1.val;
           int num2 = (l2 == null) ? 0 : l2.val;
      
           /* 6. 相加并带上轮进位 */
           int sum = num1 + num2 + carry;
      
           /* 7. 计算新进位（整除 10） */
           carry = sum / 10;
           /* 8. 新建节点，只保留个位（取模 10） */
           now.next = new ListNode(sum % 10);
           /* 9. 移动当前指针到新节点 */
           now = now.next;
      
           /* 10. 两条链表分别前进一位（如果还有） */
           if (l1 != null) l1 = l1.next;
           if (l2 != null) l2 = l2.next;
       }
      
       /* 11. 返回假头的下一个节点，即真正的结果头 */
       return fakeHead.next;
  
   }
  }
  
  **总体思路**：
  
  # 链表大数运算 · 通用思路模板
  
  &gt; 适用场景：数字用链表存，逐位做加减乘除、取模、进位等模拟运算。
  
  ---
  
  ## 通用 5 步口诀
  
  **对齐补零 → 逐位运算 → 进位/借位带走 → 假头挂链 → 末尾收尾**
  
  ---
  
  ## 1. 对齐
  
  - **逆序链表**：天然个位对齐，直接同步遍历。
  - **顺序链表**：先翻转/压栈/递归到个位，再对齐。
  
  ---
  
  ## 2. 补零
  
  ```java
  int a = (l1 == null) ? 0 : l1.val; //l1.val 继续取l1中数计算
  int b = (l2 == null) ? 0 : l2.val;
  ```
