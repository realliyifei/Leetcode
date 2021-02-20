# 0009E Palindrome Number

Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

**Example 1:**

```
Input: 121
Output: true
```

**Example 2:**

```
Input: -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```

**Example 3:**

```
Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```

**Follow up:**

Could you solve it without converting the integer to a string?

## Palindrome String

The first thing comes to mind is palindrome string conversion and comparison.

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        return str(x) == str(x)[::-1]
```

**Result:** runtime 64ms (70%), memory 14MB (79%), time $O(n)$ 

## Check One-by-One

The first method is no more than reversing and checking the whole string, but actually, we can reject the palindrome possibility once the corresponding digits from the beginning and the end don't match.

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        x = str(x)
        if len(x) == 0 or len(x) == 1:
            return True
        while len(x) > 1:
            if x[0] != x[-1]:
                return False
            x = x[1:-1]
        return True
```

**Result:** runtime 68ms (61%), memory 14MB (80%), time $O(n)$

## Reverse half of digits w/o String

How to solve it without string conversion to save space? We can reverse the latter half of `x` by module computation,  and then compare it to the first half to see whether they're identical. 

When `x` is even, we can cut half of it and compare directly. When `x` is odd, we can first put the middle digit to the latter half and then drop it during comparison, by `x==(rev//10)`.

However, now a new problem raises because it may fails to pass the cases where `x` is a multiple of 10.  To solve it, we can add a guard at first to exclude all multiples of 10. 

```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if x < 0 or (x % 10 == 0 and x): return False
        rev = 0
        while x > rev:
            last = x % 10
            x //= 10
            rev = rev * 10 + last
        return x == rev or x == (rev // 10)
```

**Result:** runtime 64ms (70%), memory 14MB (66%), time $O(n\log n)$, space $O(1)$ 

## Ref

* [回文数-官方题解](https://leetcode-cn.com/problems/palindrome-number/solution/hui-wen-shu-by-leetcode-solution/#%E6%96%B9%E6%B3%95%E4%B8%80%EF%BC%9A%E5%8F%8D%E8%BD%AC%E4%B8%80%E5%8D%8A%E6%95%B0%E5%AD%97)
* [LeetCodeAnimation/0009-Palindrome-Number.md at master · MisterBooo/LeetCodeAnimation](https://github.com/MisterBooo/LeetCodeAnimation/blob/master/0009-Palindrome-Number/Article/0009-Palindrome-Number.md)

