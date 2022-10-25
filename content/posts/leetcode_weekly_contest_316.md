---
title: "LeetCode Weekly Contest 316"
date: 2022-10-25T13:00:00+08:00
draft: false
categories: ["LeetCode"]
tags: ["Ruby"]
author: "Me"
showToc: true
TocOpen: false
hidemeta: false
comments: false
description: "LeetCode Weekly Contest 316"
canonicalURL: "https://noracami.github.io/posts/leetcode_weekly_contest_316"
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
editPost:
  URL: "https://github.com/noracami/hugo-hq/blob/main/content"
  Text: "Suggest Changes" # edit text
  appendFilePath: true # to append file path to Edit link
---

# Weekly Contest 316

###### tags: `leetcode` `contest`

[Weekly Contest 316](https://leetcode.com/contest/weekly-contest-316/)

| Problem list                                                                                                                                                        | Score |     |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----- | --- |
| [Determine if Two Events Have Conflict](https://leetcode.com/contest/weekly-contest-316/problems/determine-if-two-events-have-conflict)                             | 3     | ✅  |
| [Number of Subarrays With GCD Equal to K](https://leetcode.com/contest/weekly-contest-316/problems/number-of-subarrays-with-gcd-equal-to-k)                         | 4     | ✅  |
| [Minimum Cost to Make Array EqualConflict](https://leetcode.com/contest/weekly-contest-316/problems/minimum-cost-to-make-array-equal)                               | 6     |
| [Minimum Number of Operations to Make Arrays Similar](https://leetcode.com/contest/weekly-contest-316/problems/minimum-number-of-operations-to-make-arrays-similar) | 6     |

## `2446. Determine if Two Events Have Conflict`

You are given two arrays of strings that represent two inclusive events that happened **on the same day**, `event1` and `event2`, where:

- `event1 = [startTime1, endTime1]` and
- `event2 = [startTime2, endTime2]`.

Event times are valid 24 hours format in the form of `HH:MM`.

A **conflict** happens when two events have some non-empty intersection i.e., some moment is common to both events).

Return `true` _if there is a conflict between two events. Otherwise, return_ `false`.

#### Example 1:

    Input: event1 = ["01:15","02:00"], event2 = ["02:00","03:00"]
    Output: true
    Explanation: The two events intersect at time 2:00.

#### Example 2:

    Input: event1 = ["01:00","02:00"], event2 = ["01:20","03:00"]
    Output: true
    Explanation: The two events intersect starting from 01:20 to 02:00.

#### Example 3:

    Input: event1 = ["10:00","11:00"], event2 = ["14:00","15:00"]
    Output: false
    Explanation: The two events do not intersect.

#### Constraints:

- `evnet1.length == event2.length == 2.`
- `event1[i].length == event2[i].length == 5`
- `startTime1 <= endTime1`
- `startTime2 <= endTime2`
- All the event times follow the `HH:MM` format.

給定兩個陣列，表示一場活動的時間，如果活動時間有重疊，回傳 `ture` ，否則回傳 `false`

### 條件：

- 兩場活動在同一天
- 活動期間是連續的
- 時間格式都是 `HH:MM`

### 想法：

將字串轉成數字（表示成由 `00:00` 起經過的分鐘數）

如果一場活動的結束在另一場活動的開始之前，那他們便不會重疊，回傳 `false`

反之回傳 `true`

時間複雜度：O(1)

### 實作

```ruby
# @param {String[]} event1
# @param {String[]} event2
# @return {Boolean}
def have_conflict(event1, event2)
  to_minutes = -> _ {
    _.split(':').map.with_index { |e, idx|
      idx == 0 ? 60 * e.to_i : e.to_i
    }.sum
  }
  !(event2.map(&to_minutes)[1] < event1.map(&to_minutes)[0] ||
    event1.map(&to_minutes)[1] < event2.map(&to_minutes)[0] )
end
```

> 附註：基礎題

---

## `2447. Number of Subarrays With GCD Equal to K`

Given an integer array `nums` and an integer `k`, _return the number of **subarrays** of_ `nums` _where the greatest common divisor of the subarray's elements is_ `k`.

A **subarrays** is a contiguous non-empty sequence of elements within an array.

The **greatest common divisor of an array** is the largest integer that evenly divides all the array elements.

#### Example 1:

    Input: nums = [9,3,1,2,6,3], k = 3
    Output: 4
    Explanation: The subarrays of nums where 3 is the greatest common divisor of all the subarray's elements are:
    - [9,3,1,2,6,3]
    - [9,3,1,2,6,3]
    - [9,3,1,2,6,3]
    - [9,3,1,2,6,3]

#### Example 2:

    Input: nums = [4], k = 7
    Output: 0
    Explanation: There are no subarrays of nums where 7 is the greatest common divisor of all the subarray's elements.

#### Constraints:

- `1 <= nums.length <= 1000`
- `1 <= nums[i], k <= 10^9`

給定一個一維整數陣列 `nums` 及整數 `k`，找出所有符合條件的 `nums` 子陣列個數，其中子陣列的最大公因數為 `k`

### 條件

- `1 <= 陣列長度 <= 1000`
- `1 <= 陣列元素、k <= 10^9`

### 想法

對陣列每個元素，從本身出發，列舉可能的子陣列，直到遇到元素無法被 `k` 整除

接著檢查子陣列的最大公因數是否為 `k`，累計符合條件的子陣列

時間複雜度 O(n^3)

### 實作

```ruby
# @param {Integer[]} nums
# @param {Integer} k
# @return {Integer}
def subarray_gcd(nums, k)
  ans = 0

  nums.each_index { |idx|
    i = nums[idx..].index { |e| e % k != 0} || 1000

    nums[idx, i].reduce(nums[idx]) { |memo, ele|
      ans += 1 if memo.gcd(ele) == k
      memo.gcd(ele)
    }
  }

  ans
end
```

### discuss

[[Python3] Brute Force + Early Stopping, Clean & Concise](https://leetcode.com/problems/number-of-subarrays-with-gcd-equal-to-k/discuss/2734224/)

> 附註：暴力解，基礎題?
