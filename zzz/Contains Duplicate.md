```python
class Solution:
    def containsDuplicate(self, nums: list[int]) -> bool:
        return len(set(nums)) < len(nums)
```


# Test cases:

```python
# Copy and paste this block to test locally
s = Solution()

# Case 1: Standard duplicate at the end
# Expected: True (1 is repeated)
print(f"Test 1: {s.containsDuplicate([1, 2, 3, 1])}") 

# Case 2: All unique
# Expected: False
print(f"Test 2: {s.containsDuplicate([1, 2, 3, 4])}")

# Case 3: Multiple duplicates / unsorted
# Expected: True (1, 3, 4, 2 are all repeated)
print(f"Test 3: {s.containsDuplicate([1, 1, 1, 3, 3, 4, 3, 2, 4, 2])}")
```

