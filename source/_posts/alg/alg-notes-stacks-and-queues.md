---
title: Algorithm Notes -- Stacks and Queues
date: 2019-07-21 22:52:18
tags: Algorithm
categories: Algorithm Notes
---

**Stack vs. Queue**
- stack *LIFO* (last in first out)
	- `push`, `pop`
- queue *FIFO* (first in first out)
	- `enqueue`, `dequeue`

<!-- more -->

**Efficient use of memory**
When implementing stacks using array in Java:

{% codeblock lang:java %}
public String pop() {
	return s[--N];
}
{% endcodeblock %}

The above implementation has a problem called *loitering*, as `s[N]` after `pop()` is still in the array and being referenced. To have a more efficient use of memory, do:

{% codeblock lang:java %}
public String pop() {
	String ret = s[--N];
	s[N] = null;
	return ret;
}
{% endcodeblock %}

so that the garbage collector can reclaim the memory.