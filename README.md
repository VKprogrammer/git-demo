# Sum of Two Prime Powers

## Description

A number is called a `prime power sum` if it can be expressed as the sum of two prime powers, where both bases are prime numbers and both exponents are greater than $1$.

Formally, a number $x$ is a prime power sum if :
             $(x=a^p+b^q)$

$a$ and $b$ are prime numbers $(a,b E {2,3,5,7,11,13,....})$
$p$ and $q$ are integers greater than $1$ $(p,q>1)$

For example: 
$13$ is a prime power sum because $13=2^2+3^2$
$29$ is a prime power sum because $29=2^2+5^2$
$68$ is a prime power sum because $68=2^3+2^6$ ($a$ and $b$ can be the same prime number)
$10$ is NOT a prime power sum (cannot be expressed as the sum of two prime powers with $ \text{exponents}>1$)


Given a range $[l,r]$, your task it to count how many numbers in this range are prime power sums.


### Note:
- The same prime can be usd with different exponents (e.g., $2^2+2^3$ is valid)
- $a$ and $b$ can be equal (e.g., $2^2+2^2=8$ is valid)
- The order doesn't matter ($a^p+b^q$ and $b^q+a^p$ represent the same  number)
- Prime number are $2,3,5,7,11,13,17,19,23,29,31,....$
- Exponent must be greater than $1$: so $2^1=2$ is NOT a valid prime power for this problem 
- Both a and b must be prime : so $4^2=16$ is NOT valid ($4$ is not prime)




## Input Format

<!-- HOW: Technical schema only. Field names, types, JSON structure. No need to re-explain the problem goal. -->

Input is a JSON object with the following fields:

- `l` (integer): The lower bound of the range (inclusive)
- `r` (integer): The upper bound of the range (inclusive)

Example input:
```json
{
  "l": 1,
  "r": 20
}
```

## Output Format

<!-- HOW: Technical schema only. Describe what type/structure is returned, not why. -->

Output is a JSON value (type:integer):

The count of prime power sums in the range $[l,r]$ (inclusive).

Example output:
```json
6
```

## Constraints
- $1 \leq l \leq r \leq 5*10^6$
- Constra
- $1 \le a,b $
- Time limit: 2000ms
- Memory limit: 256MB

## Examples

### Example 1

**Input:**
```json
{
  "l": 1,
  "r": 20
}
```

**Output:**
```json
7
```

**Explanation:** 

Prime power sum in range $[1,20]$:
- $8=2^2+2^2=4+4$, has prime power sum
- $12=2^2+2^3=4+8$, has prime power sum
- $13=2^2+3^2=4+9$, has prime power sum
- $16=2^3+2^3=8+8$, has prime power sum
- $17=2^3+3^2=8+9$, has prime power sum
- $18=3^2+3^2=9+9$, has prime power sum
- $20=2^2+2^4=4+16$, has prime power sum
No any other element in the range $[1,20]$ has prime power sum
Total count:7

### Example 2

**Input:**
```json
{
  "l": 1,
  "r": 30
}
```

**Output:**
```json
11
```


**Explanation**:
Prime power sum in range $[1,30]$:
- $8=2^2+2^2=4+4$, has prime power sum
- $12=2^2+2^3=4+8$, has prime power sum
- $13=2^2+3^2=4+9$, has prime power sum
- $16=2^3+2^3=8+8$, has prime power sum
- $17=2^3+3^2=8+9$, has prime power sum
- $18=3^2+3^2=9+9$, has prime power sum
- $20=2^2+2^4=4+16$, has prime power sum
- $25=2^4+3^2=16+9$, has prime power sum
- $28=2^2+5^2=4+25$, has prime power sum
- $29=2^2+5^2=4+25$, has prime power sum


