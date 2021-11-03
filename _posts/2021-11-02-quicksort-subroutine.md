---
layout: post
title:  "An important subroutine used by quicksort"
summary: "Quicksort is an in-place sorting algorithm that uses an important subroutine for partitioning"
excerpt: "Quicksort is an in-place sorting algorithm that uses an important subroutine for partitioning"
date:   2021-11-02 06:00:00 -0700
tags: algorithm, sorting
published: true
mathjax: true
image: /assets/quicksort-partition.png
---

Quicksort is an in-place sorting algorithm that uses an important subroutine for partitioning.  The following JavaScript code implements quicksort.

```javascript
const partition = require('./partition');

const quicksort = (arr, p, r) => {
    if (p < r) {
        const q = partition(arr, p, r);
        quicksort(arr, p, q - 1);
        quicksort(arr, q + 1, r);
    }
}

module.exports = quicksort;
```

This algorithm uses the divide-and-conquer approach, which **divides** the original problem into multiple problems that are similar but smaller, **conquers** these sub-problems by solving them recursively, and then combines all the solutions to the sub-problems to create a solution to the original problem.

Quicksort uses the `partition` subroutine to divide the original problem.  It computes the index `q` and rearranges the array `arr[p..r]` such that all the values in the array with a smaller index than `q`, the `arr[p..q-1]` sub-array, are less than or equal to the value `arr[q]`.  On the other hand, all the values that have an index greater than `q`, the `arr[q+1..r]` sub-array, are greater than `arr[q]`.  Here is an implementation of this subroutine.

**partition.js**
```javascript
module.exports = (arr, p, r) => {
    let i = p - 1;
    for (let j = p; j < r; j++) {
        if (arr[j] <= arr[r]) {
            i += 1;
            exchange(arr, i, j);
        }
    }
    exchange(arr, i + 1, r);
    return i + 1;
}

// helper function
const exchange = (arr, i, j) => {
    const temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

After the division comes the conquest.  So, the sub-arrays `arr[p..q-1]` and `arr[q+1..r]` are then sorted by recursive calls to quicksort.  Since everything was done in place, the entire array `arr[p..r]` is sorted after these recursive calls and no additional work is needed to combine all the solutions to the sub-problems.

