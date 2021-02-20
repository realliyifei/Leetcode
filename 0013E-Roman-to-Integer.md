# 0013E Roman to Integer

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, two is written as `II` in Roman numeral, just two one's added together. Twelve is written as, `XII`, which is simply `X` + `II`. The number twenty seven is written as `XXVII`, which is `XX` + `V` + `II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

* `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
* `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
* `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

**Example 1:**

```
Input: "III"
Output: 3
```

**Example 2:**

```
Input: "IV"
Output: 4
```

**Example 3:**

```
Input: "IX"
Output: 9
```

**Example 4:**

```
Input: "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```

**Example 5:**

```
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

## Hashmap 

We bulid a dictionary {roman-integer}, and then check from left to right. 

Based on the roman numerals mechanism, if the current integer is smaller/larger than the later one, we should substract/add the current integer.

Python3:

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        dic = {'I':   1, 'V':   5, 'X':  10, 'L':  50,
               'C': 100, 'D': 500, 'M':1000           }
        integer = 0
        for i in range(len(s)-1):
            curr, later = dic[s[i]], dic[s[i+1]]
            if curr >= later:
                integer += curr
            else:
                integer -= curr
        integer += dic[s[-1]]
        return integer
```

**Result:** runtime 36ms (98%), memory 14MB (99%)

We can also replace the special expressions with general ones first, and then convert it by dictionary.

Python3:

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        dic = {'I':   1, 'V':   5, 'X':  10, 'L':  50,
               'C': 100, 'D': 500, 'M':1000           }
        s = s.replace("IV", "IIII").replace("IX", "VIIII")
        s = s.replace("XL", "XXXX").replace("XC", "LXXXX")
        s = s.replace("CD", "CCCC").replace("CM", "DCCCC")
        return sum(dic[c] for c in s)
```

**Result:** runtime 40ms (95%), memory 14MB (54%)

