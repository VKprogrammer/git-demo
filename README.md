# Bitwise Time Machine

## Description

You are given an array $A$ of $n$ integers $(1-indexed)$. You need to process $Q$ queries of the following types:

### Query Types:

**$1$. POINT_UPDATE**
- Update the value at index $i$ to $val$
- Format: $1$ $i$ $val$
- Effect: $A[i]$ = $val$

**$2$. RANGE_XOR**
- XOR all elements in range $[l, r]$ with value $k$
- Format: $2$ $l$ $r$ $k$
- Effect: For all $i$ where $l$ ≤ $i$ ≤ $r$ , $A[i]$ = $A[i]$ $XOR$ $k$

**$3$. POPCOUNT_SUM**
- Return the sum of all elements in range $[l, r]$ that have exactly $x$ set bits in their binary representation
- Format: $3$ $l$ $r$ $x$
- Output: Sum of qualifying elements

**$4$. WEIGHTED_POPCOUNT_SUM**
- Return the weighted sum where each element with exactly $x$ set bits is multiplied by its position within the range
- Format: $4$ $l$ $r$ $x$
- Output: $\sum $A[i]$ \cdot ($i$ - $l$ + $1$)$ for all $i$ in $[l, r]$ where $popcount(A[i])$ = $x$

**$5$. TIME_QUERY**
- Answer query type $3$ at version $t$ (the state of the array after the $t$-th update operation)
- Format: $5$ $l$ $r$ $x$ $t$
- Output: $POPCOUNTSUM$ for the array state at version $t$
- Note: This is a read-only operation; current state remains unchanged

**$6$. ROLLBACK**
- Rollback the array and all data structures to the state after the $t$-th update operation
- Format: $6$ $t$
- Effect: All future versions are discarded, and subsequent operations continue from version $t$
- Note: Only update operations (types $1$ and $2$) create new versions

## Input Format

Input is a JSON object with the following fields:

- $n$ (integer): The size of the array $(1 ≤ n ≤ 10,000)$
- $array$ (array of integers): The initial array of $n$ integers, each in range $[0, 10^9]$
- $queries$ (array of objects): Array of query objects, where each query has:
  - $type$ (integer): Query type identifier $(1-6)$
  - Additional fields based on query type:
    - **Type $1$ (POINT_UPDATE)**: $i$ (integer), $val$ (integer)
    - **Type $2$ (RANGE_XOR)**: $l$ (integer), $r$ (integer), $k$ (integer)
    - **Type $3$ (POPCOUNT_SUM)**: $l$ (integer), $r$ (integer), $x$ (integer)
    - **Type $4$ (WEIGHTED_POPCOUNT_SUM)**: $l$ (integer), $r$ (integer), $x$ (integer)
    - **Type $5$ (TIME_QUERY)**: $l$ (integer), $r$ (integer), $x$ (integer), $t$ (integer)
    - **Type $6$ (ROLLBACK)**: $t$ (integer)

Example input:
```json
{
  "n": 5,
  "array": [5, 7, 3, 9, 6],
  "queries": [
    {
      "type": 3,
      "l": 1,
      "r": 5,
      "x": 2
    },
    {
      "type": 1,
      "i": 2,
      "val": 15
    },
    {
      "type": 2,
      "l": 1,
      "r": 3,
      "k": 4
    },
    {
      "type": 4,
      "l": 1,
      "r": 5,
      "x": 2
    },
    {
      "type": 5,
      "l": 1,
      "r": 5,
      "x": 2,
      "t": 1
    },
    {
      "type": 6,
      "t": 1
    }
  ]
}
```

**Note:** Array indices are $1-based$. Version numbers start from $0$ (initial state) and increment with each update operation (types $1$ and $2$).

## Output Format

Output is a JSON array of integers:

An array containing the results of all query operations that produce output (types 3, 4, and 5). Query types 1, 2, and 6 do not produce output. Results are ordered by the sequence in which queries appear in the input.

Example output:
```json
[23, 66, 23]
```

**Note:** In the example above:
- First value (23) is the result of the first type 3 query
- Second value (63) is the result of the type 4 query
- Third value (26) is the result of the type 5 query

## Constraints

