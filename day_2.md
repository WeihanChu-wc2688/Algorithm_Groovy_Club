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

Time Complexity:

Space Complexity:

---




