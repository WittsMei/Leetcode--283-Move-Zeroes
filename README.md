# Leetcode--283-Move-Zeroes


class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        firstzero = 0
        for i in range(len(nums)):
            if nums[i] != 0:
                nums[i], nums[firstzero] = nums[firstzero], nums[i]
                firstzero += 1