- $1 \le n, Q \le 10^5$
- $0 \le A[i] \le 10^9$
- $1 \le i \le n$
- $1 \le l \le r \le n$
- $1 \le type \le 6$
- $0 \le k \le 10^9$
- $0 \le x \le 30$ (maximum set bits in $10^9$)
- $0 \le t \le$ current version number
- Update operations (types $1$, $2$) create new versions numbered sequentially starting from $0$


## Examples

### Example 1

**Input:**
```json
{
  "n": 5,
  "array": [5, 7, 3, 9, 6],
  "queries": [
    {
      "type": 3,
      "l": 1,
      "r": 5,
      "x": 2
    },
    {
      "type": 1,
      "i": 2,
      "val": 15
    },
    {
      "type": 3,
      "l": 1,
      "r": 5,
      "x": 2
    },
    {
      "type": 2,
      "l": 1,
      "r": 3,
      "k": 4
    },
    {
      "type": 3,
      "l": 1,
      "r": 5,
      "x": 2
    },
    {
      "type": 4,
      "l": 1,
      "r": 5,
      "x": 2
    },
    {
      "type": 5,
      "l": 1,
      "r": 5,
      "x": 2,
      "t": 1
    },
    {
      "type": 6,
      "t": 1
    },
    {
      "type": 3,
      "l": 1,
      "r": 5,
      "x": 2
    },
    {
      "type": 1,
      "i": 1,
      "val": 8
    },
    {
      "type": 5,
      "l": 1,
      "r": 5,
      "x": 2,
      "t": 2
    },
    {
      "type": 4,
      "l": 2,
      "r": 4,
      "x": 2
    }
  ]
}

```

**Output:**
```json
[23,26,17,63,26,26,5,24]
```

**Explanation:** 
**Initial Array (Version 0):** $[5, 7, 3, 9, 6]$

Binary representations:
- $5 = 101 (2 bits)$
- $7 = 111 (3 bits)$
- $3 = 011 (2 bits)$
- $9 = 1001 (2 bits)$
- $6 = 110 (2 bits)$
- $11 = 1011 (3 bits)$
- $15 = 1111 (4 bits)$

$1.$ $3$ $1$ $5$ $2$: Elements with $2$ set bits: $5$, $3$, $9$, $6$ → $Sum = 23$
$2.$ $1$ $2$ $15$: $A[2] = 15$ → $Array = [5, 15, 3, 9, 6]$ (Version 1)
$3.$ $3$ $1$ $5$ $2$: Elements with $2$ set bits: $5$, $3$, $9$, $6$ → $Sum = 23$
$4.$ $2$ $1$ $3$ $4$: XOR $[1,3]$ with $4$ → $Array = [1, 11, 7, 9, 6]$ (Version 2)
$5.$ $3$ $1$ $5$ $2$: Elements with $2$ set bits: $9$, $6$ → $Sum = 15$
$6.$ $4$ $1$ $5$ $2$: Weighted $sum = 9×4 + 6×5 = 36 + 27 = 66$
$7.$ $5$ $1$ $5$ $2$ $1$: Query at version $1$: $[5, 15, 3, 9, 6]$, elements with $2$ bits: $5$, $3$, $9$, $6$ → $Sum = 23$
$8.$ $6$ $1$: Rollback to version $1$ → $Array = [5, 15, 3, 9, 6]$
$9.$ $3$ $1$ $5$ $2$: Elements with $2$ set bits: $5$, $3$, $9$, $6$ → $Sum = 23$
$10.$ $1$ $1$ $8$: $A[1] = 8$ → $Array = [8, 15, 3, 9, 6]$ (Version $2$, old version $2$ discarded)
$11.$ $5$ $1$ $5$ $2$ $2$: Query at version $2$: $[8, 15, 3, 9, 6]$, elements with $2$ bits: $3$, $9$, $6$ → $Sum = 18$
$12.$ $4$ $2$ $4$ $2$: Range $[2,4]$, elements with $2$ bits: $3$ at pos $3$, $9$ at pos $4$ → $3×2 + 9×3 = 6 + 27 = 33$

## Notes

- Popcount (population count) refers to the number of $1$s in the binary representation
- Versions are created only by update operations (types $1$ and $2$)
- TIME_QUERY reads from a past version without modifying current state
- ROLLBACK permanently reverts to a past version and discards all future versions
- After ROLLBACK to version $t$, the next update will create version $t+1$ (overwriting old history)

