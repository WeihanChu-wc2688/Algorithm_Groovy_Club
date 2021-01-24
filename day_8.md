# day8_双指针总结

---

## Leetcode Review - 相向双指针专题

---

[1. Two Sum](https://leetcode.com/problems/two-sum/)

```
#用字典存储index，然后直接查找差返回
class Solution(object):
    def twoSum(self, nums, target):
        h = {}
        for i,num in enumerate(nums):
            n = target - num
            if n in h:
                return [h[n],i]
            h[num]=i
```
**Analysis**

Time Complexity: 

Space Complexity: 

[167. Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

```
#经典相向双指针，用while循环逻辑找不同条件的判断模式
class Solution(object):
    def twoSum(self, numbers, target):
        """
        :type numbers: List[int]
        :type target: int
        :rtype: List[int]
        """
        left, right = 0, len(numbers)-1
        while left<= right:
            if numbers[left]+numbers[right]< target:
                left+=1
            elif numbers[left]+numbers[right]> target:
                right-=1
            else:
                return [left+1,right+1]
        return [-1,-1]
```
**Analysis**

Time Complexity: 

Space Complexity: 


