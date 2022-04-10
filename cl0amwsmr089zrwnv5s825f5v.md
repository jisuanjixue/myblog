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

**Rounding up and rounding down**, these two operations are opposite, so the code is also similar, here only how to round down. Since it is rounded down, then according to the characteristics of the binary search tree, the value must be on the left side of the root node. Just keep traversing the left subtree until the value of the current node is no longer greater than or equal to the required value, and then determine whether the node still has the right subtree. If there is, continue the recursive judgment above.


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
Rank, which is used to get the rank of a given value or the value of the node with the rank. These two operations are also opposite, so this only introduces how to get the value of the node with the rank. For this operation, we need to modify the code slightly so that each node has a size property. This property indicates how many child nodes (including itself) are under this node.


```
class Node {
constructor(value) {
this.value = value
this.left = null
this.right = null
// modify the code
this.size = 1
}
}
// add code
_getSize(node) {
return node ? node.size : 0
}
_addChild(node, v) {
if (!node) {
return new Node(v)
}
if (node.value > v) {
// modify the code
node.size++
node.left = this._addChild(node.left, v)
} else if (node.value < v) {
// modify the code
node.size++
node.right = this._addChild(node.right, v)
}
return node
}
select(k) {
let node = this._select(this.root, k)
return node ? node.value : null
}
_select(node, k) {
if (!node) return null
// First get a few nodes under the left subtree
let size = node.left ? node.left.size : 0
// Determine if size is greater than k
// If it is greater than k, it means the desired node is on the left node
if (size > k) return this._select(node.left, k)
// If it is less than k, it means the desired node is on the right node
// Note that k needs to be recalculated here, minus the number of nodes in the root node except the right subtree
if (size < k) return this._select(node.right, k - size - 1)
return node
}

``` 

The next step is to explain the most difficult part of binary search trees: deleting nodes. Because for deleting nodes, there will be the following situations

- The node that needs to be deleted has no subtree
- The node to be deleted has only one subtree
- The node to be deleted has two left and right trees

It is easy to solve the first two cases, but the third case is difficult, so let’s implement a relatively simple operation first: delete the smallest node, for deleting the smallest node, there is no third case, delete the largest node The node operation is the opposite of deleting the smallest node, so it will not be repeated here.


```
delectMin() {
this.root = this._delectMin(this.root)
console.log(this.root)
}
_delectMin(node) {
// keep recursing on the left subtree
// If the left subtree is empty, determine whether the node has the right subtree
// If there is a right subtree, replace the node to be deleted with the right subtree
if ((node ​​!= null) & !node.left) return node.right
node.left = this._delectMin(node.left)
// Finally, the `size` of the next node needs to be re-maintained
node.size = this._getSize(node.left) + this._getSize(node.right) + 1
return node
}
``` 
The last thing to explain is how to delete any node. For this operation, T. Hibbard in 1962 proposed a solution to this problem, that is, how to solve the third case.

When this happens, the successor node of the current node (that is, the smallest node of the right subtree of the current node) needs to be taken out to replace the node that needs to be deleted. Then assign the left subtree of the node to be deleted to the successor node, and assign the right subtree to it after deleting the successor node.

If you are in doubt about this solution, consider it this way. Because of the nature of binary search trees, the parent node must be larger than all left child nodes and smaller than all right child nodes. Then when the parent node needs to be deleted, it is bound to take out a node larger than the parent node to replace the parent node. This node must not exist in the left subtree, but must exist in the right subtree. Then you need to keep the parent node smaller than the right child node, then you can take out the smallest node in the right subtree to replace the parent node.


```
delect(v) {
this.root = this._delect(this.root, v)
}
_delect(node, v) {
if (!node) return null
// The searched node is smaller than the current node, go to the left subtree to find
if (node.value < v) {
node.right = this._delect(node.right, v)
} else if (node.value > v) {
// The searched node is larger than the current node, go to the right subtree to find
node.left = this._delect(node.left, v)
} else {
// Entering this condition indicates that the node has been found
// First determine whether the node has one of the left and right subtrees
// If yes, return the subtree, which is the same as `_delectMin`
if (!node.left) return node.right
if (!node.right) return node.left
// Enter here, representing that the node has left and right subtrees
// First take the successor node of the current node, that is, take the minimum value of the right subtree of the current node
let min = this._getMin(node.right)
// After taking the minimum value, delete the minimum value
// Then assign the subtree after deleting the node to the minimum node
min.right = this._delectMin(node.right)
// left subtree does not move
min.left = node.left
node = min
}
// maintain size
node.size = this._getSize(node.left) + this._getSize(node.right) + 1
return node
}
``` 
# Trie

