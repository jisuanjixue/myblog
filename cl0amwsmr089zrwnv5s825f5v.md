## common data structures(Continuous update)

In this chapter we will learn about data structures in the future. People often ask: Is learning data structures or algorithms useful for front-end engineers?

there are only two skills that are still useful to me:

1.  Basic subject content, such as: network knowledge, data structure algorithm

2. programming ideas

> This chapter contains a lot of information and is not suitable for reading in a non-computer environment. Please open the code editor and type the code line by line. You cannot learn the data structure simply by reading.

# time complexity

Before getting to the point, let's first understand what time complexity is.

Usually the worst time complexity is used to measure the quality of an algorithm.

Constant time O(1) means that this operation has nothing to do with the amount of data, it is a fixed time operation, such as four arithmetic operations.

For an algorithm, the number of operations may be calculated as aN + 1, where N represents the amount of data. Then the time complexity of this algorithm is O(N). Because when we calculate the time complexity, the amount of data is usually very large, and the low-order terms and constant terms can be ignored at this time.

Of course, the time complexity of both algorithms may be O(N), so to compare the quality of the two algorithms, we need to compare the low-order terms and constant terms.

# stack

## concept

A stack is a linear structure, a fairly common data structure in computers.

The characteristic of the stack is that data can only be added or deleted at one end, following the principle of first in, last out

![1637b785d2d68735.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1646289467787/Hl-_l6Sjl.webp)

## accomplish

Each data structure can be implemented in many ways. In fact, the stack can be regarded as a subset of the array, so the array is used here to implement

```
class Stack {
  constructor() {
    this.stack = []
  }
  push(item) {
    this.stack.push(item)
  }
  pop() {
    this.stack.pop()
  }
  peek() {
    return this.stack[this.getCount() - 1]
  }
  getCount() {
    return this.stack.length
  }
  isEmpty() {
    return this.getCount() === 0
  }
}

``` 
## application

Selected topic number 20 on LeetCode

The meaning of the question is to match parentheses, which can be completed through the characteristics of the stack.

```
var isValid = function (s) {
  let map = {
    '(': -1,
    ')': 1,
    '[': -2,
    ']': 2,
    '{': -3,
    '}': 3
  }
  let stack = []
  for (let i = 0; i < s.length; i++) {
    if (map[s[i]] < 0) {
      stack.push(s[i])
    } else {
      let last = stack.pop()
      if (map[last] + map[s[i]] != 0) return false
    }
  }
  if (stack.length > 0) return false
  return true
};
``` 
# queue

## concept

A queue is a linear structure characterized by adding data at one end and deleting data at the other end, following the principle of first-in, first-out.

![1637cba2a6155793.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1646289905431/S6eL3KY1N.webp)

## accomplish

Two ways to implement queues will be explained here, namely single-chain queues and circular queues.

### single chain queue

```
class Queue {
  constructor() {
    this.queue = []
  }
  enQueue(item) {
    this.queue.push(item)
  }
  deQueue() {
    return this.queue.shift()
  }
  getHeader() {
    return this.queue[0]
  }
  getLength() {
    return this.queue.length
  }
  isEmpty() {
    return this.getLength() === 0
  }
}

``` 
Because the single-chain queue requires O(n) time complexity when dequeuing, a circular queue is introduced. The dequeue operation of a circular queue is on average O(1) time complexity.

### circular queue

```
class SqQueue {
constructor(length) {
this.queue = new Array(length + 1)
// team leader
this.first = 0
// end of line
this.last = 0
// current queue size
this.size = 0
}
enQueue(item) {
// Determine whether the tail + 1 is the head of the queue
// If it is, it means that the array needs to be expanded
// % this.queue.length is to prevent the array from going out of bounds
if (this.first === (this.last + 1) % this.queue.length) {
this.resize(this.getLength() * 2 + 1)
}
this.queue[this.last] = item
this.size++
this.last = (this.last + 1) % this.queue.length
}
deQueue() {
if (this.isEmpty()) {
throw Error('Queue is empty')
}
let r = this.queue[this.first]
this.queue[this.first] = null
this.first = (this.first + 1) % this.queue.length
this.size--
// Determine whether the current queue size is too small
// In order to ensure that no space is wasted, when the queue space is equal to a quarter of the total length
// and not 2, reduce the total length to half of the current
if (this.size === this.getLength() / 4 && this.getLength() / 2 !== 0) {
this.resize(this.getLength() / 2)
}
return r
}
getHeader() {
if (this.isEmpty()) {
throw Error('Queue is empty')
}
return this.queue[this.first]
}
getLength() {
return this.queue.length - 1
}
isEmpty() {
return this.first === this.last
}
resize(length) {
let q = new Array(length)
for (let i = 0; i < length; i++) {
q[i] = this.queue[(i + this.first) % this.queue.length]
}
this.queue = q
this.first = 0
this.last = this.size
}
}
``` 
# linked list

## concept

The linked list is a linear structure, but also a natural recursive structure. The linked list structure can make full use of the computer memory space and realize flexible dynamic memory management. However, the linked list loses the advantage of random reading of the array, and the space overhead of the linked list is relatively large due to the increase of the pointer field of the node.

![16388487759b1152.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1646290373484/IFxcIKXlE.webp)

## accomplish

### Singly linked list


```
class Node {
constructor(v, next) {
this.value = v
this.next = next
}
}
class LinkList {
constructor() {
// length of the linked list
this.size = 0
// virtual header
this.dummyNode = new Node(null, null)
}
find(header, index, currentIndex) {
if (index === currentIndex) return header
return this.find(header.next, index, currentIndex + 1)
}
addNode(v, index) {
this.checkIndex(index)
// When inserting to the end of the linked list, prev.next is empty
// In other cases, because the node is to be inserted, the inserted node
// next should be prev.next
// Then set prev.next to the inserted node
let prev = this.find(this.dummyNode, index, 0)
prev.next = new Node(v, prev.next)
this.size++
return prev.next
}
insertNode(v, index) {
return this.addNode(v, index)
}
addToFirst(v) {
return this.addNode(v, 0)
}
addToLast(v) {
return this.addNode(v, this.size)
}
removeNode(index, isLast) {
this.checkIndex(index)
index = isLast ? index - 1 : index
let prev = this.find(this.dummyNode, index, 0)
let node = prev.next
prev.next = node.next
node.next = null
this.size--
return node
}
removeFirstNode() {
return this.removeNode(0)
}
removeLastNode() {
return this.removeNode(this.size, true)
}
checkIndex(index) {
if (index < 0 || index > this.size) throw Error('Index error')
}
getNode(index) {
this.checkIndex(index)
if (this.isEmpty()) return
return this.find(this.dummyNode, index, 0).next
}
isEmpty() {
return this.size === 0
}
getSize() {
return this.size
}
}
``` 
