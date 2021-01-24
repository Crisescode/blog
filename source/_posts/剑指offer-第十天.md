title: 剑指offer 第十天
date: 2020-07-27 23:23:17
tags: 剑指offer
category: 刷题
---
### JZ29 最小的K个数
**题目描述：**
> 输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4。

**解法思路：**
这题第一印象是直接将列表从小到大进行排序然后返回前K个数即可。所以本题第一种方法就是利用快排然后返回前K个数得到结果，这个方法没什么好说的，只要对快排熟悉即可。
```
    def GetLeastNumbers_Solution(self, tinput, k):
        # write code here
        if tinput == [] or k <= 0 or k > len(tinput):
            return []

        return self.quick_sort(tinput)[:k]

    def quick_sort(self, array):
        if len(array) <= 1:
            return array

        pivot = array[0]
        less_pivot = [num for num in array if num < pivot]
        more_pivot = [num for num in array if num > pivot]

        return self.quick_sort(less_pivot) + [pivot] + self.quick_sort(more_pivot)
```
**代码测试：**
```
if __name__ == "__main__":
    array = [4, 5, 1, 6, 2, 7, 3, 8]
    print(Solution().GetLeastNumbers_Solution(array, 4))

>>> [1, 2, 3, 4]
```
<!--more-->
### JZ28 数组中出现次数超过一半的数字
**题目描述：**
> 数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。
由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

**解法思路：**
很朴素的做法，利用哈希进行统计出数字出现的次数，然后再与数组的长度的一半进行比较即可得到结果。
```
class Solution:
    def MoreThanHalfNum_Solution(self, numbers):
        # write code here
        if len(numbers) == 0:
            return 0

        length = len(numbers)
        hash_map = {}
        for num in numbers:
            if num in hash_map:
                hash_map[num] += 1
            else:
                hash_map[num] = 1
        nums = [num for num in hash_map.keys() if hash_map[num] > length / 2]
        return nums[0] if nums else 0
```
**代码测试：**
```
if __name__ == "__main__":
    numbers = [1, 2, 3, 2, 2, 2, 5, 4, 2]
    print(Solution().MoreThanHalfNum_Solution(numbers))

>>> 2

```

### JZ30 连续子数组的最大和
**题目描述：**
> HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,
当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？
例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。给一个数组，返回它的最大连续子序列的和，
你会不会被他忽悠住？(子向量的长度至少是1)

**解法思路：**
动态规划方法。
```
class Solution:
    def FindGreatestSumOfSubArray(self, array):
        # write code here
        if len(array) == 0:
            return 0

        max_num = array[0]
        continuous_sum = 0
        for num in array:
            if continuous_sum > 0:
                continuous_sum += num
            else:
                continuous_sum = num
            max_num = max(continuous_sum, max_num)

        return max_num
```
**代码测试：**
```
if __name__ == "__main__":
    array = [6, -3, -2, 7, -15, 1, 2, 2]
    print(Solution().FindGreatestSumOfSubArray(array))
    
>>> 8
```
### JZ50 数组中重复的数字
**题目描述：**
> 在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。
请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2。

**解法思路：**
```
class Solution:
    # 这里要特别注意~找到任意重复的一个值并赋值到duplication[0]
    # 函数返回True/False
    def duplicate(self, numbers, duplication):
        # write code here
        if len(numbers) == 0:
            return False, 0
        stack = []
        for num in numbers:
            if num > len(numbers) - 1:
                return False, 0
            if num not in stack:
                stack.append(num)
            else:
               duplication.append(num)
               return True, duplication[0]

        return False
```

**代码测试：**
```
if __name__ == "__main__":
    # numbers = [2, 3, 1, 0, 2, 5, 3]
    numbers = [2, 1, 3, 1, 4]
    print(Solution().duplicate(numbers, []))
    
>>> (True, 1)
```
