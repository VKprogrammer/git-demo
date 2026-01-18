### Analytical Solution (Mathematical)

For \( n \) segments \( [a_i, b_i] \):

\[
\text{length}_i = b_i - a_i
\]

\[
\text{midpoint}_i = \frac{a_i + b_i}{2}
\]

The balance point is computed as:

\[
c = \frac{\sum_{i=1}^{n} (b_i - a_i)\cdot \frac{(a_i + b_i)}{2}}{\sum_{i=1}^{n} (b_i - a_i)}
\]

Equivalently,

\[
c = \frac{1}{2}\cdot \frac{\sum_{i=1}^{n} (b_i - a_i)(a_i + b_i)}{\sum_{i=1}^{n} (b_i - a_i)}
\]
