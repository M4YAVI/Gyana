
```python
def two_sum(nums: list[int], target: int) -> list[int]:
    seen = {}  # Map value -> index
    for index, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], index]
        seen[num] = index
    return [] # Should not reach here per constraints
```

- **Time Complexity**: O(N). We traverse the list containing N elements exactly once. Dictionary lookups and insertions are O(1) on average.
- **Space Complexity**: O(N). In the worst case (no pair found until the very end), we store N−1 elements in the dictionary.
- **Bottleneck**: Hash collisions could theoretically degrade lookup to O(N), making total time O(N2), but this is extremely rare in Python's optimized implementation.

Nums = [2, 7, 11]
Target = 9
Seen = {}

index=0 → val=2 → seen={2:0}
Index=1 → val=7 → complement=2 → seen[2]=0 → return [0,1]
