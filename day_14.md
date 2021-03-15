# day14: 数组专题--前缀和

---

## Leetcode Review 

---

[560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

```python
#同样用前缀和算法，先开字典记录差为0时要+1，后面循环遍历数组如果看到prefix-k 出现在字典里count就加1！
#注意prefix_sum - k的意义在于既能找到前缀和等于k的组合，也能找到不从原点出发的subarray等于k的组合！
class Solution(object):
    def subarraySum(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        count = 0 
        prefix_sum = 0
        #索引为每一位的前缀和，数值为出现次数
        prefix_sum_to_times = {0: 1}
        
        for num in nums:
            
            prefix_sum += num 
            #prefix-k的目的是既能找到原点出发等于k的，也能找到subarray等于k的！不受原点限制
            if prefix_sum - k in prefix_sum_to_times:
                
                count += prefix_sum_to_times[prefix_sum - k]
            #如果prefix_sum在字典中出现过，那个索引对应的数值+1,否则创建新索引    
            if prefix_sum in prefix_sum_to_times:
                
                prefix_sum_to_times[prefix_sum] += 1 
                
            else:
                
                prefix_sum_to_times[prefix_sum] = 1 
            
        return count
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[206. Interval Sum](https://www.lintcode.com/problem/interval-sum/description)

```python
#前缀和解法：因为要O(logn)复杂度无法分层for遍历，所以先创建前缀和数组然后阶段性取index做差
#先建立多一位的数组for循环存储A的前缀和，再遍历每个query的index做差求加和
"""
Definition of Interval.
class Interval(object):
    def __init__(self, start, end):
        self.start = start
        self.end = end
"""

class Solution:
    """
    @param A: An integer list
    @param queries: An query list
    @return: The result list
    """
    def intervalSum(self, A, queries):
        prefix = [0 for _ in range(len(A)+1)]
        #记得处理数组界限！！防止越界
        for i in range(1,len(prefix)):
            prefix[i] = prefix[i-1] + A[i-1]
        res = []
        for par in queries:
            left = par.start
            #记得处理数组界限！右边要+1
            right = par.end + 1
            res.append(prefix[right] - prefix[left])
        return res
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

```python
#贪心/dp，连续加和如果只返回极值/方案数得话可以考虑动规！
#这里连续累加大于0的前一项，不停更新数组中连续n位累加和，最后返回最大值即可
class Solution:
    def maxSubArray(self, nums):
        for i in range(1,len(nums)):
            if nums[i-1]>0:
                nums[i]+=nums[i-1]
        return max(nums)

        
        """
        :type nums: List[int]
        :rtype: int
        """
        
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[44. Minimum Subarray](https://www.lintcode.com/problem/minimum-subarray/description)

```python
#和max一样，贪心查找前一位小于0的累加和，记得处理边界情况！
class Solution:
    """
    @param: nums: a list of integers
    @return: A integer indicate the sum of minimum subarray
    """
    def minSubArray(self, nums):
        #处理边界情况
        if len(nums) == 0:
            return 0
        if len(nums) == 1:
            return nums[0]
        n = len(nums)
        for i in range(1,n):
            #一定要前一位小于0才能累加而不是当前位置
            if nums[i - 1] < 0:
                nums[i]+=nums[i-1]
        return min(nums)
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[523. Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)

```python
#本题要分两种情况，因为如果想找k或者k的倍数要用累加和取k余数但是当k=0时无法取余！所以要分0和非0讨论
#当k=0时直接找数组中是否有连续两个0或以上就可以
#当不等于0时创建数组建立每一位累加和跟k的余数，当出现一次余数相同而且位数之差大于1即为找到，否则没有
#解释：余数重合意味着: n*k + mod = cum_sum; (n+m)*k + mod = cum_sum_new ===> cum_sum_new - cum_sum = m*k
#意味着余数相同，之间必会出现一段连续加和为k或k倍数的数组！！！！即使子数组不是从数组头部计算也可以这样取到
class Solution(object):
    def checkSubarraySum(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: bool
        """
        n = len(nums)
        if n < 2:
            return False
        elif k == 0:
            for i in range(1,n):
                if nums[i] == 0 and nums[i-1] == 0:
                    return True
            return False
        
        else:
            mods, cum_sum_mod_k = {0: -1}, 0
            for i, n in enumerate(nums):
                cum_sum_mod_k = (cum_sum_mod_k + n) % k
                if cum_sum_mod_k in mods and i - mods[cum_sum_mod_k] > 1:
                    return True
                if cum_sum_mod_k not in mods:
                    mods[cum_sum_mod_k] = i
            return False
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

```python
#前缀和的变形：前缀积！！
#数组中每个位置除了本身的乘积等于他左边乘积和右边乘积的乘积！
#所以只要左边算一遍前缀积，右边算一遍前缀，然后直接相乘就可以了！
class Solution:
    # @param {integer[]} nums
    # @return {integer[]}
    def productExceptSelf(self, nums):
        head = 1
        n = len(nums)
        output = []
        for i in range(0,n):
            output.append(head)
            head = head * nums[i]
        tail = 1
        for i in range(n-1,-1,-1):
            output[i] = output[i] * tail
            tail = tail * nums[i]
        return output
        
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

