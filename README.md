# MSB Unique Coin Selection

## Description

There are **N Coins** arranged in a line, labeled **1 to N** from left to right. Coin **i** has value equal to **i**.

```
1 — 2 — 3 — ... — (N-1) — N
```

The **Most Significant Bit (MSB)** of a positive integer is the position of its highest set bit: $\text{msb}(i) = \lfloor \log_2 i \rfloor$.

You want to take **exactly K coins**. A selection is **valid** if:

1. **No two selected coins are adjacent** in the line (labels differ by atleast $2$).
2. **No two selected coins share the same MSB** 

Find the **number of valid selections** modulo **$10^9+7$**.


## Input Format

Input is a JSON object with the following fields:

- `n` (integer): The number of coins
- `k` (integer): The exact number of coins to select

Example input:
```json
{
  "n": 7,
  "k": 2
}
```

## Output Format

Output is a JSON value (type: integer):

The number of valid selections modulo **$10^9+7$**. If no valid selection exists, return `0`.

Example output:
```json
12
```

## Constraints

- $1 \leq N \leq 100{,}000$
- $1 \leq K \leq \lfloor \log_2 N \rfloor + 1$
- Time limit: $2000 \text{ms}$
- Memory limit: $256 \text{MB}$

## Examples

### Example 1

**Input:**
```json
{
  "n": 7,
  "k": 2
}
```

**Output:**
```json
7
```

**Explanation:** 
For N=7, the bit groups are: Group 0 = {1}, Group 1= {2,3}, Group 2 = {4,5,6,7}.

We pick 2 non-adjacent coins from two **distinct** bit groups:

| Selection | MSBs | Non-adjacent? | Valid? |
|-----------|------|---------------|--------|
| (1, 3) | [0, 1] | Yes (gap=2) | ✓ |
| (1, 4) | [0, 2] | Yes (gap=3) | ✓ |
| (1, 5) | [0, 2] | Yes | ✓ |
| (1, 6) | [0, 2] | Yes | ✓ |
| (1, 7) | [0, 2] | Yes | ✓ |
| (2, 4) | [1, 2] | Yes (gap=2) | ✓ |
| (2, 5) | [1, 2] | Yes | ✓ |
| (2, 6) | [1, 2] | Yes | ✓ |
| (2, 7) | [1, 2] | Yes | ✓ |
| (3, 5) | [1, 2] | Yes (gap=2) | ✓ |
| (3, 6) | [1, 2] | Yes | ✓ |
| (3, 7) | [1, 2] | Yes | ✓ |

### Example 2

**Input:**
```json
{
  "field1": "test",
  "field2": [4, 5]
}
```

**Output:**
```json
9
```

From Group $2$, we pick any coin $c\in\{4,5,6,7\}$


