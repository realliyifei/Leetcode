# 0017M Letter Combinations of a Phone Number

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

**Example:**

```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**Note:**

Although the above answer is in lexicographical order, your answer could be in any order you want.

## General

Python3:

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits: return []
        phone = {
            '2':['a','b','c'], '3':['d','e','f'], '4':['g','h','i'], 
            '5':['j','k','l'], '6':['m','n','o'], '8':['t','u','v'], 
            '7':['p','q','r','s'], '9':['w','x','y','z']
            }
        ans = [""]
        for digit in digits:
            tmp, ans = ans, []
            for comb in tmp:
                for letter in phone[digit]:
                    ans.append(comb + letter)
        return ans
```

**Result:** runtime 32ms (64%), memory 14MB 54(%)

## DFS Recursion

![02b0ec926e3da5f12a0a118293b8ac10dc236741ccb04414ded44a30f7fc70af-1573829897(1).jpg 554×328 pixels](https://pic.leetcode-cn.com/02b0ec926e3da5f12a0a118293b8ac10dc236741ccb04414ded44a30f7fc70af-1573829897(1).jpg)

[lc-sol](https://leetcode.com/problems/letter-combinations-of-a-phone-number/solution/). Python3:

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        phone = {
            '2':['a','b','c'], '3':['d','e','f'], '4':['g','h','i'], 
            '5':['j','k','l'], '6':['m','n','o'], '8':['t','u','v'], 
            '7':['p','q','r','s'], '9':['w','x','y','z']
            }
        
        def dfs(comb, i):
            if i == len(digits):
                ans.append(comb)
            else:
                for letter in phone[digits[i]]:    
                    dfs(comb + letter, i + 1)
        
        ans = []
        if digits:
            dfs("", 0)
        return ans
```

**Result:** runtime 32ms (64%), memory 14MB (38%)

What happens:

```
dfs("", 0)   
    dfs("a", 1)
        dfs("ad", 2)
        dfs("ae", 2)
        dfs("af", 2)
    dfs("b", 1)
        dfs("bd", 2)
        dfs("be", 2)
        dfs("bf", 2)
    dfs("c", 1)
        dfs("cd", 2)
        dfs("ce", 2)
        dfs("cf", 2)
```