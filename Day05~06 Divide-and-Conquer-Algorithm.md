# 分治算法（Divide-and-Conquer-Algorithm）

## 1. 分治算法简介

### 1.1 分治算法的定义

> **分治算法（Divide and Conquer）**：字面上的解释是「分而治之」，就是把一个复杂的问题分成两个或更多的相同或相似的子问题，直到最后子问题可以简单的直接求解，原问题的解即子问题的解的合并。

简单来说，分治算法的基本思想就是： **把规模大的问题不断分解为子问题，使得问题规模减小到可以直接求解为止。**

![](../../images/20220413153059.png)


## 4. 分治算法的应用

### 4.1 归并排序

#### 4.1.1 题目链接

- [912. 排序数组 - 力扣（LeetCode） ](https://leetcode.cn/problems/sort-an-array/)

#### 4.1.2 题目大意

**描述**：给定一个整数数组 $nums$。

**要求**：对该数组升序排列。

**说明**：

- $1 \le nums.length \le 5 * 10^4$。
- $-5 * 10^4 \le nums[i] \le 5 * 10^4$。

**示例**：

```python
输入    nums = [5,2,3,1]
输出    [1,2,3,5]
```

#### 4.1.3 解题思路(会超时)

我们使用归并排序算法来解决这道题。

1. **分解**：将待排序序列中的 $n$ 个元素分解为左右两个各包含 $\frac{n}{2}$ 个元素的子序列。
2. **求解**：递归将子序列进行分解和排序，直到所有子序列长度为 $1$。
3. **合并**：把当前序列组中有序子序列逐层向上，进行两两合并。

使用归并排序算法对数组排序的过程如下图所示。

![](../../images/20220414204405.png)

#### 4.1.4 代码

```python
class Solution:
    def merge(self, left_arr, right_arr):           # 合并
        arr = []
        while left_arr and right_arr:               # 将两个排序数组中较小元素依次插入到结果数组中
            if left_arr[0] <= right_arr[0]:
                arr.append(left_arr.pop(0))
            else:
                arr.append(right_arr.pop(0))
                
        while left_arr:                             # 如果左子序列有剩余元素，则将其插入到结果数组中
            arr.append(left_arr.pop(0))
        while right_arr:                            # 如果右子序列有剩余元素，则将其插入到结果数组中
            arr.append(right_arr.pop(0))
        return arr                                  # 返回排好序的结果数组

    def mergeSort(self, arr):                       # 分解
        if len(arr) <= 1:                           # 数组元素个数小于等于 1 时，直接返回原数组
            return arr
        
        mid = len(arr) // 2                         # 将数组从中间位置分为左右两个数组。
        left_arr = self.mergeSort(arr[0: mid])      # 递归将左子序列进行分解和排序
        right_arr =  self.mergeSort(arr[mid:])      # 递归将右子序列进行分解和排序
        return self.merge(left_arr, right_arr)      # 把当前序列组中有序子序列逐层向上，进行两两合并。

    def sortArray(self, nums: List[int]) -> List[int]:
        return self.mergeSort(nums)
```
### 4.2 二分查找

#### 4.2.1 题目链接

- [704. 二分查找 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-search/)

#### 4.2.2 题目大意

**描述**：给定一个含有 $n$ 个元素有序的（升序）整型数组 $nums$ 和一个目标值 $target$。

**要求**：返回 $target$ 在数组 $nums$ 中的位置，如果找不到，则返回 $-1$。

**说明**：

- 假设 $nums$ 中的所有元素是不重复的。
- $n$ 将在 $[1, 10000]$ 之间。
- $-9999 \le nums[i] \le 9999$。

**示例**：

```python
输入    nums = [-1,0,3,5,9,12], target = 9
输出    4
解释    9 出现在 nums 中并且下标为 4
```

#### 4.2.3 解题思路

我们使用分治算法来解决这道题。与其他分治题目不一样的地方是二分查找不用进行合并过程，最小子问题的解就是原问题的解。

1. **分解**：将数组的 $n$ 个元素分解为左右两个各包含 $\frac{n}{2}$ 个元素的子序列。
2. **求解**：取中间元素 $nums[mid]$ 与 $target$ 相比。
   1. 如果相等，则找到该元素；
   2. 如果 $nums[mid] < target$，则递归在左子序列中进行二分查找。
   3. 如果 $nums[mid] > target$，则递归在右子序列中进行二分查找。

二分查找的的分治算法过程如下图所示。

![](../../images/20211223115032.png)

#### 4.2.4 代码

二分查找问题的非递归实现的分治算法代码如下：

```python
def binary_search(nums, target):
    left = 0
    right = len(nums)-1
    while left <= right:
        mid = left + (right-left)//2
        if nums[mid] > target:
            right = mid - 1
        elif nums[mid] < target:
            left = mid + 1
        else:
            return mid
    return -1
```

### 4.3 求指数

#### 4.3.1题目链接 [0050. Pow(x, n)](https://leetcode.cn/problems/powx-n/)

#### 4.3.2 题目大意

**描述**：给定浮点数 $x$ 和整数 $n$。

**要求**：计算 $x$ 的 $n$ 次方（即 $x^n$）。

**说明**：

- $-100.0 < x < 100.0$。
- $-2^{31} \le n \le 2^{31} - 1$。
- $n$ 是一个整数。
- $-10^4 \le x^n \le 10^4$。

**示例**：

- 示例 1：

```python
输入：x = 2.00000, n = 10
输出：1024.00000
```

- 示例 2：

```python
输入：x = 2.00000, n = -2
输出：0.25000
解释：2-2 = 1/22 = 1/4 = 0.25
```

#### 解题思想：

#### 解题代码：
```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if n < 0:
            x = 1/x
            n = -n
        if n == 0:
            return 1
        res = self.myPow(x, n//2)
        return res*res if n % 2 == 0 else res*res*x
```
### 4.4 多数元素
#### 4.4.1题目链接 [0169. 多数元素](https://leetcode.cn/problems/majority-element/)

#### 4.4.2 题目大意

**描述**：给定一个大小为 $n$ 的数组 `nums`。

**要求**：返回其中相同元素个数最多的元素。

**说明**：

- $n == nums.length$。
- $1 \le n \le 5 * 10^4$。
- $-10^9 \le nums[i] \le 10^9$。

**示例**：

- 示例 1：

```python
输入：nums = [3,2,3]
输出：3
```

- 示例 2：

```python
输入：nums = [2,2,1,1,1,2,2]
输出：2
```
#### 解题思想
初始化： 票数统计 votes = 0 ， 众数 x。
循环： 遍历数组 nums 中的每个数字 num 。
当 票数 votes 等于 0 ，则假设当前数字 num 是众数。
当 num = x 时，票数 votes 自增 1 ；当 num != x 时，票数 votes 自减 1 。
返回值： 返回 x 即可。
#### 解题代码
```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        votes = 0
        for num in nums:
            if votes == 0:
                x = num
            votes += 1 if num == x else -1
        return x
```
### 4.5 最大子数组和
#### 4.5.1题目链接 [0053. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)

#### 4.5.2 题目大意

**描述**：给定一个整数数组 `nums`。

**要求**：找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**说明**：

- **子数组**：指的是数组中的一个连续部分。
- $1 \le nums.length \le 10^5$。
- $-10^4 \le nums[i] \le 10^4$。

**示例**：

- 示例 1：

```python
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6。
```

- 示例 2：

```python
输入：nums = [1]
输出：1
```
#### 解题思想（暴力枚举）：
tmp记录当前和，max_记录最大和，遍历一遍，当tmp为负时，从当前索引数组元素重新开始
时空复杂度都为O（n）
#### 解题代码：
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        tmp = nums[0]
        max_ = tmp
        for i in range(1, len(nums)):
            if tmp > 0:
                max_ = max(max_, nums[i]+tmp, tmp)
                tmp += nums[i]
            else:
                max_ = max(max_, nums[i])
                tmp = nums[i]
        return max_
```
#### 解题思路（分治法）;
把数组分为两部分，最大子数组可能出现的情况有三种，可能出现在右边，可能出现在左边，也有可能出现在中间，左右两部分的数组都包含一部分。
#### 解题代码：
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]
        else:
            max_right = self.maxSubArray(nums[len(nums)//2:])
            max_left = self.maxSubArray(nums[:len(nums)//2])
        max_r = nums[len(nums)//2]
        tmp = 0
        for i in range(len(nums)//2, len(nums)):
            tmp += nums[i]
            max_r = max(tmp, max_r)
        max_l = nums[len(nums)//2-1]
        tmp = 0
        for i in range(len(nums)//2 - 1, -1, -1):
            tmp += nums[i]
            max_l = max(max_l, tmp)
        return max(max_left, max_right, max_l+max_r)
```

### 4.6 漂亮数组
#### 4.6.1题目链接 [0932. 漂亮数组](https://leetcode.cn/problems/beautiful-array/)
#### 4.6.2 题目大意

**描述**：给定一个整数 $n$。

**要求**：返回长度为 $n$ 的任一漂亮数组。

**说明**：

- **漂亮数组**（长度为 $n$ 的数组 $nums$ 满足下述条件）：
  - $nums$ 是由范围 $[1, n]$ 的整数组成的一个排列。
  - 对于每个 $0 \le i < j < n$，均不存在下标 $k$（$i < k < j$）使得 $2 \times nums[k] == nums[i] + nums[j]$。
- $1 \le n \le 1000$。
- 本题保证对于给定的 $n$ 至少存在一个有效答案。

**示例**：

- 示例 1：

```python
输入：n = 4
输出：[2,1,4,3]
```

- 示例 2：

```python
输入：n = 5
输出：[3,1,2,5,4]
```
#### 解题思路：
因为要满足题目所给不等式，左边的2*k一定是一个偶数，所以只需要让左边i+j为一个奇数，那么不等式必然满足。
采用分治思想分成左右两部分，左边为奇数右边为偶数，奇数个数一定为（n+1）//2，偶数个数一定为n//2，递归至整数一开始分别把左右两边的数奇数化和偶数化
#### 解题代码：
```python
class Solution:
    def beautifulArray(self, n: int) -> List[int]:
        if n == 1:  
            return [1]
        else:
            a = self.beautifulArray((n+1)//2)
            b = self.beautifulArray(n//2)
        return [2*x-1 for x in a] + [2*x for x in b]
```
### 4.7 为运算表达式设计优先级

#### 4.7.1题目链接 [0241. 为运算表达式设计优先级](https://leetcode.cn/problems/different-ways-to-add-parentheses/)

#### 4.7.2 题目大意

**描述**：给定一个由数字和运算符组成的字符串 `expression`。

**要求**：按不同优先级组合数字和运算符，计算并返回所有可能组合的结果。你可以按任意顺序返回答案。

**说明**：

- 生成的测试用例满足其对应输出值符合 $32$ 位整数范围，不同结果的数量不超过 $10^4$。
- $1 \le expression.length \le 20$。
- `expression` 由数字和算符 `'+'`、`'-'` 和 `'*'` 组成。
- 输入表达式中的所有整数值在范围 $[0, 99]$。

**示例**：

- 示例 1：

```python
输入：expression = "2-1-1"
输出：[0,2]
解释：
((2-1)-1) = 0 
(2-(1-1)) = 2
```

- 示例 2：

```python
输入：expression = "2*3-4*5"
输出：[-34,-14,-10,-10,10]
解释：
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
```
#### 解题思想：
分解：按运算符分成左右两部分，分别求解
解决：实现一个递归函数，输入算式，返回算式解
合并：根据运算符合并左右两部分的解，得出最终解
#### 解题代码：
```python
class Solution:
    def diffWaysToCompute(self, expression: str) -> List[int]:
        if expression.isdigit():
            return [int(expression)]
        res = []
        for i, char in enumerate(expression):
            if char in '+-*':
                left = self.diffWaysToCompute(expression[:i])
                right = self.diffWaysToCompute(expression[i+1:])
                for l in left:
                    for r in right:
                        if char == '+':
                            res.append(l+r)
                        elif char == '-':
                            res.append(l-r)
                        else:
                            res.append(l*r)
        return res
```

### 4.8
#### 4.8.1题目链接 [0023. 合并K个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)

####  4.8.2题目大意

**描述**：给定一个链表数组，每个链表都已经按照升序排列。

**要求**：将所有链表合并到一个升序链表中，返回合并后的链表。

**说明**：

- $k == lists.length$。
- $0 \le k \le 10^4$。
- $0 \le lists[i].length \le 500$。
- $-10^4 \le lists[i][j] \le 10^4$。
- $lists[i]$ 按升序排列。
- $lists[i].length$ 的总和不超过 $10^4$。

**示例**：

- 示例 1：

```python
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

- 示例 2：

```python
输入：lists = []
输出：[]
```
