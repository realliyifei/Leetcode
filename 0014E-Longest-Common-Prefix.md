# 0014E Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

**Example 1:**

```
Input: ["flower","flow","flight"]
Output: "fl"
```

**Example 2:**

```
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

**Note:**

All given inputs are in lowercase letters `a-z`.

## Horizontal Scanning

We repeatedly compare and shorten the common prefix of the current prefix and the next word. ([lc-sol](https://leetcode.com/problems/longest-common-prefix/solution/))

Python3:

```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs:
            return ""
        prefix, n = strs[0], len(strs[0])
        for word in strs:
            while not word.startswith(prefix):
                if n == 0:
                    break
                n -= 1
                prefix = prefix[0:n]
        return prefix
```

**Result:** runtime 24ms (99%), memory 14MB (35%). Time $O(n)$ where $n$ is the total length of all words, and Space $O(1)$.

## Vertical Scanning

We can compare each char from top to bottom. ([lc-sol](https://leetcode.com/problems/longest-common-prefix/solution/))

Python3:

```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs:
            return ""
        prefix = ""
        for i in range(len(min(strs)):
            c = strs[0][i]
            if all(word[i]==c for word in strs):
                prefix += c
            else:
                break
        return prefix
```

**Result:** runtime 24ms (99%), memory 14MB (35%). Time $O(n)$ where $n$ is the total length of all words, and Space $O(1)$.

## Common Prefix of Min and Max

We can first get the min and max of words, and then get their common prefix as the result.

Python3:

```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs: return ""
        prefix, minword, maxword = "", min(strs), max(strs)
        for i in range(len(minword)):
            if minword[i] == maxword[i]:
                prefix += minword[i]
            else:
                break
        return prefix 
```

**Result:** runtime 32ms (82%), memory 14MB (30%). Time $O(n)$ where $n$ is the total length of all words, and Space $O(1)$.

## Trie

We can also put the words to a trie and then find the prefix whose ending point is the last node that *only* contains one child or is the end of a word. ([lc-sol](https://leetcode.com/problems/longest-common-prefix/solution/)) Notes that for this problem, the trie implement wastes space a bit but it’s never hurt to try.

Python3:

```python
class TrieNode():
    def __init__(self):
        self.chars = {}
        self.isEndOfWord = False

class Trie:    
    def __init__(self):
        self.root = TrieNode()
        
    def insert(self, word):
        node = self.root
        for c in word:
            if c not in node.chars:
                node.chars[c] = TrieNode()
            node = node.chars[c]
        node.isEndOfWord = True
            
    def findPrefix(self):
        node = self.root
        prefix = ""
        while len(node.chars) == 1 and not node.isEndOfWord:
            for c in node.chars:  
                prefix += c
                node = node.chars[c]
        return prefix
    
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs: 
            return ""
        trie = Trie()
        for word in strs:
            trie.insert(word) 
        return trie.findPrefix()
```

**Result:** runtime 40ms (48%), memory 14MB (35%). Time $O(n)$ where $n$ is the total length of all words, and Space $O(n)$. 

