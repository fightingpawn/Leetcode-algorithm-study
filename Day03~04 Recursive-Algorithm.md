# 递归算法（Recursive Algorithm）


## 1. 递归简介
> **递归（Recursive）**：

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


