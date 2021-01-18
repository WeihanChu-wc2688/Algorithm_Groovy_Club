# day3_二分搜索树

---

## Leetcode Review

---

[69. Sqrt(x)](https://leetcode.com/problems/sqrtx/)

```
##二分法模板：1：mid要在while里；2：确定返回结果的left还是right
class Solution(object):
    def mySqrt(self, x):
        """
        :type x: int
        :rtype: int
        """
        if x is None:
            return None
        left,right = 0, x
        
        while left+1 < right:
            mid = (left + right)/2
            if mid*mid > x:
                right = mid
            elif mid*mid == x:
                return mid
            else:
                left = mid
        if left*left == x:
            return left
        if right*right == x:
            return right
        return left
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
