# 04.03.01 回溯算法（第 07 ~ 09 天）

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