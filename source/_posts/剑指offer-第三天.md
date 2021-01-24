title: 剑指offer 第三天
tags: 剑指offer
category: 刷题
date: 2020-07-08 22:33:32
---
### JZ11 二进制中1的个数
**题目描述：**
> 输入一个整数，输出该数32位二进制表示中1的个数。其中负数用补码表示。

**解法思路：**

<!--more-->
```
    def NumberOf1(self, n):
        # write code here
        count = 0

        if n < 0:
            n = n & 0b11111111111111111111111111111111

        while n != 0:
            count += 1
            n = n & (n - 1)

        return count
```
**代码测试：**
```
if __name__ == "__main__":
    print(Solution().NumberOf1(-11))
    
>>> 30
```

### JZ27 字符串的排列
**题目描述：**
> 输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

> 输入一个字符串，长度不超过9（可能有字符重复），字符只包括大小写字母。

**解法思路：**


```
class Solution:
    def Permutation(self, ss):
        # write code here
        pass
```

**代码测试：**
```

```
### JZ47 求1 + 2 + 3 + ... + n
**题目描述：**
> 求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

**解法思路：**
```
class Solution:
    def Sum_Solution(self, n):
        # write code here
        res = n
        try:
            res % n
            res += self.Sum_Solution(n - 1)
        except ZeroDivisionError:
            return 0

        return res


class Solution2:
    def Sum_Solution(self, n):
        # write code here
        res = n
        tmp = (res and self.Sum_Solution(n - 1))
        res += tmp

        return res
```
**代码测试：**
```
if __name__ == "__main__":
    print(Solution().Sum_Solution(5))
    print(Solution2().Sum_Solution(6))
    
>>> 15
21
```

### JZ51 构建乘积数组

**题目描述：**
> 给定一个数组A[0,1,...,n-1],请构建一个数组B[0,1,...,n-1],其中B中的元素B[i]=A[0]*A[1]*...*A[i-1]*A[i+1]*...*A[n-1]。不能使用除法。（注意：规定B[0] = A[1] * A[2] * ... * A[n-1]，B[n-1] = A[0] * A[1] * ... * A[n-2];）

**解法思路：**
```
class Solution:
    def multiply(self, A):
        # write code here
        pass
```

**代码测试：**
