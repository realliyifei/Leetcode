# 0076H Minimum Window Substring

Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

**Example:**

```
Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
```

**Note:**

- If there is no such window in S that covers all characters in T, return the empty string `""`.
- If there is such window, you are guaranteed that there will always be only one unique minimum window in S.

## Sliding Window w/ Hashmap

We can first build a *dictionary* `dic` to store T as {char-frequency}. 

Then, we maintain a *sliding window* with left side `l` and right side `r` to check whether the current window in S can cover all characters in T with the help of `dic`. Notes that the frequency of char in dictionary can be negative because redundant target chars in the result string don't matter. The window slides following the procedures demonstrated as below. ([more detail](https://leetcode-cn.com/problems/minimum-window-substring/solution/tong-su-qie-xiang-xi-de-miao-shu-hua-dong-chuang-k/))

```
S:         ADOBECODEBANC    T = "ABC"
window#1   ^**^*^         - DONE
window#2      ^*^         - moves `l` to the next target char inside
window#3      ^*^****^    - expends `r` until the next target char outside, DONE
```

Python3:

```python
from collections import defaultdict
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        # the index of left and right of window as result
        index = (0, float('inf'))
        
        # set dictionary
        dic = defaultdict(int)
        for char in t:
            dic[char] += 1
        t_count = len(t)
        
        # check by sliding window
        l, r = 0, 0
        while r != len(s):
            
            # move the right side of window
            char = s[r]
            if char in dic: 
                if dic[char] > 0:
                    t_count -= 1
                dic[char] -= 1
            r += 1

            if t_count == 0:
                # move the left side of window
                while True:
                    char = s[l]
                    if char in dic:
                        if dic[char] == 0:
                            break
                        dic[char] += 1
                    l += 1
                # store result    
                if (r - l) < (index[1] - index[0]):
                    index = (l, r)
                # go to the next window checking
                dic[s[l]] += 1
                t_count += 1
                l += 1
        
        return "" if index[1] > len(s) else s[index[0] : index[1]]  
```

**Result:** runtime 80ms (99%), memory 15MB (50%)

**Varient 1:** we can use list to store the next position of left side instead of incrementing `l`, and thus trade time with space.

**knowledge:** we can find template of **substring problem** [here](https://leetcode.com/problems/minimum-window-substring/discuss/26808/Here-is-a-10-line-template-that-can-solve-most-'substring'-problems).

PS. When storing the result, here we just store the index `(l, r)` instead of the temporary string result `s[l:r]` and its corresponding temporary length minimum to save space. 