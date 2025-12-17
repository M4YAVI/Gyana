

```python
class Solution:
    def maxProfit(self, prices: list[int]) -> int:
        buy, profit = float('inf'), 0
        for p in prices:
            if p < buy: buy = p
            elif p - buy > profit: profit = p - buy
        return profit
```

P - buy > profit: profit = p - buy meaning :
**if the new price minus what you bought it for is greater than your current profit, update the profit to that difference**


---
#  Big‑O & Bottlenecks

- **Time Complexity:** $O(N)$ — We traverse the list exactly once (Single Pass).
    
- **Space Complexity:** $O(1)$ — We only store two variables (`buy`, `profit`) regardless of input size.
    
- **Bottlenecks:** Input I/O speed. The algorithm itself is optimal; you cannot solve this without looking at every price at least once.
    

---

###  Golf‑vs‑Speed Notes

- **Skipped `min()`/`max()`:** Inside the loop, `if p < buy` is significantly faster than `buy = min(buy, p)` in Python because function calls have overhead.
    
- **Inline Assignment:** Used `buy, profit = ...` for brevity.
    
- **No Indexing:** Iterating `for p in prices` is faster and cleaner than `for i in range(len(prices))`.
    
- **Float Infinity:** Used `float('inf')` to guarantee the first element becomes the minimum without needing special handling for an empty list (though constraints usually say `1 <= prices.length`).
    



