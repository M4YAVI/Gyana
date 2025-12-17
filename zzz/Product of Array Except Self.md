```python
def productExceptSelf(nums: list[int]) -> list[int]:
    n = len(nums)
    res = [1] * n
    prefix = 1
    for i in range(n):
        res[i] = prefix
        prefix *= nums[i]
    suffix = 1
    for i in range(n - 1, -1, -1):
        res[i] *= suffix
        suffix *= nums[i]
    return res

# Test Cases
print(productExceptSelf([1, 2, 3, 4]))      # Output: [24, 12, 8, 6]
print(productExceptSelf([-1, 1, 0, -3, 3])) # Output: [0, 0, 9, 0, 0]
print(productExceptSelf([5, 5]))            # Output: [5, 5]
```

### 2) Big-O & Bottlenecks

- **Time Complexity**: **$O(n)$**. We iterate through the list exactly twice (one forward pass, one backward pass).
    
- **Space Complexity**: **$O(1)$** (Auxiliary). We only use the output array `res` (which standard analysis ignores) and two scalar variables (`prefix`, `suffix`).
    

### 3) Golf-vs-Speed Notes

- **Why not Division?**: A solution calculating `total_product // nums[i]` is shorter but fails immediately if any element is `0` (ZeroDivisionError). If we add logic to handle zeros, the code becomes bloated and less readable than the prefix/suffix approach.
    
- **Micro-optimizations**:
        
    - Inlining the multiplication (`res[i] *= suffix`) avoids creating temporary lists.


#### Step-by-Step Visualization

**Input:** `nums = [1, 2, 3, 4]`

**Step 1: The Prefix Pass (Left -> Right)** We create a result array initialized to `1`. We carry a running `prefix` product.

- `i=0`: No elements to the left. `res[0]` stays `1`. Update `prefix` to `1 * 1 = 1`.
    
- `i=1`: Left element is `1`. `res[1]` becomes `1`. Update `prefix` to `1 * 2 = 2`.
    
- `i=2`: Left elements are `1, 2`. `res[2]` becomes `2`. Update `prefix` to `2 * 3 = 6`.
    
- `i=3`: Left elements are `1, 2, 3`. `res[3]` becomes `6`.
    

**Current State of `res`:** `[1, 1, 2, 6]` (This represents the product of everything to the left of each index).

**Step 2: The Suffix Pass (Right -> Left)** Now we iterate backwards, carrying a running `suffix` product and **multiplying it** into our existing `res` array.

- `i=3`: No elements to the right. `res[3]` stays `6 * 1 = 6`. Update `suffix` to `1 * 4 = 4`.
    
- `i=2`: Right element is `4`. `res[2]` becomes `2 * 4 = 8`. Update `suffix` to `4 * 3 = 12`.
    
- `i=1`: Right elements are `3, 4`. `res[1]` becomes `1 * 12 = 12`. Update `suffix` to `12 * 2 = 24`.
    
- `i=0`: Right elements are `2, 3, 4`. `res[0]` becomes `1 * 24 = 24`.
    

**Final Result:** `[24, 12, 8, 6]`

#### Why this works for Zeros

If the input is `[-1, 1, 0, -3, 3]`:

- At the index of `0`, the Left product will be non-zero, and the Right product will be non-zero. The result will be the product of the non-zero numbers.
    
- At any _other_ index, the `0` will appear in either the Left or Right part, forcing the product to `0`.
    
- This logic handles single or multiple zeros automatically without any special `if/else` statements.
    

**Next Step:** Would you like me to show you the "Follow Up" version of this problem involving parallel processing or how to optimize this for memory mapped files?