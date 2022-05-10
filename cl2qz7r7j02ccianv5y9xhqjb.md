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
# sort

The following two functions are general functions that will be used in sorting, so i will not write them one by one 

```
function checkArray(array){
   if (!array) return
}

function swap(array, left, right){
    let rightValue = array[right]
    array[right] = array[left]
   array[left] = rightValue
 }

``` 
## Bubble Sort

The principle of bubble sort is as follows, starting from the first element, comparing the current element with the next index element. If the current element is large, then swap positions and repeat the operation until the last element is compared, then the last element is the largest number in the array. Repeat the above operations in the next round, but at this time the last element is already the maximum number, so there is no need to compare the last element, only the position of length - 2 needs to be compared.


![162b895b452b306c.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1651754849831/lnqCt2daB.gif align="left")

Below is the code that implements the algorithm

```
function bubble(array){
   checkArray(array)
  for(let i = array.length - 1; i > 0; i--){
     // Traverse from 0 to `length - 1`
   for (let j = 0; j < i; j++) {
      if (array[j] > array[j + 1]) swap(array, j, j + 1)
    }
  }
return array
}
``` 
The number of operations of this algorithm is an arithmetic sequence n + (n - 1) + (n - 2) + 1 , after removing the constant term, the time complexity is O(n * n)

## selection sort

The principle of selection sort is as follows. Traverse the array and set the index of the minimum value to 0. If the retrieved value is smaller than the current minimum value, replace the minimum value index. After the traversal is complete, swap the first element with the value on the minimum value index. After the above operation, the first element is the minimum value in the array, and the next traversal can start from index 1 and repeat the above operation.


![162bc8ea14567e2e.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1651846399229/pK4fY9qB7.gif align="left")


```
function selection(array) {
  checkArray(array);
  for (let i = 0; i < array.length - 1; i++) {
    let minIndex = i;
    for (let j = i + 1; j < array.length; j++) {
      minIndex = array[j] < array[minIndex] ? j : minIndex;
    }
    swap(array, i, minIndex);
  }
  return array;
}

``` 
The number of operations of the algorithm is an arithmetic sequence n + (n - 1) + (n - 2) + 1 , after removing the constant term, the time complexity is O(n * n)

## Insertion sort


![162b895c7e59dcd1.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1651966748126/-Q1q_ixE2.gif align="left")

Below is the code that implements the algorithm

```
function insertion(array) {
  checkArray(array);
  for (let i = 1; i < array.length; i++) {
    for (let j = i - 1; j >= 0 && array[j] > array[j + 1]; j--)
      swap(array, j, j + 1);
  }
  return array;
}
``` 
The number of operations of this algorithm is an arithmetic sequence n + (n - 1) + (n - 2) + 1 , after removing the constant term, the time complexity is O(n * n)

##  merge sort

The principle of merge sort is as follows. Recursively divide the arrays in pairs until they contain at most two elements, then sort and merge the arrays, finally merging into a sorted array. Suppose I have a set of arrays [3, 1, 2, 8, 9, 7, 6], the middle index is 3, sort the array [3, 1, 2, 8] first. On this left array, continue splitting until the array contains two elements (if the array length is odd, there will be a split array containing only one element). Then sort the arrays [3, 1] and [2, 8], then sort the arrays [1, 3, 2, 8], so that the left array is sorted, then sort the right array according to the above ideas, and finally put the array [1, 2, 3, 8] and [6, 7, 9] sort.

![162be13c7e30bd86.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1652100192854/0q0MkfI-s.gif align="left")

Below is the code that implements the algorithm

