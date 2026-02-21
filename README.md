# Solution Tutorial — MSB Unique Coin Selection (Combinatorial Approach)

## Key Insights

1. **MSB Groups Are Contiguous Intervals of Bounded Count:** The Most Significant Bit function partitions `{1, ..., N}` into at most **17 non-overlapping contiguous ranges** (for `N ≤ 100000`):
   ```
   Group 0: [1]
   Group 1: [2, 3]
   Group 2: [4, 5, 6, 7]
   Group b: [2^b, min(2^(b+1)-1, N)]
   ```
   Because we must pick **exactly one coin per group** (the MSB uniqueness constraint), the total number of chosen groups equals `K` and `K ≤ 17` always. This bounded structure is the key to an efficient solution.

2. **Adjacency Violations Only Occur at Group Boundaries:** Since we pick one coin per selected group, there are no intra-group adjacency conflicts. For coins from groups that differ by index 2 or more, the minimum gap is:
   ```
   min(group b+2) - max(group b) = 2^(b+2) - (2^(b+1) - 1) = 2^(b+1) + 1 ≥ 3
   ```
   So they are **always safe**. The **only dangerous pair** is:
   - Last coin of group `b` = `2^(b+1) - 1`
   - First coin of group `b+1` = `2^(b+1)`

   These differ by exactly 1 (adjacent!). Therefore we only need to worry about boundaries between **consecutively-indexed chosen groups**.

3. **Inclusion-Exclusion on Bad Boundaries Decouples the Groups:** For a fixed K-subset of groups, define a "bad boundary" at position `j` when `chosen[j+1] == chosen[j] + 1`. Using inclusion-exclusion over subsets of bad boundaries:
   - **Activating** a bad boundary at `j` forces: group `j` → pick last element, group `j+1` → pick first element (1 choice each).
   - All groups remain **independent** after fixing constraints, so the count for each term is just a **product** of per-group choices.
   - The sign of each term is `(-1)^(number of activated boundaries)`.

4. **Constraint Encoding as 2-Bit Flags:** Each group's freedom under activated boundaries is captured by a 2-bit value:
   | Value | Meaning | Choices |
   |-------|---------|---------|
   | `0` | Free — no boundary activated | `s` (group size) |
   | `1` | Forced FIRST — left boundary activated | `1` |
   | `2` | Forced LAST — right boundary activated | `1` |
   | `3` | Forced BOTH — squeezed by two boundaries | `1` if size=1, else `0` (contradiction) |

---

## Algorithm

### Step 1: Build MSB Groups
Iterate `b = 0, 1, 2, ...` while `2^b ≤ N`. For each `b`:
```
lo = 2^b
hi = min(2^(b+1) - 1, N)
groupSize[b] = hi - lo + 1
L++
```
If `K > L`, return `0` (impossible). If `K == 0`, return `1`.

### Step 2: Enumerate All C(L, K) Group Subsets via Gosper's Hack
Use the standard bitmask trick to iterate over all `K`-element subsets of `{0, 1, ..., L-1}` in O(1) per step:
```
subset = (1 << K) - 1      // smallest K-bit integer
while subset < (1 << L):
    process(subset)
    c = subset & (-subset)
    r = subset + c
    subset = (((r ^ subset) >> 2) / c) | r
```
Extract the sorted chosen group indices into `chosen[0..K-1]`.

### Step 3: Identify Bad Boundaries
For each consecutive pair `(j, j+1)` in `chosen[]`, check if `chosen[j+1] == chosen[j] + 1`. If yes, record position `j` in `badPos[]`:
```
numBad = 0
for j in 0..K-2:
    if chosen[j+1] == chosen[j] + 1:
        badPos[numBad++] = j
```

### Step 4: Inclusion-Exclusion Over Bad Boundary Subsets
For each `bmask` in `0 .. 2^numBad - 1`:

**4a.** Compute `sign = (-1)^popcount(bmask)`.

**4b.** Reset all `constraint[i] = 0`. For each activated boundary `bb`:
```
j = badPos[bb]
constraint[j]     |= 2    // force LAST
constraint[j + 1] |= 1    // force FIRST
```

**4c.** Compute the product of per-group choices:
```
product = 1
for i in 0..K-1:
    s = groupSize[chosen[i]]
    if constraint[i] == 3 and s >= 2: term is invalid, skip
    if constraint[i] == 0: product *= s
    // constraints 1 or 2: multiply by 1 (implicit)
```

**4d.** Accumulate: `answer += sign * product  (mod 10^9 + 7)`.

