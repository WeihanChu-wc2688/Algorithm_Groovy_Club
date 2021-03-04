# day13: 数组-游标操作



---

## leetcode & lintcode review

---

[88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

```python
#三指针，分别指向nums1,nums2和nums1的空集，遍历同时两个数组并将最小值依次填入剩余的空集
class Solution(object):
    def merge(self, nums1, m, nums2, n):
        """
        :type nums1: List[int]
        :type m: int
        :type nums2: List[int]
        :type n: int
        :rtype: None Do not return anything, modify nums1 in-place instead.
        """
        index1, index2, insert = m-1, n-1, m+n-1
        
        while index1 >= 0 and index2 >= 0:
            if nums1[index1] <= nums2[index2]:
                nums1[insert] = nums2[index2]
                index2-=1
                insert-=1
            else:
                nums1[insert] = nums1[index1]
                index1-=1
                insert-=1
        #如果nums2比nums1长要把剩下的直接填入nums1空集中
        while index2 >= 0:
            nums1[insert] = nums2[index2]
            index2-=1
            insert-=1
```
**Analysis**

Time Complexity: 

Space Complexity: 

---


[6. Merge Two Sorted Arrays](https://www.lintcode.com/problem/merge-two-sorted-arrays/description)

```python
#逻辑一定要清楚！
#相加时想清楚：如果A小只加A，B小或等于只加B，后面满足条件后再while循环添加任何一个剩下的元素！！！
class Solution:
    """
    @param A: sorted integer array A
    @param B: sorted integer array B
    @return: A new sorted integer array
    """
    def mergeSortedArray(self, A, B):
        m = len(A)
        n = len(B)
        
        res = []
        indexa, indexb = 0, 0
        while indexa < m and indexb < n:
            if A[indexa] < B[indexb]:
                res.append(A[indexa])
                indexa+=1
            else:
                res.append(B[indexb])
                indexb+=1
        while indexa < m:
            res.append(A[indexa])
            indexa+=1
        while indexb < n:
            res.append(B[indexb])
            indexb+=1
        return res
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[39. Recover Rotated Sorted Array](https://www.lintcode.com/problem/recover-rotated-sorted-array/description)

```python
#第一次没看答案2次debug就做出来的题！
#单向指针找转折点然后移除前半部分
class Solution:
    """
    @param nums: An integer array
    @return: nothing
    """
    def recoverRotatedSortedArray(self, nums):
        if len(nums) == 0 or len(nums) == 1:
            return nums
        #记得处理数组越界！如果后面有i+1得话循环要在n-1终止以防越界！    
        for i in range(len(nums)-1):
            if nums[i+1] < nums[i]:
                #记得处理下标和位置的关系！要把第i个数挪走的话是i+1
                nums.extend(nums[:i+1])
                del nums[:i+1]
                #找到就提前剪枝
                break
        return nums
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[54. Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)

```python
#思路：设定坐标，行列移动方向数组还有行列方向三个指针，让坐标满足数组不越界并且没重复遍历的前提下一直下去，否则就换方向走直到走完所有元素
class Solution(object):
    def spiralOrder(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        """
        if not matrix:
            return None
        R, C = len(matrix), len(matrix[0])
        #构建全false矩阵
        seen = [[False]*C for _ in matrix]
        
        ans = []
        #行的方向
        dr = [0, 1, 0, -1]
        #列的方向
        dc = [1, 0, -1, 0]
        #初始化行，列，方向指针
        r = c = di = 0
        
        for _ in range(R*C):
            #在ans中加遍历到的元素，并且在seen中改标记
            ans.append(matrix[r][c])
            seen[r][c] = True
            #当前的行列指针（current r, current c)
            cr, cc = r + dr[di], c + dc[di]
            #如果cr,cc符合数组范围并且当前元素被还没被加入过ans, 那r,c更新为rc,cc
            if 0 <= cr < R and 0 <= cc <C and not seen[cr][cc]:
                r, c = cr, cc
            #如果cr,cc碰壁或者遍历到重复元素了，那就换方向遍历
            else:
                #用余数滚动因为di可能迭代多次已经大于4了
                di = (di + 1) % 4
                #更新r,c位置
                r, c = r + dr[di], c + dc[di]
        return ans
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[189. Rotate Array](https://leetcode.com/problems/rotate-array/)

```python
#读清题意！！！题目要求原地置换不能开新数组！！
#k要特殊处理：k = k % l因为如果k > l直接取余数旋转即可
#用extend操作先把旋转部分接到原数组尾部再把前面的去掉即可
class Solution(object):
    def rotate(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        if len(nums) == 0 or len(nums) == 1:
            return nums
        l = len(nums)
        k = k % l
        nums.extend(nums[:l-k])
        
        del nums[:l-k]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[144. Interleaving Positive and Negative Numbers](https://www.lintcode.com/problem/interleaving-positive-and-negative-numbers/description)

```python
#双指针in-place处理
class Solution:
    """
    @param: A: An integer array.
    @return: nothing
    """
    def rerange(self, A):
        #判断异常条件，如果不符合直接返回
        if A is None or len(A) <= 1:
            return
        
        # Assume A does not contain 0
        #此题前提为数组中无0！！！！
        #建立相向双指针
        lo, hi = 0, len(A)-1
        #循环遍历，用排序的方式把负数换到数组前半部，正数后半部
        while lo <= hi:
            #找当前循环中第一个正数
            while lo <= hi and A[lo] < 0:
                lo += 1
            #找当前循环中第一个负数    
            while lo <= hi and A[hi] > 0:
                hi -= 1
            #如果找到在不越界的前提下找到了当前循环中的第一个反顺序数那就互换位置，双指针移动依次准备下次循环调换，最终排序。
            if lo <= hi:
                A[lo], A[hi] = A[hi], A[lo]
                lo += 1
                hi -= 1
                
        # A[:lo] are neg number (amout: lo)
        # A[hi+1:] are pos number (amount: len(A)-hi-1)
        
        neg_count = lo
        pos_count = len(A)-hi-1
        
        # the amount of pos and neg number should at most diff by one
        #交错意味着正负数不能相差多于一个，不符合直接返回；
        if abs(neg_count - pos_count) > 1:
            return
        #如果负数多一个那就让负数分布在尾，意味着负数不能打头要从第二位开始
        lo = 1 if neg_count >= pos_count else 0
        #正数同理不过反过来
        hi = len(A)-2 if pos_count >= neg_count else len(A)-1
        
        #交错处理，相向双指针每两位换一次位置
        while lo < hi:

            A[lo], A[hi] = A[hi], A[lo]
            
            lo += 2
            hi -= 2
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[414. Third Maximum Number](https://leetcode.com/problems/third-maximum-number/)

```python
#学会灵活应用hashmap!!!!!!!!
#hashmap返回最大值是O（1）速度！！
#所以先判断边界条件（长度是否过3），如果没过直接返回最大值；
#如果过了移除（hashmap移除也是O（1））两次最大值就是第三大！
class Solution(object):
    def thirdMax(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        nums = set(nums)
        maxnums = max(nums)
        
        if len(nums) < 3:
            return maxnums
        
        nums.remove(maxnums)
        sec_max = max(nums)
        nums.remove(sec_max)
        return max(nums)
```
**Analysis**

Time Complexity: 

Space Complexity: 

---


[66. Plus One](https://leetcode.com/problems/plus-one/)

```python
#倒序遍历数组，如果遇到9就变成0，然后直到下次循环到最后一个非9数字再+1返回数组退出循环；
#如果全是9得话就全变成0退出循环最后输出前加个1就完事儿
class Solution:
    # @param {int[]} digits a number represented as an array of digits
    # @return {int[]} the result
    def plusOne(self, digits):
        n = len(digits)
        for i in range(n-1,-1,-1):
            
            if digits[i] == 9:
                ###他妈了隔壁看清楚赋值是=，不是==！！！！！！！！！
                digits[i] = 0
            else:
                digits[i] += 1
                return digits
            

        return [1] + digits
            
```
**Analysis**

Time Complexity: 

Space Complexity: 

---


[1053. Previous Permutation With One Swap](https://leetcode.com/problems/previous-permutation-with-one-swap/)

```python
#思想：只调换一次，找比原数组小的最大值-->尽可能调换最小数位和它右边的最大值即可成为比原来小的最大值
#所以先找到最小调换数位，然后找这个数位右面最大值调换！
class Solution(object):
    def prevPermOpt1(self, A):
        i = len(A) - 2
        #找到第一个（最小）能调换的数位（当前数字大于后一位）
        while i >= 0 and A[i] <= A[i+1]:
            i -= 1
            
        if i >= 0:
            #存储第一个能调换下标后一位的下标
			max_ = i + 1
			# max number greater on right that less than A[i]
            #这层循环是为了找右半边小于能调换下标处最大的数进行最后出循环调换位置，因为此题为了找一次swap小于原数组的最大值！
			for j in range(max_ + 1, len(A)):
				if A[max_] < A[j] < A[i]: 
					max_ = j
            #找到之后互换位置
			A[max_], A[i] = A[i], A[max_]
        return A
        
```
**Analysis**

Time Complexity: 

Space Complexity: 

---


[31. Next Permutation](https://leetcode.com/problems/next-permutation/)

```python
#没啥意义这题，看九章题解理解一下
#题解抄的，记得重做！！！！！
class Solution(object):
    def nextPermutation(self, nums):
        """
        :type nums: List[int]
        :rtype: void Do not return anything, modify nums in-place instead.
        """
        # Use two-pointers: two pointers start from back
        # first pointer j stop at descending point
        # second pointer i stop at value > nums[j]
        # swap and sort rest
        if not nums: return None
        i = len(nums)-1
        j = -1 # j is set to -1 for case `4321`, so need to reverse all in following step
        while i > 0:
            if nums[i-1] < nums[i]: # first one violates the trend
              j = i-1
              break
            i-=1
        for i in xrange(len(nums)-1, -1, -1):
            if nums[i] > nums[j]: # 
                nums[i], nums[j] = nums[j], nums[i] # swap position
                nums[j+1:] = sorted(nums[j+1:]) # sort rest
                return
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[387. The Smallest Difference](https://www.lintcode.com/problem/the-smallest-difference/description)

```python
#因为要时间复杂度O(nlogn)意味着要用排序/二分法
#先合并数组，合并用tuple这样可以鉴别来自哪个数组
#然后遍历排序过的新数组迭代找相邻位置来自不同数组的最小差值
class Solution:
    """
    @param A: An integer array
    @param B: An integer array
    @return: Their smallest difference.
    """
    def smallestDifference(self, A, B):
        c = []
        for i in A:
            c.append((i, 'A'))
        for i in B:
            c.append((i,'B'))
        res = 0x7fffffff
        n = len(c)
        c.sort()
        for i in xrange(n-1):
            if c[i][1] != c[i+1][1]:
                #排过序所以不用绝对值相减了
                res = min(res,c[i+1][0]-c[i][0])
        return res
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
