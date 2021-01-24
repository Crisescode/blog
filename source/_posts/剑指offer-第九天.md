title: 剑指offer 第九天
tags: 剑指offer
category: 刷题
date: 2020-07-25 22:05:00
---
### JZ64 滑动窗口的最大值
**题目描述：**
> 给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个：
{[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}，
{2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。

这道题其实是昨天最后一道题[`JZ60`](http://localhost:4000/blog/2020/07/19/%E5%89%91%E6%8C%87offer-%E7%AC%AC%E4%B8%83%E5%A4%A9/)的升级版，其实也就是层次遍历二叉树（BFS）。层次遍历思想可以参见上一道题，只是此题需要结合二叉树的深度`depth`进行解题。在每次需要打印的地方，当深度`depth`对2取余，为0则从左到右打印，为1则反转打印。

**解法思路：**
```
class Solution:
    def maxInWindows(self, num, size):
        # write code here
        if num is None or size <= 0:
            return []

        max_list = []
        for i in range(len(num) - size + 1):
            range_ = num[i: i+size]
            max_list.append(max(range_))
        return max_list
```
**代码测试：**
```
if __name__ == "__main__":
    print(Solution().maxInWindows([2, 3, 4, 2, 6, 2, 5, 1], 2))

>>> [3, 4, 4, 6, 6, 5, 5]
```
<!--more-->
### JZ40 数组中只出现一次的数字
**题目描述：**
> 一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。

**解法思路：**

<!--more-->
```
class Solution:
    # 返回[a,b] 其中ab是出现一次的两个数字
    def FindNumsAppearOnce(self, array):
        # write code here
        if array is None:
            return

        hash_map = {}
        for num in array:
            if num in hash_map:
                hash_map[num] += 1
            else:
                hash_map[num] = 1

        res = [num for num in hash_map.keys() if hash_map[num] == 1]
        return res
```
**代码测试：**
```
if __name__ == "__main__":
    print(Solution().FindNumsAppearOnce([2, 3, 1, 5, 1, 3, 6, 11, 6, 0, 11, 5]))

>>> [2, 0]

```