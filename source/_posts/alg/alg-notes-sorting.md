---
title: Algorithm Notes -- Sorting
date: 2019-08-17 18:58:32
tags: Algorithm
categories: Algorithm Notes
---

{% asset_link slides-merge-sort.pdf Lecture slides -- merge sort %}


## Sorting in Java

Implement the `Comparable` interface so that any sorting function can call back object's `compareTo()` when performing sorting.

[*Callback Function*](https://stackoverflow.com/questions/824234/what-is-a-callback-function)

<!-- more -->

```java
public interface Comparable<Item> {
    public int compareTo(Item other);
}
```

Sort implementation:

```java
public static void sort(Comparable[] arr) {
    // ...
    if (arr[i].compareTo(arr[j]) < 0) {
        // ...
    }
}
```

## Selection Sort

In the ith iteration, find the ith smallest element and swap with `arr[i]`.

```java
for (int i = 0; i < arr.length; i++) {
    int curMin = i;

    for (int j = i + 1; j < arr.length; j++) {
        if (less(arr, j, curMin))
            curMin = j;
    }

    swap(arr, i, curMin);
}
```

**Time complexity:** O(n^2)

## Insertion Sort

In the ith iteration, move the ith element to its left until in the right position.
(like taking a new card and placing it to the right position when playing poker)

```java
for (int i = 0; i < arr.length; i++) {
    for (int j = i; j > 0; j--) {
        if (less(arr, j, j-1))
            swap(arr, j, j-1);
        else
            break; // already in the right pos
    }   
}
```

**Time complexity**
- O(n) for *partially sorted arrays*.
    \# of `swap` equals # of *inversions*
- O(n^2) worst case

*Partially sorted array*: where # of inversions <= c * N
*Inversion*: a pair of keys that are out of order.

## Shellsort

Based on insertion sort. Move elements to the left **more than one position** at a time by *h-sorting* the array multiple times.

*h-sort*: insertion sort with stride length *h*.

E.g.: For `arr` with length 7, 7-sort, 3-sort, and finally 1-sort (insertion sort). `arr` remains 7-sorted after 3-sorted. Since `arr` becomes more partially sorted at each step, each element is moved few positions in each sorting operation, which makes the underlying insertion sort efficient.

[TODO](https://www.bilibili.com/video/av9005901/?p=10)

## Shuffle Sort (just shuffling)

Knuth shuffle / Fisher-Yates shuffle:
- In the ith iteration, generate a random number r between **0 and i**, then swap `arr[r]` and `arr[i]`.
- O(n) time complexity.


