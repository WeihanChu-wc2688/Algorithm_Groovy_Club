# day7_排列式dfs

---

## Leetcode Review

---

[46. Permutations](https://leetcode.com/problems/permutations/)

```
#排列类dfs经典模板，时间复杂度=O（方案总数*每个方案个数） = O(N!*N)。
class Solution(object):
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        #建立结果列表，临时存储排列的列表和标记是否读取得集合
        res = []
        permutation = []
        visited = set()
        self.dfs(nums,permutation,visited,res)
        return res
        
    def dfs(self,nums,permutation,visited,res):
        #递归出口：如果临时的排列列表把nums读完了就证明全排列完成了，退出递归
        if len(nums) == len(permutation):
            #每次加入结果队列的是permutation的复制，否则最后输出结果全是最后一个permutation的结果！
            res.append(list(permutation))
            return
        #在当前递归循环中遍历nums并且讲没读取过的元素加进permutation和visited做标记，再调用递归后回溯删除以防后面递归时重复计数
        for i in nums:
            if i in visited:
                continue
            #回溯操作可以在递归函数里操作：permutation+[i]，这样就不用在for循环中先append递归后再pop了
            visited.add(i)
            self.dfs(nums,permutation+[i],visited,res)
            visited.remove(i)
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[47. Permutations II](https://leetcode.com/problems/permutations-ii/submissions/)

```
#经典dfs模板，区别在于：由于要判断重复要先sort，再根据index进行dfs，访问标记从set变成一排boolean
class Solution(object):
    def permuteUnique(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        res = []
        #因为要对index进行判断
        visited = [False]*len(nums)
        permutation = []
        #有重复要先排序
        nums = sorted(nums)
        self.dfs(nums,visited,permutation,res)
        return res
    
    def dfs(self,nums,visited,permutation,res):
        if len(permutation) == len(nums):
            #要复制permutation加进去
            res.append(permutation[:])
            return
        for i in range(len(nums)):
            #！！！！
            #判断好逻辑，两种情况任一发生都要跳过循环！
            #1：visited中为True即为访问过；2：数组不越界，而且nums中当前位和上一位相同，而且visited中上一位为false，证明当前的重复数字在上一位数字递归遍历时已经全排列过了不能再重复了！
            if visited[i] or (i >0 and nums[i-1] == nums[i] and not visited[i-1]):
                continue
            visited[i] = True
            self.dfs(nums,visited,permutation+[nums[i]],res)
            visited[i] = False
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[77. Combinations](https://leetcode.com/problems/combinations/)

```
#同样复用dfs模板，递归出口变成了组合队列长度等于要求长度时就复制permutation加入res
class Solution(object):
    def combine(self, n, k):
        """
        :type n: int
        :type k: int
        :rtype: List[List[int]]
        """
        res = []
        index = 1
        permutation = []
        self.dfs(n,k,index,permutation,res)
        return res
    
    def dfs(self,n,k,index,permutation,res):
        if k == len(permutation):
            res.append(permutation[:])
            return
        for i in range(index , n+1):
            
            ##一定记住for循环里的递归指针要改成指针+1!!!!因为指针就是index的变动范围，index是死的这样每次出来的结果都是从index+1开始的！！！！
            self.dfs(n,k,i+1,permutation+[i],res)
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

```
#dfs模板，并且运用分治二叉树思维，因为必须先有左括号再有右括号，所以每次用dfs的方式先递归左“子树”加左括号再递归右“子树”加右括号并时刻更新左右括号的数量
class Solution(object):
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        res = []
        now = ''
        left = 0
        right = 0
        self.dfs(n,left,right,now,res)
        return res
    
    def dfs(self,n,left,right,now,res):
        #记得判断left&right的各种递归出口：1-数量超过n；2-左括号数量小于右边；3-左右都满足n个括号时，把now的结果append进结果，不需要回溯（因为在递归中已经自主回溯了）
        if left > n or right > n:
            return
        if left < right:
            return
        if left == n and right == n:
            res.append(now)
            
        self.dfs(n,left+1,right,now+'(',res)
        self.dfs(n,left,right+1,now+')',res)
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

```
KEYBOARD = {
    '2': 'abc',
    '3': 'def',
    '4': 'ghi',
    '5': 'jkl',
    '6': 'mno',
    '7': 'pqrs',
    '8': 'tuv',
    '9': 'wxyz',
}
##------
#dfs模板，先用字典存储每个号码的字母，然后调用递归每次对每个号码的每个字母添加下一位的每个字母，注意数据结构的处理方式！
class Solution(object):
    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        if not digits:
            return []
        res = []
        index = 0
        permutation = []
        self.dfs(digits,0,[],res)
        return res
    
    def dfs(self,digits,index,permutation,res):
        #这里的递归出口的处理方法是用null.join(permutation)，join已经实现了复制功能
        if index == len(digits):
            res.append(''.join(permutation))
            return
        #字母遍历
        for letter in KEYBOARD[digits[index]]:
            permutation.append(letter)
            #这里完成了for循环中每个字母每个号码的遍历，所以除了letter遍历，index也要+1每次遍历
            self.dfs(digits,index+1,permutation,res)
            permutation.pop()
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
