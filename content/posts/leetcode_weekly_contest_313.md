---
title: "LeetCode Weekly Contest 313"
date: 2022-10-03T18:05:11+08:00
draft: false
categories: ["LeetCode"]
tags: ["Ruby"]
author: "Me"
showToc: true
TocOpen: false
hidemeta: false
comments: true
description: "LeetCode Weekly Contest 313"
canonicalURL: "https://noracami.github.io/posts/leetcode_weekly_contest_313"
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

[Weekly Contest 313](https://leetcode.com/contest/weekly-contest-313/)

## `2427. Number of Common Factors`

> Given two positive integers a and b, return the number of common factors of a and b.
>
> An integer x is a common factor of a and b if x divides both a and b.
>
> Constraints:
>
> `1 <= a, b <= 1000`

給定正整數`a, b`，回傳他們的公因數的數量

### 條件：

- `1 <= a, b <= 1000`

### 想法：

x 從 1 開始到 min(a,b)，如果 a 可以被 x 整除 且 b 可以被 x 整除，x 即為 (a,b) 的公因數

時間複雜度：O(n)

### 實作

```ruby
# @param {Integer} a
# @param {Integer} b
# @return {Integer}
def common_factors(a, b)
  result = 0
  1.upto([a, b].min) do |x|
    result += 1 if (a % x).zero? && (b % x).zero?
  end
  result
end
```

### 改進：

1. 呼叫 gcd 找出最大公因數，再計算他的因數個數

時間複雜度：O(n)

```ruby
def common_factors(a, b)
  [*1..a.gcd(b)].filter { |x| (a % x + b % x).zero? }.size
end
```

或使用 `count` 並傳入 `block`

```ruby
def common_factors(a, b)
  1.upto(a.gcd(b)).count { |x| (a % x + b % x).zero? }
end
```

> 附註：基礎題

## `2428. Maximum Sum of an Hourglass`

> You are given an m x n integer matrix grid.
>
> We define an hourglass as a part of the matrix with the following form:
> ![](https://assets.leetcode.com/uploads/2022/08/21/img.jpg)
> Return the maximum sum of the elements of an hourglass.
>
> Note that an hourglass cannot be rotated and must be entirely contained within the matrix.
>
> Constraints:
>
> `m == grid.length`
>
> `n == grid[i].length`
>
> `3 <= m, n <= 150`
>
> `0 <= grid[i][j] <= 106`

給定一個 m x n 的二維數字陣列，定義一個圖案（ hourglass, 沙漏）如圖示，找出圖案覆蓋的最大數字和

### 條件

- 圖案不能翻轉，且不能超出範圍
- `3 <= 邊長 <= 150`
- `0 <= 數值 <= 1_000_000`

### 想法

依照題目要求窮舉所有圖案，並計算數字和

時間複雜度 O(n^2)

### 實作

```ruby
# @param {Integer[][]} grid
# @return {Integer}
def max_sum(grid)
  v = 0
  j_len = grid.size - 3
  0.upto(grid[0].size - 3).with_index do |_, i|
    0.upto(j_len).with_index do |_, j|
      v = [
        grid[j][i] + grid[j][i + 1] + grid[j][i + 2] + grid[j + 1][i + 1] + grid[j + 2][i] + grid[j + 2][i + 1] + grid[j + 2][i + 2], v
      ].max
    end
  end
  v
end

p max_sum([[6, 2, 1, 3], [4, 2, 1, 5], [9, 2, 8, 7], [4, 1, 2, 9]])
```

### 改進

```ruby
def max_sum(grid)
  v = 0
  j_len = grid.size - 3
  0.upto(grid[0].size - 3) do |i|
    0.upto(j_len) do |j|
      v = [v, [grid[j][i, 3], grid[j + 1][i + 1], grid[j + 2][i, 3]].flatten.sum].max
    end
  end
  v
end
```

> 附註：暴力解，基礎題
