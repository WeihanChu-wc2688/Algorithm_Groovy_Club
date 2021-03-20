# day18: 字符串--哈希

---

## Leetcode Review 

---

[387. First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)

```python
#用counter存储字符串中每个字母出现的频率，然后遍历字符串，找到第一个value为1的返回下标，否则返回-1
from collections import Counter
class Solution(object):
    def firstUniqChar(self, s):
        """
        :type s: str
        :rtype: int
        """
        cnt = Counter(s)
        for char in s:
            if cnt[char] == 1:
                return s.index(char)
        return -1
```
**Analysis**

Time Complexity: 

Space Complexity: 

---


[242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)

```python
#hashtable + 计数器
#先遍历s存储出现过的字母，再遍历t把出现过的字母减去
#如果字典中所有索引的值都恰好为0，那就是anagram，否则就不是
class Solution(object):
    def isAnagram(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        ##此处为方法2，用counter计算s和t的频率，然后遍历其中一个判断是否出现频率相同
        if len(s) != len(t):
            return False
        cnt_s = collections.Counter(s)
        cnt_t = collections.Counter(t)
        for char in s:
            if char not in t or cnt_s[char] != cnt_t[char]:
                return False
        return True
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[55. Compare Strings](https://www.lintcode.com/problem/compare-strings/my-submissions)

```python
#用counter来记录每个Key和出现次数然后直接分别对比
class Solution:
    """
    @param A: A string
    @param B: A string
    @return: if string A contains all of the characters in B return true else return false
    """
    def compareStrings(self, A, B):
        from collections import Counter
        if not B:
            return True
        if not A:
            return False
        cna = Counter(A)
        cnb = Counter(B)
        
        for key in cnb:
            if key not in cna or cnb[key] > cna[key]:
                return False
        return True
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[55. Compare Strings](https://www.lintcode.com/problem/compare-strings/my-submissions)

```python
#用counter来记录每个Key和出现次数然后直接分别对比
class Solution:
    """
    @param A: A string
    @param B: A string
    @return: if string A contains all of the characters in B return true else return false
    """
    def compareStrings(self, A, B):
        from collections import Counter
        if not B:
            return True
        if not A:
            return False
        cna = Counter(A)
        cnb = Counter(B)
        
        for key in cnb:
            if key not in cna or cnb[key] > cna[key]:
                return False
        return True
```
**Analysis**

Time Complexity: 

Space Complexity: 

---


[720. Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)

```python
#建立空hashset和结果空字符串，遍历字符串数组，如果当前字符串抹掉最后一位出现在hashset中，就先加入hashset便于后续查重，再和结果空字符串对比长度迭代找最长的返回结果即可
class Solution(object):
    def longestWord(self, words):
        """
        :type words: List[str]
        :rtype: str
        """
        if not words:
            return None
        words.sort()
        seen, longest = set(['']), ''
        for word in words:
            #[:-1]的意思是抹掉最后一个元素的遍历，如果出现过说明这是可以被拼出来的单词，加进seen迭代找下一个拼出来的更长单词
            if word[:-1] in seen:
                seen.add(word)
                #对比长度，迭代更新最长单词
                if len(word) > len(longest):
                    longest = word
        return longest
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[171. Anagrams](https://www.lintcode.com/problem/anagrams/description)

```python
class Solution:
    # @param strs: A list of strings
    # @return: A list of strings
    def anagrams(self, strs):
        # write your code here
        dict = {}
        for word in strs:
            #每次循环动态更新存储排序后的字符串
            sortedword = ''.join(sorted(word))
            #如果不在字典里，把word存进去，如果在就把这个word给append进去
            if sortedword not in dict:
                dict[sortedword] = [word] 
            else:
                dict[sortedword] += [word]
        res = []
        #如果sortedword索引对应的数值list长度超过2即为anagram，加进res返回即可
        for item in dict:
            if len(dict[item]) >= 2:
                res += dict[item]
        return res
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)

