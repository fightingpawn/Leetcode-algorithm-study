# 04.01.01 枚举算法（Enumeration Algorithm）
## 1、枚举算法简述

> **枚举算法（Enumeration Algorithm）:枚举算法（Enumeration algorithm）也称为暴力搜索法和穷举法，该算法就是通过遍历所有问题可能的情况来找到满足问题要求的答案。

枚举算法核心思想是通过系统遍历所有解空间来找到满足问题要求的解。

该算法的特点：

1.简单直接全面：思想简单，编程实现比较容易，遍历所有情况，如果问有解，一定可以找到所有解。

2.效率低下：时空复杂度高。

该算法适用场景：

1.无更高效解法。

2.所需内存空间较小，且时间复杂度不会很高。

3.作为复杂问题的子过程。

## 2、枚举算法解题

著名的例子：「百钱买百鸡问题」这里小小拓展为「n钱买n鸡问题」

> n钱买n鸡问题**：鸡翁一，值钱五；鸡母一，值钱三；鸡雏三，值钱一；n钱买n鸡，则鸡翁、鸡母、鸡雏各几何？

   ```python
    def buy_chicken(money, numbers):
    a = [[]]
    a.pop(0)
    for i in range(money//5+1):
        for j in range(money//3+1):
            if (numbers-i-j) % 3 == 0 and 5*i+3*j+(numbers-i-j)/3 == money:
                b = [i, j, numbers-i-j]
                a.append(b)
    return a


    print(buy_chicken(100, 100))
   ```

## 3、枚举算法应用

### 3.1 两数之和

#### 3.1.1 题目链接

- [1. 两数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/two-sum/)

#### 3.1.2 题目大意

**描述**：给定一个整数数组 $nums$ 和一个整数目标值 $target$。

**要求**：在该数组中找出和为 $target$ 的两个整数，并输出这两个整数的下标。可以按任意顺序返回答案。

**说明**：

- $2 \le nums.length \le 10^4$。
- $-10^9 \le nums[i] \le 10^9$。
- $-10^9 \le target \le 10^9$。
- 只会存在一个有效答案。

**示例**：

- 示例 1：

```python
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

- 示例 2：

```python
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

#### 3.1.3 解题思路

利用两重for循环列举数组中所有元素两两组合的情况。

#### 代码
```python
class Solution:

    def two_sum(self, nums, target):
        """
        :param nums: List[int]
        :param target: int
        :return:List[int]
        """
        for i in range(len(nums)):
            for j in range(len(nums)):
                if i != j and nums[i] + nums[j] == target:
                    return i, j
```

#### 复杂度分析
- **时间复杂度**：$O(n^2)$
- **空间复杂度**：$O(1)$

#### 利用哈希思想解答：
#### 代码
```python
class Solution:

    def two_sum(self, nums, target):
        """
        :param nums: List[int]
        :param target: int
        :return:List[int]
        """
        nums_dict = dict()
        for i in range(len(nums)):
            if target - nums[i] in nums_dict:
                return [i, nums_dict[target-nums[i]]]
            nums_dict[nums[i]] = i
```

#### 复杂度分析
- **时间复杂度**：$O(n)$
- **空间复杂度**：$O(n)

### 3.2 计数质数
#### 3.2.1 题目链接
- [204. 计数质数 - 力扣（leetcode）](https://leetcode.cn/problems/count-primes/)

#### 3.2.2 题目大意

**描述**：给定 一个非负整数 $n$。

**要求**：统计小于 $n$ 的质数数量。

**说明**：

- $0 \le n \le 5 \times 10^6$。

**示例**：

- 示例 1：

```python
输入 n = 10
输出 4
解释 小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7。
```

- 示例 2：

```python
输入：n = 1
输出：0
```
##### 解题思路
创建一个布尔数组 is_prime，索引表示数字，值表示该数字是否是质数。
从2开始遍历到 sqrt(n)，如果一个数是质数，那么它的所有倍数都不是质数。因此，将所有这些倍数的位置在 is_prime 中标记为False。
##### 枚举算法
```python
class Solution(object):
    def countPrimes(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n<=1:
            return 0
        is_prime = [True]*n
        is_prime[0] = is_prime[1] = False
        for i in range(2, int(n**0.5)+1):
            if is_prime[i]:
                for j in range(i*i, n, i):
                    is_prime[j] = False
        return sum(is_prime)
```

### 3.3 统计平方和三元组的数目

#### 3.3.1 题目链接

- [1925. 统计平方和三元组的数目 - 力扣（LeetCode）](https://leetcode.cn/problems/count-square-sum-triples/)

#### 3.3.2 题目大意

**描述**：给你一个整数 $n$。

**要求**：请你返回满足 $1 \le a, b, c \le n$ 的平方和三元组的数目。

**说明**：

- **平方和三元组**：指的是满足 $a^2 + b^2 = c^2$ 的整数三元组 $(a, b, c)$。
- $1 \le n \le 250$。

**示例**：

- 示例 1：

```python
输入 n = 5
输出 2
解释 平方和三元组为 (3,4,5) 和 (4,3,5)。
```

- 示例 2：

```python
输入：n = 10
输出：4
解释：平方和三元组为 (3,4,5)，(4,3,5)，(6,8,10) 和 (8,6,10)。
```

#### 代码（会超时）
```python
class Solution(object):
    def countTriples(self, n):
        """
        :type n: int
        :rtype: int
        """
        cnt = 0
        for i in range(1, n+1):
            for j in range(1, n+1):
                for k in range(1, n+1):
                    if i*i+j*j == k*k:
                        cnt += 1
        return cnt
```
#### 采用哈希思想代码
```python
class Solution(object):
    def countTriples(self, n):
        """
        :type n: int
        :rtype: int
        """
        cnt = 0
        tdict = dict()
        for i in range(n+1):
            tdict[i] = i
        for i in range(1, n+1):
            for j in range(1, n+1):
                if (i*i+j*j)**0.5 in tdict:
                    cnt += 1
        return cnt
```
