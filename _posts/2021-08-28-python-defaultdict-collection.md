---
layout: post
title: "Python defaultdict collection"
date: 2021-08-28 18:00:00 -0700
tags: python, leetcode, arrays, two_sum
published: true
---

In python, `defaultdict` and `dict` are almost the same except that the former never raises a `KeyError`.  Instead, it provides a default value for the key that does not exist.  In the code below, `seen` is a `defaultdict` that has `list[int]` as its default value.

```python
from typing import List
from collections import defaultdict 

def two_sum(nums: List[int], target: int) -> List[int]:
    seen = defaultdict(list[int])
    for j in range(len(nums)):
        complement = target - nums[j]
        if complement in seen:
            return [seen[complement][0],j]
        seen[nums[j]].append(j)
    return []
```

On the other hand, using `dict` would look something like this.

```python
from typing import List

def two_sum(nums: List[int], target: int) -> List[int]:
    seen = dict()
    for j in range(len(nums)):
        complement = target - nums[j]
        if complement in seen:
            return [seen[complement][0], j]
        if nums[j] in seen:
            seen[nums[j]].append(j)
        else:
            seen[nums[j]] = [j] 
    return []
```

I like the first implementation better for its conciseness and human readability.
