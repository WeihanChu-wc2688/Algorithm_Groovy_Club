# day12: 动态规划之--背包动规集合

本系列全部来自lintcode

---

## 0-1背包：

---

[92. Backpack](https://www.lintcode.com/problem/backpack/description)

```python
#经典背包01动规+滚动数组优化，熟读背诵!
#思路：1：判断边界特殊情况，这样循环时无需考虑n+1情况（数组不会越界因为0被作为异常情况排除）
#      2：创建dp全0数组，滚动优化后只需1维代表目前体积，每次循环数组迭代一遍第i个物品的背包
#      3：外循环i个物品，内循环优化为从m倒序遍历至A[i]位置，因为正序当前状态依赖的上个状态会被提前覆盖
#      4：状态转移方程被优化为max(前i个背包体积，前i个背包体积加当前物品的体积和)
#      5：返回出循环数组末位即为最大体积
class Solution:
    """
    @param m: An integer m denotes the size of a backpack
    @param A: Given n items with size A[i]
    @return: The maximum size
    """
    def backPack(self, m, A):
        if len(A) == 0 or m == 0:
            return 0
        n = len(A)
        dp = [0 for _ in range(m+1)]
        
        for i in range(n):
            #反向遍历，当前遍历终止处为A[i]，因为正向遍历j位会被提前覆盖，这里j代表了体积性质特殊
            for j in range(m, A[i] - 1, -1):
                dp[j] = max(dp[j],dp[j - A[i]] + A[i])
        return dp[m]        
                
```
**Analysis**

Time Complexity: 

Space Complexity: 

---


[125. Backpack II](https://www.lintcode.com/problem/backpack-ii/description)

```python
#滚动数组01背包模板，理解背诵！
#带价值的背包直接把状态优化方程中后部分的A[i]换成V[i]就行，这样外循环代表i个物品，内循环代表体积为j，数值即为价值
class Solution:
    """
    @param m: An integer m denotes the size of a backpack
    @param A: Given n items with size A[i]
    @param V: Given n items with value V[i]
    @return: The maximum value
    """
    def backPackII(self, m, A, V):
        if len(A) == 0 or m == 0 or len(V) == 0:
            return 0
        n = len(A)
        dp = [0 for _ in range(m+1)]
        
        for i in range(n):
            for j in range(m, A[i]-1, -1):
                dp[j] = max(dp[j], dp[j-A[i]] + V[i])
        return dp[m]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[563. Backpack V](https://www.lintcode.com/problem/backpack-v/description)

```python
class Solution:
    """
    @param nums: an integer array and all positive numbers
    @param target: An integer
    @return: An integer
    """
    def backPackV(self, nums, target):
        dp = [0 for _ in range(target+1)]
        #初始化边界，拿出0只有一种方案：啥也不拿
        dp[0] = 1
        for a in nums:
            #这次循环为倒序，每次都是找target-a的方案数最后累加回来
            for j in range(target,a-1,-1):
                #类比坐标形，当前总方案等于拿上一个物品时体积已经达到j的方案数+这次拿物品时还差当前物品体积的方案数，不需要加一因为如果拿了这次这也是一个方案里面的
                dp[j] += dp[j-a]
        return dp[target]

```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[800. Backpack IX](https://www.lintcode.com/problem/backpack-ix/description)

```python
#滚动数组优化01背包模板
#至少一个学校的概率，就意味着1个，2个……n个分别的最大概率，极其复杂
#所以只要求一个学校都没有的概率，最后用1减即为结果
#所以问题转换成背包体积为n,每个物品体积prices，每个物品价值probability的01背包
#只要把概率改为1-prob[i]即为每个学校没拿到offer的概率
#状态转移方程记得最后要换成乘法，因为同时都没收到要取概率交集
class Solution:
    """
    @param n: Your money
    @param prices: Cost of each university application
    @param probability: Probability of getting the University's offer
    @return: the  highest probability
    """
    def backpackIX(self, n, prices, probability):
        if len(prices) == 0 or len(probability) == 0 or n == 0:
            return 0
        #dp数组记得要根据背包体积最大设定而不是物品数量记清楚！
        dp = [1.0 for _ in range(n+1)]
        newp = [1.0 - probability[k] for k in range(len(probability))]
        
        for i in range(len(prices)):
            
            for j in range(n, prices[i]-1, -1):
                dp[j] = min(dp[j], dp[j - prices[i]]*newp[i])
        return 1-dp[n]

```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[1538. Card Game II](https://www.lintcode.com/problem/card-game-ii/description)

```python
#动态数组背包最大方案模板
class Solution:
    """
    @param cost: costs of all cards
    @param damage: damage of all cards
    @param totalMoney: total of money
    @param totalDamage: the damage you need to inflict
    @return: Determine if you can win the game
    """
    def cardGame(self, cost, damage, totalMoney, totalDamage):
        
        n = len(cost)
        dp = [0 for _ in range(totalMoney+1)]
        #注意处理长度坐标，如果把0异常检测了就要n+1，不检测0就n
        for i in range(n):
            for j in range(totalMoney,cost[i]-1,-1):
                dp[j] = max(dp[j],dp[j-cost[i]] + damage[i])
        
        if dp[totalMoney] >= totalDamage:
            return True
        else:
            return False
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[724. Minimum Partition](https://www.lintcode.com/problem/minimum-partition/description)

```python
#0-1背包滚动数组模板去找接近数组之和一半得最大值，如果找到最大值那和另一半能凑满数组之和得部分做差就是最小差了
class Solution:
    """
    @param nums: the given array
    @return: the minimum difference between their sums 
    """
    def findMin(self, nums):
        sumN = sum(nums)
        target = sumN // 2
        n = len(nums)
        dp = [[False] * (target + 1) for _ in range(2)]
        
        dp[0][0] = True
        
        for i in range(1, n + 1):
            dp[i % 2][0] = True
            for j in range(target + 1):
                if nums[i - 1] > j:
                    dp[i % 2][j] = dp[(i - 1) % 2][j]
                else:
                    dp[i % 2][j] = dp[(i - 1) % 2][j] or dp[(i - 1) % 2][j - nums[i - 1]]
        
        for j in range(target, -1, -1):
            if dp[n % 2][j]:
                return sumN - 2 * j
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

## 完全背包：物品无限拿
---

[440. Backpack III](https://www.lintcode.com/problem/backpack-iii/description)

```python
#滚动数组优化完全背包：
#跟0-1背包最大价值很像，但区别在于：滚动j循环时变为正向因为每个物品可以无限取用所以需要每次i循环后对j循环重新覆盖一遍
class Solution:
    """
    @param A: an integer array
    @param V: an integer array
    @param m: An integer
    @return: an array
    """
    def backPackIII(self, A, V, m):
        n = len(A)
        dp = [0 for _ in range(m+1)]
        for i in range(n):
            for j in range(A[i],m+1):
                dp[j] = max(dp[j],dp[j-A[i]] + V[i])
                
        return dp[m]
#如果正序 j 循环就可以反复放入同一物品(与backpack II问题需要倒序的情况正好相反)。 正由于同一种物品的个数无限，所以我们可以在任意容量 j 的背包尝试装入当前物品，j 从小向大枚举可以保证所有包含第 i 种物品， 体积不超过 j - Ai 的状态被枚举到。
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[562. Backpack IV](https://www.lintcode.com/problem/backpack-iv/description)

```python
#和0-1背包方案总数类似
#完全背包只需要调整内循环遍历方向因为每次都要枚举到每个物品放进去的总何无论外层i循环多少
#完全背包滚动优化也是用这种单维数组
class Solution:
    """
    @param nums: an integer array and all positive numbers, no duplicates
    @param target: An integer
    @return: An integer
    """
    def backPackIV(self, nums, target):
        dp = [0 for _ in range(target+1)]
        dp[0] = 1
        for num in nums:
            for j in range(num,target+1):
                dp[j] += dp[j-num]
        return dp[target]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[801. Backpack X](https://www.lintcode.com/problem/backpack-x/description)

Question:

You have a total of n yuan. Merchant has three merchandises and their prices are 150 yuan, 250 yuan and 350 yuan. And the number of these merchandises can be considered as infinite. After the purchase of goods need to be the remaining money to the businessman as a tip, finding the minimum tip for the merchant.

```python
#恭喜，人生第一次bug-free做完全背包dp！
#完全背包记得内循环设置为正向
#滚动数组模板记清楚
#特殊类型的多重背包，体积为cost，价值也为cost，记得识别清楚外层指针是下标还是价值！
class Solution:
    """
    @param n: the money you have
    @return: the minimum money you have to give
    """
    def backPackX(self, n):
        cost = [150,250,350]
        dp = [0 for _ in range(n+1)]
        
        for i in cost:
            for j in range(i,n+1):
                
                dp[j] = max(dp[j],dp[j-i] + i)
                
        return n - dp[n]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[749. John's backyard garden](https://www.lintcode.com/problem/johns-backyard-garden/description)

```python
#完全背包滚动数组优化模板
#想清楚初始边界条件是当没墙（x=0)的时候直接返回YES！因为不管用不用砖都能搞定
class Solution:
    """
    @param x: the wall's height
    @return: YES or NO
    """
    def isBuild(self, x):
        dp = [False for _ in range(x+1)]
        height = [3,7]
        #初始条件想明白！
        dp[0] = True
        for i in height:
            #多重背包正向循环
            for j in range(i,x+1):
                #也是模板，当前状态要么是已经铺满，要么是这个砖铺完满了，满足一个即为true
                dp[j] = dp[j] or dp[j-i]
        return 'YES' if dp[x] else 'NO'
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[700. Cutting a Rod](https://www.lintcode.com/problem/cutting-a-rod/description)

```python
#带价值完全背包滚动数组，每次切割可以重复切割上一次的长度
#如果价值数组和体积数组不同记得区分！记得外层循环要成为下标才能在内层循环中分别提取体积和价值数组中对应的数值！所以记得要从1开始循环以防价值/体积数组下标越界
class Solution:
    """
    @param prices: the prices
    @param n: the length of rod
    @return: the max value
    """
    def cutting(self, prices, n):
        
        dp = [0 for _ in range(n+1)]
        for i in range(1,n+1):
            for j in range(i,n+1):
                dp[j] = max(dp[j],dp[j-i] + prices[i-1])
        return dp[n]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[669. Coin Change](https://www.lintcode.com/problem/coin-change/description)

```python
#完全背包滚动数组求最小方案数，状态转移时前一个状态有数否则没法用min做对比
class Solution:
    """
    @param coins: a list of integer
    @param amount: a total amount of money amount
    @return: the fewest number of coins that you need to make up
    """
    def coinChange(self, coins, amount):
        inf = sys.maxsize
        dp = [inf for _ in range(amount+1)]
        dp[0] = 0
        
        for i in coins:
            for j in range(i,amount+1):
                #状态转移判断条件：要确保上一个面值有方案数，因为inf+1无意义陷入死循环
                if dp[j - i] != inf:
                    dp[j] = min(dp[j],dp[j-i]+1)
        return dp[amount] if dp[amount] != inf else -1
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

## 多重背包: 每个物品有有限个个数，不一定只有一个

---

[798. Backpack VII](https://www.lintcode.com/problem/backpack-vii/description)

Question:

Assume that you have n yuan. There are many kinds of rice in the supermarket. Each kind of rice is bagged and must be purchased in the whole bag. Given the weight, price and quantity of each type of rice, find the maximum weight of rice that you can purchase.

```python
#多！重！背包模板：J循环正向因为每个物品不是无限，所以j前再加一层每个物品数量的循环，剩下的套用0-1背包即可
class Solution:
    """
    @param n: the money of you
    @param prices: the price of rice[i]
    @param weight: the weight of rice[i]
    @param amounts: the amount of rice[i]
    @return: the maximum weight
    """
    def backPackVII(self, n, prices, weight, amounts):
        dp = [0 for _ in range(n+1)]
        for i in range(len(prices)):
            for _ in range(amounts[i]):
                for j in range(n,prices[i]-1,-1):
                    dp[j] = max(dp[j],dp[j-prices[i]]+weight[i])
        return dp[n]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[799. Backpack VIII](https://www.lintcode.com/problem/backpack-viii/description)

Question:

Given coins with different values and quantities, find out how many ways of total values in the range of [1, n] can be formed?

```python
#多重背包变形，理解为主
class Solution:
    """
    @param n: the value from 1 - n
    @param value: the value of coins
    @param amount: the number of coins
    @return: how many different value
    """
    def backPackVIII(self, n, value, amount):
        #dp在这里表达是否有被拼成i面值的可能
        dp = [0 for i in range(n + 1)]
        #面值为0可以被拼出来
        dp[0] = 1
        #记录总数的结果
        res = 0
        m = len(value)
        #外循环硬币的代表每种面值
        for i in range(m):
            #每个面值都初始化一次count，代表当前面值使用过的个数
            cnt = [0 for x in range(n + 1)]
            #完全背包内循环（因为外面有cnt来限制amount，所以用完全背包正循环）
            for j in range(value[i], n + 1):
                #当目前dp没被访问过并且上一步能拼出来并且cnt里数值在总数量范围内时更新结果
                if dp[j] == 0 and dp[j - value[i]] == 1 and cnt[j - value[i]] < amount[i]:
                    dp[j] = 1
                    res = res + 1
                    cnt[j] = cnt[j - value[i]] + 1
        return res
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
