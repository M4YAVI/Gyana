```python
def maxSubArray(nums: list[int]) -> int:
    cur = res = nums[0]
    for x in nums[1:]:
        cur = max(x, cur + x)
        res = max(res, cur)
    return res

# Test Cases
print(maxSubArray([-2, 1, -3, 4, -1, 2, 1, -5, 4])) # Output: 6  (from [4, -1, 2, 1])
print(maxSubArray([1]))                            # Output: 1
print(maxSubArray([5, 4, -1, 7, 8]))               # Output: 23
```

#### Step-by-Step Visualization

**Input:** `[-2, 1, -3, 4, -1, 2, 1, -5, 4]`

We track `cur` (Best sum ending _right here_) and `res` (Best sum seen _anywhere_).

|**Step**|**Number (x)**|**Logic: max(x, cur + x)**|**New cur**|**New res**|**Interpretation**|
|---|---|---|---|---|---|
|**Start**|**-2**|Init|**-2**|**-2**|Start with first element.|
|**1**|**1**|`max(1, -2+1)` -> `max(1, -1)`|**1**|**1**|Previous sum was negative (-2). Discard it. Start fresh at 1.|
|**2**|**-3**|`max(-3, 1-3)` -> `max(-3, -2)`|**-2**|**1**|Extending is better than starting fresh at -3, even though sum drops.|
|**3**|**4**|`max(4, -2+4)` -> `max(4, 2)`|**4**|**4**|Previous sum (-2) drags us down. **Start fresh** at 4.|
|**4**|**-1**|`max(-1, 4-1)` -> `max(-1, 3)`|**3**|**4**|Add -1. Sum is 3. Still profitable to keep going.|
|**5**|**2**|`max(2, 3+2)` -> `max(2, 5)`|**5**|**5**|Add 2. Sum is 5. New Global Max found!|
|**6**|**1**|`max(1, 5+1)` -> `max(1, 6)`|**6**|**6**|Add 1. Sum is 6. **New Global Max!**|
|**7**|**-5**|`max(-5, 6-5)` -> `max(-5, 1)`|**1**|**6**|Big drop, but we hold onto the history (1 is > -5).|
|**8**|**4**|`max(4, 1+4)` -> `max(4, 5)`|**5**|**6**|Recovery, but didn't beat the record of 6.|

#### Why it works (Dynamic Programming)

Kadane's Algorithm is essentially optimized Dynamic Programming.

- **Subproblem:** `dp[i]` = Maximum subarray sum ending at index `i`.
    
- **Recurrence:** `dp[i] = max(nums[i], nums[i] + dp[i-1])`.
    
- **Space Optimization:** Since `dp[i]` only depends on `dp[i-1]`, we don't need an array. We just need one variable `cur`.
    

**Next Step:** Would you like to see how to modify this to return the **start and end indices** of that subarray instead of just the sum?