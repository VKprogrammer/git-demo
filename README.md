# Bitwise Time Machine

## Description

You are given an array $A$ of $n$ integers ($1$-indexed). You need to process $Q$ queries of the following types:

### Query Types:

**$1$. POINT_UPDATE $i$ $val$**
- Update the value at index $i$ to $val$
- Format: $1$ $i$ $val$
- Effect: $A[i]$ = $val$

**$2$. RANGE_XOR $l$ $r$ $k$**
- XOR all elements in range $[l, r]$ with value $k$
- Format: $2$ $l$ $r$ $k$
- Effect: For all $i$ where $l$ ≤ $$i$ ≤ $r$ , $A[i]$ = $A[i]$ XOR $k$

**$3$. POPCOUNT_SUM $l$ $r$ $x$**
- Return the sum of all elements in range $[l, r]$ that have exactly $x$ set bits in their binary representation
- Format: $3$ $l$ $r$ $x$
- Output: Sum of qualifying elements

**$4$. WEIGHTED_POPCOUNT_SUM $l$ $r$ $x$**
- Return the weighted sum where each element with exactly $x$ set bits is multiplied by its position within the range
- Format: $4$ $l$ $r$ $x$
- Output: $\sum $A[i]$ \cdot ($i$ - $l$ + $1$)$ for all $i$ in $[l, r]$ where $popcount(A[i])$ = $x$

**$5$. TIME_QUERY $l$ $r$ $x$ $t$**
- Answer query type $3$ $(POPCOUNT_SUM)$ at version $t$ (the state of the array after the $t$-th update operation)
- Format: $5$ $l$ $r$ $x$ $t$
- Output: $POPCOUNT_SUM$ for the array state at version $t$
- Note: This is a read-only operation; current state remains unchanged

**$6$. ROLLBACK $t$**
- Rollback the array and all data structures to the state after the $t$-th update operation
- Format: $6$ $t$
- Effect: All future versions are discarded, and subsequent operations continue from version $t$
- Note: Only update operations (types $1$ and $2$) create new versions

## Input Format

<!-- HOW: Technical schema only. Field names, types, JSON structure. No need to re-explain the problem goal. -->

Input is a JSON object with the following fields:

- `field1` (type): Description
- `field2` (type): Description

Example input:
```json
{
  "field1": value1,
  "field2": value2
}
```

## Output Format

<!-- HOW: Technical schema only. Describe what type/structure is returned, not why. -->

Output is a JSON value (type: [number/string/array/object]):

[Describe what the output represents]

Example output:
```json
result_value
```

## Constraints

- Constraint 1
- Constraint 2
- $1 \leq n \leq 10^5$
- Time limit: 2000ms
- Memory limit: 256MB

## Examples

### Example 1

**Input:**
```json
{
  "field1": "example",
  "field2": [1, 2, 3]
}
```

**Output:**
```json
6
```

**Explanation:** 
[Explain why this is the correct output]

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

