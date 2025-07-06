# Leetcode--283-Move-Zeroes


# 🧠 LeetCode Solutions by Witts

This repo contains my clean and well-documented solutions to selected **LeetCode problems**, primarily using **Python**.  
Each problem is organized by its ID and includes an explanation, solution code, and complexity analysis.

---

## 📌 Progress

| #   | Title         | Difficulty | Tags            | Solution Link                |
|-----|---------------|------------|------------------|------------------------------|
| 283 | Move Zeroes   | Easy       | Array, Two Pointers | [🔗 Solution](0283-move-zeroes/) |

---

## 🧰 Languages & Tools

- Python 3.x
- Markdown
- Git / GitHub


class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        firstzero = 0
        for i in range(len(nums)):
            if nums[i] != 0:
                nums[i], nums[firstzero] = nums[firstzero], nums[i]
                firstzero += 1
