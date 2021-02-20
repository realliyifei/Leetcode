# 0035E Search Insert Position

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

**Example 1:**

```
Input: [1,3,5,6], 5
Output: 2
```

**Example 2:**

```
Input: [1,3,5,6], 2
Output: 1
```

**Example 3:**

```
Input: [1,3,5,6], 7
Output: 4
```

**Example 4:**

```
Input: [1,3,5,6], 0
Output: 0
```

## Pointer

Two cases: 

1. If there's an identical number, return its index

2. Otherwise, return the index of insert position, which is also the index of the smallest larger number 

They can be put in one line expression. Python3:

```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        for i in range(len(nums)):
            if nums[i] >= target: return i
        # if list is empty or target is larger than all numbers
        return len(nums) 
```

**Result:** runtime 48ms (87%), memory 14MB (5%)

