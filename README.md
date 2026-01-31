# Solution Tutorial

## Key Insights

1. **Pre-computation is Key:** Instead of checking each number in the range $[l, r]$ individually to see if it can be expressed as a sum of two prime powers, we can pre-generate all valid prime power sums up to the maximum constraint $(5 \times 10^6)$ and then use binary search to count how many fall within any given range.
2. **Limited Number of Prime Powers:** Since we need prime powers $p^k$ where $k \ge 2$ and the result must be $\le 5\times 10^6$, there are only a limited number of valid prime powers $(\approx 433)$. For example, $2^2 = 4, 2^3 = 8, \dots, 2^{22} = 4{,}194{,}304$, and larger primes quickly exceed the limit with even small exponents $(e.g., 2243^2 > 5 \times 10^6)$.
3. **Efficient Prime Generation:** We only need to generate primes up to $\sqrt{5 \times 10^6} \approx 2236$, because any prime power $p^k \text{where}  \ge 2$
 exceeding $5 \times 10^6$ requires $p > \sqrt{5 \times 10^6}$.
4. **Set for Deduplication:** Using a set to store all possible sums automatically handles duplicates. For instance, $2^2 + 2^3 = 2^3 + 2^2 = 12$, but it only gets counted once in our set.

## Algorithm


1. Step 1: Generate All Prime Numbers up to $(\sqrt{\text{MAX}})$
We generate all prime numbers up to $\sqrt{5 \times 10^6} \approx 2236$ using trial division:

``` cpp
for(int i = 2; i <= 2236; i++) {
    if(isprime(i)) {
        primes.push_back(i);
    }
}
```
The `isprime()` function checks divisibility by testing all odd numbers from 3 up to $\sqrt{n}$

2. Step 2: Generate All Prime Powers
For each prime p, we generate all powers $p^k$ where $k \ge 2$ and $p^k \le \text{MAX}$:

``` cpp
for(int prime : primes) {
    long long power = (long long)prime * prime;  // Start with p²
    while(power <= MAX) {
        primepowers.push_back(power);
        if(power > (MAX / prime)) break;  // Prevent overflow
        power = power * prime;  // Next power: p³, p⁴, ...
    }
}

```
This generates approximately $433$ prime powers

3. Step 3: Generate All Sums of Two Prime Powers
We create all possible sums by trying every pair (including pairs of the same prime power):

``` cpp
for(size_t i = 0; i < primepowers.size(); i++) {
    for(size_t j = 0; j < primepowers.size(); j++) {
        long long sum = primepowers[i] + primepowers[j];
        if(sum <= MAX) {
            validnums.insert(sum);
        }
    }
}
```
The nested loop generates all combinations, and the set automatically handles duplicates. This creates roughly $50{,}000 \text{ to } 100{,}000$ unique valid numbers.

4. Step 4: Convert Set to Sorted Vector
Convert the set to a vector for efficient binary search:

```cpp
vector<long long> res(validnums.begin(), validnums.end());
```
Since sets maintain sorted order, this vector is already sorted.


5. Step 5: Count Numbers in Range $[l, r]$ Using Binary Search
Use `lower_bound` to find the $\text{first element} ≥ l$ and `upper_bound` to find the $\text{first element} > r$:

``` cpp

auto left_it = lower_bound(res.begin(), res.end(), l);
auto right_it = upper_bound(res.begin(), res.end(), r);
int count = right_it - left_it;

```
The difference gives us the count of valid numbers in $[l, r]$.

## Complexity Analysis

