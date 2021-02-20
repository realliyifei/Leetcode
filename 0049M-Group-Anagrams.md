# 0049M Group Anagrams

Given an array of strings, group anagrams together.

**Example:**

```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**Note:**

- All inputs will be in lowercase.
- The order of your output does not matter.

## List-Hashmap of Sorted-Char Tuples

By [lc-cn-sol](https://leetcode-cn.com/problems/group-anagrams/solution/zi-mu-yi-wei-ci-fen-zu-by-leetcode/), we have two methods. The first one is to build a list hashmap with sorted tuple of characters as keys.

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        dic = collections.defaultdict(list)
        for word in strs:
            dic[tuple(sorted(word))].append(word)
        return dic.values()
```

**Result:** runtime 112ms (62%), memory 18MB (38%)

Time $O(NK\log K)$, where $N$ is the length of `strs`, and $K$ is the longest length of `word`.

Space $O(NK)$ 

## List-Hashmap of Char-Frequency Tuples

The second method is to count the frequency of each 26 characters as keys of hashmap. It saves more time because it doesn’t need to sort the characters for each word. 

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        dic = collections.defaultdict(list)
        for word in strs:
            count = [0] * 26
            for char in word:
                count[(ord(char) - ord('a'))] += 1
            dic[tuple(count)].append(word)
        return dic.values()
```

**Result:** runtime 124ms (48%), memory 19MB (5%)

Time $O(NK)$, where $N$ is the length of `strs`, and $K$ is the longest length of `word`.

Space $O(NK)$ 

