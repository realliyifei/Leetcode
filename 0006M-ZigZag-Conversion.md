# 0006M ZigZag Conversion

The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: `"PAHNAPLSIIGYIR"`

Write the code that will take a string and make this conversion given a number of rows:

```
string convert(string s, int numRows);
```

**Example 1:**

```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```n

**Example 2:**

```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:

P     I    N
A   L S  I G
Y A   H R
P     I
```

## Modular Operation - Best

We can simplify the ZigZag expression, say **Example 2**, as below, which is grouped by ZigZag Period (ZP).

```
         ZP#1  ZP#2  ZP#3                  
line 1: |P    |I    |N
line 2: |A  L |S  I |G
line 3: |Y  A |H  R |
line 4: |P    |I    |
```

It's clearly a modular problem. We have $group(n)=[0],[1],[2],...[n-1]$ where $n=2\times\#Row-2$ (Why: because in each ZP, the coverage is two whole columns except two elements on the top-right and bottom-right corners). A similar approach, Visit by Row, is on [lc-sol](https://leetcode.com/problems/zigzag-conversion/solution/).

For instance, in **Example 2**, we have $n=2\times4-2=6$ and it can be demonstrated algebraically as below.

```
line 1: [0]   [0]   [0]          [0]     [0]     [0]
line 2: [1][5][1][5][1]   i.e.   [1][6-1][1][6-1][1]
line 3: [2][4][2][4]             [2][6-2][2][6-2]
line 4: [3]   [3]                [3]     [3]
```

Python3:

```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows == 1: return s
        lines = [''] * numRows
        n = 2 * numRows - 2
        for i, c in enumerate(s):
            mod = i % n
            if mod < numRows:
                lines[mod] += c
            else:
                lines[n - mod] += c
        return ''.join(lines)
```

**Result:** runtime 36ms (100%), memory 14MB (42%); Time $O(n)$ where $n=len(s)$, and Space $O(n)$.

It can be rewrote by building dictionary {mod - line} first.

```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows == 1: return s
        mod_to_line, lines = {}, [''] * numRows
        n = 2 * numRows - 2
        for mod in range(n):
            mod_to_line[mod] = mod if mod < numRows else n - mod
        for i, c in enumerate(s):
            lines[mod_to_line[i % n]] += c
        return ''.join(lines)
```

## Pinball-Like Printing

We can also print it directly following the ZigZag printing mechanism. Like the pinball game, the scanner moves between the topmost and the bottommost rows. A similar approach, Sort by Row, is on [lc-sol](https://leetcode.com/problems/zigzag-conversion/solution/).

```
The movement of added amount 

line 1: +1      +1 
line 2: +1   -1 +1    ...
line 3: +1 -1   +1 -1
line 4: -1      -1
```

Python3:

```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows == 1: return s
        # added amount is +1 (-1) when goes downward (upward)
        amount, row, lines = 1, 0, [''] * numRows
        for c in s:
            lines[row] += c
            row += amount
            # boundary: reverses direction 
            if row == numRows-1 or row == 0: amount *= -1
        return ''.join(lines)
```

**Result:** runtime 56ms (88%), memory 14MB (54%); Time $O(n)$ where $n=len(s)$, and Space $O(n)$.