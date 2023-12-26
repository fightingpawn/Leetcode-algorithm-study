# 回溯算法（Backtracking-Algorithm）

## 1. 回溯算法简介

> **回溯算法（Backtracking）**：我愿称之为灵活的枚举。当它探索到某一步时发现当前的选择不满足题目要求的条件了，它就返回至上一步重新选择，放弃当前错误的选择。回溯算法就是会遍历所有可能但会及时止损，撞到了南墙就知道回头。
> 如果说枚举是海王加舔狗，那么回溯就是不做舔狗的海王
## 2. 从全排列问题开始理解回溯算法

以求解 $[1, 2, 3]$ 的全排列为例，我们来讲解一下回溯算法的过程。

1. 选择以 $1$ 为开头的全排列。
   1. 选择以 $2$ 为中间数字的全排列，则最后数字只能选择 $3$。即排列为：$[1, 2, 3]$。
   2. 撤销选择以 $3$ 为最后数字的全排列，再撤销选择以 $2$ 为中间数字的全排列。然后选择以 $3$ 为中间数字的全排列，则最后数字只能选择 $2$，即排列为：$[1, 3, 2]$。
2. 撤销选择以 $2$ 为最后数字的全排列，再撤销选择以 $3$ 为中间数字的全排列，再撤销选择以 $1$ 为开头的全排列。然后选择以 $2$ 开头的全排列。
   1. 选择以 $1$ 为中间数字的全排列，则最后数字只能选择 $3$。即排列为：$[2, 1, 3]$。
   2. 撤销选择以 $3$ 为最后数字的全排列，再撤销选择以 $1$ 为中间数字的全排列。然后选择以 $3$ 为中间数字的全排列，则最后数字只能选择 $1$，即排列为：$[2, 3, 1]$。
3. 撤销选择以 $1$ 为最后数字的全排列，再撤销选择以 $3$ 为中间数字的全排列，再撤销选择以 $2$ 为开头的全排列，选择以 $3$ 开头的全排列。
   1. 选择以 $1$ 为中间数字的全排列，则最后数字只能选择 $2$。即排列为：$[3, 1, 2]$。
   2. 撤销选择以 $2$ 为最后数字的全排列，再撤销选择以 $1$ 为中间数字的全排列。然后选择以 $2$ 为中间数字的全排列，则最后数字只能选择 $1$，即排列为：$[3, 2, 1]$。
总结一下全排列的回溯过程：

- **按顺序枚举每一位上可能出现的数字，之前已经出现的数字在接下来要选择的数字中不能再次出现。** 
- 对于每一位，进行如下几步：
  1. **选择元素**：从可选元素列表中选择一个之前没有出现过的元素。
  2. **递归搜索**：从选择的元素出发，一层层地递归搜索剩下位数，直到遇到边界条件时，不再向下搜索。
  3. **撤销选择**：一层层地撤销之前选择的元素，转而进行另一个分支的搜索。直到完全遍历完所有可能的路径。
## 3. 回溯算法的通用模板

根据上文全排列的回溯算法代码，我们可以提炼出回溯算法的通用模板，回溯算法的通用模板代码如下所示：

```python
res = []    # 存放所欲符合条件结果的集合
path = []   # 存放当前符合条件的结果
def backtracking(nums):             # nums 为选择元素列表
    if 遇到边界条件:                  # 说明找到了一组符合条件的结果
        res.append(path[:])         # 将当前符合条件的结果放入集合中
        return

    for i in range(len(nums)):      # 枚举可选元素列表
        path.append(nums[i])        # 选择元素
        backtracking(nums)          # 递归搜索
        path.pop()                  # 撤销选择

backtracking(nums)
```
## 5. 回溯算法的应用

### 5.1 子集

#### 5.1.1 题目链接

- [78. 子集 - 力扣（LeetCode）](https://leetcode.cn/problems/subsets/)

#### 5.1.2 题目大意

**描述**：给定一个整数数组 $nums$，数组中的元素互不相同。

**要求**：返回该数组所有可能的不重复子集。可以按任意顺序返回解集。

**说明**：

- $1 \le nums.length \le 10$。
- $-10 \le nums[i] \le 10$。
- $nums$ 中的所有元素互不相同。

**示例**：

- 示例 1：

```python
输入 nums = [1,2,3]
输出 [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

- 示例 2：

```python
输入：nums = [0]
输出：[[],[0]]
```
#### 解题思想：

#### 解题代码：
```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        res = []
        path = []
        def traceback(nums, index):
            res.append(path[:])
            if index >= len(nums):
                return 
            for i in range(index, len(nums)):
                path.append(nums[i])
                traceback(nums, i+1)
                path.pop()
        traceback(nums, 0)
        return res
```
### 5.2 N 皇后

#### 5.2.1 题目链接

- [51. N 皇后 - 力扣（LeetCode）](https://leetcode.cn/problems/n-queens/)

#### 5.2.2 题目大意

**描述**：给定一个整数 $n$。

**要求**：返回所有不同的「$n$ 皇后问题」的解决方案。每一种解法包含一个不同的「$n$ 皇后问题」的棋子放置方案，该方案中的 `Q` 和 `.` 分别代表了皇后和空位。

**说明**：

- **n 皇后问题**：将 $n$ 个皇后放置在 $n \times n$ 的棋盘上，并且使得皇后彼此之间不能攻击。
- **皇后彼此不能相互攻击**：指的是任何两个皇后都不能处于同一条横线、纵线或者斜线上。
- $1 \le n \le 9$。

**示例**：

- 示例 1：

```python
输入：n = 4
输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
解释：如下图所示，4 皇后问题存在 2 个不同的解法。
```

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)
#### 解题代码
```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        if not n: return 
        res = []
        board = [['.']*n for _ in range(n)]
        def isvalid(board, row, col):
            for i in range(n):
                if board[i][col] == 'Q':
                    return False
            i, j = row - 1, col - 1
            while i >= 0 and j >= 0:
                if board[i][j] == 'Q':
                    return False
                i -= 1
                j -= 1
            i, j = row - 1, col + 1
            while i >= 0 and j < n:
                if board[i][j] == 'Q':
                    return False
                i -= 1
                j += 1
            return True
        def traceback(board, row):
            if row == n:
                temp_res = []
                for temp in board:
                    temp_path = "".join(temp)
                    temp_res.append(temp_path)
                res.append(temp_res)
            for col in range(n):
                if not isvalid(board, row, col):
                    continue
                board[row][col] = 'Q'
                traceback(board, row+1)
                board[row][col] = '.'
        traceback(board, 0)
        return res
```
