# 0020E Valid Parentheses

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

**Example 1:**

```
Input: "()"
Output: true
```

**Example 2:**

```
Input: "()[]{}"
Output: true
```

**Example 3:**

```
Input: "(]"
Output: false
```

**Example 4:**

```
Input: "([)]"
Output: false
```

**Example 5:**

```
Input: "{[]}"
Output: true
```

## Stack

We use stack to store and offset the pop-up parentheses. It's valid if the stack is empty in the end.

Python3:

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        for parenthese in s:
            if (stack and 
                ((stack[-1] == '(' and parenthese == ')') or
                 (stack[-1] == '[' and parenthese == ']') or
                 (stack[-1] == '{' and parenthese == '}'))):
                    del stack[-1]
            else:
                stack.append(parenthese)
        return not stack
```

**Result:** runtime 28ms (84%), memory 14MB (59%)

## Improved Stack w/ Hashmap

The code can be rewrote as below by adding dict {closing-bracket - opening-bracket}. Moreover, we can use dummy value to denote that the stack is empty to streamline the expression. ([lc-sol](https://leetcode.com/articles/valid-parentheses/))

Python3:

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack, pair = [], {')':'(', ']':'[', '}':'{'}
        for parenthese in s:
            # If it's a closing bracket
            if parenthese in pair:
                # Pop the topmost element if non-empty
                # Otherwise dummy value '#'
                top = stack.pop() if stack else '#'
                # The poped top element is NOT 
                # the desired opening backet, return False
                if top != pair[parenthese]:
                    return False
            # Push opening bracket in stack
            else:
                stack.append(parenthese)
        return not stack
```

**Result:** runtime 28ms (86%), memory 14MB (59%)

## - Remove Parentheses Pairs - Time Exceeded

We can remove the parentheses pairs in `s` continually, but this method is time consuming. 

```python
class Solution:
    def isValid(self, s: str) -> bool:
        while "()" in s or "[]" in s or "{}" in s:
            s.replace("()", "").replace("[]", "").replace("{}", "")
        return True is not s else False
```

~~**Result:** runtime ms (%), memory MB (%)~~

