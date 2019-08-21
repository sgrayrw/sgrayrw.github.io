---
title: Algorithm Notes -- Sorting
date: 2019-08-17 18:58:32
categories: Algorithm Notes
---

Lecture slides -- {% asset_link slides-elementary-sorts.pdf elementary sorts %} / {% asset_link slides-merge-sort.pdf merge sort %} / {% asset_link slides-quick-sort.pdf quick sort %}


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

*Knuth shuffle / Fisher-Yates shuffle*
In the ith iteration, generate a random number r between **0 and i**, then swap `arr[r]` and `arr[i]`.

**Time complexity:** O(n)


## Merge Sort

Divide the array into two subarrays, **recursively** sort them, and then merge them into one sorted array.

Implementation: using an extra array `aux` to hold data, then moving elements back to `arr` in sorted order by merging.

{% asset_img merge.png in-place merge w/ aux array %}

```java
public static void merge(Comparable[] arr, Comparable[] aux, int lo, int mid, int hi) {
    // precondition: first half sorted
    assert sorted(arr, lo, mid);
    // precondition: second half sorted
    assert sorted(arr, mid+1, hi);

    // copy arr to aux
    for (int k = lo; k <= hi; k++) {
        aux[i] = arr[i];
    }

    // merge
    int k = lo; int i = lo; j = mid + 1;
    while (k <= hi) {
        // first half out of elements
        if      (i > mid)           arr[k++] = aux[j++];
        // second half out of elements
        else if (j > hi)            arr[k++] = aux[i++];
        // pick smaller element from two subarrays
        else if (less(aux, i, j))   arr[k++] = aux[i++];
        else                        arr[k++] = aux[j++];
    }

    // postcondition: whole array sorted
    assert sorted(arr, lo, hi);
}
```

Then `sort()` will be:

```java
public static void sort(Comparable[] arr, Comparable[] aux, int lo, int hi) {
    // base case
    if (lo >= hi)   return;

    int mid = lo + hi / 2;

    // sort two halves
    sort(arr, aux, lo, mid);
    sort(arr, aux, mid+1, hi);

    // merge
    merge(arr, aux, lo, mid, hi);
}

public static void sort(Comparable[] arr) {
    // create aux array
    // important: create here and pass in as an arg instead of declaring
    // and initializing new `aux` in each recursion to avoid cost
    aux = new Comparable[arr.length];

    // sort
    sort(arr, aux, 0, arr.length-1);
}
```

**Time complexity:** [O(n log n)](https://softwareengineering.stackexchange.com/a/297203)

**Improvements**

1. use insertion sort for tiny subarrays

    ```java
    if (hi - lo < CUTOFF) {
        Insertion.sort(arr, lo, hi);
        return;
    }
    ```
2. return if already sorted (last element in first half <= first in second half)