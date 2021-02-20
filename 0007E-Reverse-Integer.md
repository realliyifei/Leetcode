# 0007E Reverse Integer

Given a 32-bit signed integer, reverse digits of an integer.

**Example 1:**

```
Input: 123
Output: 321
```

**Example 2:**

```
Input: -123
Output: -321
```

**Example 3:**

```
Input: 120
Output: 21
```

**Note:**
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231, 231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

## Pop and Push

On this problem, we shouldn’t convert it to string, which is space-wasting. Instead, we should use module to pop the last element of `x` continually and push into `rev` as the result here.  

Python3

```python
class Solution:
    def reverse(self, x: int) -> int:
        minint, maxint = -2**31, 2**31-1
        isNeg = False
        if x < 0:
            isNeg = True
            x = abs(x)
        rev = 0
        while x != 0:
            pop = x % 10
            x //= 10
            rev = rev * 10 + pop
        if isNeg:
            rev *= -1
        return 0 if (rev < minint or rev > maxint) else rev
```

**Result:** time 28ms (90%), memory 14MB (52%)

C language

```c
int reverse(int x){
    int minint = 0x80000000, maxint = 0x7fffffff;
    long rev = 0; // check overflow by type long
    for (; x; rev = rev * 10 + x % 10, x /= 10); // no need to check sign
    return rev < minint || rev > maxint ? 0 : rev;
}
```

**Result:** time 4ms (52%), memory 5MB (34%)


