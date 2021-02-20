# 0011M Container With Most Water

Given $n$ non-negative integers $a_1, a_2, ..., a_n$ , where each represents a point at coordinate $(i, a_i)$. $n$ vertical lines are drawn such that the two endpoints of line $i$ is at $ (i, a_i)$ and $(i, 0)$. Find two lines, which together with x-axis forms a container, such that the container contains the most water.

**Note:** You may not slant the container and *n* is at least 2.

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

**Example:**

```
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
```

## Brute Force

Python3:

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        ans, n = 0, len(height)
        for i in range(n):
            for j in range(i, n):
                ans = max(ans, min(height[i], height[j]) * (j-i))
        return ans
```

**Result:** time limit exceeded; Time $O(n^2)$ where $n$ is the length of `length`, and Space $O(1)$.

## Narrowing Window

Uses two pointers from the start and end of `height` and always moves the shorter one inward. (lc-cn-sol [#1](https://leetcode-cn.com/problems/container-with-most-water/solution/container-with-most-water-shuang-zhi-zhen-fa-yi-do) [#2](https://leetcode-cn.com/problems/container-with-most-water/solution/sheng-zui-duo-shui-de-rong-qi-by-leetcode-solution/)) 

The key is to understand that the inward movement above would *never* miss any possible largest area.

Python3:

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        ans, l, r = 0, 0, len(height)-1
        while l < r:
            ans = max(ans, min(height[l], height[r]) * (r-l))
            if height[l] < height[r]: l += 1
            else:                     r -= 1
        return ans
```

**Result:** runtime 132ms (80%), memory 15MB (27%); Time $O(n)$ where $n$ is the length of `length`, and Space $O(1)$.

Same but C++:

```python
class Solution {
public:
    int maxArea(vector<int>& height) {
        int res = 0, l = 0, r = height.size() - 1;
        while(l < r){
            res = height[l] < height[r] ? 
                max(res, (r - l) * height[l++]): 
                max(res, (r - l) * height[r--]); 
        }
        return res;
    }
};
```

**Result:** runtime 32ms (80%), memory 14MB (20%)

