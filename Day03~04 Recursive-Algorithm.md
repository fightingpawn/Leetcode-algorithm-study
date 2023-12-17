# 递归算法（Recursive Algorithm）


## 1. 递归简介
> **递归（Recursive）**：是一种通过重复将原问题分解为同类的子问题而解决的方法。感觉思想有点类似与数学中的分步积分。

## 5. 递归的应用（Application of recursion）
### 5.1 斐波那契数
#### 5.1.1 题目链接

- [509. 斐波那契数 - 力扣（LeetCode](https://leetcode.cn/problems/fibonacci-number/)
#### 5.1.2 题目大意

**描述**：给定一个整数 $n$。

**要求**：计算第 $n$ 个斐波那契数。

**说明**：

- 斐波那契数列的定义如下：
  - $f(0) = 0, f(1) = 1$。
  - $f(n) = f(n - 1) + f(n - 2)$，其中 $n > 1$。


**示例**：

- 示例 1：

```python
输入：n = 2
输出：1
解释：F(2) = F(1) + F(0) = 1 + 0 = 1
```

- 示例 2：

```python
输入：n = 3
输出：2
解释：F(3) = F(2) + F(1) = 1 + 1 = 2
```

#### 5.1.3 解题思路

##### 思路 1：递归算法
根据我们的递推三步走策略，写出对应的递归代码。

1. 写出递推公式：$f(n) = f(n - 1) + f(n - 2)$。
2. 明确终止条件：$f(0) = 0, f(1) = 1$。
3. 翻译为递归代码：
   1. 定义递归函数：`fib(self, n)` 表示输入参数为问题的规模 $n$，返回结果为第 $n$ 个斐波那契数。
   2. 书写递归主体：`return self.fib(n - 1) + self.fib(n - 2)`。
   3. 明确递归终止条件：
      1. `if n == 0: return 0`
      2. `if n == 1: return 1`

##### 思路 1：代码

```python
class Solution:
    def fib(self, n: int) -> int:
        if n == 0:
            return 0
        if n == 1:
            return 1
        return self.fib(n - 1) + self.fib(n - 2)
```
##### 思路2：代码
```python
class Solution:
    def fib(self, n):
        if n < 0:
            raise ValueError("输入值不能小于0")
        elif n <= 1:
            return n
        
        dp = [0] * (n + 1)
        dp[1] = 1

        for i in range(2, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]

        return dp[n]
```
##### 思路3：代码
```python
class Solution:
    def fb(self, n):
        if n < 0:
            raise ValueError("输入值不能小于0")
        elif n <= 1:
            return n
        
        a, b = 0, 1
        for _ in range(2, n + 1):
            a, b = b, a + b
        return b
```

### 5.2 爬楼梯
#### 5.2.1 题目链接
- [0070. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

#### 5.1 题目大意

**描述**：假设你正在爬楼梯。需要 $n$ 阶你才能到达楼顶。每次你可以爬 $1$ 或 $2$ 个台阶。现在给定一个整数 $n$。

**要求**：计算出有多少种不同的方法可以爬到楼顶。

**说明**：

- $1 \le n \le 45$。

**示例**：

- 示例 1：

```python
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```

- 示例 2：

```python
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```
#### 枚举思路1(内存占用较多但是耗时很短)：
遍历走两步的所有可能步数，然后再进行排列组合
#### 代码1：
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        num = 1
        for i in range(1, int(n/2)+1):
            a = 1
            b = 1
            for j in range(n-i, n-2*i, -1):
                a = a * j
            for k in range(1, i+1):
                b = b * k
            num += a/b
        return int(num)
```
#### 递归思路2（会超时，思想简单）：
如果 n = 1，只有一种方法（爬1个台阶）。
如果 n = 2，有两种方法（爬两次1个台阶或一次爬2个台阶）。
#### 代码2：
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n == 1:
            return 1
        elif n == 2:
            return 2
        else:
            return self.climbStairs(n - 1) + self.climbStairs(n - 2)
```
### 5.3 翻转二叉树
#### 5.3.1 题目链接

- [0226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

#### 5.3.2 题目大意

**描述**：给定一个二叉树的根节点 $root$。

**要求**：将该二叉树进行左右翻转。

**说明**：

- 树中节点数目范围在 $[0, 100]$ 内。
- $-100 \le Node.val \le 100$。

**示例**：

- 示例 1：

![](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

```python
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

- 示例 2：

![](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

```python
输入：root = [2,1,3]
输出：[2,3,1]
```
#### 递归思路：
交换左右子树，分别递归调用
#### 代码：
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if root is None:
            return None

        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        self.invertTree(root.right)

        return root
```
### 5.4 反转链表

#### 5.4.1 题目链接 [0206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

#### 5.4.2 题目大意

**描述**：给定一个单链表的头节点 $head$。

**要求**：将该单链表进行反转。可以迭代或递归地反转链表。

**说明**：

- 链表中节点的数目范围是 $[0, 5000]$。
- $-5000 \le Node.val \le 5000$。

**示例**：

```python
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]

解释
翻转前    1->2->3->4->5->NULL
反转后    5->4->3->2->1->NULL
```
#### 5.4.3解题思想
遍历到下一个节点为空或者当前节点为空的节点，然后依次反转

#### 5.4.4解题代码
```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head == None or head.next == None:
            return head
        Listp = self.reverseList(head.next)
        head.next.next = head
        head.next = None
        return Listp
```
### 5.5 反转链表 II

#### 5.5.1 题目链接 [0092. 反转链表 II](https://leetcode.cn/problems/reverse-linked-list-ii/)

#### 5.5.2 题目大意

**描述**：给定单链表的头指针 $head$ 和两个整数 $left$ 和 $right$ ，其中 $left \le right$。

**要求**：反转从位置 $left$ 到位置 $right$ 的链表节点，返回反转后的链表 。

**说明**：

- 链表中节点数目为 $n$。
- $1 \le n \le 500$。
- $-500 \le Node.val \le 500$。
- $1 \le left \le right \le n$。

**示例**：

- 示例 1：

```python
输入：head = [1,2,3,4,5], left = 2, right = 4
输出：[1,4,3,2,5]
```

- 示例 2：

```python
输入：head = [5], left = 1, right = 1
输出：[5]
```
