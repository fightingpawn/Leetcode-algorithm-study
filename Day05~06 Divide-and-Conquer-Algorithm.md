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
