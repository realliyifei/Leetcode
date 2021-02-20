# 0003M Longest Substring w/o Repeating Characters

## Description

Given a string, find the length of the longest substring without repeating characters.

Example 1:

> Input: "abcabcbb"
> Output: 3 
> Explanation: The answer is "abc", with the length of 3. 

Example 2:

> Input: "bbbbb"
> Output: 1
> Explanation: The answer is "b", with the length of 1.

Example 3:

> Input: "pwwkew"
> Output: 3
> Explanation: The answer is "wke", with the length of 3. 
> Note that the answer must be a **substring**, "pwke" is a *subsequence* and not a substring.

Initial code:

```python
def lengthOfLongestSubstring(self, s: str) -> int:
```

## Sliding Window - Accepted

We maintain a *sliding window* (滑动窗口), denotes `l...r`, with left side `l` and right side `r` . This window contains a substring without any repeating character and tries to expand its size. The right side  `r`  keeps moving to the right while checking whether the new character is identical to any character in the current window. If yes, we compare the size of current window to the longest recorded one, and then move the left side `l`  to the position next to the first repeating character. This iteration goes on and on until the right side of window reachs the end of string. The size of the longest window should be the solution.

For instance,

```
String:   a b c d e b x y z w       (repeat: b)
          l r->                      
window#1: l.......r                  size: 4
              l   r->                b repeats => reset l,r to RHS of the first b
window#2:     l.............r        size: 8  -- longest
```

Python3:

```python
l, r, longest = 0, 0, 0
while r != len(s):
    if s[r] in s[l:r]:
        longest = max(r-l, longest)
        l += s[l:r].index(s[l]) + 1
        r -= 1
    r += 1
return max(r-l, longest)
```

**Result:** runtime 88ms (41%), memory 14MB (78%)

 PS1. The initial value of `r` should be `0` but not `1`, otherwise the length of `“”` would be `1`.

PS2. After `r` checks each item, we should do `max(r-l, longest)` one more time, otherwise it would fail to pass the case where the last window is the longest one.

## Sliding Window w/ Hashmap - Best

We can enhance the algorithm by deploying hashmap.

```python
l, longest = 0, 0
dic = {}
for r, char in enumerate(s):
    if char in dic and dic[char] >= l:
        longest = max(r-l, longest)
        l = dic[char] + 1
    dic[char] = r
return max(len(s)-l, longest)
```

**Result:** runtime 44ms (99%), memory 14MB (82%)

## Ref

* [LeetCodeAnimation/0003-Longest-Substring-Without-Repeating-Characters.md at master · MisterBooo/LeetCodeAnimation](https://github.com/MisterBooo/LeetCodeAnimation/blob/master/0003-Longest-Substring-Without-Repeating-Characters/Article/0003-Longest-Substring-Without-Repeating-Characters.md)
* [LeetCode/002. Longest Substring Without Repeating Characters at master · pezy/LeetCode](https://github.com/pezy/LeetCode/tree/master/002.%20Longest%20Substring%20Without%20Repeating%20Characters) 