# Solution Tutorial

## Key Insights

1. Randomly choose a line; one side will have a higher area than the other. We need to adjust the line.
2. Sometimes we will shift the line to the left and other times to the right. Eventually, we will find a balance point.
3. Where to shift the line indicates a binary search based on whether to update the left or right side of the binary.
4. The balance point is essentially the **weighted median** of all segment midpoints, where each segment's weight is its length.
5. Binary search works because the function (left_total - right_total) consistently behaves in a predictable way based on the cutting position.
6. We only need to check positions where segment endpoints exist, as the balance point will be at or between these important points.

## Algorithm

### Binary Search Approach

1. **Find the search range:**
   - Set `left` to the minimum start point of all segments.
   - Set `right` to the maximum end point of all segments.

2. **Define the balance function:**
   - For a given position `x`, calculate:
     - `left_total` as the sum of all segment lengths to the left of `x`.
     - `right_total` as the sum of all segment lengths to the right of `x`.
   - For each segment `[a, b]`:
     - If `x <= a`, the entire segment is to the right, contributing `(b - a)` to right_total.
     - If `x >= b`, the entire segment is to the left, contributing `(b - a)` to left_total.
     - If `a < x < b`, the segment is split, contributing `(x - a)` to left_total and `(b - x)` to right_total.

3. **Binary search iteration:**
```
   while (right - left) > 1e-7:
       mid = (left + right) / 2
       calculate left_total and right_total at mid
       
       if left_total < right_total:
           # Need more length on the left, move right
           left = mid
       else:
           # Need more length on the right (or balanced), move left
           right = mid
```

4. **Return the result:**
   - The final `mid` value rounded to 5 decimal places is the balance point.

### Step-by-step execution:

1. Parse all segments and find min/max coordinates.
2. Initialize binary search bounds.
3. In each iteration, compute total lengths on both sides.
4. Adjust search bounds based on which side has more length.
5. Continue until convergence (difference < 1e-7).
6. Format the answer to exactly 5 decimal places.

## Complexity Analysis

- **Time Complexity:** $O(n \log(R))$ — where $n$ is the number of segments and $R$ is the range of coordinates. Binary search runs in $O(\log(R/\epsilon))$ iterations where $\epsilon = 10^{-7}$, and each iteration processes all $n$ segments. With coordinates up to $10^9$ and precision $10^{-7}$, this results in approximately $O(n \times 50)$ iterations.

- **Space Complexity:** $O(n)$ — We only need to store the input segments. No extra data structures are needed beyond the input.

## Edge Cases

1. **Single segment:** The balance point is at the midpoint of the segment. Example: `[0, 10]` → balance at `5.0`.

2. **Identical overlapping segments:** All segments have the same coordinates. The balance point is at their common midpoint. Example: Two `[0, 10]` segments → balance at `5.0`.

3. **Non-overlapping segments:** Segments spread across the number line. The algorithm naturally handles gaps between segments.

4. **All segments starting/ending at the same point:** Example: `[0, 5], [0, 10], [0, 15]` — the balance point will still be calculated correctly based on total lengths.

5. **Very large coordinate values:** Ensure precision is maintained. Use `double` or `float64` to handle coordinates up to $10^9$ while keeping $10^{-5}$ precision.

6. **Segments with small lengths:** Even tiny segments contribute to the balance; ensure they aren't lost to floating-point errors.

## Common Pitfalls

- **Insufficient precision in binary search:** Using a loose convergence criterion (e.g., `1e-4`) may not yield accurate results. Use at least `1e-7` to ensure the final answer is accurate to 5 decimal places.

- **Incorrect segment splitting logic:** When a vertical line at position `x` cuts through segment `[a, b]`, the left part has length `(x - a)` and the right part has length `(b - x)`. Reversing these or using incorrect formulas will give wrong results.

- **Not handling overlapping segments independently:** Each segment must be counted separately, even if they overlap completely. Do not try to merge or deduplicate segments.

- **Integer overflow with large coordinates:** When coordinates approach $10^9$, use floating-point arithmetic throughout to avoid overflow issues.

- **Forgetting to format output correctly:** The output must be exactly 5 decimal places. Use appropriate formatting (e.g., `%.5f` in C, `.toFixed(5)` in JavaScript).

- **Wrong binary search direction:** If `left_total < right_total`, you need to move the line RIGHT (increase `left` bound) to add more length to the left side, not move it left.

## Alternative Approaches

### Approach 1: Weighted Median (Optimal)
- The balance point is the weighted median where weights are segment lengths and positions are segment midpoints.
- Sort all segments by their midpoints.
- Accumulate lengths until half the total length is reached.
- The position where this occurs is the balance point.
- **Time Complexity:** $O(n \log n)$ — Dominated by sorting segments.
- **Space Complexity:** $O(n)$ — For storing sorted segments.
- **Note:** This is more efficient than binary search for the final implementation.

### Approach 2: Ternary Search
- Similar to binary search but divides the range into three parts.
- Evaluates the "imbalance" (absolute difference between left and right totals) at two points.
- Narrows the search range based on which third has less imbalance.
- **Time Complexity:** $O(n \log(R))$ — Similar to binary search, slightly different constant factors.
- **Space Complexity:** $O(n)$ — Same as binary search.
- **Note:** Works because the imbalance function has a single minimum at the balance point.

### Approach 3: Event-Based Sweep Line
- Create events for all segment start and end points.
- Sort events by position.
- Sweep from left to right, maintaining running totals.
- At each event point, check if the totals are balanced.
- Use interpolation between events if the balance point is between two events.
- **Time Complexity:** $O(n \log n)$ — Sorting events.
- **Space Complexity:** $O(n)$ — Storing events.
- **Note:** More complex to implement but can provide high precision by checking exact critical points.

### Approach 4: Analytical Solution (Mathematical)
- The balance point can be computed directly with the formula for weighted centroid.
- Balance point $c = \frac{\sum_{i=1}^{n} \text{length}_i \times \text{midpoint}_i}{\sum_{i=1}^{n} \text{length}_i}$.
- For segment $[a, b]$: length = $(b-a)$, midpoint = $(a+b)/2$.
- **Time Complexity:** $O(n)$ — Single pass through all segments.
- **Space Complexity:** $O(1)$ — Only need accumulators.
- **Note:** This is the most efficient approach and gives exact results directly.

**Recommended Approach:** The **Analytical Solution (Approach 4)** is the most elegant and efficient, computing the answer in $O(n)$ time with a simple formula.
