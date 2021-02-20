# 0022M Generate Parentheses

Given *n* pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given *n* = 3, a solution set is:

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

## Stupid Brute Force

We can brutally insert "()" in each position of each string in the last combinations. Notes that actually we don't need to sort the list in return but the test cases on leetcode are sorted, so the runtime here is exaggerated a bit.

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        comb = [""]
        while n != 0:
            new_comb = []
            for s in comb:
                for i in range(0, len(s)+1):
                    newstr = s[:i] + "()" + s[i:]
                    if newstr not in new_comb:
                        new_comb.append(newstr)
            comb = new_comb
            n -= 1
        return sorted(comb)
```

**Result:** runtime 180ms (5%), memory 14MB (68%)

There is another brute force on [lc-sol](https://leetcode.com/articles/generate-parentheses/) (approach 1).

## Recursion Backtracking

We can insert '(' and ')' when it's valid by backtracking recursion. A more detailed explanation is on [lc-sol](https://leetcode.com/articles/generate-parentheses/) (approach 2). 

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        ans = []
        def backtrack(s = "", l = 0, r = 0):
            # Base case
            if len(s) == 2 * n: ans.append(s)
            # recursion
            if l < n: backtrack(s+'(', l+1, r)
            if r < l: backtrack(s+')', l, r+1)
        backtrack()
        return ans
```

**Result:** runtime 28ms (95%), memory 14MB (76%)

Time: $O(\frac{4^n}{\sqrt{n}})$, which is equal to n-th *Catalan Number*  $\frac{1}{n+1}P_{2n}^{n} < \frac{4^n}{n\sqrt{n}}$ 

Space: $O(\frac{4^n}{\sqrt{n}})$ 

## Recursion: Closure Number

We can generate `""(" + seq1 + ")" + seq2" ` recursively. A more detailed explanation is on [lc-sol](https://leetcode.com/articles/generate-parentheses/) (approach 3). 

Python3:

```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        ans = []
        # Base case
        if n == 0: return [""]
        elif n == 1: return ["()"]
        elif n == 2: return ["(())", "()()"]
        # Recursion
        for i in range(n):
            seq1 = self.generateParenthesis(i)
            seq2 = self.generateParenthesis(n-i-1)
            ans.extend(["(%s)%s" % (p, q) for p in seq1 for q in seq2])
        return sorted(ans)
```

**Result:** runtime 32ms (84%), memory 14MB (22%)

Time & Space: $O(\frac{4^n}{\sqrt{n}})$ 