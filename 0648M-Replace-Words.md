# 0648M Replace Words

In English, we have a concept called `root`, which can be followed by some other words to form another longer word - let's call this word `successor`. For example, the root `an`, followed by `other`, which can form another word `another`.

Now, given a dictionary consisting of many roots and a sentence. You need to replace all the `successor` in the sentence with the `root`forming it. If a `successor` has many `roots` can form it, replace it with the root with the shortest length.

You need to output the sentence after the replacement.

**Example 1:**

```
Input: dict = ["cat","bat","rat"], sentence = "the cattle was rattled by the battery"
Output: "the cat was rat by the bat"
```

**Constraints:**

- The input will only have lower-case letters.
- `1 <= dict.length <= 1000`
- `1 <= dict[i].length <= 100`
- 1 <= sentence words number <= 1000
- 1 <= sentence words length <= 1000

**Notes (by me):** the length of words is not necessarily the same, and the roots may have duplicate parts, for instance, we have

```
Input: dict = ["catt","cat","bat","rat”], sentense = "the cattle was rattled by the battery”
Output: "the cat was rat by the bat”
```

## Basic: loops

Check the prefixs directly. ([link](https://leetcode.com/problems/replace-words/discuss/105755/Python-Straightforward-with-Explanation-(Prefix-hash-Trie-solutions)))

```python
from collections import defaultdict
class Solution:
    def replaceWords(self, dict: List[str], sentence: str) -> str:
        rootset = set(dict)
        
        def replaceWord(word):
            for i in range(1, len(word)):
                if word[:i] in rootset:
                    return word[:i]
            return word
            
        return ' '.join(map(replaceWord, sentence.split()))
```

**Result:** runtime 240ms (31%), memory 18MB (57%)

Another solution:

```python
from collections import defaultdict
class Solution:
    def replaceWords(self, dict: List[str], sentence: str) -> str:
        words = sentence.split()
        for i in range(len(words)):
            for root in dict:
                if words[i].startswith(root):
                    words[i] = root
        return ' '.join(words)
```

**Result:** time 292ms (22%), memory 18.5MB (55%)

## Improve: Hashmap

We first put roots to hashmap as {initials-roots}, then check and replace the words one-by-one with the help of dictionary.

Python3:

```python
from collections import defaultdict
class Solution:
    def replaceWords(self, dict: List[str], sentence: str) -> str:
        
        # Dictionary {initials - roots}
        root_dict = defaultdict(list)
        for root in dict:
            root_dict[root[0]].append(root)
        
        # Check and replace matched words with roots
        words = sentence.split()
        for i in range(len(words)):
            for root in root_dict[words[i][0]]:
                if words[i].startswith(root):
                    words[i] = root
        
        return ' '.join(words)
```

**Result:** runtime 40ms (100%), memory 18MB (90%)

We can also rewrite the code as below.

```python
from collections import defaultdict
class Solution:
    def replaceWords(self, dict: List[str], sentence: str) -> str:
        
        # Dictionary {initials - roots}
        root_dict = defaultdict(list)
        for root in dict:
            root_dict[root[0]].append(root)
        
        # Check and replace matched words with roots
        def replaceWord(word):
            for root in root_dict[word[0]]:
                if word.startswith(root):
                    word = root
            return word
        
        return ' '.join(map(replaceWord, sentence.split()))
```

## Improve: Trie

We can build a [trie](https://en.wikipedia.org/wiki/Trie) ([前缀树/字典树](https://zh.wikipedia.org/wiki/Trie)) to store the chars of roots. Besides char indicators, each node has a boolean flag that determines whether the current node is the end of a root.

Python3: 

```python
class TrieNode():
    def __init__(self):
        self.chars = {}
        self.isEndOfRoot = False

class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word):
        node = self.root
        for c in word:
            if c not in node.chars:
                node.chars[c] = TrieNode()
            node = node.chars[c]
        node.isEndOfRoot = True
        
    def replaceWord(self, word):
        node = self.root
        prefix = ""
        for c in word:
            if c not in node.chars:
                break
            node = node.chars[c]
            prefix += c
            if node.isEndOfRoot:
                return prefix
        return word
        
class Solution:
    def replaceWords(self, dict: List[str], sentence: str) -> str:
        trie = Trie()
        for root in dict:
            trie.insert(root)
        return ' '.join(map(trie.replaceWord, sentence.split()))
```

**Result:** runtime 112ms (81%), memory 35MB (40%)

