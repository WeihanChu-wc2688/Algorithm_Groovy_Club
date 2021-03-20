# day17: 链表

---

## Leetcode Review 

---

[21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

```python
#调用递归对l1,l2大小关系进行判断，处理完边界条件后调用递归
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def mergeTwoLists(self, l1, l2):
        #定义递归出口，如果l1扫描完毕直接插上l2
        if l1 is None:
            return l2
        #反之递归出口指向l1
        elif l2 is None:
            return l1
        #开始递归，谁小递归谁的下一位
        elif l1.val<l2.val:
            l1.next = self.mergeTwoLists(l1.next,l2)
            return l1
        else:
            l2.next = self.mergeTwoLists(l1,l2.next)
            return l2    
        
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
```
**Analysis**

Time Complexity: 

Space Complexity: 

---


[148. Sort List](https://leetcode.com/problems/sort-list/)

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
#---------------------------------
#迭代法归并排序+断链操作，理解背诵
#理解本质：自底向上归并，写好断链，取长度还有归并元素的helper function，通过dummy结点先两个，再四个直到左右半边归并
class Solution(object):
    def sortList(self, head):
            """
            :type head: ListNode
            :rtype: ListNode
            """
            #用dummynode实现归并链表
            def mergeTwoList(head_1, head_2):
                if not head_1 and not head_2:
                    return None
                #建立dummynode链表准备按顺序归并两个链表
                #new_temp为动态指针
                new_head = ListNode(-1)
                new_tmp = new_head
                #只要两个链表有元素就不停排序
                while head_1 and head_2:
                    #归并时如果1更小，让空链表链接1，并且让原来的1链表连接到下一个结点
                    if head_1.val <= head_2.val:
                        new_tmp.next = head_1
                        head_1 = head_1.next
                    else:
                        new_tmp.next = head_2
                        head_2 = head_2.next
                    #临时结点后移    
                    new_tmp = new_tmp.next
                #如果链表长度不等就把那个剩下的结点加上    
                new_tmp.next = head_1 if head_1 else head_2
                #返回删除dummynode后的链表
                return new_head.next

            def getLength(head):
                count = 0
                while head:
                    count += 1
                    head = head.next
                return count
            #断链（取后面的部分）
            def split(head, step):
                if not head:
                    return None
                index = 1
                while head.next and index < step:
                    head = head.next
                    index += 1
                cur = head.next
                head.next = None
                return cur
            
            #建立dummynode接在头结点前，做完迭代归并后返回dummy.next也就是新结点
            if not head or not head.next:
                return head
            length = getLength(head)
            dummy = ListNode(-1)
            dummy.next = head
            i = 1
            #迭代归并
            while i < length:
                pre = dummy
                cur = dummy.next
                #自底向上，先一对2个对比，后面i*2后一对4个对比，直到最后左半边右半边对比
                while cur:
                    h1 = cur #第一部分链表的头结点：h1--------
                    h2 = split(h1, i) #第二部分的头结点：h1-----(i个)，h2------
                    cur = split(h2, i) #剩下的头结点：h1---(i个),h2----(i个)，cur-----
                    tmp = mergeTwoList(h1, h2)#把切好的h1,h2两段长度为i的链表归并
                    pre.next = tmp #存储在pre.next里
                    while pre.next:
                        pre = pre.next #把指针指向更新后的末尾，一遍下次循环后加入新的链表
                #每次切掉两个，所以i作为index要2倍数迭代，因为每次循环pre,cur都是新的完整的dummy传进 
                i *= 2
            return dummy.next
```
**Analysis**

Time Complexity: 

Space Complexity: 

---


[138. Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, x, next=None, random=None):
        self.val = int(x)
        self.next = next
        self.random = random
"""
#本题目的是创造带有随机指针链表的deep copy，本题中链表除了有指向下一个的指针外还有一个随机指针指向另外一个随机位置
#所以我们先创造全局变量字典，索引为目标元素，值为要复制的元素
#通过调用递归不停建立新链表并且把目标链表的数值一步步赋值过去

class Solution(object):
    #建立全局变量字典
    def __init__(self):
        self.visited = {}
        
    def copyRandomList(self, head):
        """
        :type head: Node
        :rtype: Node
        """
        #异常检测，也是递归终点
        if not head:
            return None
        #递归出口，如果找到要复制的结点就在当前结点返回他的值
        if head in self.visited:
            return self.visited[head]
        #deep copy，创建新结点并且把老结点值赋值
        node = Node(head.val, None, None)
        self.visited[head] = node
        #调用递归，对next和random两个指针调用
        node.next = self.copyRandomList(head.next)
        node.random = self.copyRandomList(head.random)
        #最终返回结果
        return node
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)

```python
#本题不是回文子串，只要求判断回文串，所以直接copy进list,然后把数组反过来判断和原来是否相同即可
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def isPalindrome(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        res = []
        current_node = head
        
        while current_node is not None:
            res.append(current_node.val)
            current_node = current_node.next
            
        return res == res[::-1]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[160. Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

```python
#双指针！思路：两个链表循环各自到头后换位遍历，一定会在交点处相会！
#例子：1-2-3-4-5， 2-3-4-5， 交点为3
#两个结点先遍历各自节点到头后换位遍历的路径分别为：
#1-2-3-4-5-2-3
#2-3-4-5-1-2-3
#两个相连链表分开遍历的长度一定相等，一定会在相交处停下！！！！
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """
        if headA is None or headB is None:
            return None
        pa = headA
        pb = headB
        
        while pa is not pb:
            if pa is None:
                pa = headB
            else:
                pa = pa.next
            if pb is None:
                pb = headA
            else:
                pb = pb.next
        return pa
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[708. Insert into a Sorted Circular Linked List](https://leetcode.com/problems/insert-into-a-sorted-circular-linked-list/)

```python
#循环链表，分情况进行对应操作：1：找到能插入的直接插；2：插入值为目前的最大/最小值：插在头尾间；3：数组重复数，插头尾；

#知识点：如果循环终止条件不好设置，就先while true，然后在循环主体中在设置终止条件

"""
# Definition for a Node.
class Node(object):
    def __init__(self, val=None, next=None):
        self.val = val
        self.next = next
"""

class Solution(object):
    def insert(self, head, insertVal):
        """
        :type head: Node
        :type insertVal: int
        :rtype: Node
        """
        #注意异常值处理！如果是空数组要确保新生成结点是循环的！
        if head is None:
            temp = Node(insertVal,None)
            temp.next = temp
            return temp
        #快慢指针遍历整个数组
        prev = head
        curr = head.next
        #创建状态指针，方便所有满足条件的情况统一进行处理，节约代码量
        insert = False
        #因为终止条件是回到头结点，但prev初始就是头结点，所以终止条件写在循环里，这里用true
        while True:
            #情况一，记录insert状态
            if prev.val <= insertVal <= curr.val:
                insert = True
            #这是循环到头尾之间了    
            elif prev.val > curr.val:
                #情况二，也是记录状态
                if insertVal >= prev.val or insertVal <= curr.val:
                    insert = True
            #满足2种状态的就加结点，返回最终链表
            if insert:
                prev.next = Node(insertVal,curr)
                return head
            #更新快慢指针
            prev = curr
            curr = curr.next
            #终止条件
            if prev == head:
                break
        #情况三，循环不会返回结果，所以直接加在头尾之间        
        prev.next = Node(insertVal,curr)
        return head
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
