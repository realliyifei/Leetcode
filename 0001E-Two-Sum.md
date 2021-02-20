# 0001E - Two Sum

## Description

Given an array of integers, return *indices* of the two numbers such that they add up to a specific target.

You may assume that each input would have *exactly* one solution, and you may not use the same element twice.

Example:

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

Initial code:

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
```

## Brute Force Loops - Time Exceeded

Better than nothing tho.

```python
for i in range(len(nums)):
    lefts = nums[i+1:]
    for j in range(len(lefts)):
        if nums[i] + lefts[j] == target:
            return [i, j+i+1]
```

## Enhanced Loops - Accepted

Idea: Get the desired value of the second item by subtracting the target value by the first item, so we don’t need to do the computation before comparison in each step of loop, and thus save time (hint#2).

```python
for i in range(len(nums)):
    complement = target - nums[i] # get the difference first
    lefts = nums[i+1:]
    for j in range(len(lefts)):
        if lefts[j] == complement:
            return [i, j+i+1]
```

**Result:** runtime 5484ms, memory 15MB

We can also simplifed it to avoid nested for-loop.  

```python
count = 0
for num in nums:
    count += 1
    if (target - num) in nums[count:]:
        return [count-1, nums[count:].index(target-num)+count]
```

**Result:** runtime 968ms, memory 14.9MB

Notes that the last line cannot be modified as `return [count-1, nums.index(target-item)]`, otherwise it would fail to pass the test where two numbers are the same. For instance, when `nums = [3,3]`, it would return `[0,0]` instead of `[0,1]`.

## Hashmap - Best

We can use hashmap to speed up the search by trading space for time (hint#3). It reduces the time cost from *O(n)* to *O(1)*. 

Here, we create an empty hashmap first, then we keep storing the number's index and complement {target - nums[i]} in hashmap in the form of {index - complement}. In this process, we check whether the current number exists in the hashmap. If yes, return the solution immediately because there’s *exactly* one solution.

```python
hashmap = {}
for index, num in enumerate(nums):
    if num in hashmap:
        return [hashmap[num], index]
    hashmap[target-num] = index
```

**Result:** runtime 64ms, memory 15MB

## Ref

- [LeetCode (1) Two Sum (python). 紀錄一下刷題, 第一題Two Sum | by 邱德旺 | Medium](https://medium.com/@havbgbg68/leetcode-1-two-sum-python-8d77c223abd3)

- [LeetCodeAnimation/0001-Two-Sum.md at master · MisterBooo/LeetCodeAnimation](https://github.com/MisterBooo/LeetCodeAnimation/blob/master/0001-Two-Sum/Article/0001-Two-Sum.md)

- [LeetCode/000. Two Sum at master · pezy/LeetCode](https://github.com/pezy/LeetCode/tree/master/000.%20Two%20Sum)

    