- **Time Complexity:**  
  $O\!\left(M\sqrt{M} + P \log(\text{MAX}) + {P'}^{2}\log N + \log N\right)$
  where $(M = \sqrt{\text{MAX}} \approx 2236)$,  
  $(P \approx 335)$ (number of primes),  
  $(P' \approx 433)$ (number of prime powers),  
  and $(N \approx 50{,}000\!-\!100{,}000)$ (number of valid sums).

 - **Generating primes (Step 1):**  
  $(O(M\sqrt{M}))$. For each number up to $2236$, `isprime()` runs in $(O(\sqrt{n}))$.

 - **Generating prime powers (Step 2):**  
  $(O(P \log(\text{MAX})))$. Each prime generates powers until exceeding MAX.

 - **Generating sums (Step 3):**  
  $(O({P'}^{2}\log N))$. We iterate over all prime-power pairs and insert into a set.

 - **Converting set to vector (Step 4):**  
  $(O(N))$.

 - **Binary search (Step 5):**  
  $(O(\log N))$ per query.

  **Overall dominant term:**  
$[
O({P'}^{2}\log N) \approx 433^{2} \cdot \log_2(10^5)
]$
which is about $3.2$ million operations.



- **Space Complexity:** $O(P+N+P')$ 
 - $P ≈ 335$ for storing primes
 - $P' ≈ 433$ for storing prime powers
 - $N ≈ 50,000-100,000$ for storing all valid prime power sums
 - Total: $O(100,000) ≈ 800 KB$ for storing long long values

## Edge Cases

1. **Minimum Range $[1, 1]$:** The number $1$ cannot be expressed as a sum of two prime powers (minimum is $2^2 + 2^2 = 8$), so the answer is 0.
2. **Small Range with No Valid Numbers:** For example, $[2, 7]$ contains no prime power sums since the smallest is $8$. The binary search correctly returns 0.
3. **Maximum Constraints $[1, 5000000]$:** The pre-computation handles the entire range efficiently. All $~50k-100k$ valid numbers are counted.
4. **Range Containing Only One Valid Number:** For example, $[8, 8]$ should return $1$. The binary search handles this correctly since $\text{upper_bound}(8)$-$\text{lower_bound}(8)=1$
5. **Overflow Prevention:** When computing $\text{power} \cdot\ \text{prime}$, we check if $(\text{power} > (\text{MAX} / \text{prime}))$ to prevent overflow before multiplication.

## Common Pitfalls

- **Integer Overflow:** When computing prime powers, multiplying large numbers can cause overflow. The solution uses long long and checks if $(\text{power} > (\text{MAX} / \text{prime}))$ before multiplying to prevent this.
- **Incorrect Exponent Constraint:** Remember that exponents must be $> 1$, $\text{not} ≥ 1$. Starting with $\text{prime} * \text{prime}$ $(p^2)$ rather than prime ensures $k ≥ 2$.
- **Off-by-One in Binary Search:** Using `lower_bound` for the left boundary (inclusive) and `upper_bound` for the right boundary (exclusive) correctly counts all numbers in the closed interval $[l, r]$.
- **Not Handling Duplicates:** Using a set instead of a vector for initial storage automatically handles duplicates like $2^2 + 2^3 = 2^3 + 2^2 = 12$.
- **Inefficient Primality Testing:** The isprime function could be optimized with a sieve for better performance, but trial division is sufficient given we only need primes up to $2236$.

## Alternative Approaches

### Approach 1: Naive 
- For each number $n$ in $[l, r]$, check if it can be expressed as a sum of two prime powers by iterating through all prime power pairs.
- **Time Complexity:** $O((r-l)\cdot P^2)$ — For each of the $(r - l)$ numbers, we check all $~433²$ pairs of prime powers. For large ranges, this becomes extremely slow.
- **Space Complexity:** $O(P)$ — Only need to store the prime powers.
- **Verdict:** Inefficient for large ranges; would timeout on most test cases.

### Approach 2: Sieve-Based Prime Generation
- Instead of trial division for each number, use the Sieve of Eratosthenes to generate all primes up to $2236$ in one pass

``` cpp
vector<bool> sieve(2237, true);
sieve[0] = sieve[1] = false;
for(int i = 2; i * i <= 2236; i++) {
    if(sieve[i]) {
        for(int j = i * i; j <= 2236; j += i) {
            sieve[j] = false;
        }
    }
}
```

- **Time Complexity:** $O(M \log\log M + P \log M + P^2 + \log N)$  — Sieve is $O(n log log n)$, slightly faster than checking each number individually.
- **Space Complexity:** $O(M+P+N)$ — Additional $O(2237)$ for the sieve array.
- **Verdict:**This is an implementation improvement, not a fundamentally different approach.
