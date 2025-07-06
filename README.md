# Leetcode-283-Move-Zeroes


# 🧠 LeetCode Solutions by Witts

This repo contains my clean and well-documented solutions to selected **LeetCode problems**, primarily using **Python**.  
Each problem is organized by its ID and includes an explanation, solution code, and complexity analysis.


## 🧰 Languages & Tools

- Python 3.0
- Markdown
- Git / GitHub

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
