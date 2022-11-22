---
title: "when_to_use_after_commit_with_active_record"
date: 2022-10-09T23:03:57+08:00
draft: false
categories: ["Rails"]
tags: ["Active Record Callback", "Active Record"]
author: "Me"
showToc: true
TocOpen: false
hidemeta: false
comments: true
description: "[Rails] after_commit, 何時會用到它"
canonicalURL: "https://noracami.github.io/posts/when_to_use_after_commit_with_active_record"
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

`after_commit` 是一種 `Actice Record` 的 `Callback`

當我們透過 `Active Record` 與資料庫互動時，有許多的時機點可以觸發 `Active Record Callback`，對資料做檢查或是儲存後的其他操作.

## 使用情境

舉一個之前專案的例子，在網站上有許多的組織以及其中的成員，當我們新增成員加入組織時，我們需要寄信通知組織的管理員（有成員加入組織等等...)

我們可以在 `model.rb` 中利用 `after_save` 去執行寄信，當成員成功加入組織後(`user.save`)，執行寄信程式，儘管看起來沒問題，寄信程式的確會在 `user.save` 成功後執行。

但從資料庫的角度來看，如果新增成員後還有其他操作，一旦失敗，資料庫便會將整個 `transaction` 退回，即 `ACID` 中的 `Atomicity` （全部成功、或全部失敗）

而這時背景執行的寄信程式，便無法正確執行

因此，我們可以改用 `after_commit callback` ，確保資料庫 `transation` 成功 `commit` 後，才呼叫寄信程式寄信。

## reference

https://blog1.westagilelabs.com/when-to-use-after-commit-in-rails-f5e53a22bb9

https://guides.rubyonrails.org/v6.1/active_record_callbacks.html
