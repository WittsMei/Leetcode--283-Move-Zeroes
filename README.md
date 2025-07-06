# LeetCode 283 - Move Zeroes

**Difficulty:** Easy  
**Tags:** Array, Two Pointers

## Problem Description

Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Note that you must do this in-place without making a copy of the array.

## Example 1

Input: `[0, 1, 0, 3, 12]`  
Output: `[1, 3, 12, 0, 0]`

## Example 2

Input: nums = [0]
Output: [0]
 

## Languages

- Python 3.0

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Moves all 0s to the end of the array in-place while preserving the order of non-zero elements.
        Time Complexity: O(n)
        Space Complexity: O(1)
        """
        firstzero = 0
        for i in range(len(nums)):
            if nums[i] != 0:
                nums[i], nums[firstzero] = nums[firstzero], nums[i]
                firstzero += 1
```

## Explanation

Use a two-pointer approach: one pointer `firstzero` tracks the next zero position to fill, while the loop pointer traverses the list.

