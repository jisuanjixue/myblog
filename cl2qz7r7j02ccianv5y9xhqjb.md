## Analysis of Common Exam Algorithm Questions

# bit operation

Before getting into the topic, let's learn about bitwise operations. Because bit operations are useful in arithmetic, they can be much faster than arithmetic operations.

Before learning bit operations, you should know how to convert decimal to binary and how to convert binary to decimal. Here is a simple calculation method.

- Decimal 33 can be regarded as 32 + 1, and 33 should be six-digit binary (because 33 is approximately 32, and 32 is the fifth power of 2, so it is six digits), then decimal 33 is 100001, as long as it is 2 times square, then it is 1, otherwise both are 0.
- Then the binary 100001 is the same, the first digit is 2^5 and the last digit is 2^0 , adding up to 33

## left shift <<

```
10 << 1  // -> 20
``` 
Left shift is to move all the binary to the left. 10 is represented as 1010 in binary. After shifting one bit to the left, it becomes 10100, which is converted to decimal, which is 20, so the left shift can basically be regarded as the following formula a * (2 ^ b )

## Arithmetic shift right >>

Arithmetic right shift is to move all the binary to the right and remove the redundant right side. 10 is represented as 1010 in binary, and after shifting one bit to the right, it becomes 101, which is 5 when converted to decimal, so the right shift can basically be regarded as the following formula int v = a / (2^b)

Right shift is very useful, for example, it can be used to take the middle value in the binary algorithm

```
13 >> 1 // -> 6
``` 
## bitwise operation

### bitwise AND
Each bit is 1, the result is 1

```
8 & 7 // -> 0
// 1000 & 0111 -> 0000 -> 0
``` 
### bitwise OR
One of them is 1, the result is 1

```
8 | 7 // -> 15
// 1000 | 0111 -> 1111 -> 15
``` 
### Bitwise XOR
Each bit is different, the result is 1

From the above code, it can be found that the bitwise XOR is the addition without carry

**Interview Question:** Sum of Two Numbers Without Using Four Arithmetic

In this question, bitwise XOR can be used, because bitwise XOR is an addition without carry, 8 ^ 8 = 0 If it is carried, it is 16, so we only need to XOR the two numbers, and then carry. Then that is to say, where both binary bits are 1, there should be a carry 1 on the left, so the following formula a + b = (a ^ b) + ((a & b) << 1) can be obtained, and then by iteration way of simulating addition


```
function sum(a, b) {
    if (a == 0) return b
    if (b == 0) return a
    let newA = a ^ b
    let newB = (a & b) << 1
    return sum(newA, newB)
}

``` 



