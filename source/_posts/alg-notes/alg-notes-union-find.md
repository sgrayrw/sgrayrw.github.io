---
title: Algorithm Notes -- Union Find
date: 2019-07-08 14:25:01
tags: Algorithm
categories: Algorithm Notes
---

## Dynamic Connectivity
Two main ops:
- union command: connect two objects.
- find/connected query: is there a path connecting two objects?
	(not actually finding the path, which is in course part II)

<!-- more -->

"is connected to" is an *equivalence relation*. (reflexive, symmetric, transitive)

*Connected components*: maximal *set* of objects that are mutually connected.

This way,
- find -> check if two objects are in the same component
- union -> union two components


## Quick Find
An *eager algorithm* to solve dynamic connectivity problem.

**Data structure**
- each number as index of an array, initially `arr[n] = n`

**Operation**
- find: num `p` and `q` are connected if `arr[p] == arr[q]`
- union: to union `p` and `q`, for all `i` where `arr[i] == arr[p]`, `arr[i] = arr[q]`

**Time complexity**
- find: O(1)
- union: O(n), too expensive


## Quick Union
A *lazy approach*.

**Data structure**
- still `arr` of size `n`
- `arr[i]` is the parent of `i`
- *root* of `i` is `arr[arr[...arr[i]...]]`

therefore associates each item with a root, which represents the connected component.

**Operation**
- find: check if `p` and `q` have the same root
- union: if `x == root(p)` and `y == root(q)`, then `arr[x] = y`, meaning that x's parent now becomes y

**Time complexity**
- `root`: O(n)
	- find: O(n)
	- union: O(n)
Since trees could get tall, method `root` could be expensive.


## Quick Union Improvements

### Improvement 1: weighted quick union
Use weighted quick union: avoid putting larger trees under smaller trees, in terms of the number of nodes. ([doesn't quite matter](https://stackoverflow.com/a/30958496/10467797) whether comparing by size or height)

{% asset_img qu_vs_quweighted.png quick union vs weighted quick union %}

**Data Structure**
- add extra array `sz` to track # of objects in the tree rooted at `i`

**Operation**
- find: same
- union: attach roots `x` and `y` to each other based on `sz[x]` and `sz[y]`

**Time complexity**
- `root`: O(depth of tree) -> [O(log n)](https://stackoverflow.com/a/2307330/10467797)
	- for the tree here and BST in general, in terms of time, `m` time allows <code>2<sup>m</sup></code> elements to be searched, so `n` elements require at most `log n` time (inverse relation)
	- find: O(log n)
	- union: O(log n)


### Improvement 2: path compression
On the way up to the root, make every other node point to its grandparent (halfing path length w/ constant extra time)

{% codeblock lang:go %}
func root(i int) int {
	for i != arr[i] {
		arr[i] = arr[arr[i]] // rebase parent
		i = arr[i]
	}
}
{% endcodeblock %}

*WQUPC* (Weighted quick union with path compression)
> time: N + M lg* N, for M union-find ops on a set of N objects

**Time Complexity**
- find and union: practically O(1)

	> very, very nearly, but not quite 1 (amortized)


## Applications
*Percolation model*, etc.
