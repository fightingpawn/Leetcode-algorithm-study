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