```python
#defaultdict和dict的区别在于可以返回不在字典中的数值，输出的格式在defaultdict函数中自行设置，这样既可处理空串
#按照字典的思路吧每个字符串的组成字母按顺序传入defaultdict作为索引，字符串作为值，这样遇到重复组成直接加入对应值

class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        #首先这里判断anagram的逻辑是：sorted(string)相等即为anagram
        ans = collections.defaultdict(list)
        for s in strs:
            #因为要存入字典，所以索引要immutable，需要转化为tuple
            #接下来遍历，如果sorted string相等都加入对应索引，没有就是空列表
            ans[tuple(sorted(s))].append(s)
        return ans.values()
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[205. Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/)

```python
#判断方式：两个字符串中每个字母出现的index排列是否相同
#每个字母出现的index作为数值，字母作为索引分别存储在不同字典中，最后排序再对比
class Solution(object):
    def isIsomorphic(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        dict_s, dict_t = {}, {}
        for i, val in enumerate(s):
            #因为val尚未加进数值没法用list.append，所以直接用get赋予空列表然后再加进去
            dict_s[val] = dict_s.get(val,[]) + [i]
        for i, val in enumerate(t):
            dict_t[val] = dict_t.get(val,[]) + [i]
        #记得要排序再对比
        return sorted(dict_s.values()) == sorted(dict_t.values())
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[771. Jewels and Stones](https://leetcode.com/problems/jewels-and-stones/)

```python
#把jewel设置成hashset，遍历stones检查里面数量
class Solution(object):
    def numJewelsInStones(self, jewels, stones):
        """
        :type jewels: str
        :type stones: str
        :rtype: int
        """
        seen = set(tuple(jewels))
        res = 0
        for i in stones:
            if i in seen:
                res += 1
        return res
        
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[1893. the Valid String](https://www.lintcode.com/problem/the-valid-string/description)

```python
#自己反思！！
from collections import Counter
class Solution:
    """
    @param s: a String
    @return: if valid return "YES" else return "NO"
    """
    def isValid(self, s):
        if s == '':
            return 'NO'
            
        s_dict = {}
        for char in s:
            s_dict[char] = s_dict.get(char, 0) + 1
            
        cnt_set = set(s_dict.values())
        if len(cnt_set) > 2:
            return 'NO'
        if len(cnt_set) == 1:
            return 'YES'
        
        cnt_dict = {}
        for item in s_dict.values():
            cnt_dict[item] = cnt_dict.get(item, 0) + 1
        
        if 1 in cnt_dict.keys() and cnt_dict[1] == 1:
            return 'YES'
    
        small, large = min(cnt_dict.keys()), max(cnt_dict.keys()) 
        if cnt_dict[large] == 1 and large == small + 1:
            return 'YES'
        return 'NO'
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[1639. K-Substring with K different characters](https://www.lintcode.com/problem/k-substring-with-k-different-characters/description)

```python
#分段遍历字符串，对每个子字符串去重判断长度是否=K，如果是res+1并且放入另一个set去重
class Solution:
    """
    @param stringIn: The original string.
    @param K: The length of substrings.
    @return: return the count of substring of length K and exactly K distinct characters.
    """
    def KSubstring(self, stringIn, K):
        if not K or not stringIn:
            return 0
        res = 0
        seen = set()
        #搞清楚遍历的Index结尾要+1！！！！
        for i in range(len(stringIn)-K+1):
            if len(set(stringIn[i:i+K])) == K and stringIn[i:i+K] not in seen:
                res += 1
                seen.add(stringIn[i:i+K])
                
        return res
```
**Analysis**

Time Complexity: 

Space Complexity: 

---


[890. Find and Replace Pattern](https://leetcode.com/problems/find-and-replace-pattern/)

```python
#字符串趋势匹配：通过双字典记录字符串每个字母出现下标的排序组合，如果相同即为同趋势
#调用检查趋势子函数遍历数组中字符串，把符合条件的加进结果数组
class Solution(object):
    def findAndReplacePattern(self, words, pattern):
        """
        :type words: List[str]
        :type pattern: str
        :rtype: List[str]
        """
        res = []
        
        if not pattern or not words:
            return res
        

        for i in words:
            if self.check_pattern(i,pattern):
                res.append(i)
        return res
    
    def check_pattern(self,train,test):
        
        train_dict = {}
        test_dict = {}
        
        for i, char in enumerate(train):
            train_dict[char] = train_dict.get(char,[]) + [i]
            
        for i, char in enumerate(test):
            test_dict[char] = test_dict.get(char,[]) + [i]
            
        return sorted(train_dict.values()) == sorted(test_dict.values())
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[1485. Holy Grail spell](https://www.lintcode.com/problem/holy-grail-spell/description)

```python
#用字母堆中存储位置作为遍历方式，倒序从z往a数，找到第一个大小写同时出现在字符串中的字母返回其大写即可
class Solution:
    """
    @param Spell: The Spell
    @return: nothing
    """
    def holyGrailspell(self, Spell):
        for num in range(ord('z'),ord('a')-1,-1):
            char = chr(num)
            if char in Spell and char.upper() in Spell:
                return char.upper()
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
