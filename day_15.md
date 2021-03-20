# day15: 数组专题--滑动窗口

---

## Leetcode Review 

---

[604. Window Sum](https://www.lintcode.com/problem/window-sum/my-submissions)

```python
#不要再用简单粗暴地方法每次循环遍历窗口内数组！要想方法优化时空复杂度
#窗口内的部分有共享就不要再总重复计算
#滑动窗口，左指针0右指针k-1，先算出初始位sum然后当右指针小于k-1时动态维护窗口，挪掉左边加入右边
class Solution:
    """
    @param nums: a list of integers.
    @param k: length of window.
    @return: the sum of the element inside the window at each moving.
    """
    def winSum(self, nums, k):
        n = len(nums)
        if not nums:
            return []
        l,r,sum = 0, k-1, 0
        res = []
        for i in range(k):
            sum += nums[i]
        res.append(sum)
        #注意数组越界，小于n-1!
        while r < n-1 :
            sum-=nums[l]
            l+=1
            r+=1
            sum+=nums[r]
            res.append(sum)
        return res
```
**Analysis**

Time Complexity: 

Space Complexity: 

---


[692. Sliding Window Unique Elements Sum](https://www.lintcode.com/problem/sliding-window-unique-elements-sum/description)

```python
#滑动窗口+字典，索引为数字，数值为出现次数
class Solution:
    """
    @param nums: the given array
    @param k: the window size
    @return: the sum of the count of unique elements in each window
    """
    def slidingWindowUniqueElementsSum(self, nums, k):
        seen = {}
        unique = 0
        res = 0
        n = len(nums)
        if k > n:
            #搞清楚赋值顺序！如果k比n大就让k = n ! 否则k就爆了无法遍历
            k = n
        #首先对第一个窗口进行处理，把结果加进res，后续可进行滑动（每次去首填尾）
        for i in range(k):
            if nums[i] not in seen:
                seen[nums[i]] = 1
                unique += 1
            #注意如果这个数字已经出现过并且只出现过一次要uniqe-=1！如果不是一次不用减！    
            else:
                if seen[nums[i]] == 1:
                    unique -= 1
                seen[nums[i]] += 1
        res += unique
        #遍历滑动，先把当前窗口的头去掉，通过头出现的次数进而进行后续操作
        for i in range(k,n):
            #先判断要去掉的元素（当前窗口头），后面再判断准备加进来的元素
            prev = nums[i - k]
            seen[prev] -= 1
            #如果还有一次说明上个窗口不是独立的，这个窗口变成独立unique+=1;
            if seen[prev] == 1:
                unique += 1
            #如果等于0说明上次是独立这次没了，unique-=1    
            elif seen[prev] == 0:
                unique -= 1
            #如果新元素没出现过，或者曾经出现过但被挪掉了，就加进来    
            if (nums[i] not in seen) or (seen[nums[i]] == 0):
                seen[nums[i]] = 1
                unique += 1
            #如果新元素出现过而且出现过1次，那把unique-=1,再出现次数+=1！
            else:
                if seen[nums[i]] == 1:
                    unique -= 1
                seen[nums[i]] += 1
            #想清楚！每个窗口都要加一次！而不是最后全局叠加！因为每一次滑动数字都会变化！  
            res += unique
        
        return res
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[406. Minimum Size Subarray Sum](https://www.lintcode.com/problem/minimum-size-subarray-sum)

```python
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

---

[384. Longest Substring Without Repeating Characters](https://www.lintcode.com/problem/longest-substring-without-repeating-characters)

```python
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

---


[32. Minimum Window Substring](https://www.lintcode.com/problem/minimum-window-substring)

```python
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

---

[386. Longest Substring with At Most K Distinct Characters](https://www.lintcode.com/problem/longest-substring-with-at-most-k-distinct-characters/description)

```python
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

---

[1375. Substring With At Least K Distinct Characters](https://www.lintcode.com/problem/substring-with-at-least-k-distinct-characters/my-submissions)

```python
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

---

[487. Max Consecutive Ones II](https://leetcode.com/problems/max-consecutive-ones-ii/)

```python
#一维动态规划，用temp存储之前翻一个0后1的长度，len_flip动态更新找后面1的长度，res存储每次循环最长结果
class Solution(object):
    def findMaxConsecutiveOnes(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        len_temp_flip, len_flip, res = 0, 0, 0
        for i in nums:
            #如果此处为1，两个指针都+1更新
            if i == 1:
                len_temp_flip += 1
                len_flip += 1
            #如果为0，就翻1，并且把当前1长度记录在temp里，然后再把len_flip归0循环找后面的
            else:
                len_flip += 1
                len_temp_flip, len_flip = len_flip, 0
            #在每次循环里对比两指针找最大值即为结果，最终循环输出最大值
            res = max(res, len_temp_flip, len_flip)
        return res
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[1870. Number of Substrings with All Zeroes](https://www.lintcode.com/problem/number-of-substrings-with-all-zeroes/description)

```python
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

---


[1052. Grumpy Bookstore Owner](https://leetcode.com/problems/grumpy-bookstore-owner/)

```python
#滑动窗口模板，理解熟读背诵
#本题先求不用X时最大满意人数，然后滑窗找出X使用时能最大变为满意的人数然后最终加起来
class Solution(object):
    def maxSatisfied(self, customers, grumpy, X):
        """
        :type customers: List[int]
        :type grumpy: List[int]
        :type X: int
        :rtype: int
        """
        #异常检测，如果两个长度不等直接退出
        if len(customers) != len(grumpy):
            return None
        #三个指针分别是：动态指针，当前最大值的指针，还有原始最大值
        turn_satisfy, max_satisfy, initial_satisfy = 0, 0, 0
        #找原始最大值
        for i in range(len(grumpy)):
            if grumpy[i] == 0:
                initial_satisfy += customers[i]
        #滑窗，右指针遍历，如果碰到grumpy就把这分钟人数加进动态指针临时存储；
        #如果到了X长度移左指针，如果左指针此刻就是grumpy得话把动态指针里的人数吐出来再加右边的
        #最后循环末尾记录最大人数，最终循环结束就是结果
        left = 0
        for right in range(len(customers)):
            if grumpy[right] == 1:
                turn_satisfy += customers[right]
            #记得下标处理要+1
            if right - left + 1 > X:
                if grumpy[left] == 1:
                    turn_satisfy -= customers[left]
                left += 1
            max_satisfy = max(turn_satisfy, max_satisfy)
        return initial_satisfy + max_satisfy
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
