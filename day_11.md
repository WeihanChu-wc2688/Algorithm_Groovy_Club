# day11: 动态规划之--序列动规

---

## Leetcode Review 

---

[1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

```python
##忘了做，复习补上！
class Solution(object):
    def longestCommonSubsequence(self, text1, text2):
        """
        :type text1: str
        :type text2: str
        :rtype: int
        """
        
```
**Analysis**

Time Complexity: 

Space Complexity: 

---


[256. Paint House](https://leetcode.com/problems/paint-house/)

```python
#思路：滚动数组优化后的dp，dp为n*3矩阵，代表第i个房子涂三种颜色的最小花费为j；
##状态转移方程：当前i房子和j颜色的最小花费 = i-1个房子非j颜色的最小花费+j颜色的花费
class Solution(object):
    def minCost(self, costs):
        """
        :type costs: List[List[int]]
        :rtype: int
        """
        n = len(costs)
        
        if n == 0:
            return 0
        #先设置无穷大数值
        INF = 0x7fffffff
        #dp存储i个房子三种颜色的最小花费
        dp = [costs[0],[0,0,0]]
        
        for i in range(1,n):
            for j in range(3):
                #把当前设为无穷大，便于后面对比找最小花费
                dp[i%2][j] = INF
                #遍历上一个房子跟j不同的颜色，不停找花费最小值，这里拿dp[i%2][j]本身对比找最小是因为它存储了当下k遍历中前一个k时前一个房子的最小花费，省空间
                for k in range(3):
                    if j != k:
                        dp[i%2][j] = min(dp[i%2][j], dp[i%2-1][k]+costs[i][j])
        #返回最后一行中最小值，也就是最后一个房子花费最小的颜色                
        return min(dp[n%2-1])
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[198. House Robber](https://leetcode.com/problems/house-robber/)

```python
#一维动规模板，状态转移方程：每个房子的总收益=前两个房子总收益+当前房子收益和前一个总收益的最大值，意味着：如果前两个房子加当前收益还不如前一个房子，那就打劫前一个放弃这个，反正放弃后当前房子总收益也是前一个房子的总收益
class Solution(object):
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums)==0:
            return 0
        if len(nums)==1:
            return nums[0]
        if len(nums)==2:
            return max(nums[0],nums[1])
        
        dp = [nums[0],max(nums[0],nums[1])]
              
        for i in range(2, len(nums)):
            dp.append(max(dp[i-1],dp[i-2]+nums[i]))
              
        return dp[-1]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[213. House Robber II](https://leetcode.com/problems/house-robber-ii/)

```python
#思路：因为是环形，头尾不能同取，所以左边：0——n-1，右边：1——n，然后左右套用之前的状态转移方程（当前收入 = max（前两个房子+当前房子，前一个房子））
class Solution(object):
    def rob(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        
        if n == 0:
            return 0
        if n == 1:
            return nums[0]
        L = [0] * n
        R = [0] * n
        #设定好初始边界条件：L,R第一位起点分别是0和1！！
        L[1] = nums[0]
        R[1] = nums[1]
        #从2起不断套用之前的模板，记得处理L和R的下标区别！
        for i in range(2,n):
            L[i] = max(L[i-2]+nums[i-1],L[i-1])
            R[i] = max(R[i-2]+nums[i],R[i-1])
        
        return max(L[n-1],R[n-1])
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[139. Word Break](https://leetcode.com/problems/word-break/solution/)

```python
##这题不难找时间补上！！！！！！
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: bool
        """
        
```
**Analysis**

Time Complexity: 

Space Complexity: 

---


[121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/submissions/)

```python
#思路：一维动态规划变形，一个指针遍历整个数组不停迭代找最低买入点（上一步的买入值和当前买入值的最小值），一个指针存储当前最大获利（上一步获利和当前卖出减去买入差值得最大值即为最大获利）
class Solution(object):
    def maxProfit(self, prices):
        if not prices:
            return 0
        res = 0
        buy = prices[0]
        for p in prices:
            buy = min(buy,p)
            res = max(res,p-buy)
        return res
            
        """
        :type prices: List[int]
        :rtype: int
        """
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
