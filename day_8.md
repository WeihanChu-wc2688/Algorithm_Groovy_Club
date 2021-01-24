# day8_双指针总结

---

## Leetcode Review - 相向双指针专题



[1. Two Sum](https://leetcode.com/problems/two-sum/)

```
#用字典存储index，然后直接查找差返回
class Solution(object):
    def twoSum(self, nums, target):
        h = {}
        for i,num in enumerate(nums):
            n = target - num
            if n in h:
                return [h[n],i]
            h[num]=i
```
**Analysis**

Time Complexity: 

Space Complexity: 

[167. Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

```
#经典相向双指针，用while循环逻辑找不同条件的判断模式
class Solution(object):
    def twoSum(self, numbers, target):
        """
        :type numbers: List[int]
        :type target: int
        :rtype: List[int]
        """
        left, right = 0, len(numbers)-1
        while left<= right:
            if numbers[left]+numbers[right]< target:
                left+=1
            elif numbers[left]+numbers[right]> target:
                right-=1
            else:
                return [left+1,right+1]
        return [-1,-1]
```
**Analysis**

Time Complexity: 

Space Complexity: 


[170. Two Sum III - Data structure design](https://leetcode.com/problems/two-sum-iii-data-structure-design/)

```
class TwoSum(object):
    #hashtable实现O（1）插入，O（n）查询
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.count = {}
        
    #建立全局变量count：key为加入的Number，value为每个number出现的次数
    def add(self, number):
        """
        Add the number to an internal data structure..
        :type number: int
        :rtype: None
        """
        if number in self.count:
            self.count[number]+=1
        else:
            self.count[number] = 1
        
        
    #遍历hashtable的keys,如果目标值减去当前key后的剩余值不等于key，而且remain还在字典中说明找到了two sum，如果self.count中记录了不止一个number的Key，而且加和恰好为target也是true,除此之外皆为false
    def find(self, value):
        """
        Find if there exists any pair of numbers which sum is equal to the value.
        :type value: int
        :rtype: bool
        """
        for num in self.count.keys():
            remain = value - num
            if  num != remain:
                if remain in self.count:
                    return True
            elif self.count[num] >1:
                return True
        return False
```
**Analysis**

Time Complexity: 

Space Complexity: 

[653. Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/)

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
#因为是bst，所以可以inorder遍历然后把每次递归遍历到的root值得remain存进set中，接下来如果哪次root.val等于set里的remain就立刻返回当前的值和remain
class Solution(object):
    def findTarget(self, root, k):
        """
        :type root: TreeNode
        :type k: int
        :rtype: bool
        """
        if not root:
            return None
        temp = set()
        res = []
        self.dfs(root,k,temp,res)
        return res
    def dfs(self,root,k,temp,res):
        #记得异常检测和递归出口！
        if not root:
            return
        self.dfs(root.left,k,temp,res)
        if root.val in temp:
            res.append([k-root.val,root.val])
        else:
            temp.add(k-root.val)
        self.dfs(root.right,k,temp,res)
```

**Analysis**

Time Complexity: 

Space Complexity: 

[15. 3Sum](https://leetcode.com/problems/3sum/)

```
#a+b+c=0 ==> a+b = -c,所以只要把其中一个element设为target，然后用双指针像two-sum那样直接寻找剩下两个就好；

class Solution(object):
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        res = []
        if not nums:
            return res
        #双指针永远要排序！
        nums = sorted(nums)
        #for循环遍历数组，对于非重复元素调用双指针函数寻找他的另外两个数加和为0
        #因为three-sum，range就设为length-2，省掉最后一层无用循环提升时间
        for i in range(len(nums)-2):
            if i>0 and nums[i] == nums[i-1]:
                continue
            self.twoPointer(nums,i+1,len(nums)-1,-nums[i],res)
        return res
    
    def twoPointer(self,nums,left,right,target,res):
        #last_pair记录每次数字，防止出现一摸一样的数字组合（-1，-1，-1，-1，2，2）
        last_pair = None
        while left < right:
            if nums[left]+nums[right] == target:
                if (nums[left],nums[right])!= last_pair:
                    res.append([-target,nums[left],nums[right]])
                last_pair= (nums[left],nums[right])
                left+=1
                right-=1
            elif nums[left]+nums[right] > target:
                right-=1
            else:
                left+=1
```

**Analysis**

Time Complexity: 

Space Complexity: 

[259. 3Sum Smaller](https://leetcode.com/problems/3sum-smaller/)

```
#首先跟three sum一样，先排序再固定nums[i]然后通过双指针从nums[i]数组右侧查找范围内的数字满足条件，如果找到的话就多了right-left种可能，因为在固定左指针后，左指针右边直到右指针前都是满足小于target的，由此在while循环中不停根据这个条件挪左/右指针。
class Solution(object):
    def threeSumSmaller(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        count = 0
        nums = sorted(nums)
        #three sum一定要至少两重循环，先固定一个数值遍历，再针对这个数值做while循环双指针找符合的数量
        for i in range(len(nums)-2):
            left = i+1
            right = len(nums)-1
            while left < right:
                if nums[left]+nums[right]+nums[i] < target:
                    #固定左指针，右边所有直到右指针之前都满足要求，所以直接right-left即可
                    count+=right-left
                    left+=1
                else:
                    right-=1
        return count
```

**Analysis**

Time Complexity: 

Space Complexity: 

[16. 3Sum Closest](https://leetcode.com/problems/3sum-closest/)

```
#相向双指针，通过ans,sum打擂台不停跟新最小值
class Solution(object):
    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        #！！注意！不能设置为0！因为closet=0的时候会导致混淆！让ans直接被赋值为当前的sum而不是最小的！要注意
        ans = None
        nums = sorted(nums)
        for i in range(len(nums)-2):
            left = i+1
            right = len(nums)-1
            while left < right:
                sum = nums[i]+nums[right]+nums[left]
                if ans is None or abs(sum-target) < abs(ans-target):
                    ans = sum
                if sum <= target:
                    left+=1
                else:
                    right-=1
        return ans
```

**Analysis**

Time Complexity: 

Space Complexity: 


[18. 4Sum](https://leetcode.com/problems/4sum/)

```
#双重循环固定前两个指针，然后在每次循环中调用双指针找剩下两个的two sum,然后在循环中一次添加进res
class Solution(object):
    def fourSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        if not nums:
            return []
        res = []
        #记得排序！！！！双指针一定要记得排序！
        nums = sorted(nums)
        for i in range(len(nums)):
            ##记住去重！重复得不要循环
            if i >0 and nums[i] == nums[i-1]:
                continue
            for j in range(i+1,len(nums)):
                ##！！！！
                ##判断指针合法性：只要判断指针大于range的起点就可以！
                if j > i+1 and nums[j] == nums[j-1]:
                    continue
                #这里用pairs不直接把res传进去是因为每一次循环i,j都会出现不同的c,d，所以无法一次性在twoPointer里把四个指针都传出来，只能在双重For循环中把c,d传进来！
                pair = self.twoPointer(nums,j+1,len(nums)-1,target-nums[i]-nums[j])
                for (c,d) in pair:
                    res.append([nums[i],nums[j],c,d])
        return res
    
    def twoPointer(self,nums,left,right,target):
        #既是存储c,d的列表，又用来判断c,d是否重复出现，要一直加入distinct指针！
        pairs = []
        while left < right:
            if nums[left]+nums[right] < target:
                left+=1
            elif nums[left]+nums[right] > target:
                right-=1
            else:
                #记得判断是否为重复相同指针！！如果是不要操作！
                if not pairs or (nums[left],nums[right]) != pairs[-1]:
                    pairs.append((nums[left],nums[right]))
                ##!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
                ##一定要记住，left,right写在if外面！因为无论是否是重复得指针，都要left,right同时一动一次，要不然会卡死在重复指针这里！！！！！！！！
                left+=1
                right-=1
        return pairs
```

**Analysis**

Time Complexity: 

Space Complexity: 

[11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

```
#灵活运用相向双指针，每次移动计算当前面积，最后返回最大值即可
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        res = 0
        left = 0
        right = len(height)-1
        while left < right:
            if height[left] < height[right]:
                area = height[left]*(right-left)
                left+=1
            else:
                area = height[right]*(right-left)
                right-=1
            res = max(res,area)
        return res
```

**Analysis**

Time Complexity: 

Space Complexity: 

[363. Trapping Rain Water](https://www.lintcode.com/problem/trapping-rain-water/description)

```
##这是一道hard题，恭喜！
##首先根据题里给的图示，可以通过双指针来寻找左右最高处，然后每次每个指针所在位置能接到的雨水就是当前高度和左/右最高处的差值（因为当前位置的水会被左/右最高处围住不会流走），这样在while循环中迭代寻找左右最高处，并且求当前高度和最高处的差值作为water迭代累加出结果
class Solution:
    """
    @param heights: a list of integers
    @return: a integer
    """
    def trapRainWater(self, heights):
        if not heights:
            return 0
        water = 0
        left, right = 0, len(heights)-1
        left_maxH,right_maxH = heights[left], heights[right]
        while left <= right:
            if left_maxH <= right_maxH:
                left_maxH = max(left_maxH,heights[left])
                water+=left_maxH-heights[left]
                left+=1
            else:
                right_maxH = max(right_maxH,heights[right])
                water+=right_maxH-heights[right]
                right-=1
        return water
```

**Analysis**

Time Complexity: 

Space Complexity: 

### 背向双指针

[845. Longest Mountain in Array](https://leetcode.com/problems/longest-mountain-in-array/)

```
#相向双指针：通过定义分别求上升，下降长度，最后通过res不断叠加最大结果
class Solution(object):
    def longestMountain(self, arr):
        """
        :type arr: List[int]
        :rtype: int
        """
        if len(arr) < 3:
            return 0
        #如果对多个变量赋一个值要一直用等号，如果用逗号，等号两边数量要想等
        res = up = down = 0
        #从1起要保证数组下标不会越界！终点为len(arr)因为这是在遍历下标不会越界不要-1！
        for i in range(1,len(arr)):
            #判断山脉数列结束，标志为：在down不为0的情况下（已经在下降）前一个数值比当前数值小（出现上升趋势），或者出现重复数字（非下降/上升趋势）此时更新up,down为0进行后续山脉长度判断
            #!!!!!!!!
            #不能写成<=，因为==的情况不用down出现！！判断好逻辑！！
            if (down and arr[i-1] <= arr[i]) or arr[i-1] == arr[i]:
                up = down = 0
            #满足条件的话up+1
            up+= arr[i-1] < arr[i]
            #同理
            down+= arr[i-1] > arr[i]
            #在up,down都不为0时说明有完整的山脉数列了，开始对res迭代长度，一直保存最大值
            if up and down:
                res = max(res,up+down+1)
        return res
```

**Analysis**

Time Complexity: 

Space Complexity: 

---

## Leetcode review: 同向双指针/滑动窗口

[26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

```
#同向双指针模板记熟练！
class Solution(object):
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return 0
        #设定初始j指针
        j = 1
        for i in range(len(nums)):
            #让j指针在当前i位置继续前进直到找到不重复得数字或者遍历完数组
            while j < len(nums) and nums[j] == nums[i]:
                j+=1
            #如果j找不到不重复的就直接中断循环
            if j >= len(nums):
                break
            #如果j找到不重复的了就把不重复的那个数换到i+1位置,这样就把重复的都挪走了
            nums[i+1] = nums[j]
        #返回不重复的长度    
        return i+1
```

**Analysis**

Time Complexity: 

Space Complexity: 

[80. Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)

```
#套用同向双指针模板!理解基础上背诵
class Solution(object):
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        left = 0
        count = 1
        
        if len(nums) < 3:
            return n
        #固定一个指针，遍历另一个指针，在内部条件中挪动固定指针
        for right in range(1,n):
            #因为已经排序过，如果前后指针等值那么count+1
            if nums[left] == nums[right]:
                count +=1
            #如果不等值/等值后又遇到了不等值，count重新归1，继续后面的搜索
            else:
                count = 1
            #如果count不大于2个，Left指针右移继续查找，并且left数值变为right，此时：如果还没进行过删除那么left,right始终处于同位；但如果count>2发生过的话left指针在那次循环没有右移过所以就一直保持在重复大于2的位置，但right已经移动到了大于2的不等值处，所以通过把第一个count>2的不重复数字互换到重复大于2的第一个位置的方式完成删除操作，此后就一直进行赋值把后面的位置都挪到left指针旁边来
            if count <= 2:
                left+=1
                nums[left] = nums[right]
        #返回的结果就是right在原始数组遍历完后新数组的长度，也就是left指针的终点+1（新删除完数组的最后一个位置）
        return left+1
```

**Analysis**

Time Complexity: 

Space Complexity: 

[27. Remove Element](https://leetcode.com/problems/remove-element/)

```
#同向双指针，用j不停遍历nums，然后i记录不为val的位置数值，这样就在本身的list上删除了val因为循环时等于val直接跳过
class Solution(object):
    def removeElement(self, nums, val):
        """
        :type nums: List[int]
        :type val: int
        :rtype: int
        """
        i = 0
        j = 0
        while j < len(nums):
            #只要j位置不是待删除元素就把指针也拉过来，反之i指针保留在上次的位置，这样在循环中就会跳过val
            if (nums[j] != val):
                nums[i] = nums[j]
                i+=1
            j+=1
                
        return i
```

**Analysis**

Time Complexity: 

Space Complexity: 


[19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

```
#套用同向双指针模板，先让快指针先走n步，然后再让慢指针出发，这样他俩始终保持n个差距，这样当快指针走到终点时慢指针就是倒数第n个位置了，然后让慢指针此处的链表指向下一个结点即可删除倒数第n个结点
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        #结果链表
        res = ListNode(0)
        res.next = head
        temp = res
        for _ in range(n):
            head = head.next
        while head is not None:
            head = head.next
            temp = temp.next
        #指向下一个点，这样就删除了倒数第n个点
        temp.next = temp.next.next
        
        return res.next
```

**Analysis**

Time Complexity: 

Space Complexity: 


[283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)

```
#同向双指针模板，如果此位数非0，左右指针同位调换位置无差别；如果为0后，左右指针互换就始终把0往右边换直到终点
class Solution(object):
    def moveZeroes(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        if not nums:
            return 0
        left = 0
        right = 0
        while right < len(nums):
            if nums[right] != 0:
                nums[left], nums[right] = nums[right], nums[left]
                left+=1
            right+=1
```

**Analysis**

Time Complexity: 

Space Complexity: 

[1089. Duplicate Zeros](https://leetcode.com/problems/duplicate-zeros/)

```
#去看管家军给的路线图！看清题意！多复制的0把原本后面的元素挤掉而不是原地添加！
class Solution(object):
    def duplicateZeros(self, arr):
        n = len(arr)
        cntZero = arr.count(0)
        newLen = n + cntZero # Length of new array if we don't trim up to length `n`
        
        # Let's copy values from the end
        i = n - 1
        j = newLen - 1
        while j >= 0:
            if j < n: arr[j] = arr[i]
            j -= 1
            if arr[i] == 0: # Copy twice if arr[i] == 0
                if j < n: arr[j] = arr[i]
                j -= 1
            i -= 1
```

**Analysis**

Time Complexity: 

Space Complexity: 


[532. K-diff Pairs in an Array](https://leetcode.com/problems/k-diff-pairs-in-an-array/)

```
#双指针模板记熟！while循环j指针--在里面用各种条件判定i的走向，大多数情况下i,j不同位置出发/出发顺序不同
class Solution(object):
    def findPairs(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        nums = sorted(nums)
        i = 0
        #j要从1走！！要不然都是本身！
        j = 1
        res = 0
        while j < len(nums):
            #当两指针差值小于K,j右挪I不变使差值变大
            if nums[j] - nums[i] < k:
                j+=1
            #当两指针差值大于k，i右挪j不变使差值变小
            elif nums[j] - nums[i] > k:
                i+=1
            else:
                #记得同时更新i,j的位置！！！
                res+=1
                i+=1
                j+=1
                #判断i,j是否出现重复值，重复值跳过不再计算！
                while j < len(nums) and nums[j] == nums[j-1]:
                    j+=1
                while i < j and nums[i] == nums[i-1]:
                    i+=1
            #如果k=0，那么可能每一位本身都是自己的k-diff，这种情况下要让j右挪防止出现k-diff为自己本身的情况
            if i == j:
                j+=1
        return res
```

**Analysis**

Time Complexity: 

Space Complexity: 


[406. Minimum Size Subarray Sum](https://www.lintcode.com/problem/minimum-size-subarray-sum/description)

```
#双指针算法：
#1：此题不需要排序因为这里双指针没用到大小关系，是滑动窗口挨个累加对比结果
#2：res值要设为正无穷以防出现cur_sum为负数，所以不能用0
#3：for循环是为了遍历右边指针，被固定的左指针只有在满足条件时才会动
class Solution:
    """
    @param nums: an array of integers
    @param s: An integer
    @return: an integer representing the minimum size of subarray
    """
    def minimumSize(self, nums, s):
        left = 0
        right = 0
        res = float('inf')
        cur_sum = 0
        if not nums:
            return -1
        for right in range(len(nums)):
            cur_sum+=nums[right]
            while cur_sum >= s:
                #res累加器，循环记录最小长度
                res = min(res,right-left+1)
                #左指针移动找最短长度时要记得在cur_sum中减去挪走指针数值
                cur_sum-=nums[left]
                left+=1
        if res == float('inf'):
            return -1
        else:
            return res
```

**Analysis**

Time Complexity: 

Space Complexity: 


[384. Longest Substring Without Repeating Characters](https://www.lintcode.com/problem/longest-substring-without-repeating-characters/description)

```
#滑动窗口模板：因为是字符串判断所以用set做高效检索，要有max_len做累加器，cur_len记录当前结果，在每次循环中不断求最大
class Solution:
    """
    @param s: a string
    @return: an integer
    """
    def lengthOfLongestSubstring(self, s):
        window = set()
        left = 0
        max_len = 0
        cur_len = 0
        #这里字符就变成了右指针
        for ch in s:
            while ch in window:
                #删除之前加进来的重复字符，保留刚加进来的，指针左移继续遍历
                window.remove(s[left])
                cur_len-=1
                left+=1
            window.add(ch)
            #记得更新长度
            cur_len+=1
            max_len = max(cur_len,max_len)
        return max_len

```

**Analysis**

Time Complexity: 

Space Complexity: 

[32. Minimum Window Substring](https://www.lintcode.com/problem/minimum-window-substring/description)

```
from collections import defaultdict
class Solution:
    """
    @param source : A string
    @param target: A string
    @return: A string denote the minimum window, return "" if there is no such a string
    """
    def minWindow(self, source , target):
        # 初始化counter_s和counter_t
        counter_s = defaultdict(int)
        counter_t = defaultdict(int)
        for ch in target:
            counter_t[ch] += 1
        left = 0
        valid = 0
        # 记录最小覆盖子串的起始索引及长度
        start = -1
        minlen = float('inf')
        for right in range(len(source)):
            # 移动右边界, ch 是将移入窗口的字符
            ch = source[right]
            if ch in counter_t:
                counter_s[ch] += 1
                if counter_s[ch] == counter_t[ch]:
                    valid += 1
            
            # 判断左侧窗口是否要收缩
            while valid == len(counter_t):
                # 更新最小覆盖子串
                if right - left < minlen:
                    minlen = right - left
                    start = left
                # left_ch 是将移出窗口的字符
                left_ch = source[left]
                # 左移窗口
                left += 1
                # 进行窗口内数据的一系列更新
                if left_ch in counter_s:
                    counter_s[left_ch] -= 1
                    if counter_s[left_ch] < counter_t[left_ch]:
                        valid -= 1
        # 返回最小覆盖子串
        if start == -1:
            return ""
        return source[start: start + minlen + 1]

```

**Analysis**

Time Complexity: 

Space Complexity: 

[386. Longest Substring with At Most K Distinct Characters](https://www.lintcode.com/problem/longest-substring-with-at-most-k-distinct-characters/description)

```
#滑动窗口同向双指针：用字典存储出现次数，指针每次右移，判断当时位置是否出现在字典中，或者字典中对应的数字为0，如果是的话就cnt+1，把字典里得数值设为0（如果之前没出现过），最后无论如何字典中指针对应数值得个数都+1，指针右移；
#维护滑动窗口并且整体右移遍历，用cnt和k的关系判断，如果cnt比k大时：字典中对应左指针次数-1，如果减成0了就cnt-1，最后无论如何左指针都右移，最后在整体中更新最长长度，因为cnt是独立数字个数，长度用right-left来表示
class Solution:
    """
    @param s: A string
    @param k: An integer
    @return: An integer
    """
    def lengthOfLongestSubstringKDistinct(self, s, k):
        if k == 0 or len(s) == 0:
            return 0
        left = 0
        #cnt代表的是不同数字得个数，这样可以解决重复数字出现的问题
        ans, cnt = 0, 0
        char_dict = {}
        
        for right in range(len(s)):
            if s[right] not in char_dict or char_dict[s[right]] == 0:
                cnt+=1
                char_dict.setdefault(s[right],0)
            char_dict[s[right]]+=1
            right+=1
            
            while cnt > k:
                char_dict[s[left]]-=1
                if char_dict[s[left]]==0:
                    cnt-=1
                left+=1
            ans = max(ans,right-left)
            
        return ans

```

**Analysis**

Time Complexity: 

Space Complexity: 


[1375. Substring With At Least K Distinct Characters](https://www.lintcode.com/problem/substring-with-at-least-k-distinct-characters/description)

```
#先滑动窗口找到至少包含k个字符的左右指针，那么左右指针间子串长度加上任何一个右边的字符都满足条件，那么总数就是字符串长度-至少k个字符字串的长度
#用字典，key为每个出现的字符，value为每个字符出现的个数，那么len(字典)就是去重字符数量
#for循环左指针，每次循环中while循环右指针，当小于k个字符出现时存入字典记录个数，如果找到等于k时直接返回结果
#最后在每次for循环中把左指针右挪，不停滑动窗口进行维护，最终输出结果
class Solution:
    """
    @param s: a string
    @param k: an integer
    @return: the number of substrings there are that contain at least k distinct characters
    """
    def kDistinctCharacters(self, s, k):
        ans = 0
        char_to_count = {}
        right = 0
        for left in range(len(s)):
            # if distinct chars < k, keep increasing right
            while len(char_to_count) < k and right < len(s):
                char_to_count[s[right]] = char_to_count.get(s[right], 0) + 1
                right += 1
            # all the indices after current right is also valid
            if len(char_to_count) == k:
                # (len(s) - 1) - (right - 1) + 1
                ans += len(s) - right + 1
            # Remove current left for the next round
            char_to_count[s[left]] -= 1
            if char_to_count[s[left]] == 0:
                del char_to_count[s[left]]
        return ans;
```

**Analysis**

Time Complexity: 

Space Complexity: 

[883. Max Consecutive Ones II](https://www.lintcode.com/problem/max-consecutive-ones-ii/description)

```
#上次太累了没做，这次复习做，滑动窗口
```

**Analysis**

Time Complexity: 

Space Complexity:


[1870. number of substrings with all zeroes](https://www.lintcode.com/problem/number-of-substrings-with-all-zeroes/description)

```
#思路：先找到每个0字串，然后把每个0字串能形成的子串个数用等差数列求和累加就是最终的结果
##同向双指针开始遍历
class Solution:
    """
    @param str: the string
    @return: the number of substrings 
    """
    def stringCount(self, str):
        if not str:
            return 0
        res = 0
        #i目的：要么找到一个单独的0的指针，要么找到一个连续0的起点（也就是找每一段的0字串）
        for i in range(len(str)):
            if str[i]=='1':
                continue
            if i>0 and str[i] == str[i-1]:
                continue
            #j为了找这段连续0的长度
            j = i
            #只要当前位置不为1，就j+1，一直循环计算出每段0的长度
            while j < len(str) and str[j] != '1':
                j+=1
            #对这次i循环中等差求和计算总共数量，然后累加
            res+= (j-i)*(j-i+1)/2
        return res
```

**Analysis**

Time Complexity: 

Space Complexity:
