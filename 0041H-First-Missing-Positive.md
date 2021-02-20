# 0041H First Missing Positive

Given an unsorted integer array, find the smallest missing positive integer.

**Example 1:**

```
Input: [1,2,0]
Output: 3
```

**Example 2:**

```
Input: [3,4,-1,1]
Output: 2
```

**Example 3:**

```
Input: [7,8,9,11,12]
Output: 1
```

**Follow up:**

Your algorithm should run in *O*(*n*) time and uses constant extra space.

## Basic (Doesn’t meet requirement)

Notes that this code is a stepping stone and *doesn’t* meet the requirement. It has TC $O(n\log n)+O(n)$ due to the revoke of sorting method.

Python3:

```python
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        if not nums: return 1
        nums.sort()
        i = 0
        # Skip all non-positive integers
        while nums[i] <= 0: 
            i += 1
            if i == len(nums): 
                return 1
        # Find smallest missing positive integer
        # Return 1 if the first positive integer is larger than 1
        if nums[i] > 1:
                return 1
        # Return the increment of the positive integer 
        # whose gap with its next is larger than 1
        while i <= len(nums) - 2:
            if nums[i+1] - nums[i] > 1:
                return nums[i] + 1
            i += 1
        # Return the increment of the largest positive integer
        # if all of integers are continuous  
        return nums[i] + 1
```

**Result:** runtime 32ms (90%), memory 14MB (93%)

Another basic method is to enumerate all the integers started at 1 and return the first one that is not in `nums`.

## In-Place Hashmap

Using a external hashmap would fail to meet the space complexity requirement, therefore the key is to use the list `nums` *itself* as a hashmap. [lc-cn-sol](https://leetcode-cn.com/problems/first-missing-positive/solution/que-shi-de-di-yi-ge-zheng-shu-by-leetcode-solution/). 

![fig1](https://assets.leetcode-cn.com/solution-static/41/41_fig1.png)

Python3:

```python
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        n = len(nums)
        for i in range(n):
            if nums[i] <= 0: 
                nums[i] = n + 1
        
        for i in range(n):
            num = abs(nums[i])
            if num <= n and nums[num - 1] > 0:
                nums[num - 1] *= -1 
        
        for i in range(n):
            if nums[i] > 0:
                return i + 1
        
        return n + 1
```

**Result:** runtime ms (%), memory MB (%); Time $O(n)$ and Space $O(1)$.

## In-Place Exchange

Puts each positive integer back to their corrospoding position, i.e. trys to construct a sorted list, $[1, 2, 3, …]$. Then, the first integer not in the ordered position is the desired smallest missing positve integer. [Demonstration](https://leetcode-cn.com/problems/first-missing-positive/solution/tong-pai-xu-python-dai-ma-by-liweiwei1419/):

![0041-14.png](https://pic.leetcode-cn.com/1e4f3f1c9a6fb37c2aa515069508f5f3ef9d72cc55b586790f9bec9705052d17-0041-14.png)

[lc-cn-sol](https://leetcode-cn.com/problems/first-missing-positive/solution/que-shi-de-di-yi-ge-zheng-shu-by-leetcode-solution/). Python3:

```python
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        n = len(nums)
        for i in range(n):
            # When the number is an index but not in the right position
            while 1 <= nums[i] <= n and nums[nums[i] - 1] != nums[i]:
                # Swap two numbers
                nums[nums[i] - 1], nums[i] = nums[i], nums[nums[i] - 1]
        for i in range(n):
            if nums[i] != i + 1:
                return i + 1
        return n + 1
```

**Result:** runtime 36ms (75%), memory 14MB (14%); Time $O(n)$ and Space $O(1)$.

