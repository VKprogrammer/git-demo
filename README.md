# Solution Tutorial

## Key Insights

1. Imagine placing a vertical line randomly on the number line. Naturally, the total segment length on one side of the line will be greater than on the other.
2. To fix this imbalance, we gradually shift the line left or right, depending on which side currently has less total length.
3. Deciding *which direction to move* the line follows a clear rule, which strongly hints at a **binary search** strategy.
4. The final balancing position turns out to be the **weighted median** of all segment midpoints, where each segment’s weight is simply its length.
5. Binary search is valid here because the function `(left_total - right_total)` changes monotonically as the cutting position moves.
6. We only need to care about positions defined by segment endpoints, since the balance point will always lie at or between these critical locations.


## Algorithm

### Binary Search Approach

1. **Find the search range:**
   - Set `left` to the minimum start point among all segments
   - Set `right` to the maximum end point among all segments

2. **Define the balance function:**
   - For a given position `x`, compute:
     - `left_total`: total length of all segment parts to the left of `x`
     - `right_total`: total length of all segment parts to the right of `x`
   - For each segment `[a, b]`:
     - If `x <= a`: the entire segment lies on the right → add `(b - a)` to `right_total`
     - If `x >= b`: the entire segment lies on the left → add `(b - a)` to `left_total`
     - If `a < x < b`: the segment is split by `x`
       - Left contribution: `(x - a)`
       - Right contribution: `(b - x)`

3. **Binary search iteration:**
```
   while (right - left) > 1e-7:
       mid = (left + right) / 2
       calculate left_total and right_total at mid
       
       if left_total < right_total:
           # Need more length on left, move right
           left = mid
       else:
           # Need more length on right (or balanced), move left
           right = mid
```



4. **Return the result:**
   - The final `mid` value, rounded to **exactly 5 decimal places**, is the balance point.

### Step-by-step execution

1. Read all segments and determine the minimum and maximum coordinates
2. Initialize the binary search bounds
3. In each iteration, compute total lengths on both sides of the mid-point
4. Shift the search range toward the heavier side
5. Repeat until the interval size is less than `1e-7`
6. Format the answer to exactly 5 decimal places

## Complexity Analysis

- **Time Complexity:**  
  $O(n \log(R))$ — Each binary search iteration processes all `n` segments, and the number of iterations is about $\log(R/\epsilon)$.  
  With coordinates up to $10^9$ and precision $10^{-7}$, this results in roughly **50 iterations**, i.e., $O(n \times 50)$.

- **Space Complexity:**  
  $O(n)$ — Only the input segments are stored; no extra data structures are required.

## Edge Cases

1. **Single segment:**  
   The balance point is simply the midpoint.  
   Example: `[0, 10]` → balance at `5.0`

2. **Identical overlapping segments:**  
   All segments share the same range, so the balance point is their common midpoint.  
   Example: two `[0, 10]` segments → balance at `5.0`

3. **Non-overlapping segments:**  
   Segments may be far apart with gaps in between. The algorithm naturally handles these cases.

4. **Same start or end point:**  
   Example: `[0, 5], [0, 10], [0, 15]` — the balance point is still correctly determined based on total lengths.

5. **Very large coordinates:**  
   Use `double` / `float64` to maintain precision up to $10^{-5}$ even when values approach $10^9$.

6. **Very small segments:**  
   Even tiny segments matter. Make sure floating-point precision does not eliminate their contribution.

## Common Pitfalls

- **Low binary search precision:**  
  Stopping too early (e.g., at `1e-4`) can produce inaccurate results. Always use at least `1e-7`.

- **Incorrect split calculation:**  
  For a segment `[a, b]` cut at `x`, the split must be `(x - a)` on the left and `(b - x)` on the right.

- **Merging overlapping segments:**  
  Never merge segments. Each segment contributes independently, even if they fully overlap.

- **Integer overflow:**  
  Avoid integer arithmetic with large coordinates. Use floating-point values throughout.

- **Wrong output format:**  
  The answer must be printed with **exactly 5 decimal places**.

- **Incorrect binary search direction:**  
  If `left_total < right_total`, move the line **to the right**, not left.

## Alternative Approaches

### Approach 1: Weighted Median (Optimal)

- Treat each segment midpoint as a position
- Use the segment length as its weight
- Sort segments by midpoint
- Accumulate lengths until half of the total length is reached
- That position is the balance point
- **Time Complexity:** $O(n \log n)$
- **Space Complexity:** $O(n)$
- **Note:** More efficient than binary search and conceptually clean

### Approach 2: Ternary Search

- Divide the range into three parts instead of two
- Compare imbalance (absolute difference of totals) at two points
- Narrow the range toward smaller imbalance
- **Time Complexity:** $O(n \log(R))$
- **Space Complexity:** $O(n)$
- **Note:** Works because the imbalance function has a single minimum

### Approach 3: Event-Based Sweep Line

- Create events for all segment start and end points
- Sort events and sweep from left to right
- Maintain running totals of left and right lengths
- Interpolate if the balance lies between two events
- **Time Complexity:** $O(n \log n)$
- **Space Complexity:** $O(n)$
- **Note:** Very precise but more complex to implement

### Approach 4: Analytical Solution (Mathematical)

- Compute the balance point directly using a weighted centroid formula:
-  
  Balance point  
  $$
  c = \frac{\sum_{i=1}^{n} (\text{length}_i \times \text{midpoint}_i)}{\sum_{i=1}^{n} \text{length}_i}
  $$
- For segment `[a, b]`:
  - Length = `(b - a)`
  - Midpoint = `(a + b) / 2`
- **Time Complexity:** $O(n)$
- **Space Complexity:** $O(1)$
- **Note:** Fastest, simplest, and gives an exact result

**Recommended Approach:**  
The **Analytical Solution (Approach 4)** is the cleanest and most efficient choice, delivering the balance point in a single pass with minimal code and maximum clarity.