## concept

In computer science, a trie, also known as a prefix tree or dictionary tree, is an ordered tree used to hold associative arrays, the keys of which are usually strings.

To put it simply, the role of this structure is mostly to facilitate the search for strings. The tree has the following characteristics

- The root node represents an empty string, each node has N (if you search for English characters, there are 26) links, each link represents a character
- Nodes do not store characters, only paths are stored, which is different from other tree structures
- From the root node to any node, connecting the characters along the way is the string corresponding to the node


![163e1d2f6cec3348.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1649596777435/-JQQTadAc.webp)

## accomplish
In general, the implementation of Trie is much simpler than other tree structures, and the implementation takes searching for English characters as an example.


```
class TrieNode {
constructor() {
// Represents the number of times each character passes through the node
this.path = 0
// There are several strings representing the node
this.end = 0
// Link
this.next = new Array(26).fill(null)
}
}
class Trie {
constructor() {
// root node, representing the null character
this.root = new TrieNode()
}
// insert string
insert(str) {
if (!str) return
let node = this.root
for (let i = 0; i < str.length; i++) {
// Get the index corresponding to the character first
let index = str[i].charCodeAt() - 'a'.charCodeAt()
// If the index corresponds to no value, create it
if (!node.next[index]) {
node.next[index] = new TrieNode()
}
node.path += 1
node = node.next[index]
}
node.end += 1
}
// search for the number of occurrences of the string
search(str) {
if (!str) return
let node = this.root
for (let i = 0; i < str.length; i++) {
let index = str[i].charCodeAt() - 'a'.charCodeAt()
// If the index corresponds to no value, it means there is no string to search for
if (!node.next[index]) {
return 0
}
node = node.next[index]
}
return node.end
}
// delete string
delete(str) {
if (!this.search(str)) return
let node = this.root
for (let i = 0; i < str.length; i++) {
let index = str[i].charCodeAt() - 'a'.charCodeAt()
// If the Path of the node corresponding to the index is 0, it represents the string passing through the node
// already one, just delete it
if (--node.next[index].path == 0) {
node.next[index] = null
return
}
node = node.next[index]
}
node.end -= 1
}
}
``` 
## and check

# concept
Union search is a special tree structure used to deal with some disjoint merge and query problems. Each node in the structure has a parent node. If there is only one current node, then the parent node of the node points to itself.

There are two important operations in this structure, namely:

- Find: Determines which subset the element belongs to. It can be used to determine whether two elements belong to the same subset.

-Union: Combines two sub-sets into the same set.


![163e45b56fd25172.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1649597163594/kmSqV6WyA.webp)

## accomplish

```
class DisjointSet {
// initialize the sample
constructor(count) {
// When initialized, the parent node of each node is itself
this.parent = new Array(count)
// Used to record the depth of the tree to optimize the search complexity
this.rank = new Array(count)
for (let i = 0; i < count; i++) {
this.parent[i] = i
this.rank[i] = 1
}
}
find(p) {
// Find whether the parent node of the current node is yourself, if not, it means that it has not been found
// Start the path compression optimization
// Assume the parent node of the current node is A
// Mount the current node to the parent node of node A to achieve the purpose of compression depth
while (p != this.parent[p]) {
this.parent[p] = this.parent[this.parent[p]]
p = this.parent[p]
}
return p
}
isConnected(p, q) {
return this.find(p) === this.find(q)
}
// merge
union(p, q) {
// find the parent node of the two numbers
let i = this.find(p)
let j = this.find(q)
if (i === j) return
// Determine the depth of the two trees, add the smaller depth to the tree with the larger depth
// If the two trees have the same depth, it doesn't matter how to add
if (this.rank[i] < this.rank[j]) {
this.parent[i] = j
} else if (this.rank[i] > this.rank[j]) {
this.parent[j] = i
} else {
this.parent[i] = j
this.rank[j] += 1
}
}
}
``` 







