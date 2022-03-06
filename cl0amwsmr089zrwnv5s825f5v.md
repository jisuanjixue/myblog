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

# Tree

## binary tree

Trees have many structures. Binary tree is the most commonly used structure in trees, and it is also a natural recursive structure.

A binary tree has a root node, and each node has at most two child nodes: the left node and the right node. The bottom node of the tree is called a leaf node. When the number of leaves of a tree is full, the tree can be called a full binary tree.

![163884f74c9f4e4d.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1646533549895/vJ4VTjXv0.webp)

## binary search tree

A binary search tree is also a binary tree and has the characteristics of a binary tree. But the difference is that the value of each node of the binary search tree is larger than the value of its left subtree and smaller than the value of the right subtree.

This storage method is very suitable for data search. As shown in the figure below, when 6 needs to be searched, because the value to be searched is larger than the value of the root node, it only needs to be searched on the right subtree of the root node, which greatly improves the search efficiency.

## accomplish

class Node {
  constructor(value) {
    this.value = value
    this.left = null
    this.right = null
  }
}
class BST {
  constructor() {
    this.root = null
    this.size = 0
  }
  getSize() {
    return this.size
  }
  isEmpty() {
    return this.size === 0
  }
  addNode(v) {
    this.root = this._addChild(this.root, v)
  }
  // When adding a node, you need to compare the added node value with the current
 // size of node value
  _addChild(node, v) {
    if (!node) {
      this.size++
      return new Node(v)
    }
    if (node.value > v) {
      node.left = this._addChild(node.left, v)
    } else if (node.value < v) {
      node.right = this._addChild(node.right, v)
    }
    return node
  }
}

The above is the most basic binary search tree implementation, and then the tree traversal is implemented.

For tree traversal, there are three traversal methods, namely pre-order traversal, in-order traversal, and post-order traversal. The difference between the three traversals is when the nodes are visited. In the process of traversing the tree, each node will be traversed three times, traversing to itself, traversing the left subtree and traversing the right subtree. If you need to implement preorder traversal, you only need to do the operation when the node is traversed for the first time.


```
// Preorder traversal can be used to print the structure of the tree
// Preorder traversal first visits the root node, then the left node, and finally the right node.
preTraversal() {
  this._pre(this.root)
}
_pre(node) {
  if (node) {
    console.log(node.value)
    this._pre(node.left)
    this._pre(node.right)
  }
}
// Inorder traversal can be used for sorting
// For BST, in-order traversal can be implemented in one traversal
// get sorted values
// Inorder traversal means visiting the left node first, then the root node, and finally the right node.
midTraversal() {
  this._mid(this.root)
}
_mid(node) {
  if (node) {
    this._mid(node.left)
    console.log(node.value)
    this._mid(node.right)
  }
}
// Postorder traversal can be used to manipulate child nodes first
// Re-manipulate the scene of the parent node
// Postorder traversal means visiting the left node first, then the right node, and finally the root node.
backTraversal() {
  this._back(this.root)
}
_back(node) {
  if (node) {
    this._back(node.left)
    this._back(node.right)
    console.log(node.value)
  }
}
``` 
The above types of traversal can be called depth traversal, and the corresponding traversal is called breadth traversal, which is to traverse the tree layer by layer. For breadth traversal, we need to use the queue structure mentioned earlier.


```
breadthTraversal() {
  if (!this.root) return null
  let q = new Queue()
 // enqueue the root node

  q.enQueue(this.root)
 // Loop to determine whether the queue is empty, it is empty
// Indicates that the tree traversal is complete

  while (!q.isEmpty()) {
    // Dequeue the team leader to determine whether there are left and right subtrees
// If there is, enter the queue from left to right

    let n = q.deQueue()
    console.log(n.value)
    if (n.left) q.enQueue(n.left)
    if (n.right) q.enQueue(n.right)
  }
}
``` 
Next, we will introduce how to find the minimum or maximum number in a tree. Because of the characteristics of binary search trees, the minimum value must be at the far left of the root node, and the maximum value is opposite.


```
getMin() {
  return this._getMin(this.root).value
}
_getMin(node) {
  if (!node.left) return node
  return this._getMin(node.left)
}
getMax() {
  return this._getMax(this.root).value
}
_getMax(node) {
  if (!node.right) return node
  return this._getMin(node.right)
}
``` 

Rounding up and rounding down, these two operations are opposite, so the code is also similar, here only how to round down. Since it is rounded down, then according to the characteristics of the binary search tree, the value must be on the left side of the root node. Just keep traversing the left subtree until the value of the current node is no longer greater than or equal to the required value, and then determine whether the node still has the right subtree. If there is, continue the recursive judgment above.


```
floor(v) {
  let node = this._floor(this.root, v)
  return node ? node.value : null
}
_floor(node, v) {
  if (!node) return null
  if (node.value === v) return v
// If the current node value is still greater than the required value, continue the recursion

  if (node.value > v) {
    return this._floor(node.left, v)
  }

// Determine if the current node has the right subtree
  let right = this._floor(node.right, v)
  if (right) return right
  return node
}
``` 



