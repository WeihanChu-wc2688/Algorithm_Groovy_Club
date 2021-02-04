# day10: 动态规划之--坐标动规

---

## Leetcode Review 

---

[746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

```python
#一维动规模板
#状态转移方程：最后费用等于前面一步/两步path sum的最小值
#注意此题为跨过台阶，所以最后一位也要跨过，需要遍历终点+1
class Solution(object):
    def minCostClimbingStairs(self, cost):
        """
        :type cost: List[int]
        :rtype: int
        """
        if len(cost) < 2:
            return 0
        if len(cost) == 2:
            return min(cost[0], cost[1])
        dp = [0,0]
        
        for i in range(2, len(cost)+1):
            dp.append(min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2]))
        return dp[-1]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

```python
#一维动规模板：状态转移方程为当前总数=前一步总数+前两部总数，一维就是for循环所以也不用坐标记录了直接append就好
#记得初始化边界条件：1，2
class Solution(object):
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        
        if n <= 2:
            return n
        
        dp = [1,2]
        
        for i in range(n-2):
            dp.append(dp[-1]+dp[-2])
        return dp[-1]
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

[674. Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/)

```python
#思路：首先排除corner case，长度为0，1时直接返回0，1；
#     其次生成迭代指针和记录数组，然后从第一位遍历数组记录当前指针和前一位的关系，如果递增了指针加一，没递增归1，每次得指针都记录在数组里，最后返回数组最大值即可
class Solution(object):
    def findLengthOfLCIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 0:
            return 0
        if len(nums) == 1:
            return 1
        
        inc = 1
        res = []
        
        for i in range(1,len(nums)):
            if nums[i] > nums[i-1] :
                inc+=1
                res.append(inc)
            else:
                inc = 1
                res.append(inc)
                
        return max(res)
                
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[62. Unique Paths](https://leetcode.com/problems/unique-paths/)

```python
#动规模板，三步
#第一步：状态转移方程，终点等于上面和左边的步数加和
#第二步，边界条件：第一行第一列没有上边/左边，初始化为1
#第三步，输出结果为最后一行最后一列的方案综合
class Solution(object):
    def uniquePaths(self, m, n):
        """
        :type m: int
        :type n: int
        :rtype: int
        """
        if not m or not n:
            return None
        
        dp = [[0]*n for _ in range(m)]
        
        for i in range(m):
            dp[i][0] = 1
            
        for j in range(n):
            dp[0][j] = 1
        
        for i in range(1,m):
            for j in range(1,n):
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
                
        return dp[m-1][n-1]
                
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[109. Triangle](https://www.lintcode.com/problem/triangle/description)

```python
#动规自顶向下模板
#第一步：判断状态转移方程——每一个结点的pathsum等于上面连接他结点pathsum的最小值加上自己的值；
#第二部：判断边界条件：头结点和每层最左最右点的pathsum只等于各自节点上面的唯一的pathsum
#第三步：判断输出结果：最底层每个节点都是各自最小pathsum，取底层节点最小的一个就是全局最小
class Solution:
    """
    @param triangle: a list of lists of integers
    @return: An integer, minimum path sum
    """
    def minimumTotal(self, triangle):
        #不需要再判断每层列宽，因为每层列宽等于层数！
        n = len(triangle)
        #由于reference原因，需要用这种方式构建结果dp矩阵
        dp = [[0]*(w+1) for w in range(n)]
        #头结点只等于本身
        dp[0][0] = triangle[0][0]
        #边界条件的pathsum先填写
        for i in range(1,n):
            dp[i][0] = dp[i-1][0] + triangle[i][0]
            dp[i][i] = dp[i-1][i-1] + triangle[i][i]
        #处理好行列循环的起始位置！j<i!!因为三角形！
        for i in range(2,n):
            for j in range(1,i):
                dp[i][j] = min(dp[i-1][j-1],dp[i-1][j]) + triangle[i][j]
                
        return min(dp[n-1])
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

```python
#坐标dp模板变形
#如果填充边界时遇到了障碍直接break掉填充过程，后面全是0因为走不过去了
#在双重循环计算dp每个坐标的总路线时要先判断此处是否为障碍，如果是直接跳过本次循环不做操作
class Solution(object):
    def uniquePathsWithObstacles(self, obstacleGrid):
        """
        :type obstacleGrid: List[List[int]]
        :rtype: int
        """
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        dp = [[0]*n for _ in range(m)]
        
        for i in range(m):
            if obstacleGrid[i][0] == 1:
                break
            dp[i][0] = 1
        
        for i in range(n):
            if obstacleGrid[0][i] == 1:
                break
            dp[0][i] = 1
        
        for i in range(1,m):
            for j in range(1,n):
                if obstacleGrid[i][j] == 1:
                    continue
                dp[i][j] = dp[i-1][j] + dp[i][j-1]
                
        return dp[m-1][n-1]     
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

```python
#累加类型的坐标dp模板，有两点注意：

class Solution(object):
    def minPathSum(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        if not grid:
            return 0
        m = len(grid)
        n = len(grid[0])
        dp = [[0]*n for _ in range(m)]
        #！！！！！！！！！！！！！！
        #方案累加，头结点要提前设好！要不然在循环里会被遍历两次加重复一遍！
        dp[0][0] = grid[0][0]
        #因为要求path sum，边界值不是直接设定而是累加！是每一个结点上一步累加得来的！跟方案总和区别！
        for i in range(1,m):
            dp[i][0] = dp[i-1][0] + grid[i][0]
            
        for i in range(1,n):
            dp[0][i] = dp[0][i-1] + grid[0][i]
        
        for i in range(1,m):
            for j in range(1,n):
                dp[i][j] = min(dp[i-1][j],dp[i][j-1]) + grid[i][j]
                
        return dp[m-1][n-1]     
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

```python
#累加类型的坐标dp模板，有两点注意：

class Solution(object):
    def minPathSum(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        if not grid:
            return 0
        m = len(grid)
        n = len(grid[0])
        dp = [[0]*n for _ in range(m)]
        #！！！！！！！！！！！！！！
        #方案累加，头结点要提前设好！要不然在循环里会被遍历两次加重复一遍！
        dp[0][0] = grid[0][0]
        #因为要求path sum，边界值不是直接设定而是累加！是每一个结点上一步累加得来的！跟方案总和区别！
        for i in range(1,m):
            dp[i][0] = dp[i-1][0] + grid[i][0]
            
        for i in range(1,n):
            dp[0][i] = dp[0][i-1] + grid[0][i]
        
        for i in range(1,m):
            for j in range(1,n):
                dp[i][j] = min(dp[i-1][j],dp[i][j-1]) + grid[i][j]
                
        return dp[m-1][n-1]     
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
