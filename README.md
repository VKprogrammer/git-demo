You are given a number line and several horizontal **line segments** on this number line. Each segment is defined by its **start** and **end** point on the number line.
Your goal is to find **minimum** position x = c at which the vertical line perfectly balances all of the segments. A vertical line is considered balanced when:

- The sum of the lengths of all segment parts left of the line is equal to the sum of the lengths of all segment parts right of the line.

- The equality must hold up to 5 decimal places of precision.

#### Important Note:
- Whenever a vertical line cuts through a segment, it divides the segment into two parts on the left side and on the right side. Both of these contribute to the total length on the left side or on the right side, respectively. 
- Overlapping segments are counted separately. In the case of many segments covering the same region, every segment's length in the covered region adds up independently to the total length. 
- Two values are taken to be equal if the absolute difference is less than 10^-5, i.e., they agree up to five decimal places.