### Step 5: Return `answer mod 10^9 + 7`

---

## Worked Example

**N = 7, K = 3.** Groups: `{1}` (size 1), `{2,3}` (size 2), `{4..7}` (size 4). Only K-subset: `{0, 1, 2}`. Both boundaries are bad (`numBad = 2`).

| `bmask` | Activated Boundaries | Constraints [G0, G1, G2] | Product | Sign | Term |
|---------|---------------------|--------------------------|---------|------|------|
| `00` | none | [free, free, free] | 1 × 2 × 4 = 8 | +1 | **+8** |
| `01` | bdry(0,1) | [last, first, free] | 1 × 1 × 4 = 4 | -1 | **-4** |
| `10` | bdry(1,2) | [free, last, first] | 1 × 1 × 1 = 1 | -1 | **-1** |
| `11` | both | [last, **both**, first] | G1 size=2, c=3 → **invalid** | +1 | **0** |

**Answer = 8 − 4 − 1 + 0 = 3** ✓

---

## Complexity Analysis

- **Time Complexity:** $O\!\left(\binom{L}{K} \cdot 2^{K} \cdot K\right)$ where $L \leq 17$ and $K \leq 17$.

  This is completely **independent of N**. In the worst case ($L = 17$, $K = 8$): $\binom{17}{8} \cdot 2^8 \cdot 8 = 24310 \cdot 256 \cdot 8 \approx 50M$ operations — well within the 2-second time limit. In practice, most inputs have far fewer bad boundaries per subset, making the inner loop much faster.

- **Space Complexity:** $O(L) = O(\log N)$ — only fixed-size stack arrays of length ≤ 17 are used (`groupSize`, `chosen`, `badPos`, `constraint`). No dynamic allocation.

---

## Edge Cases

1. **K = 0:** No coins to select. Return `1` immediately (the empty selection is vacuously valid). Handled by the early return before the main loop.

2. **K = 1:** No boundaries exist at all (`numBad = 0`), so only `bmask = 0` runs for each group subset. The answer equals the sum of all group sizes = `N`. Correct — any single coin is valid.

3. **K > L (more coins needed than groups exist):** Impossible — return `0` immediately. This guards against invalid input even though the problem guarantees `K ≤ ⌊log₂N⌋ + 1 = L`.

4. **K = L (all groups selected):** All consecutive chosen pairs are bad boundaries. `numBad = L - 1` up to 16, giving `2^16 = 65536` inclusion-exclusion terms. Still well within time limits.

5. **Group with size 1 squeezed by two activated boundaries (constraint = 3, size = 1):** The first and last element are the same coin, so the constraint is satisfiable with exactly 1 choice. The code correctly allows this — only `size ≥ 2` with `constraint = 3` is rejected.

6. **Partial last group (N is not of the form 2^b − 1):** The formula `hi = min(2^(b+1) − 1, N)` correctly computes the reduced group size. All arithmetic still applies — the last group simply has fewer available choices.

---

## Common Pitfalls

- **Negative modular result from inclusion-exclusion signs:** `sign * product` is negative when `sign = -1`. Without the fix `((answer + sign * product) % MOD + MOD) % MOD`, the final answer can be negative. Always apply the double-mod guard after each accumulation.

- **Confusing "adjacent in chosen[] array" with "bad boundary":** Not every consecutive pair in `chosen[]` is a bad boundary. Only pairs where `chosen[j+1] == chosen[j] + 1` (consecutive *group indices*) are dangerous. Groups `{0, 2}` chosen together have no bad boundary even though they are neighbors in `chosen[]`.

- **Treating constraint = 3 as always invalid:** A group of size 1 satisfies constraint = 3 (its single coin is simultaneously first and last). Only `size ≥ 2` with constraint = 3 produces a contradiction.

- **Off-by-one in Gosper's hack termination:** The loop condition must be `subset < (1 << L)` (strict). Using `<=` processes a non-existent `(L+1)`-th bit position, causing undefined behavior or reading garbage from `groupSize`.

- **Forgetting that `badPos` stores position indices, not group indices:** `badPos[bb] = j` stores the *position in `chosen[]`*, not the group number. When applying constraints, use `constraint[j]` and `constraint[j+1]`, not `constraint[chosen[j]]`.

- **Integer overflow in Gosper's hack for large L:** `(1 << L)` where `L = 17` gives `131072`, safely within `int` range. However, if the problem ever extended to `N > 2^30`, switching to `long long` for the bitmask would be necessary.
