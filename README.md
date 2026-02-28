# Peak Prefix

## Description

This is an **interactive problem**.

A hidden sequence of $N$ values exists, where each value is either $-1$ or $+1$. You cannot observe the sequence directly. Instead, you may query any contiguous range $[l, r]$ (0-indexed) and receive the **sign of the sum** of elements in that range.

Your goal is to find the index $i$ (0-indexed) such that the **prefix sum** $\sum_{j=0}^{i} a[j]$ is maximized. If multiple indices achieve the same maximum prefix sum, return the **smallest** such index.

## Interaction Protocol

**IMPORTANT:** Your solution must:
1. Read from standard input (stdin)
2. Write to standard output (stdout) — always flush after each query
3. Use JSON Lines format for all communication (one JSON object per line)

### Step 1: Read initial input from stdin

Read the first line from stdin and parse it as JSON. It contains:
```json
{"n": 8, "max_queries": 8}
```

### Step 2: Query the oracle

Write a query as a single JSON line to stdout (flush immediately), then read the response from stdin:

**Query format:**
```json
{"type": "query_range", "l": <left>, "r": <right>}
```

- `l` and `r` are 0-indexed, inclusive: $0 \leq l \leq r < n$
- The oracle returns the sign of $\sum_{j=l}^{r} a[j]$:
  - `+1` if the sum is positive
  - `-1` if the sum is negative
  - `0` if the sum is zero

**Response format:**
```json
{"sign": 1}
```

### Step 3: Submit your answer

When you have determined the answer, write the following JSON line to stdout:
```json
{"type": "answer", "value": i}
```

## Input Format

The initial input is a JSON object with the following fields:

- `n` (integer): The length of the hidden sequence
- `max_queries` (integer): The maximum number of queries allowed

Example input:
```json
{"n": 8, "max_queries": 8}
```

## Output Format

Output is a JSON value (type: integer):

The 0-indexed position of the maximum prefix sum (smallest index if tied).

Example output:
```json
1
```

## Constraints

- $1 \leq n \leq 10^4$
- Each element of the hidden sequence is either $-1$ or $+1$
- You may make at most $\lceil 2\log_2 n \rceil + 2$ queries
- Time limit: $2000\text{ms}$
- Memory limit: $256\text{MB}$

## Examples

### Example 1

**Input:**
```json
{"n": 8, "max_queries": 8}
```

**Output:**
```json
1
```

**Explanation:**
The hidden sequence is $[+1, +1, -1, +1, -1, -1, +1, -1]$.

Computing prefix sums at each index:

| Index | Element | Prefix Sum |
|-------|---------|------------|
| 0 | +1 | 1 |
| 1 | +1 | 2 |
| 2 | -1 | 1 |
| 3 | +1 | 2 |
| 4 | -1 | 1 |
| 5 | -1 | 0 |
| 6 | +1 | 1 |
| 7 | -1 | 0 |

The maximum prefix sum is $2$, achieved at both index $1$ and index $3$. Since ties are broken by returning the smallest index, the answer is $1$.

### Example 2

**Input:**
```json
{"n": 4, "max_queries": 6}
```

**Output:**
```json
0
```

**Explanation:**
The hidden sequence is $[-1, -1, -1, -1]$.

Computing prefix sums at each index:

| Index | Element | Prefix Sum |
|-------|---------|------------|
| 0 | -1 | -1 |
| 1 | -1 | -2 |
| 2 | -1 | -3 |
| 3 | -1 | -4 |

All prefix sums are negative and strictly decreasing. The maximum (least negative) value is $-1$ at index $0$, so the answer is $0$.

### Example 3

**Input:**
```json
{"n": 6, "max_queries": 8}
```

**Output:**
```json
0
```

**Explanation:**
The hidden sequence is $[+1, -1, +1, -1, +1, -1]$.

Computing prefix sums at each index:

| Index | Element | Prefix Sum |
|-------|---------|------------|
| 0 | +1 | 1 |
| 1 | -1 | 0 |
| 2 | +1 | 1 |
| 3 | -1 | 0 |
| 4 | +1 | 1 |
| 5 | -1 | 0 |

The maximum prefix sum is $1$, achieved at indices $0$, $2$, and $4$. The smallest of these is $0$, so the answer is $0$.

## Notes

- **Always flush stdout** after writing each query — omitting this causes a deadlock
- The query budget is tight: querying each element individually costs $n$ queries and will exceed the limit for large $n$. You must find a smarter strategy.
