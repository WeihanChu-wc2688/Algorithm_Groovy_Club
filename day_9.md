# day9_排序：快排/归并

---

## Leetcode Review 

---

[75. Sort Colors](https://leetcode.com/problems/sort-colors/)

```
#3指针+partition，建议背诵典型题目。（数组分成多类）
#设定left,right,index三个指针，left指针以及其左边存储0，right指针及其右边存储2，中间为1
#思路：index对数组遍历，遇到0和left互换位置并更新，遇到2和right互换并更新，遇到1不操作寻找下一位，直到和right重合为止

class Solution(object):
    def sortColors(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        left, right, index = 0, len(nums)-1, 0
        
        #注意是<=！！！！
        while index <= right:
            #右挪需要时刻更新index
            #因为index就是从左向右出发更新，所以左边一定为0/1，所以index时刻更新
            if nums[index] == 0:
                nums[left], nums[index] = nums[index], nums[left]
                left+=1
                index+=1
            #左挪不用更新index：因为right换给index的可能是2要继续判断是否需要继续和right+1互换！
            elif nums[index] == 2:
                nums[right], nums[index] = nums[index], nums[right]
                right-=1
                
            else:
                index+=1
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

```
#快排模板变形，理解上背诵
class Solution(object):
    def findKthLargest(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        if not nums:
            return -1
        return self.quickSelect(nums,0,len(nums)-1,k)
    
    def quickSelect(self,nums,start,end,k):
        #递归终点，不停调用quickSelect被pivot分割后的分段直到遍历完整个数组
        if start == end:
            return nums[start]
        #相向双指针
        i,j = start,end
        pivot = nums[(i+j)/2]
        #快排模板
        while i <= j:
            #记得把nums[i]和pivot关系搞清楚，因为现在是找第k个最大，数组要降序排列！
            while i <= j and nums[i] > pivot:
                i+=1
            while i <= j and nums[j] < pivot:
                j-=1
            #不满足降序排列，左右元素互换，就算要互换也要判断i,j的合法性
            if i <= j:
                nums[i],nums[j] = nums[j],nums[i]
                i+=1
                j-=1
        #因为快排中循环终点是i<=j，所以出循环后i>j，数组被分成了三部分：
        #start----------j-i---------end；所以找k大个数被分成前中后三种情况：
        if (start + k - 1) <= j:
            return self.quickSelect(nums,start,j,k)
        if (start + k - 1) >= i:
            #这里的k不再是从start数的k了，是要从i开始数！所以记得更新k的index！
            return self.quickSelect(nums,i,end,k - (i- start))
        #如果都没找到，就返回j,i之间的那个数
        return nums[j+1]    
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[148. Sort List](https://leetcode.com/problems/sort-list/)

```
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
#---------------------------------
#迭代法归并排序+断链操作，理解背诵
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
                new_head = ListNode(-1)
                new_tmp = new_head
                while head_1 and head_2:
                    #归并时如果1更小，让空链表链接1，并且让原来的1链表连接到下一个结点
                    if head_1.val <= head_2.val:
                        new_tmp.next = head_1
                        head_1 = head_1.next
                    else:
                        new_tmp.next = head_2
                        head_2 = head_2.next
                    #删除头上的dummynode    
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

[1157. Shortest Unsorted Continuous Subarray](https://www.lintcode.com/problem/shortest-unsorted-continuous-subarray/description)

```
#变形双指针，ans倒序走通过对比sorted list的数字找到原始列表中最后一次出现非升序的位置；
#temp正序走也是通过对比sorted list的数字和原始列表找到第一次出现非升序的位置；
#最后两者做差即为长度
class Solution:
    """
    @param nums: an array
    @return: the shortest subarray's length
    """
    def findUnsortedSubarray(self, nums):
        temp = 0
        ans = len(nums)
        sortNums = sorted(nums)
        
        while ans > 0 and  nums[ans-1] == sortNums[ans-1]:
            ans-=1
        for temp in range(ans):
            if nums[temp] == sortNums[temp]:
                temp+=1
            else:
                break
        return ans - temp  
```
**Analysis**

Time Complexity: 

Space Complexity: 
