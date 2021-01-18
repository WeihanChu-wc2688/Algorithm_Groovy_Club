# Day2_二叉树

---

## LeetcodeReview

---

[144:Binary Tree Preorder Traversal 二叉树前序遍历](https://leetcode.com/problems/binary-tree-preorder-traversal/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    ##--------------------------------------------------------------------------
    ##建立类函数dfs协助进行递归遍历，如果node存在就加入list然后依次调用左右node，然后在主体函数中调用DFS函数
    def preorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        ##建立空列表存储结果，调用self.dfs后返回结果
        result = []
        self.dfs(root,result)
        return result
    
    def dfs(self,root,result):
        ##考虑边界情况root为空，后再加入结果列表，再加左右结点
        if root:
            result.append(root.val)
            self.dfs(root.left,result)
            self.dfs(root.right,result)
```
**Analysis**

Time Complexity: O(n)--每次调用递归进行遍历，每个点1次

Space Complexity: O(n)--res线性添加遍历出的节点

---

[94:Binary Tree Inorder Traversal 二叉树中序遍历](https://leetcode.com/problems/binary-tree-inorder-traversal/)

```
class Solution(object):
    
    ##------------------------------------------------
    ##先建立helper function dfs来进行中序排列递归，然后在主体函数中调用dfs来存储结果列表
    def inorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        res = []
        self.dfs(root,res)
        return res
    ##helper function:
    def dfs(self,root,res):
        if root:
            self.dfs(root.left,res)
            res.append(root.val)
            self.dfs(root.right,res)
```

**Analysis**

Time Complexity: O(n)--每次调用递归进行遍历，每个点1次

Space Complexity: O(n)--res线性添加遍历出的节点

---


[94:Binary Tree Postorder Traversal 二叉树后序遍历](https://leetcode.com/problems/binary-tree-postorder-traversal/)

```
class Solution(object):
    ##道理同preorder,inorder
    def postorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        res = []
        self.dfs(root,res)
        return res
    
    def dfs(self,root,res):
        if root:
            self.dfs(root.left,res)
            self.dfs(root.right,res)
            res.append(root.val)
```

**Analysis**

Time Complexity: O(n)--每次调用递归进行遍历，每个点1次

Space Complexity: O(n)--res线性添加遍历出的节点

---

[94:Binary Tree Postorder Traversal 二叉树后序遍历](https://leetcode.com/problems/binary-tree-postorder-traversal/)

```
class Solution(object):
    ##道理同preorder,inorder
    def postorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        res = []
        self.dfs(root,res)
        return res
    
    def dfs(self,root,res):
        if root:
            self.dfs(root.left,res)
            self.dfs(root.right,res)
            res.append(root.val)
```

**Analysis**

Time Complexity: O(n)--每次调用递归进行遍历，每个点1次

Space Complexity: O(n)--res线性添加遍历出的节点

---

[102:Binary Tree Level Order Traversal 二叉树层级遍历](https://leetcode.com/problems/binary-tree-level-order-traversal/)
```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    ##----------------------------------------
    ##先确定有节点，再通过建立helper；
    ##结果列表装有不同层节点和层数，先通过判断层数和结果列表中子列表的数量关系来判断是否进入新的一层（加新的子列表），再通过判断左右节点来添加子列表内相应节点
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        res = []
        if not root:
            return res
        
        def helper(root,level):
            ##判断子列表数量和层数关系：如果相等就加一个子列表因为：层数从0起，数量从1起！
            if len(res)==level:
                res.append([])
                
            res[level].append(root.val)
            
            if root.left:
                helper(root.left,level+1)
            if root.right:
                helper(root.right,level+1)
                
        helper(root,0)
        return res
```
**Analysis**

Time Complexity: O(n)--每次调用递归进行遍历，每个点1次

Space Complexity: O(n)--res线性添加遍历出的节点

---


[107:Binary Tree Level Order Traversal 二叉树层级遍历2](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)
题目：倒序层级遍历
```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    ##----------------------------------
    ##变形题，跟top-bottom遍历二叉树同理，只需切片时倒序即可！！
    def levelOrderBottom(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        res = []
        if root == None:
            return []
        
        def helper(root,level):
            ##注意先加子列表，再在子列表里填节点！
            if len(res) == level:
                res.append([])
                
            res[level].append(root.val)
            
            if root.left:
                helper(root.left,level+1)
            if root.right:
                helper(root.right,level+1)    
                
        helper(root,0)
        return res[::-1]
```
**Analysis**

Time Complexity: O(n)--每次调用递归进行遍历，每个点1次

Space Complexity: O(n)--res线性添加遍历出的节点

---


[107:Binary Tree Zigzag Level Order Traversal 二叉树蛇形层级遍历](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)
题目：蛇形遍历，单数层正向排列，双数反之
```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution(object):
    def zigzagLevelOrder(self, root):
        ##-----------------------------------------------------
        ##建立deque，可正向加可反向加，前部分套用之前结果，如果单数就正向加反之反向append
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        
        res = []
        if root is None:
            return res
        
        def helper(root,level):
            if len(res) == level:
                res.append(deque([root.val]))
                
            else:
            
                if level %2 == 0 :
                    res[level].append(root.val)
                else:
                    res[level].appendleft(root.val)
                    
            for next_node in [root.left, root.right]:
                
                if next_node is not None:
                    helper(next_node, level+1)
                    
        helper(root,0)
        return res
```
**Analysis**

Time Complexity: O(n)--每次调用递归进行遍历，每个点1次

Space Complexity: O(n)--res线性添加遍历出的节点

---

[1469:Find the lonely node 找孤结点](https://leetcode.com/problems/find-all-the-lonely-nodes/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    ##------------------------------------------
    ##分两个方向，先定义递归出口：如果node有左孩子没右孩子就在结果列加坐边；反之加右边；
    ##递归方向：左边递归左孩子；反之右边
    def getLonelyNodes(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        res = []
        if root is None:
            return res
        
        def helper(root):
            if root.left and not root.right:
                res.append(root.left.val)
            if root.right and not root.left:
                res.append(root.right.val)
                
            if root.left:
                helper(root.left)
            if root.right:
                helper(root.right)
                
        helper(root)
        return res
```
**Analysis**

Time Complexity: O(n)--每次调用递归进行遍历，每个点1次

Space Complexity: O(n)--res线性添加遍历出的节点

---


[515. Find Largest Value in Each Tree Row](https://leetcode.com/problems/find-largest-value-in-each-tree-row/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##---------------------------------------------------------------
##一样的原理，先遍历再返回每层最大值
class Solution(object):
    def largestValues(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        res = []
        if root is None:
            return []
        
        def helper(root,level):
            if len(res) == level:
                res.append([])
            res[level].append(root.val)
            
            if root.left:
                helper(root.left, level+1)
            if root.right:
                helper(root.right, level+1)
        
        helper(root,0)
        return [max(k) for k in res]
```
**Analysis**

Time Complexity: O(n)--每次调用递归进行遍历，每个点1次

Space Complexity: O(n)--res线性添加遍历出的节点

---


[513. Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##---------------------------------------------------------------
##先遍历再提取最后一排最左边
class Solution(object):
    def findBottomLeftValue(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        res = []
        if not root:
            return []
        
        def helper(root,level):
            if len(res) == level:
                res.append([])
            res[level].append(root.val)
            
            if root.left:
                helper(root.left,level+1)
            if root.right:
                helper(root.right, level+1)
            
        helper(root,0)
        return res[-1][0]
```
**Analysis**

Time Complexity: 

Space Complexity:

---


[404. Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##--------------------------------------------------------------
##用stack，用dfs前序判断每个节点左边是否为叶子，是的话存储进total，不是的话/是右节点的话存stack里再pop出来重复遍历直到stack清空
class Solution(object):
    def sumOfLeftLeaves(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        total = 0
        if root is None:
            return 0
        def is_leaf(node):
            return node is not None and node.left is None and node.right is None
        stack = [root]
        
        while stack:
            sub_root = stack.pop()
            if is_leaf(sub_root.left):
                total += sub_root.left.val
            if sub_root.right:
                stack.append(sub_root.right)
            if sub_root.left:
                stack.append(sub_root.left)
        return total
```
**Analysis**

Time Complexity: 

Space Complexity:

---


[199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##---------------------------------------------------
##bfs遍历，每层最右边就是
class Solution(object):
    def rightSideView(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if root is None:
            return []
        res = []
        
        def helper(root,level):
            if len(res)==level:
                res.append([])
            res[level].append(root.val)
            
            if root.left:
                helper(root.left,level+1)
            if root.right:
                helper(root.right,level+1)
        helper(root,0)
            
        
        return [node[-1] for node in res]
```
**Analysis**

Time Complexity: 

Space Complexity:

---


[106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##----------------------------------------------------
##preorder数组是完美倒序，可以通过pop来返回元素构建树，但分不出左右树；
##inorder数组由root分左右树，但左右树无序而且找不到root
##所以只能通过两个数组搭配，通过postorder来pop的元素当作每个node，通过inorder的数组元素的##index来标记这个元素是左node还是右node来协助，进行左右递归构建根节点下的左右子树
class Solution(object):
    def buildTree(self, inorder, postorder):
        """
        :type inorder: List[int]
        :type postorder: List[int]
        :rtype: TreeNode
        """
        def helper(in_left,in_right):
            if in_left > in_right:
                return None
            
            val = postorder.pop()
            root = TreeNode(val)
            
            index = idx_map[val]
            
            root.right = helper(index+1, in_right)
            root.left = helper(in_left, index-1)
            return root
        
        idx_map = {val:idx for idx, val in enumerate(inorder)}
        return helper(0, len(inorder)-1)
```
**Analysis**

Time Complexity: 

Space Complexity:

---


[105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##---------------------------------------------------
##通过递归不断调用本身建立根，左右节点，和postorder和inorder问题同一个思想不过处理顺序不再是postorder的pop了
class Solution(object):
    def buildTree(self, preorder, inorder):
        """
        :type preorder: List[int]
        :type inorder: List[int]
        :rtype: TreeNode
        """
    
        if inorder:
            ind = inorder.index(preorder.pop(0))
            root = TreeNode(inorder[ind])
            root.left = self.buildTree(preorder, inorder[0:ind])
            root.right = self.buildTree(preorder, inorder[ind+1:])
            return root
```
**Analysis**

Time Complexity: 

Space Complexity:

---


[889. Construct Binary Tree from Preorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##---------------------------------------------------
##因为preorder的根在前,postorder的根在后，所以每一个子树中根的分部也是同理，所以preorder的左子树起点是index=1，postorder左子树起点的index和preorder左子树起点index之差就是左子树长度，进而能够分出左子树右子树分别的index范围，接下来调用递归开始建立根节点和左右结点。
class Solution(object):
    def constructFromPrePost(self, pre, post):
        """
        :type pre: List[int]
        :type post: List[int]
        :rtype: TreeNode
        """
        if not pre: return None
        root = TreeNode(pre[0])
        if len(pre) == 1: return root

        L = post.index(pre[1]) + 1
        root.left = self.constructFromPrePost(pre[1:L+1], post[:L])
        root.right = self.constructFromPrePost(pre[L+1:], post[L:-1])
        return root        
```
**Analysis**

Time Complexity: 

Space Complexity:


---


[222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##---------------------------------------------------
##调用递归，每次如果左边node都在就+1，不在就是0
class Solution(object):
    def countNodes(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        return 1+self.countNodes(root.left)+self.countNodes(root.right) if root else 0
```
**Analysis**

Time Complexity: 

Space Complexity:

---


[100. Same Tree](https://leetcode.com/problems/same-tree/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##---------------------------------------------------
##递归，首先判断两节点，再用并集判断各自的左右点
class Solution(object):
    def isSameTree(self, p, q):
        """
        :type p: TreeNode
        :type q: TreeNode
        :rtype: bool
        """
        # p and q are both None
        if not p and not q:
            return True
        # one of p and q is None
        if not q or not p:
            return False
        if p.val != q.val:
            return False
        return self.isSameTree(p.right, q.right) and self.isSameTree(p.left, q.left)
```
**Analysis**

Time Complexity: 

Space Complexity:

---


[101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##---------------------------------------------------
##判断条件一定要写好！！！判断好写在里面还是外面

class Solution(object):
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        

        def check(l,r):
            if not l and not r:
                return True
            if not l or not r:
                return False
            return l.val == r.val and check(l.left,r.right) and check(l.right,r.left)
        return check(root,root)
    
#        def isSymmetric(self, root):
#        def isSym(L,R):
#            if not L and not R: return True
#           if L and R and L.val == R.val: 
 #               return isSym(L.left, R.right) and isSym(L.right, R.left)
 #           return False
#        return isSym(root, root)
```
**Analysis**

Time Complexity: 

Space Complexity:

---


[112. Path Sum](https://leetcode.com/problems/path-sum/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##---------------------------------------------------
##先判断边介条件，再给出递归出口（如果走到叶子是否等于0），倒推左右节点
class Solution(object):
    def hasPathSum(self, root, sum):
        """
        :type root: TreeNode
        :type sum: int
        :rtype: bool
        """
        if not root:
            return False
        sum = sum - root.val
        if not root.left and not root.right:
            return sum == 0
        return self.hasPathSum(root.left,sum) or self.hasPathSum(root.right,sum)
```
**Analysis**

Time Complexity: 

Space Complexity:

---


[104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##---------------------------------------------------
class Solution(object):
    def maxDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0

        return 1 + max(self.maxDepth(root.left),self.maxDepth(root.right))
```
**Analysis**

Time Complexity: 

Space Complexity:


---


[111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##---------------------------------------------------
##需要分情况讨论：如果只有单边子树的话需要取最大长度因为最小值为0而树依旧在分裂；
##              如果双边都有的话取最短路径
##              如果到了叶子为0
class Solution(object):
    def minDepth(self, root):

        if not root:
            return 0
        if None in [root.left,root.right]:
            return 1 + max(self.minDepth(root.left),self.minDepth(root.right))
        else:
            return 1 + min(self.minDepth(root.left),self.minDepth(root.right))
        
```
**Analysis**

Time Complexity: 

Space Complexity:

---


[110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##---------------------------------------------------
##注意读题，题中要求每一个节点的左右都要高度平衡所以高度是结点数量差！空结点要减一！！然后每一个结点都要判断一遍所以还要递归调用整个函数的左右结点
class Solution(object):
    def isBalanced(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if not root:
            return True
        
        def height(root):
            if not root:
                return -1
            return 1+max(height(root.left),height(root.right))
        
        return abs(height(root.left)-height(root.right)) < 2 and self.isBalanced(root.left) and self.isBalanced(root.right)
```
**Analysis**

Time Complexity: 

Space Complexity:

---


[250. Count Univalue Subtrees](https://leetcode.com/problems/count-univalue-subtrees/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##---------------------------------------------------
##先读题，题目要求有两点：1-当前节点无子节点 或者 2-当前节点左右子节点相等或者只有一个节点
##所以建立helper，一个变量记录是否为uni的boolean，一个变量计数，只有满足条件才会计数，helper里通过递归实现
class Solution(object):
    def countUnivalSubtrees(self, root):
        self.count = 0
        self.checkUni(root)
        return self.count

    # bottom-up, first check the leaf nodes and count them, 
    # then go up, if both children are "True" and root.val is 
    # equal to both children's values if exist, then root node
    # is uniValue suntree node. 
    def checkUni(self, root):
        if not root:
            return True
        l, r = self.checkUni(root.left), self.checkUni(root.right)
        if l and r and (not root.left or root.left.val == root.val) and \
        (not root.right or root.right.val == root.val):
            self.count += 1
            return True
        return False
```
**Analysis**

Time Complexity: 

Space Complexity:

---


[235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##---------------------------------------------------
##根据bst性质，lca要么是根，要么是左子树结点，要么是右边子树结点，因为左子树一定更小，所以通过每次递归根节点和q,q的关系就好了
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if not root:return root;
        if p.val > q.val:return self.lowestCommonAncestor(root, q, p)
        if root.val >= p.val and root.val <= q.val: return root;
        if root.val < p.val:return self.lowestCommonAncestor(root.right, p, q)
        if root.val > q.val:return self.lowestCommonAncestor(root.left, p, q)
```
**Analysis**

Time Complexity: 

Space Complexity:


---


[236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
##---------------------------------------------------
##首先读懂题目！如果p,q只找到一个返回其中找到的节点就可以！
##用dfs先找左右子树是否其中携带p,q，如果一个节点的左右子树分别包含p,q那他就是lca；
##所以left,right分别对每个节点的左右子树进行查找，如果找不到返回null，找得到要么是P要么是q，如果left,right都存在了意味着找到p,q那lca就是当前节点；否则只有p或者q的话就也返回当前节点了
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if not root or root == p or root == q: return root
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)
        if left and right:
            return root
        return left if left else right
```
**Analysis**

Time Complexity: 

Space Complexity:
