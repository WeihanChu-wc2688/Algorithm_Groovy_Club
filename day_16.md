# day16: 数组专题--枚举隔板

---

## Leetcode Review 

---

[42. Maximum Subarray II](https://www.lintcode.com/problem/maximum-subarray-ii/description)

```python
#中心线枚举+前缀和
#因为数组不重复，而且是子数组必须连续，所以两个数组一左一右必有中心线
#所以先算出数组最大前缀，后缀和
#再算某一个位置下最大连续数组
#最后遍历枚举每一个位置左右找最大连续数组和
class Solution:
    """
    @param nums: A list of integers
    @return: An integer denotes the sum of max two non-overlapping subarrays
    """
    def maxTwoSubArrays(self, nums):
        # write your code here   
        # write your code here   
        n = len(nums)
        #先建立前缀和数组，任意位置最大连续数组
        prefix_forward = nums[:]
    	prefix_max = nums[:]
    	#处理枚举中心线左侧
        for i in range(1, n):
            #先算前缀和
            prefix_forward[i] = max(nums[i], prefix_forward[i-1] + nums[i])
            #再算任意位置最大连续前缀和
            prefix_max[i] = max(prefix_forward[i], prefix_max[i-1])
        endfix_backward = nums[:]
    	endfix_max = nums[:]
    	#处理美剧中心线右侧，具体路线同理
        for i in range(n-2, -1, -1):
            endfix_backward[i] = max(endfix_backward[i+1] + nums[i], nums[i])
            endfix_max[i] = max(endfix_backward[i], endfix_max[i+1])
        mx = -65535
        #最后枚举中心线找最大左右连续数组和
        for i in range(n - 1):
            mx = max(prefix_max[i]+endfix_max[i+1], mx)

        return mx
```
**Analysis**

Time Complexity: 

Space Complexity: 

---


[45. Maximum Subarray Difference](https://www.lintcode.com/problem/maximum-subarray-difference/description)

```python
#跟maximum subarray sum一个思路，中心线枚举，只不过要变成一边最大一边最小
#但是要注意中心线两侧的最大最小都要各找一遍再枚举找最大找以防漏过！！！！！
class Solution:
    """
    @param nums: A list of integers
    @return: An integer indicate the value of maximum difference between two substrings
    """
    def maxDiffSubArrays(self, nums):
        n = len(nums)
        forward = nums[:]
        max_forward = nums[:]
        min_forward = nums[:]
        backward = nums[:]
        max_backward = nums[:]
        min_backward = nums[:]
        for i in range(1,n):
            forward[i] = max(nums[i],forward[i-1] + nums[i])
            max_forward[i] = max(max_forward[i-1],forward[i])
        for i in range(1,n):
            forward[i] = min(nums[i],forward[i-1] + nums[i])
            min_forward[i] = min(min_forward[i-1],forward[i])
        for i in range(n-2,-1,-1):
            backward[i] = max(nums[i],backward[i+1] + nums[i])
            max_backward[i] = max(max_backward[i+1], backward[i])
        for i in range(n-2,-1,-1):
            backward[i] = min(nums[i],backward[i+1] + nums[i])
            min_backward[i] = min(min_backward[i+1], backward[i])
        mx = -sys.maxsize
        for i in range(n-1):
            mx = max(abs(max_forward[i] - min_backward[i+1]),abs(min_forward[i]- max_backward[i+1]),mx)
        return mx
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

```python
#最长回文子串，5月末算法班劝退题终于学懂了
#中心线枚举，本质为背向双指针，遍历数组每个位置起向左右扩散判断左右是否相等，奇数一次偶数一次取最大
class Solution(object):
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        if not s:
            return ""
        longest = ""
        for mid in range(len(s)):
            #奇数处理，左右指针同一个位置出发
            temp = self.helper(s,mid,mid)
            if len(temp) > len(longest):
                longest = temp
            #偶数处理，左右指针不同位置出发
            temp1 = self.helper(s,mid,mid+1)
            if len(temp1) > len(longest):
                longest = temp1
                
        return longest
    #如果左右指针不越界并且两指针数值相等就继续扩散，最终返回回文子串    
    def helper(self,s,left,right):
        #判断条件要么放在while上要么在循环中break剪枝，否则超时
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
         #注意处理数组别越界，left因为在while截止前多减了一次要加回来，right不需要是因为遍历取数组本身就取到right-1,所以+1正好抵消       
        return s[left+1:right]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[1833. pen box](https://www.lintcode.com/problem/pen-box/my-submissions)

```python
#中心线枚举另外一种应用，先双指针找中心线左右等于target的最小长度，然后枚举中心线找总和长度最小
class Solution:
    """
    @param boxes: number of pens for each box
    @param target: the target number
    @return: the minimum boxes
    """
    def minimumBoxes(self, boxes, target):
        #异常检测
        if not boxes or len(boxes) < 2 or not target or target < 0:
            return -1 
        #调用双指针函数返回中心线左每一位最小和为target的长度数组
        left_min = self.get_min_len(boxes, target)
        #省空间的求中心线右边的操作，这样不用写两边函数，先把数组调过来算最后返回值再倒回来
        new_boxes = boxes[::-1]
        right_min = self.get_min_len(new_boxes, target)
        right_min = right_min[::-1]
        ans = sys.maxsize
        n = len(boxes)
        #枚举中心线，找最小长度
        for i in range(n - 1):
            if left_min[i] == sys.maxsize or right_min[i + 1] == sys.maxsize:
                continue
            ans = min(ans, left_min[i] + right_min[i + 1])
        if ans == sys.maxsize:
            return -1
        return ans 
    #双指针函数找等于target的最小长度
    def get_min_len(self, boxes, target):
        ans = []
        left = now_sum = 0
        length = sys.maxsize
        n = len(boxes)
        #双指针，如果超过target减去左边然后左指针右移，如果等于target返回当前位置最小长度，否则就直接加右指针数值
        for right in range(n):
            now_sum += boxes[right]
            while now_sum > target:
                now_sum -= boxes[left]
                left += 1 
            if now_sum == target:
                length = min(length, right - left + 1)
            #找到最小长度append进数组
            ans.append(length)
        return ans
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[1850. Pick Apples](https://www.lintcode.com/problem/pick-apples/description)

```python
#中心线枚举+前缀和，这里由于窗口长度固定且多变需要分类讨论，但是跟maximum subarray sum不同之处也在此，因为窗口长度固定就不用正反遍历一步一步找了直接按窗口尺寸算即可
class Solution:
    """
    @param A: a list of integer
    @param K: a integer
    @param L: a integer
    @return: return the maximum number of apples that they can collect.
    """
    def PickApples(self, A, K, L):
        # write your code here

        prefix_sum = [0]
        #先求前缀和
        for n in A:
            prefix_sum.append(prefix_sum[-1]+n)
        #因为苹果没负数，所以最小值对比拿-1就行，后续取最大
        ret = -1
        
        max_left = 0
        #第一种情况：K在左，L在右，中心线在K和N-L之间枚举即可
        for i in range(K, len(prefix_sum)-L):
            #第i个循环下中心线左边k个树最多的苹果，动态更新
            max_left = max(max_left, prefix_sum[i]-prefix_sum[i-K])
            #第i个循环下，中心线左右最多苹果加和
            ret = max(ret, max_left + prefix_sum[i+L]-prefix_sum[i])
            
        max_left = 0
        #第二种情况：L在左，K在右，中心线在L和N-K之间枚举
        for i in range(L, len(prefix_sum)-K):
            max_left = max(max_left, prefix_sum[i]-prefix_sum[i-L])
            ret = max(ret, max_left + prefix_sum[i+K]-prefix_sum[i])
        #由于之前ret已经存储了，这次返回即为全局最大
        return ret
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
