title: 剑指offer 第五天
tags: 剑指offer
category: 刷题
date: 2020-07-13 20:43:35
---
### JZ8 跳台阶
**题目描述：**
> 一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

**解法思路：**

<!--more-->
```
class Solution:
    def jumpFloor(self, number):
        # write code here
        if number <= 2:
            return number

        res, counter = 1, 0
        for i in range(1, number + 1):
            res = res + counter
            counter = res - counter

        return res
```
**代码测试：**
```
if __name__ == "__main__":
    print(Solution().jumpFloor(3))

>>> 3

```

### JZ9 变态跳台阶
**题目描述：**
> 一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

**解法思路：**

```
class Solution:
    def jumpFloor(self, number):
        # write code here
        if number <= 2:
            return number

        curr, prev = 1, 1
        for i in range(1, number):
            curr = 2 * prev
            prev = curr

        return curr
```
**代码测试：**
```
if __name__ == "__main__":
    print(Solution().jumpFloor(3))

>>> 4
```
### JZ10 矩形覆盖
**题目描述：**
> 我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？

**解法思路：**
```
class Solution:
    def rectCover(self, number):
        # write code here
        if number <= 2:
            return number

        res, counter = 1, 0
        for i in range(1, number + 1):
            res = res + counter
            counter = res - counter

        return res
```
**代码测试：**
```
if __name__ == "__main__":
    print(Solution().rectCover(4))

>>> 5
```

### JZ21 栈的压入、弹出序列
**题目描述：**
> 输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。
例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。
（注意：这两个序列的长度是相等的）

**解法思路：**
```
class Solution:
    def IsPopOrder(self, pushV, popV):
        # write code here
        pass
```

**代码测试：**