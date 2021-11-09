---
layout: post
title: "Two Awesome Binary Seach Problems"
subtitle: "Leetcode 1231 & Leetcode 2064"
author: "Sailorlqh"
date: 2021-11-07
header_img: /img/home-bg/15.jpg
catalog: true
tags:
  - Leetcode

---

# 1231. Divide Chocolate

Question link is [here](https://leetcode.com/problems/divide-chocolate/).

### Question

You have one chocolate bar that consists of some chunks. Each chunk has its own sweetness given by the array `sweetness`.

You want to share the chocolate with your `k` friends so you start cutting the chocolate bar into `k + 1` pieces using `k` cuts, each piece consists of some **consecutive** chunks.

Being generous, you will eat the piece with the **minimum total sweetness** and give the other pieces to your friends.

Find the **maximum total sweetness** of the piece you can get by cutting the chocolate bar optimally.

 

**Example 1:**

```
Input: sweetness = [1,2,3,4,5,6,7,8,9], k = 5
Output: 6
Explanation: You can divide the chocolate to [1,2,3], [4,5], [6], [7], [8], [9]
```

**Example 2:**

```
Input: sweetness = [5,6,7,8,9,1,2,3,4], k = 8
Output: 1
Explanation: There is only one way to cut the bar into 9 pieces.
```

**Example 3:**

```
Input: sweetness = [1,2,2,1,2,2,1,2,2], k = 2
Output: 5
Explanation: You can divide the chocolate to [1,2,2], [1,2,2], [1,2,2]
```

 

### Solution

Binary Search.

Our goal is to find the maximum total sweetness. And this value is the maximum value of the minimum value of all possible cutting plans.  So we just need to find the maximum value of these minmum values.

For the minimum value v, if we can divide the chocolate into k parts where k >= n+1 and each parts has sweetness >= v. This v would be a possible value. 

Though this v would be a valid value, i.e. there might not a part that has exactly v sweetness, but the result we are looking for must be a valid value. And it must be the greatest valid value. Because if there exists a valid value that is greater than the result value, than for the result value, there would be some left-over chocolet and we can add to this value. Which contradict with the assumption. Therefore, the max valid value would be our result.

There is one more step for this, since this valid value is in the range of [min(sweetness), sum(sweetness)//(K+1)], the fastest method to locate this value would be binary search.

So we have the following solution:

```python
class Solution:
    def maximizeSweetness(self, s: List[int], k: int) -> int:
        total = sum(s)
        left = min(s)
        right = total // (k+1)
        while left < right:
            mid = left + (right - left) // 2 + 1
            cnt = 0
            curSum = 0
            for each in s:
                curSum += each
                if curSum >= mid:
                    cnt += 1
                    curSum = 0
            if cnt >= k + 1:
                left = mid
            else:
                right = mid - 1
        return right
```
Time Complexity: $O(nlog(S/(K+1)))$ where n is the length of the chocolate, and S is the sweetness sum.

# 2064. Minimized Maximum of Products Distributed to Any Store 

Question link is [here](https://leetcode.com/contest/weekly-contest-266/problems/minimized-maximum-of-products-distributed-to-any-store/)

### Question

You are given an integer `n` indicating there are `n` specialty retail stores. There are `m` product types of varying amounts, which are given as a **0-indexed** integer array `quantities`, where `quantities[i]` represents the number of products of the `ith` product type.

You need to distribute **all products** to the retail stores following these rules:

- A store can only be given **at most one product type** but can be given **any** amount of it.
- After distribution, each store will be given some number of products (possibly `0`). Let `x` represent the maximum number of products given to any store. You want `x` to be as small as possible, i.e., you want to **minimize** the **maximum** number of products that are given to any store.

Return *the minimum possible* `x`.

 

**Example 1:**

```
Input: n = 6, quantities = [11,6]
Output: 3
Explanation: One optimal way is:
- The 11 products of type 0 are distributed to the first four stores in these amounts: 2, 3, 3, 3
- The 6 products of type 1 are distributed to the other two stores in these amounts: 3, 3
The maximum number of products given to any store is max(2, 3, 3, 3, 3, 3) = 3.
```

**Example 2:**

```
Input: n = 7, quantities = [15,10,10]
Output: 5
Explanation: One optimal way is:
- The 15 products of type 0 are distributed to the first three stores in these amounts: 5, 5, 5
- The 10 products of type 1 are distributed to the next two stores in these amounts: 5, 5
- The 10 products of type 2 are distributed to the last two stores in these amounts: 5, 5
The maximum number of products given to any store is max(5, 5, 5, 5, 5, 5, 5) = 5.
```

**Example 3:**

```
Input: n = 1, quantities = [100000]
Output: 100000
Explanation: The only optimal way is:
- The 100000 products of type 0 are distributed to the only store.
The maximum number of products given to any store is max(100000) = 100000.
```

### Solution

Binary Search

Similiar to the problem above, we are also trying the find some value in some range. 

This time, the value we are looking for is the minimized of maximum value. 

For each value v, we just need to find if all store stores at most v amount of product, how many stores we would need. And the result value is the minimum value we can find. 

```python
class Solution:
    def minimizedMaximum(self, n: int, quantities: List[int]) -> int:
        left = 1
        right = max(quantities)
        while left < right:
            mid = left + (right - left) // 2
            cnt = 0
            for q in quantities:
                cnt += (q + mid - 1) // mid
            if cnt > n:
                left = mid + 1
            else:
                right = mid
        return left
        
```

Time Complexity: $O(nlog(s))$ where n is the length of quantities, and s is the max value in quantities.