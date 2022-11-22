---
title: "LeetCode Biweekly Contest 90"
date: 2022-10-31T16:00:00+08:00
draft: false
categories: ["LeetCode"]
tags: ["Ruby"]
author: "Me"
showToc: true
TocOpen: false
hidemeta: false
comments: true
description: "LeetCode Biweekly Contest 90"
canonicalURL: "https://noracami.github.io/posts/leetcode_biweekly_contest_90"
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

# Biweekly Contest 90

###### tags: `leetcode` `contest`

[Biweekly Contest 90](https://leetcode.com/contest/biweekly-contest-90/)

| Problem list                                                                                                      | Score |     |
| ----------------------------------------------------------------------------------------------------------------- | ----- | --- |
| [2451. Odd String Difference](https://leetcode.com/problems/odd-string-difference/)                               | 3     | ✅  |
| [2452. Words Within Two Edits of Dictionary](https://leetcode.com/problems/words-within-two-edits-of-dictionary/) | 4     | ✅  |
| [2453. Destroy Sequential Targets](https://leetcode.com/problems/destroy-sequential-targets/)                     | 5     | ✅  |
| [2454. Next Greater Element IV](https://leetcode.com/problems/next-greater-element-iv/)                           | 6     |

## `2451. Odd String Difference`

You are given an array of equal-length strings `words`. Assume that the length of each string is `n`.

Each string `words[i]` can be converted into a **difference integer array** `difference[i]` of length `n - 1` where `difference[i][j] = words[i][j+1] - words[i][j]` where `0 <= j <= n - 2`. Note that the difference between two letters is the difference between their **positions** in the alphabet i.e. the position of `'a'` is `0`, `'b'` is `1`, and `'z'` is `25`.

- For example, for the string `"acb"`, the difference integer array is `[2 - 0, 1 - 2] = [2, -1]`.

All the strings in words have the same difference integer array, **except one**. You should find that string.

Return _the string in `words` that has different **difference integer array**_.

#### Example 1:

    Input: words = ["adc","wzy","abc"]
    Output: "abc"
    Explanation:
    - The difference integer array of "adc" is [3 - 0, 2 - 3] = [3, -1].
    - The difference integer array of "wzy" is [25 - 22, 24 - 25]= [3, -1].
    - The difference integer array of "abc" is [1 - 0, 2 - 1] = [1, 1].
    The odd array out is [1, 1], so we return the corresponding string, "abc".

#### Example 2:

    Input: words = ["aaa","bob","ccc","ddd"]
    Output: "bob"
    Explanation: All the integer arrays are [0, 0] except for "bob", which corresponds to [13, -13].

#### Constraints:

- `3 <= words.length <= 100`
- `n == words[i].length`
- `2 <= n <= 20`
- `words[i]` consists of lowercase English letters.

給定一個陣列，內含數個長度皆相同的字串，定義一個整數陣列，其元素由單字的每個字母，`'a' = 0`，`'b' = 1`，`'z' = 25`，以此類推，兩兩之間的差，組成該整數陣列。

對於所有的字串，恰好只有一個字串其對應的整數陣列與其他字串不同，請找出該字串。

#### 條件：

- 題目陣列長度 `3 <= words.length <= 100`
- 每個字串長度皆相同且 `2 <= n <= 20`
- 字串由英文小寫字母組成

### 想法：

將字母對映至數字

算出每個字串的**整數陣列**

找出不一樣的**整數陣列**

（此處用排序後比較第 1, 2 項 -> 相同回傳最後一項；不同回傳第一項）

回傳對應的字串

時間複雜度：

`最多 100 個字串 * 最多 20 個字母長 + 排序 n log n + 比較 1`

### 實作

```ruby
# @param {String[]} words
# @return {String}
def odd_string(words)
  diff = words.map { |w|
    w_points = w.codepoints
    l = w_points.size - 1
    w_points.map.with_index { |n, idx|
      idx != l ? w_points[idx+1] - n : nil
    }.compact
  }.map(&:join).zip words

  diff.sort! {|a, b| a[0] <=> b[0] }

  if diff[0][0] == diff[1][0]
    return diff.last[1]
  else
    return diff.first[1]
  end
end
```

### 後記

使用 `join` 會遇到假如 `[1, 11]` 跟 `[11, 1]` ，組合起來都是 `'111'`

像是 https://leetcode.com/contest/biweekly-contest-90/submissions/detail/832710717/

可以改用 `to_s` 轉字串 -> `'[1, 11]', '[11, 1]'`

或是直接對陣列比較

```ruby
def odd_string(words)
  diff = words.map { |w|
    w_points = w.codepoints
    l = w_points.size - 1
    w_points.map.with_index { |n, idx|
      idx != l ? w_points[idx + 1] - n : nil
    }.compact
  }.zip words

  diff.find { |e| diff.map(&:first).count(e[0]) == 1 }.last
end
```

> 心得：基礎題

---

## `2452. Words Within Two Edits of Dictionary`

You are given two string arrays, `queries` and `dictionary`. All words in each array comprise of lowercase English letters and have the same length.

In one **edit** you can take a word from `queries`, and change any letter in it to any other letter. Find all words from `queries` that, after a **maximum** of two edits, equal some word from `dictionary`.

Return _a list of all words from `queries`, that match with some word from `dictionary` after a maximum of **two edits**_. Return the words in the **same order** they appear in queries.

#### Example 1:

    Input: queries = ["word","note","ants","wood"], dictionary = ["wood","joke","moat"]
    Output: ["word","note","wood"]
    Explanation:
    - Changing the 'r' in "word" to 'o' allows it to equal the dictionary word "wood".
    - Changing the 'n' to 'j' and the 't' to 'k' in "note" changes it to "joke".
    - It would take more than 2 edits for "ants" to equal a dictionary word.
    - "wood" can remain unchanged (0 edits) and match the corresponding dictionary word.
    Thus, we return ["word","note","wood"].

#### Example 2:

    Input: queries = ["yes"], dictionary = ["not"]
    Output: []
    Explanation:
    Applying any two edits to "yes" cannot make it equal to "not". Thus, we return an empty array.

#### Constraints:

- `1 <= queries.length, dictionary.length <= 100`
- `n == queries[i].length == dictionary[j].length`
- `1 <= n <= 100`
- All `queries[i]` and `dictionary[j]` are composed of lowercase English letters.

給定一個陣列 `queries` 及另一個陣列 `dictionary` ，對於 `queries` 的元素，定義**一次編輯**為改變元素的其中一個字元（不變更順序）。

請回傳 `queries` 中，所有可經由兩次以內編輯，從而與 `dictionary` 其中一個元素 `MATCH` 的元素，並按照 `queries` 原本順序排列。

### 條件

- `1 <= 陣列長度 <= 100`
- `1 <= 單字長度 <= 100`
- 所有單字長度均相同
- 所有單字皆由英文字母小寫組成

### 想法

`queries` 與 `dictionary` 轉成數字

對每個 `queries` 的元素，一一與 `dictionary` 的元素比較，如果有找到一個元素，相異的字元數不超過二，紀錄為 `true` ，否則為 `false`

將結果對映至 `queries`

時間複雜度

最差情況：皆無符合條件的單字，每次均需遍歷 `dictionary` 一次，O(n^2)

### 實作

```ruby
# @param {String[]} queries
# @param {String[]} dictionary
# @return {String[]}
def two_edit_words(queries, dictionary)
  dict = dictionary.map(&:codepoints)
  que = queries.map(&:codepoints)
  que.map! { |w|
    dict.any? { |d|
      d.map.with_index { |_, i|
        w[i] - d[i]
      }.count { |_| !_.zero? } <= 2
    }
  }
  queries.zip(que).select{ |a, b| b }.map{ |a, b| a }
# queries.zip(que).map{|a, b| a if b }.compact
end
```

### discuss

[Trie vs. Hamming Distance](https://leetcode.com/problems/words-within-two-edits-of-dictionary/discuss/2756369/Trie-vs.-Hamming-Distance)

> 心得：暴力解，基礎題

---

## `2453. Destroy Sequential Targets`

You are given a **0-indexed** array `nums` consisting of positive integers, representing targets on a number line. You are also given an integer `space`.

You have a machine which can destroy targets. **Seeding** the machine with some `nums[i]` allows it to destroy all targets with values that can be represented as `nums[i] + c * space`, where `c` is any non-negative integer. You want to destroy the **maximum** number of targets in `nums`.

Return _the **minimum value** of `nums[i]` you can seed the machine with to destroy the maximum number of targets_.

#### Example 1:

    Input: nums = [3,7,8,1,1,5], space = 2
    Output: 1
    Explanation: If we seed the machine with nums[3], then we destroy all targets equal to 1,3,5,7,9,...
    In this case, we would destroy 5 total targets (all except for nums[2]).
    It is impossible to destroy more than 5 targets, so we return nums[3].

#### Example 2:

    Input: nums = [1,3,5,2,4,6], space = 2
    Output: 1
    Explanation: Seeding the machine with nums[0], or nums[3] destroys 3 targets.
    It is not possible to destroy more than 3 targets.
    Since nums[0] is the minimal integer that can destroy 3 targets, we return 1.

#### Example 3:

    Input: nums = [6,2,5], space = 100
    Output: 2
    Explanation: Whatever initial seed we select, we can only destroy 1 target. The minimal seed is nums[1].

#### Constraints:

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 109`
- `1 <= space <= 109`

給定一個正整數陣列 `nums` 及正整數 `space` ，現在有一個程式，當你指定 `nums` 其中一個元素的值為 `seed` 時，它會消除所有在 `nums` 中且值為 `seed + N * space, N為整數且 N≥0` 的元素。

請找出能消除最多元素的 `seed` ，若有多個符合，回傳最小的值。

### 條件

- `1 <= 陣列長度 <= 10^5`
- `1 <= 數值大小 <= 10^9`
- `1 <= 間隔大小 <= 10^9`

### 想法

選定 `seed` 後，被影響的數字相差 `space`

可用 `mod` 將數字分群

找出最大群的最小的數字，即為所要的 `seed`

> \*先將陣列排序後再分群，如此比較小的數字便會先放在前面，遇到消除數字個數相同直接選先儲存的紀錄即可

時間複雜度 O(n log n)

### 實作

```ruby
# @param {Integer[]} nums
# @param {Integer} space
# @return {Integer}
def destroy_targets(nums, space)
  slots = {}
  nums.sort!
  nums.each { |n|
    if slots[(n % space).to_s]
      slots[(n % space).to_s].push n
    else
      slots[(n % space).to_s] = [n]
    end
  }
  slots.max_by { |key, elements| elements.size }[1][0]
end
```

### discuss

[Trie vs. Hamming Distance](https://leetcode.com/problems/words-within-two-edits-of-dictionary/discuss/2756369/Trie-vs.-Hamming-Distance)

> 心得：同餘，基礎題吧 XD
