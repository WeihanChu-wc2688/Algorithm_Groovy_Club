# day3_二分搜索树

---

## Leetcode Review

---

[450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##先定义前继和后继指针，定义：左子树最右结点或者右子树最左节点，也就是中序排列中前/后一位的点
##再判断节点性质：
#############当为叶子节点时：直接==null
#############当只有左子树时：用前继替换，然后把前继删了
#############当只有右子树：用后继替换，再把后继删了
#############都有时：爱鸡巴换谁换谁，操
class Solution(object):
    
    def suc(self,root):
        root = root.right
        while root.left:
            root = root.left
        return root.val
    
    def pre(self,root):
        root = root.left
        while root.right:
            root = root.right
        return root.val
    
    def deleteNode(self, root, key):
        """
        :type root: TreeNode
        :type key: int
        :rtype: TreeNode
        """
        if not root:
            return None
        if key > root.val:
            root.right = self.deleteNode(root.right,key)
        elif key < root.val:
            root.left = self.deleteNode(root.left,key)
        else:
            if not (root.left or root.right):
                ##---注意是删node不是删数值！
                root = None
            elif root.right:
                root.val = self.suc(root)
                ##现在得目的是删除这个suc结点而不是原来的key了！
                root.right = self.deleteNode(root.right,root.val)
            else:
                root.val = self.pre(root)
                root.left = self.deleteNode(root.left,root.val)
        return root
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##经典递归，用val和每个结点的关系进行二分查找
class Solution(object):
    def searchBST(self, root, val):
        """
        :type root: TreeNode
        :type val: int
        :rtype: TreeNode
        """
        if not root or val == root.val:
            return root
        ##---------------------------
        ##记住求根类型的递归里要直接返回调用函数而不是root.right=self...
        ##因为主体函数里不返回结果，如果每个函数里面再不返回的话就会返回空值！！！
        if val > root.val:
            return self.searchBST(root.right,val)
        if val < root.val:
            return self.searchBST(root.left,val)
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def insertIntoBST(self, root, val):
        """
        :type root: TreeNode
        :type val: int
        :rtype: TreeNode
        """
        if root is None:
            ##------记住把数值结点化要调用TreeNode而不是直接root=val!
            return TreeNode(val)
        if val > root.val:
            root.right = self.insertIntoBST(root.right,val)
        if val < root.val:
            root.left = self.insertIntoBST(root.left,val)
        return root
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[11. Search Range in Binary Search Tree](https://www.lintcode.com/problem/search-range-in-binary-search-tree/description)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    """
    @param root: param root: The root of the binary search tree
    @param k1: An integer
    @param k2: An integer
    @return: return: Return all keys that k1<=key<=k2 in ascending order
    """
    ##和trimbst很像，但区别：trim是找到边界然后里面的遍历，这个是找到边界在里面一直append
    def searchRange(self, root, k1, k2):
        result = []
        self.travel(root, k1, k2, result)
        return result
    
    def travel(self, root, k1, k2, result):
        if root is None:
            return
    	# 剪枝，如果当前节点小于等于k1，不必访问左子树
        if root.val > k1:
            self.travel(root.left, k1, k2, result)
        if k1 <= root.val and root.val <= k2:
            result.append(root.val)
        # 剪枝，如果当前节点大于等于k2，不必访问右子树
        if root.val < k2:
            self.travel(root.right, k1, k2, result)
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[448. Inorder Successor in BST](https://www.lintcode.com/problem/inorder-successor-in-bst/description)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    """
    @param: root: The root of the BST.
    @param: p: You need find the successor node of p.
    @return: Successor of p.
    """
    def inorderSuccessor(self, root, p):
    ##和predecessor完全反过来
        # write your code here
        succ = None
        while root:
            if p.val >= root.val:
                root = root.right
            else:
                if succ == None or root.val < succ.val:
                    succ = root
                root = root.left
                
        return succ 
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[915. Inorder Predecessor in BST](https://www.lintcode.com/problem/inorder-predecessor-in-bst/description)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    """
    @param root: the given BST
    @param p: the given node
    @return: the in-order predecessor of the given node in the BST
    """
    def inorderPredecessor(self, root, p):
        # write your code here
        succ = None
        while root:
            #如果比目标大就往左查，如果找到了的话也往左因为要取左子树最右点
            if p.val <= root.val:
                root = root.left
            #如果已经在目标左边了，让结果等于当前点，然后当前点再迭代向右，这样结果总比当前点靠左一个，如果当前点向右走到空点了那么结果点正好就是最右点
            else:
                if succ == None or root.val > succ.val:
                    succ = root
                root = root.right
                
        return succ
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[701. Trim a Binary Search Tree](https://www.lintcode.com/problem/trim-a-binary-search-tree/description)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##------------------------
##如果结点小于最小值，就返回右子树
##如果结点大于最大值，就返回左子树
##如果相等或者在范围内，就遍历保留结点，最后外面返回根节点
class Solution:
    """
    @param root: given BST
    @param minimum: the lower limit
    @param maximum: the upper limit
    @return: the root of the new tree 
    """
    def trimBST(self, root, minimum, maximum):
        if not root:
            return root
        if root.val < minimum:
            return self.trimBST(root.right,minimum,maximum)
        elif root.val > maximum:
            return self.trimBST(root.left,minimum,maximum)
        else:
            root.left = self.trimBST(root.left,minimum,maximum)
            root.right = self.trimBST(root.right,minimum,maximum)
        return root
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[538. Convert BST to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##--------------
##首先建立全局变量记录每次结点的total，如果在convertBST里面写的话每次递归都会被刷新；
##如果root存在的话，先递归右子树，再让total累积每次结点数值，再重新把total赋值给节点，再遍历左子树！这样的顺序确保每次都更新右子树记录total而不会把左子树的数值累积进来
class Solution(object):
    def __init__(self):
        self.total = 0
        
    def convertBST(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """
        if root:
            self.convertBST(root.right)
            self.total += root.val
            root.val = self.total
            self.convertBST(root.left)
        return root
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##------------------
#因为是bst!!!!所以根据性质直接dfs-inorder出来就是递增数组，然后直接用zip错位相减找最小绝对值就好！
class Solution(object):
    def getMinimumDifference(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        res = []
        def dfs(root):
            if root:
                dfs(root.left)
                res.append(root.val)
                dfs(root.right)
        dfs(root)
        
        return min(b-a for a,b in zip(res,res[1:]))
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[98. Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##dfs-inorder后错位相减判断是否都大于0，否则就不是bst
class Solution(object):
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if root is None:
            return False
        res = []
        
        def checker(root,res):
            if root:
                checker(root.left,res)
                res.append(root.val)
                checker(root.right,res)
        checker(root,res)
        ##---------------------------
        ##!!!!!!!!!!!!
        ##如果数组out of index会直接报错不会返回false!!!!!
        ##边界条件确定好，如果数组只有一个的话 不！能！错！位！相！减！
        if len(res)>1:
            return min(b-a for a,b in zip(res,res[1:]))>0
        else:
            return True
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##--------------------------
##因为bst所以数组单调递增，因为要平衡所以要从数组中间建立根，当奇数时就中间，偶数就floor(中间)，用数据index的起点终点做记录，用递归调用根左右的节点
class Solution(object):
    def sortedArrayToBST(self, nums):
        """
        :type nums: List[int]
        :rtype: TreeNode
        """
        def helper(left,right):
            if left > right:
                return None
            p = (left+right)//2
            root = TreeNode(nums[p])
            root.left = helper(left,p-1)
            root.right = helper(p+1,right)
            return root
        return helper(0,len(nums)-1)
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[938. Range Sum of BST](https://leetcode.com/problems/range-sum-of-bst/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##############第一道自己写出来的bug-free算法题
#dfs-inorder+数组计算
class Solution(object):
    def rangeSumBST(self, root, low, high):
        """
        :type root: TreeNode
        :type low: int
        :type high: int
        :rtype: int
        """
        res = []
        def cum(root):
            if not root:
                return None
            cum(root.left)
            res.append(root.val)
            cum(root.right)
        cum(root)
        return sum(res[res.index(low):res.index(high)+1])
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[270. Closest Binary Search Tree Value](https://leetcode.com/problems/closest-binary-search-tree-value/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
#dfs遍历后处理数组
class Solution(object):
    def closestValue(self, root, target):
        """
        :type root: TreeNode
        :type target: float
        :rtype: int
        """
        if not root:
            return None
        dfs = []
        def helper(root):
            if root:
                helper(root.left)
                dfs.append(root.val)
                helper(root.right)
        helper(root)
        dif = []
        for i in dfs:
            dif.append(abs(target-i))
        return dfs[dif.index(min(dif))]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    ##dfs-inorder后为递增数组，然后再处理数组，注意边界条件len(res)
    def kthSmallest(self, root, k):
        """
        :type root: TreeNode
        :type k: int
        :rtype: int
        """
        
        if root is None:
            return None
        res = []
        def dfs(root):
            if root:
                dfs(root.left)
                res.append(root.val)
                dfs(root.right)
        dfs(root)        
        if len(res)>=k:
            return res[k-1]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[661. Convert BST to Greater Tree](https://www.lintcode.com/problem/convert-bst-to-greater-tree/description)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    # @param {TreeNode} root the root of binary tree
    # @return {TreeNode} the new root
    def convertBST(self, root):
        # Write your code here
        self.sum = 0
        self.helper(root)
        return root

    def helper(self, root):
        if root is None:
            return
        if root.right:
            self.helper(root.right)
        
        self.sum += root.val
        root.val = self.sum
        if root.left:
            self.helper(root.left)

```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[1188. Minimum Absolute Difference in BST](https://www.lintcode.com/problem/minimum-absolute-difference-in-bst/description)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    """
    @param root: the root
    @return: the minimum absolute difference between values of any two nodes
    """
    def getMinimumDifference(self, root):
        if not root:
            return None
        res = []
        def helper(root):
            if root:
                helper(root.left)
                res.append(root.val)
                helper(root.right)
        helper(root)
        return min(abs(b-a) for a,b in zip(res,res[1:]))

```
**Analysis**

Time Complexity: 

Space Complexity: 

---
