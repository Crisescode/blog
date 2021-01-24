title: 剑指offer 第二天
tags: 剑指offer
category: 刷题
date: 2020-07-07 22:37:49
---
### JZ49 把字符串转化成整数
**题目描述：**
> 将一个字符串转换成一个整数，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0
输入描述：
输入一个字符串,包括数字字母符号,可以为空
输出描述：
如果是合法的数值表达则返回该数字，否则返回0

>示例：
输入：
+2147483647
1a33
输出：
2147483647
0

**解法思路：**
使用了一个很巧妙的方法，用一个字典将字符串"0"-"9"与数字0 - 9做一个映射，然后再用数学上计算一个数字的公式得出这个数。比如：`123 = (1 * 10 + 2 ) * 10 + 3`。需要注意字符串开头的正负号，从而判断得到的数字是整数还是负数。
<!--more-->
```
class Solution:
    def StrToInt(self, s):
        # write code here
        if len(s) == 0:
            return 0

        str2num = {
            "0": 0, "1": 1, "2": 2, "3": 3, "4": 4,
            "5": 5, "6": 6, "7": 7, "8": 8, "9": 9,
            "+": 1, "-": -1
        }

        sum = 0
        sign = 1
        for c in s:
            if c in str2num:
                if c == "+":
                    sign = str2num["+"]
                    continue
                if c == "-":
                    sign = str2num["-"]
                    continue
                sum = sum * 10 + str2num[c]
            else:
                sum = 0
                break

        return sum * sign
```
**代码测试：**
```
if __name__ == "__main__":
    s = "-98210"
    print(Solution().StrToInt(s))

>>> 98210
```

### JZ7 斐波那契数列
**题目描述：**
> 大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0，第1项是1）(n<=39)。

**解法一思路：**
直接根据数学公式`f(n) = f(n - 1) + f(n - 2)`，截至条件是当`n = 1, f(n) = 1`，`n = 0, f(0) = 0`。利用**递归法**进行求解，这种方法时间复杂度很大，为`O(n^2)`，空间复杂度为`O(1)`。

```
class Solution:
    def Fibonacci(self, n):
        # write code here
        if n <= 1:
            return n
        else:
            return self.Fibonacci(n-1) + self.Fibonacci(n-2)
```

**解法二思路：** 
```
class Solution2:
    def Fibonacci(self, n):
        # write code here
        if n <= 1:
            return n

        n1, n2, n3 = 0, 1, 0
        for i in range(1, n):
            n3 = n1 + n2
            n1 = n2
            n2 = n3

        return n3
```
**解法三思路：**

```
class Solution3:
    def Fibonacci(self, n):
        # write code here
        if n <= 1:
            return n

        sum, one = 1, 0
        for i in range(1, n):
            sum = sum + one
            one = sum - one

        return sum
```

**代码测试：**
```
if __name__ == "__main__":
    print(Solution().Fibonacci(8))
    print(Solution2().Fibonacci(8))
    print(Solution3().Fibonacci(8))

>>> 21
>>> 21
>>> 21
```
### JZ48 不用加减乘除做加法
**题目描述：**
> 写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。

**解法思路：**
```
class Solution:
    def Add(self, num1, num2):
        # write code here
        pass
```
**代码测试：**
```

```