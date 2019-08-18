---
title: Algorithm Notes -- Stacks and Queues
date: 2019-07-21 22:52:18
tags: Algorithm
categories: Algorithm Notes
---

{% asset_link slides.pdf Lecture slides %}

**Stack vs. Queue**
- stack: *LIFO* (last in first out)
	- `push`, `pop`
- queue: *FIFO* (first in first out)
	- `enqueue`, `dequeue`

<!-- more -->

## Stack

### Efficient use of memory (`pop`)
When implementing stacks using array in Java:

```java
public String pop() {
	return arr[--N];
}
```

The above implementation has a problem called *loitering*, as `s[N]` after `pop()` is still in the array and being referenced. To have a more efficient use of memory, do:

```java
public String pop() {
	String item = arr[--N];
	arr[N] = null;
	return item;
}
```

so that the garbage collector can reclaim the memory.

### Resizing underlying array

1. Initialize with array of length 1

	```java
	public Stack() {
		this.arr = new String[1];
	}
	```


2. Double the size of the underlying array when full.

	```java
	public void push(String item) {
		// grow
		if (N == arr.length)
			resize(2 * arr.length);

		arr[N++] = item;
	}

	private void resize(int cap) {
		// resized array
		String[] arrNew = new String[cap]
		// copy elements
		for (int i = 0; i < N; i++)
			arrNew[i] = arr[i];
		arr = arrNew;
	}
	```

	Time Complexity: O(1) for `push` operation. (using [amortized analysis of algs](https://stackoverflow.com/questions/11102585/what-is-amortized-analysis-of-algorithms))

3. Half the size when array is 1/4 full. (not 1/2 to avoid *thrashing* (push-pop-push...))

	```java
	public String pop() {
		String item = arr[--N];
		arr[N] = null;

		// shrink
		if (N < arr.length / 4)
			resize(arr.length / 2)
	}
	```


## Queue

Similar to stacks, implementations including linked-list and resizing array. Both requires maintaining two pointers: first and last.

- In linked-list implementation, `enqueue` to `last` and `dequeue` from `first` to ensure that the for-each loop iterates in *FIFO* order.

## Generics

```java
// stack for type `Item`
public class Stack<Item> {
	public void push(Item item) {}
	public Item pop() {}

	public static void main(String[] args) {
		Stack<Apple> appleStack = new Stack<Apple>();
	}
}
```

## Iterator

If a class implements `Iterable` interface, it enables access with for-each loop.

`Iterable` and `Iterator` interface:

```java
interface Iterable<T> {
	Iterator<T> iterator();
}

interface Iterator<T> {
	boolean hasNext();
	T next();
	void remove(); // not implementing this here
}
```

Linked-list implementation of stacks that implements `iterable`:

```java
public class Stack<Item> implements Iterable<Item> {
	public Iterator<Item> iterator() {
		return new stackIterator();
	}

	private class StackIterator implements Iterator<Item> {
		Node current = first;

		boolean hasNext() {
			return current != null;
		}

		Item next() {
			Item item = current.item;
			current = current.next;
			return item;
		}
	}
}
```

Array implementation:

```java
public class Stack<Item> implements Iterable<Item> {
	public Iterator<Item> iterator() {
		return new stackIterator();
	}

	private class StackIterator implements Iterator<Item> {
		int i = N;

		boolean hasNext() {
			return i > 0
		}

		Item next() {
			return arr[--i];
		}
	}
}
```

## Bag

An collection of elements where order doesn't matter.

## Practice
[Queues](https://github.com/sgrayrw/alg-practices#queues)