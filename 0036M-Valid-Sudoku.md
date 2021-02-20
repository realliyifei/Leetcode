# 0036M Valid Sudoku

Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

1. Each row must contain the digits `1-9` without repetition.
2. Each column must contain the digits `1-9` without repetition.
3. Each of the 9 `3x3` sub-boxes of the grid must contain the digits `1-9` without repetition.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

A partially filled sudoku which is valid.

The Sudoku board could be partially filled, where empty cells are filled with the character `'.'`.

**Example 1:**

```
Input:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: true
```

**Example 2:**

```
Input:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being 
    modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```

**Note:**

- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated according to the mentioned rules.
- The given board contain only digits `1-9` and the character `'.'`.
- The given board size is always `9x9`.

## Brute Force

We denotes each 3x3 board as box, with ordered number, 0, 1, ..., 9, from top-left to bottom-right.

<img src="https://pic.leetcode-cn.com/2b141392e2a1811d0e8dfdf6279b1352e59fad0b3961908c6ff9412b6a7e7ccf-image.png" alt="img" style="zoom: 33%;" />

For number with row `i` and column `j`,  the index of box it belongs to is `(i/3)*3 + j/3`.

The first thought comes to mind is that, for each number in board, we check its row, column, and box, respectively, to see whether they're valid.

Pseudo-code:

```
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
    	for each num in board:
    		def isValidRow(num, board)
    		def isValidCol(num, board)
    		def isValidBox(num, board)
```

However, this method checks the board three times. Assume the size of board is `n`, then we consume time `3n`, so the time complexity is $O(n)$, and space complexity $O(1)$. ([source](https://leetcode.wang/leetCode-36-Valid-Sudoku.html))

## 3*9 Hashmaps

Instead, we can check the board once for each number. We first create the hashmap for each row, column, and box. When visiting each number in board, we store its index in the hashmap of its row, column, and box, and return *false* if there is already a same number. ([leetcode-cn-sol](https://leetcode-cn.com/problems/valid-sudoku/solution/you-xiao-de-shu-du-by-leetcode))

```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        # dictionaries
        row = [{} for i in range(9)]
        col = [{} for i in range(9)]
        box = [{} for i in range(9)]
        
        for i in range(9):
            for j in range(9):
                num = board[i][j]
                box_index = (i // 3) * 3 + j // 3
                if num != '.':
                    # check row
                    if num in row[i]:
                        return False
                    else:
                        row[i][num] = 1
                    # check column
                    if num in col[j]:
                        return False
                    else:
                        col[j][num] = 1
                    # check box
                    if num in box[box_index]:
                        return False
                    else:
                        box[box_index][num] = 1
        
        return True
```

**Result:** runtime 136ms (32%), memory 14MB (99%), time $O(1)$, space $O(1)$ 

## One Set

We can also store the result of checked number in one set `seen` instead of in 27 hashmaps. 

```python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        # set
        seen = set()
        
        for i in range(9):
            for j in range(9):
                num = board[i][j]
                box_index = (i // 3) * 3 + j // 3
                if num != '.':
                    row_str = num + " in row " + str(i)
                    col_str = num + " in col " + str(j)
                    box_str = num + " in box" + str(box_index)
                    if (row_str in seen or
                        col_str in seen or
                        box_str in seen):
                        return False
                    seen.add(row_str)
                    seen.add(col_str)
                    seen.add(box_str)
        
        return True
```

**Result:** runtime 120ms (42%), memory 14MB (14%), time $O(1)$, space $O(1)$ 

