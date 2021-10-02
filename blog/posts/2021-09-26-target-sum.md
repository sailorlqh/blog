---
layout: post
title: "Target Sum"
subtitle: "Given an array and a target, can a subarray has sum equals target"
author: "Sailorlqh"
date: 2021-09-26
header_img: /img/home-bg/12.jpg
catalog: true
tags:
  - Self-thinking
---

# Target Sum
This is one question one of my friends encounters the other day. We discussed it, and I post my solution here.

The problem is that, when given an array, and a target, and a subarray has the sum equals to target. If there exists one, return 1, otherwise -1.

**Example 1:**

```
Input: nums = [1,2,4,5], target = 7
Output: 1
Explanation:
2 + 5 = 7
```

**Example 2:**

```
Input: nums = [2,4,6], target = 7
Output: -1
```

#### Solution

##### Recursion

My first solution is to use recursion, but it's obvious not the optimal solution, since the time complexity would be $O(2^n)$, but I did it anyway.

```java
public class Solution {

    public boolean recursive(int[] nums, int index, int target) {
        if(index >= nums.length)
            return false;
        if(nums[index] == target)
            return true;
        return recursive(nums, index+1, target) || recursive(nums, index+1, target-nums[index]);
    }

    public int helper_recursive(int []nums, int target){
        if (recursive(nums, 0, target))
            return 1;
        else
            return -1;
    }
}
```

Time Complexity: $O(2^n)$

##### Dynamic Programming

The previous solution is obvious not the optimal solution, there must be another solution. So I thought DP would be a good solution.

My idea was when use $dp[i][j]$ to show that if the first i element can form a sum of j.

So we have the transition equation here:

$dp[i][j] = 1, if\ nums[i] == j$

$dp[i][j] = 1, if\ dp[i-1][j-nums[i]] == 1$

$dp[i][j] = 0, otherwise$

And when simply return 1 if $dp[length-1][target] == 1$.

The code is shown below:

```java
public class Solution {
    public int helper(int[] nums, int target) {
        int length = nums.length;
        int[][] dp = new int[length][target+1];
        for(int i = 0; i < length; i++) {
            if(i == 0) {
                dp[i][nums[i]] = 1;
                continue;
            }
            for(int j = 0; j <= target; j++) {
                if(dp[i-1][j] == 1 || (j - nums[i] >= 0 && dp[i-1][j-nums[i]] == 1) || nums[i] == j)
                    dp[i][j] = 1;
            }
        }
    if(dp[length-1][target] == 1)
        return 1;
    else
        return -1;
}
```

Time Complexity $O(length*target) \approx O(n^2)$

****