```
function sort(array) {
  checkArray(array);
  mergeSort(array, 0, array.length - 1);
  return array;
}

function mergeSort(array, left, right) {
  // The left and right indexes are the same, indicating that there is already only one number
  if (left === right) return;
  // Equivalent to `left + (right - left) / 2`
  // Safer than `(left + right) / 2`, no overflow
  // Use bitwise operations because bitwise operations are faster than arithmetic
  let mid = parseInt(left + ((right - left) >> 1));
  mergeSort(array, left, mid);
  mergeSort(array, mid + 1, right);

  let help = [];
  let i = 0;
  let p1 = left;
  let p2 = mid + 1;
  while (p1 <= mid && p2 <= right) {
    help[i++] = array[p1] < array[p2] ? array[p1++] : array[p2++];
  }
  while (p1 <= mid) {
    help[i++] = array[p1++];
  }
  while (p2 <= right) {
    help[i++] = array[p2++];
  }
  for (let i = 0; i < help.length; i++) {
    array[left + i] = help[i];
  }
  return array;
}
``` 
The above algorithm uses the idea of ​​recursion. The essence of recursion is to push the stack. Every time a function is executed recursively, the information of the function (such as parameters, internal variables, and the number of lines executed) is pushed onto the stack until it encounters a termination condition, and then pops the stack and continues to execute the function. The call trace for the above recursive function is as follows

```
mergeSort(data, 0, 6)   // mid = 3
mergeSort(data, 0, 3) // mid = 1
mergeSort(data, 0, 1) // mid = 0
mergeSort(data, 0, 0) // When terminated, go back to the previous step
mergeSort(data, 1, 1) // When terminated, go back to the previous step
    // Sort p1 = 0, p2 = mid + 1 = 1, fall back to mergeSort(data, 0, 3) to perform the next recursion
mergeSort(2, 3) // mid = 2
mergeSort(3, 3) // When terminated, go back to the previous step
  // Sort p1 = 2, p2 = mid + 1 = 3 Fallback to mergeSort(data, 0, 3) to perform merge logic, Sort p1 = 0, p2 = mid + 1 = 2,  rollback after execution, The array on the left is sorted, and the right side is also the same as the above trajectory

``` 
The number of operations of the algorithm can be calculated as follows: recurse twice, each time the amount of data is half of the array, and finally iterate the entire array once, so the expression 2T(N / 2) + T(N) ( T stands for time, and N stands for data volume). According to the expression, the formula can be applied to get the time complexity of O(N * logN)

## Quick row

The principle of quicksort is as follows. Randomly select a value in an array as the reference value, and compare the value with the reference value from left to right. Put the value smaller than the reference value on the left of the array, and put the larger value on the right. After the comparison is completed, the reference value and the first value larger than the reference value are exchanged. Then, the array is divided into two parts by the position of the reference value, and the above operation is continued recursively.


![162cd23e69ca9ea3.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1652187576292/2E-4y_C_u.gif align="left")

Below is the code that implements the algorithm

```
function sort(array) {
  checkArray(array);
  quickSort(array, 0, array.length - 1);
  return array;
}

function quickSort(array, left, right) {
  if (left < right) {
    swap(array, , right 
// Randomly take the value, and then swap it with the end, which is slightly less complicated than taking a fixed position
    let indexs = part(array, parseInt(Math.random() * (right - left + 1)) + left, right);
    quickSort(array, left, indexs[0]);
    quickSort(array, indexs[1] + 1, right);
  }
}
function part(array, left, right) {
  let less = left - 1;
  let more = right;
  while (left < more) {
    if (array[left] < array[right]) {
      // The current value is less than the base value, and both `less` and `left` are increased by one
	   ++less;
       ++left;
    } else if (array[left] > array[right]) {
      // The current value is greater than the reference value, and the current value is exchanged with the value on the right
      // And do not change `left`, because the current value has not been judged for size
      swap(array, --more, left);
    } else {
      // The same as the base value, only the subscript is moved
      left++;
    }
  }
  // Swap the base value with the first value greater than the base value
  // This way the array becomes `[smaller than base value, base value, bigger than base value]`
  swap(array, right, more);
  return [less, more];
}
``` 

The complexity of the algorithm is the same as merge sort, but the additional space complexity is less than merge sort, only O(logN), and it takes less constant time than merge sort.







