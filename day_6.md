# day6_排列式dfs

---

## Leetcode Review

---

[113. Path Sum II](https://leetcode.com/problems/path-sum-ii/)

```
##-----------------
##dfs找路径比bfs快因为要遍历路径而不是找最短路径！！树的结构不需要for循环排序数组，而是通过递归出口的因果逻辑实现，不停的通过递归寻找目标数值减去当前结点数值的方式遍历查找所有可能的路径
class Solution(object):
    def pathSum(self, root, sum):
        """
        :type root: TreeNode
        :type sum: int
        :rtype: List[List[int]]
        """
        res = []
        subset = []
        
        self.dfs(root,subset,sum,res)
        return res
    def dfs(self,root,subset,target,res):
        #如果传入的是一个class,要先异常判断！
        if not root:
            return
        target-=root.val
        subset.append(root.val)
        #如果走到叶子并且target刚好被减完：返回复制的subset以防重复（因为要找路径遍历时会重复）
        if not root.left and not root.right and target==0:
            res.append(subset[:])
        #要加因果逻辑！因为如果走到头后就不进行递归了，要设置递归出口！跟list的递归遍历不同的是:list使用for循环进行遍历所以循环终点为index大于nums终点，但树的递归出口是走到叶子所以如果if判断后面的递归需要加else!!!!
        else:
            self.dfs(root.left,subset,target,res)
            self.dfs(root.right,subset,target,res)
        #因为要找路径（找不到还会往回找），而不是遍历，所以要pop!
        subset.pop()
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[437. Path Sum III](https://leetcode.com/problems/path-sum-iii/)

```
##----------------
##和普通求path区别：普通求Path数量直接dfs然后把每个结点存在结果list里套模板即可，因为确定要从头走到尾；
##在本题当中，因为不确定是否走到尾，所以不用回溯存储结果，要用dfs先遍历结点，在遍历每个节点时再用一遍dfs来寻找当前结点path数量，所以是递归嵌套递归来确保每个结点都有不同长度的path等于结果
class Solution:
    """
    @param root: 
    @param sum: 
    @return: the num of sum path
    """
    def pathSum(self, root, sum):
        #定义累加函数，dfs遍历结点找满足加和为sum得路线数量
        if not root: return 0
        #注意全局变量！如果在其他function用要设置成全局变量
        self.count = 0
        self.dfs(root, sum)
        return self.count
    
    def dfs(self,root, sum):
        if not root: return
        #dfs每个节点，在每个结点处再次调用dfs进行递归计算
        self.dfs(root.left, sum)
        self.helper(root, sum)
        self.dfs(root.right, sum)
    
    def helper(self, root, sum):
        #最终对于每个结点，dfs求满足的path个数
        if not root: return
        if root.val==sum: self.count+=1
        self.helper(root.left, sum-root.val)
        self.helper(root.right, sum-root.val)
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[437. Path Sum III](https://leetcode.com/problems/sum-root-to-leaf-numbers/)

```
#使用递归完成。我们要做的是 把左右子树的path加起来。
#base case: root是叶子节点时(，直接计算path。我们用pre保留上一层计算出来的数字的值。那么到了当前根节点应该就是
#把上一层的值*10再加上root的值。
class Solution(object):
    """
    @param root: the root of the tree
    @return: the total sum of all root-to-leaf numbers
    """
    def sumNumbers(self, root):
        # write your code here
        return self.dfs(root, 0);
    def dfs(self, root, prev):
        #判断异常
        if(root == None) :
            return 0;
        #递归出口：叶子结点时加和结果=当前值+前一步数值*10
        sum = root.val + prev * 10;
        
        if(root.left == None and root.right == None) :
            return sum;
        #分治结果：左右子树结果相加
        #这里的sum就是dfs函数定义里面的prev
        return self.dfs(root.left, sum) + self.dfs(root.right, sum);
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[298. Binary Tree Longest Consecutive Sequence](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence/)

```
#遵循分治思想：在递归中的顺序：先设定递归出口--设定左右子树分治--根据题目条件对当前节点左右子树进行处理--最后统计最终结果，和全局变量对比结果输出
class Solution:
    """
    @param: root: the root of binary tree
    @return: the length of the longest consecutive sequence path
    """
    
    def longestConsecutive(self, root):
        #设计全局变量为后续helper使用
        self.longest = 0
        self.helper(root)
        return self.longest
        
    def helper(self, root):
        #设计递归出口
        if root == None:
            return 0
        #分治法设计左右两子树分别递归计算求结果
        left = self.helper(root.left)
        right = self.helper(root.right)
        #设定初始长度为1
        subLongest = 1
        #如果左孩子非空而且和它的左边就差1，就返回1和左孩子左子树的最大值
        if root.left != None and root.val + 1 == root.left.val:
            ##记住对比的是left+1因为1是本层的孩子！！
            subLongest = max(subLongest, left + 1)
        #右边同理
        if root.right != None and root.val + 1 == root.right.val:
            subLongest = max(subLongest, right + 1)
        #最后和全局0对比，如果大于就改数
        if subLongest > self.longest:
            self.longest = subLongest
            
        return subLongest
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[549. Binary Tree Longest Consecutive Sequence II](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence-ii/)

```
#依旧使用分治，不过这次不分递增递减所以要在每次分治左右子树处理中分情况讨论递增递减
class Solution(object):
    def longestConsecutive(self, root):
        if not root:
            return 0
        #使用全局变量
        self.length = 1
        self.search(root)
        return self.length
    
    def search(self, root):
        #定义递归出口
        if not root:
            return 
        #定义上界（比父亲小的长度）和下界（比父亲大的长度）
        up_length = 1
        down_length = 1
        #分治开始的左右：
        left = self.search(root.left)
        right = self.search(root.right)
        #处理左儿子
        if root.left:
            #如果左儿子比爹小，返回上界和左子树上界长度+1（因为要算进本层的相邻点）
            if root.left.val - 1 == root.val:
                up_length = max(up_length, left[0] + 1)
            #如果左儿子比爹大，返回下界和左子树下界长度+1
            if root.left.val + 1 == root.val:
                down_length = max(down_length, left[1] + 1)
        #右边处理同理
        if root.right:
            if root.right.val - 1 == root.val:
                up_length = max(up_length, right[0] + 1)
            if root.right.val + 1 == root.val:
                down_length = max(down_length, right[1] + 1)
        #分治结果：初始长度和上下界区间长度做对比取最大
        self.length = max(self.length, up_length + down_length - 1)
        #因为函数不返回长度，需要返回上下界数组来记录每次递归的上下界数据便于递归对比
        return [up_length, down_length]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[39. Combination Sum](https://leetcode.com/problems/combination-sum/)

```
#dfs模板，每次递归的目标值不断减去输入元素进行查找，加剪枝优化
class Solution(object):
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        res = []
        if len(candidates)==0:
            return res
        #因为元素可重复用所以先把candidates变成独立元素升序列表便于递归查找
        nums = sorted(list(set(candidates)))
        self.dfs(nums,0,target,[],res)
        return res
    
    def dfs(self,nums,k,target,subset,res):
        #递归出口：如果找到target==0就直接往res里加subset当前的结果了
        if target ==0:
            res.append(subset[:])

        for i in range(k,len(nums)):
            #剪枝：如果剩下的target已经比Nums中元素小了肯定找不到结果了，就不用再找了直接break
            if nums[i] > target:
                break
            subset.append(nums[i])
            ##i要保持不变，因为此题中要寻找可能的加和，nums中元素可重复计算，所以递归时每次都从nums第一位找直到加和为target
            self.dfs(nums,i,target-nums[i],subset,res)
            #dfs回溯
            subset.pop()
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)

```
#dfs模板，加剪枝加判定重复
class Solution(object):
    def combinationSum2(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        res = []
        if len(candidates)==0:
            return res
        #因为此题元素不能重复使用了所以只sort不转换set去重了
        nums = sorted(candidates)
        self.dfs(nums,0,target,[],res)
        return res
    
    def dfs(self,nums,k,target,subset,res):
        if target ==0:
            res.append(subset[:])
        for i in range(k,len(nums)):
            if nums[i] > target:
                break
            #去重相同组合！因为此题数组每个数只能用一次了，所以要排除原始数组中相同元素多次使用的可能所以要进行重复判定，除了第一个进入循环的元素在调用递归时使用过的元素之外，再次出现相同（使用过的元素）是不能再进行递归的要直接跳过！    
            if nums[i] == nums[i-1] and i != k:
                continue
            subset.append(nums[i])
            #需要i+1因为不能重复寻找加和了
            self.dfs(nums,i+1,target-nums[i],subset,res)
            subset.pop()
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[78. Subsets](https://leetcode.com/problems/subsets/)

```
##dfs递归回溯，建立子集记录每个路径的可能情况不断加入结果中，当递归循环到头后回溯不断删掉最后添加的元素防止重复添加
class Solution(object):
    def subsets(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        res = []
        if nums is None or len(nums) ==0:
            return res
        #一定记住要先对nums升序排列才能用index遍历
        nums = sorted(nums)
        #调用helper
        self.dfs(nums,0,[],res)
        return res
    #k是子集长度，因为要不停加子集后面的元素进结果队列，从0开始就重复了
    def dfs(self,nums,k,subset,res):
        #一定记住append的是subset里面的所有元素而不是改reference
        res.append(subset[:])
        #一定记住for循环找当前点后面的组合时要从k起防止重复
        for i in range(k,len(nums)):
            subset.append(nums[i])
            #调用递归函数找后面的元素
            self.dfs(nums,i+1,subset,res)
            #递归调用完后回溯，每返回一层就删除最近添加的元素防止重复
            del subset[-1]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[90. Subsets II](https://leetcode.com/problems/subsets/)

```
##dfs模板+for循环内剪枝
class Solution(object):
    def subsetsWithDup(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        res = []
        nums = sorted(nums)
        self.dfs(nums,0,[],res)
        return res
    
    def dfs(self,nums,k,subset,res):
        res.append(subset[:])
        for i in range(k,len(nums)):
            ##剪枝过程：
            ##如果在这轮循环中出现了重复，就不再对重复结点进行递归操作，直接下次循环，因为重复节点在上一个i结点递归操作中就已经加入了
            if i!=k and nums[i] == nums[i-1]:
                continue
            subset.append(nums[i])
            ##记住下一步是i+1，因为每次内部递归调用都是从i层开始的
            self.dfs(nums,i+1,subset,res)
            del subset[-1]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
