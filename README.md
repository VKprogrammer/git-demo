# Problem Title

Equal Length Division

## Description

<!-- WHAT & WHY: Explain the problem goal and context. Don't specify the JSON schema here. -->
<!-- Example: "Given an array of integers, return the sum of all elements." -->

You are given a number line and several horizontal **line segments** on this number line. Each segment is defined by its **start** and **end** point on the number line.
Your goal is to find **minimum** position x = c at which the vertical line perfectly balances all of the segments. A vertical line is considered balanced when:

- The sum of the lengths of all segment parts left of the line is equal to the sum of the lengths of all segment parts right of the line.

- The equality must hold up to 5 decimal places of precision.

#### Important Note:
- Whenever a vertical line cuts through a segment, it divides the segment into two parts on the left side and on the right side. Both of these contribute to the total length on the left side or on the right side, respectively. 
- Overlapping segments are counted separately. In the case of many segments covering the same region, every segment's length in the covered region adds up independently to the total length. 
- Two values are taken to be equal if the absolute difference is less than 10^-5, i.e., they agree up to five decimal places.

## Input Format

<!-- HOW: Technical schema only. Field names, types, JSON structure. No need to re-explain the problem goal. -->

## Input Format

<!-- HOW: Technical schema only. Field names, types, JSON structure. No need to re-explain the problem goal. -->

Input is a JSON object with the following fields:

- `t` (integer): Number of test cases
- `test_cases` (array of objects): Array containing test case data
- `n` (integer): Number of horizontal segments in this test case
- `segments` (array of arrays): Each element is a 2-element array [x1, x2] where x1 is the start coordinate and x2 is the end coordinate of a segment

Example input:
```json
{
  "t": 2,
  "test_cases": [
    {
      "n": 3,
      "segments": [
        [1, 5],
        [2, 8],
        [3, 7]
      ]
    },
    {
      "n": 2,
      "segments": [
        [0, 10],
        [0, 10]
      ]
    }
  ]
}
```

## Output Format

<!-- HOW: Technical schema only. Describe what type/structure is returned, not why. -->

Output is a JSON value (type: array of numbers):

An array of floating-point numbers, one for each test case, representing the x-coordinate of the balance point rounded to exactly 5 decimal places.

Example output:
```json
[4.50000, 5.00000]
```

## Constraints

- $1 \leq t \leq 100$ (number of test cases)
- $1 \leq n \leq 10^4$ (number of segments per test case)
- $0 \leq x_1 < x_2 \leq 10^9$ (segment coordinates)
- Sum of $n$ across all test cases $\leq 10^5$
- Output must be rounded to exactly 5 decimal places
- Absolute difference between left and right totals must be $\leq 10^{-5}$
- Time limit: 1000ms
- Memory limit: 256MB

## Examples

### Example 1

**Input:**
```json
{
  "t": 2,
  "test_cases": [
    {
      "n": 3,
      "segments": [
        [1, 5],
        [2, 8],
        [3, 7]
      ]
    },
    {
      "n": 2,
      "segments": [
        [0, 10],
        [5, 15]
      ]
    }
  ]
}
```

**Output:**
```json
[4.50000, 7.50000]
```

**Explanation:** 

**Test Case 1:** Balance point is at x = 4.50000
- Segments: [1, 5], [2, 8], [3, 7]
- Left side total:
  - Segment [1, 5]: length from 1 to 4.5 = 3.5
  - Segment [2, 8]: length from 2 to 4.5 = 2.5
  - Segment [3, 7]: length from 3 to 4.5 = 1.5
  - Total = 7.5
- Right side total:
  - Segment [1, 5]: length from 4.5 to 5 = 0.5
  - Segment [2, 8]: length from 4.5 to 8 = 3.5
  - Segment [3, 7]: length from 4.5 to 7 = 2.5
  - Total = 7.5
- Left = Right ✓

**Test Case 2:** Balance point is at x = 7.50000
- Segments: [0, 10], [5, 15]
- Left side total:
  - Segment [0, 10]: length from 0 to 7.5 = 7.5
  - Segment [5, 15]: length from 5 to 7.5 = 2.5
  - Total = 10.0
- Right side total:
  - Segment [0, 10]: length from 7.5 to 10 = 2.5
  - Segment [5, 15]: length from 7.5 to 15 = 7.5
  - Total = 10.0
- Left = Right ✓

### Example 2

**Input:**
```json
{
  "t": 1,
  "test_cases": [
    {
      "n": 2,
      "segments": [
        [0, 10],
        [0, 10]
      ]
    }
  ]
}
```

**Output:**
```json
[5.00000]
```

**Explanation:**

**Test Case 1:** Balance point is at x = 5.00000
- Segments: [0, 10], [0, 10] (two identical overlapping segments)
- Left side total:
  - Segment 1: length from 0 to 5 = 5.0
  - Segment 2: length from 0 to 5 = 5.0
  - Total = 10.0 (both segments counted separately!)
- Right side total:
  - Segment 1: length from 5 to 10 = 5.0
  - Segment 2: length from 5 to 10 = 5.0
  - Total = 10.0
- Left = Right ✓

This example demonstrates that overlapping segments are counted independently.

