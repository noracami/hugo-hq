---
title: "LeetCode Biweekly Contest 89"
date: 2022-10-03T01:21:46+08:00
draft: false
categories: ["LeetCode"]
tags: ["Ruby"]
author: "Me"
showToc: true
TocOpen: false
hidemeta: false
comments: false
description: "LeetCode Biweekly Contest 89"
canonicalURL: "https://noracami.github.io/posts/leetcode_biweekly_contest_89"
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
---

[Biweekly Contest 89](https://leetcode.com/contest/biweekly-contest-88/)

`2423. Remove Letter To Equalize Frequency`

> You are given a 0-indexed string word, consisting of lowercase English letters. You need to select one index and remove the letter at that index from word so that the frequency of every letter present in word is equal.
>
> Return true if it is possible to remove one letter so that the frequency of all letters in word are equal, and false otherwise.
>
> Note:
>
> The frequency of a letter x is the number of times it occurs in the string.
> You must remove exactly one letter and cannot chose to do nothing.
>
> Constraints:
>
> 2 <= word.length <= 100
> word consists of lowercase English letters only.

## 題目：給定一個字串(26 個英文字母小寫組成)，如果可以移除其中一個元素，使得該字串其他字母出現次數相等，則回傳 `true`，否則回傳 `false`

### 條件

- 字串長度 2..100
- 元素僅會出現 a..z

### 想法

將字串轉成字元陣列，從頭開始遍歷，計算整個陣列不含當前元素，每個字元出現次數，如果次數相等，則回傳 `true`

1. each_char 轉成陣列
2. 扣除當前元素，用 hash 計算字母出現次數
3. 取出 hash 的所有 value，去除零後如果只剩一種數字，即為次數相等，回傳 `true`
4. 迴圈跑完未回傳 `true`，回傳 `false`

時間複雜度：O(n^2)

### 實作

```ruby
# @param {String} word
# @return {Boolean}
def equal_frequency(word)
  word.each_char do |char|
    frequency = { char => -1 }
    word.each_char do |c|
      if frequency[c]
        frequency[c] += 1
      else
        frequency[c] = 1
      end
    end
    return true if frequency.values.reject(&:zero?).uniq.size == 1
  end
  false
end
```

### 改進

1. 先求出所有字母出現次數 -> Array(frequency_of_letter)
2. 判斷 次數-1 後, 有無符合題目要求

時間複雜度：O(n^2)

```ruby
def equal_frequency(word)
  frequency = 'a'.upto('z').map { |char| word.count(char) }.reject(&:zero?)
  frequency.each_index do |i|
    frequency[i] -= 1
    return true if frequency.reject(&:zero?).uniq.size == 1

    frequency[i] += 1
  end
  false
end
```

3. 次數求出後，分不同情境判斷
   1. `次數 >= 3 種 ==>> false`
   2. `次數 == 1 種 ==>> 等於 1 => true, 只有一種字母 => true, 其他 false`
   3. `次數 == 2 種 ==>> 分別 -1，判斷符不符合題目要求`

時間複雜度：O(n)

```ruby
def equal_frequency(word)
  frequency = 'a'.upto('z').map { |char| word.count(char) }.reject(&:zero?)
  return true if frequency.size == 1
  return true if frequency.uniq.size == 1 && frequency.uniq.last == 1
  return false if frequency.uniq.size >= 3

  frequency.sort!
  frequency[0] -= 1
  return true if frequency.reject(&:zero?).uniq.size == 1

  frequency[0] += 1
  frequency[-1] -= 1
  return true if frequency.reject(&:zero?).uniq.size == 1

  false
end
```

> ### 附註：想這題就花掉所有時間，而且還沒 AC...
