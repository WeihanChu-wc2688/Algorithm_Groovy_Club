# day4_二分搜索

---

## Leetcode Review

---

[69. Sqrt(x)](https://leetcode.com/problems/sqrtx/)

```
##二分法模板：1：mid要在while里；2：确定返回结果的left还是right
class Solution(object):
    def mySqrt(self, x):
        """
        :type x: int
        :rtype: int
        """
        if x is None:
            return None
        left,right = 0, x
        
        while left+1 < right:
            mid = (left + right)/2
            if mid*mid > x:
                right = mid
            elif mid*mid == x:
                return mid
            else:
                left = mid
        if left*left == x:
            return left
        if right*right == x:
            return right
        return left
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```
##————————————————————————
##模板灵活运用！！！！！！while后面的部分先start还是end是确定找目标的首位还是末位，如果首末位都取的话就先找首位，再找末尾，最后返回两个index即可!因为数组是递增序列所以不用担心中间的目标值有变化！加一个boolean进行判断
class Solution:
    """
    @param nums: the array of integers
    @param target: 
    @return: the starting and ending position
    """
    def searchRange(self, nums, target):
        # Write your code here.
        if not nums:
            return [-1, -1]
            
        first_index =  self.binarySearch(nums, target, True)
        if first_index == -1:
            return [-1, -1]
        last_index =  self.binarySearch(nums, target, False)
        return [first_index, last_index]
        

    
    def binarySearch(self, nums, target, isFindFirst):
        start, end = 0, len(nums) - 1 
        while start + 1 < end:
            mid = (start + end) // 2 
            if target > nums[mid]:
                start = mid
            elif target < nums[mid]:
                end = mid
            else:
                if isFindFirst:
                    end = mid
                else:
                    start = mid
        if isFindFirst:
            if nums[start] == target:
                return start 
            if nums[end] == target:
                return end 
            return -1
        
        else:
            if nums[end] == target:
                return end
            if nums[start] == target:
                return start
            return -1
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[658. Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements/)

```
##-------------------
##滑动窗口：不停的对比窗口两端数值和目标值的差，直到锁定在最小范围无法优化
##因为要找k个数字所以窗口长度为k
##因为要二分查找所以查找窗口起始位置left
##当left==right停下来即为窗口起点
class Solution(object):
    def findClosestElements(self, A, k, x):
        left, right = 0, len(A) - k
        while left < right:
            mid = (left + right) / 2
            if x - A[mid] > A[mid + k] - x:
                left = mid + 1
            else:
                right = mid
        return A[left:left + k]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[704. Binary Search](https://leetcode.com/problems/binary-search/)

```
##-----------
##模板练熟
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        if not nums:
            return -1
        left,right = 0, len(nums)-1
        while left+1 < right:
            mid = (left+right)/2
            if nums[mid] < target:
                left = mid
            elif nums[mid] == target:
                right = mid
            else:
                right = mid
        if nums[left] == target:
            return left
        if nums[right] == target:
            return right
        return -1
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[702. Search in a Sorted Array of Unknown Size](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/)

```
class Solution:
##---------------------------------
##倍增法，当前点用while不停2倍向右查找直到当前点大于目标点进而作为右边点进行二分查找

    """
    @param: reader: An instance of ArrayReader.
    @param: target: An integer
    @return: An integer which is the first index of target.
    """
    def search(self, ArrayReader, target):
        l, r = 0, 1
        while ArrayReader.get(r) < target:
            r <<= 1
        
        while (l+1 < r):
            mid = (l + r) >> 1
            if ArrayReader.get(mid) >= target:
                r = mid
            else:
                l = mid
        
        if ArrayReader.get(l) == target:
            return l
        if ArrayReader.get(r) == target:
            return r
        return -1
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[278. First Bad Version](https://leetcode.com/problems/first-bad-version/)

```
##---------------------
##直接用二分法模板，如果是bad version往左查，不是往右查，最后得左右结果应该是对错交界/全错；如果左边是错的就是全错，返回左边的；反之右边。
##要记得看清函数定义！！true的时候是！错！的！

class Solution(object):
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        if not n:
            return None
        left,right = 0, n
        while left+1<right:
            mid = (left+right)/2
            if isBadVersion(mid):
                right = mid
            else:
                left = mid
        if isBadVersion(left):
            return left
        else:
            return right
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

```
##-----------------------
##先用二分法，判断中点和起点的大小关系，如果中点左侧递增而且target数值在此区间内则mid向左挪，如果target不在此区间就mid向右挪；如果中点左侧不递增证明旋转过，再判断target和区间的关系确定mid查找的方向；
##问题的本质就是二分查找时通过target和起点&mid的关系再判断一次查找的方向
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        if not nums:
            return -1
        start,end = 0, len(nums)-1
        while start+1 < end:
            mid = (start+end)/2
            if nums[mid] > nums[start]:
                if nums[start] <= target <= nums[mid]:
                    end = mid
                else:
                    start = mid
            else:
                if nums[mid] <= target <= nums[end]:
                    start = mid
                else:
                    end = mid
                
        if nums[start] == target:
            return start
        if nums[end] == target:
            return end
        return -1
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[81. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

```
##-------------------
##三大步骤：第一：先二分找到旋转的pivot；第二：根据旋转的pivot来确定接下来二分找target的范围；
##第三：二分找到target
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: bool
        """
        if not nums:
            return False
        start,end = 0, len(nums)-1
        ##二分找Pivot：用mid值和end值对比！
        while start < end:
            mid = (start+end)//2
            ##如果mid值大于end，说明右侧数组被翻转过，pivot一定在（mid,end]，所以向mid下一位找
            if nums[mid] > nums[end]:
                start = mid+1
            ##如果mid值小于end，则右侧数组没被翻转过，向左二分查找pivot
            elif nums[mid] < nums[end]:
                end = mid
            ##如果相等：
            else:
                ##如果end值的前一位比end大说明end是第一个减小得点，pivot find;记得end>0防止越界
                if (end>0 and nums[end-1]>nums[end]):
                    start = end
                ##如果大家都相等说明遇到了重复数值，继续往左挪一位查找pivot
                else:
                    end-=1
        ##因为我们用start==end作为查找pivot终止条件，所以start作为pivot
        pivot = start
        ##第一位是pivot的话就是一个没被翻转的数组，直接标准二分查找
        if pivot == 0:
            left, right = 0, len(nums)-1
        ##-----------
        ##!!!!注意逻辑！判断pivot的条件不同一定要用if,elif和else!!!!!!!!!!!
        ##如果第一位比目标值大说明目标值在翻转的部分，二分的起始点就从pivot开始了
        elif nums[0] > target:
            left, right = pivot, len(nums)-1
        ##如果第一位比目标值小说明目标值在没被翻转的部分（数组递增），二分终点即为pivot前的点
        else:
            left, right = 0, pivot-1
        ##对找到的目标值范围进行二分查找
        while left+1 < right:
            m = (left+right)/2
            if nums[m] < target:
                left = m
            else:
                right = m
                
        if nums[left] == target:
            return True
        if nums[right] == target:
            return True
        return False
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

```
##因为反转的递增有序数组一定是后半段小前半段大，所以从最右边反向二分查找最小值
class Solution(object):
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if nums is None:
            return -1
        left,right = 0,len(nums)-1
        while left+1 < right:
            mid = (left+right)/2
            if nums[mid] > nums[right]:
                left = mid
            else:
                right = mid
        return min(nums[left],nums[right])
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[162. Find Peak Element](https://leetcode.com/problems/find-peak-element/)

```
##首先：山峰数一定左右递减才能找中间段的任意峰值；
##二分查找分两种情况分别处理：
##如果mid指针前一位更大，则当前正在下降，峰值会出现在前面，所以向左调整指针
##如果mid前一位更小，则当前在上升，峰值会出现在后面，右调整指针
##将左右mid逼近区间后对比找最大的那个返回山顶，因为最后一定会返回山顶和离山顶最近的一个数字
class Solution(object):
    def findPeakElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if not nums:
            return None
        start, end = 0, len(nums)-1
        while start+1 < end:
            mid = (start+end)/2
            if nums[mid] < nums[mid-1]:
                end = mid
            else:
                start = mid
            
        if nums[start]>nums[end]:
            return start
        else:
            return end
```

**Analysis**

Time Complexity: 

Space Complexity: 

---
[74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

```
##---------------------------------
##将二维有序数组flatten,变成单调递增一维数组，然后将下标i,j转换成i*(每行长度)+j，在进行二分模板查找，记住mid下标在matrix取数的时候要转换回二维下标取数（行是除数，列是余数）
class Solution:
    """
    @param matrix, a list of lists of integers
    @param target, an integer
    @return a boolean, indicate whether matrix contains target
    """
    def searchMatrix(self, matrix, target):
        col = len(matrix[0])
        left, right = 0, len(matrix) * col - 1
        while left+1 < right:
            mid = (right + left) / 2
            value = matrix[mid / col][mid % col]
            if value == target:
                return True
            elif value > target:
                right = mid
            else:
                left = mid 
        if matrix[right / col][right % col] == target:
            return True
        if matrix[left / col][left % col] == target:
            return True 
        return False
```

**Analysis**

Time Complexity: 

Space Complexity: 

---
[240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)

```
##-------------------
##因为数组自上而下增，自左向右增，而且没有连续重复，所以从左下角开始作为二分中点向右上角搜索，如果小了就右移，如果大了就上移，如果找到一个就用count计数后同时ij向右上移动，最终走到右上角。
class Solution(object):
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if matrix is None or matrix[0] is []:
            return 0
        row, col = len(matrix), len(matrix[0])
        i, j = row-1, 0
        count = 0
        while i >= 0 and j < col:
            if matrix[i][j] == target:
                count+=1
                ##因为没有连续重复，所以要同时i+1,j+1因为无论纯右边还是纯上边都不会再有target了，只可能是右上
                i-=1
                j+=1
            elif matrix[i][j] < target:
                j+=1
            else:
                i-=1
        return count
```

**Analysis**

Time Complexity: 

Space Complexity: 

---
