# Data Structure Runtimes
![big-o-complexity-graph](https://i.imgur.com/m3LMLob.png)
![common-data-structure-operations](https://i.imgur.com/qpFAhUS.png)
![array-sorting-algorithms](https://imgur.com/hwqgNOv.png)

**Big-O notation is a relative representation of the complexity of an algorithm**
1) O(1) constant
2) O(log n) logarithmic
3) O(n) linear
4) O(n log n)
5) O(n^2) quadratic
6) O(n^a) polynomial
7) O(2^n) exponential
8) O(n!) factorial

## Trees
### Avl Tree
- balanced binary search tree
- first data structure to be invented (woah)
- like red-black trees, they are not perfectly balanced
  - pairs of sub-trees differ in height by ast most 1
- insertions and deletions may require the tree to be rebalanced by one or more rotations
- faster than red-black trees for lookup-intensive applications because they are more strictly balanced

### Heap (Priority Queue)
- parents are greater than or equal to (max heap) or less than or equal to (min heap) their children
- used to implement priority queues
  - priority queues are often referred to as heaps
- **Advantages**
  - O(1) insert (average)
  - O(1) find min/max
- **Disadvantages**
  - If inserting in opposite of ranked order, ends up being a linked list

### Trie
- search tree used to store a dynamic set or associative array
  - keys usually strings
- all descendants of a node have a common prefix
- flag to mark a complete entry or store data about that entry
- **Run Times**
  - lookup is O(m) where m is the length of the search term
  - sorting is O(n) where n is the number of nodes in trie
    - achieved with an **in order** traversal


### Quad Tree
- each internal node has exactly four children
- often used to partition two-dimensional space by recursively subdividing it into four quadrants
- decompose space into adaptable cells
- each cell has a maximum capacity, when maximum capacity is reached, the bucket splits

## Maps
### HashMap
- makes no guarantees about ordering

### TreeMap
- will iterate according to natural orders of keys

### LinkedMap
- will iterate according to the order in which the entries were put into the map
- combines O(1) access with a linked list for consistent iteration order
  - preserves order in which (key, value) pairs are inserted

### InversionMap
- maps integer ranges to values
- `getLeast(intKey)` => gets the largest index that rangeArray[index] <= intKey from the inversion map
  - used to get the largest index that maps to a value <= the passed key
