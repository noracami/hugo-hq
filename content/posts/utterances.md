---
title: "Add utterances to Hugo"
date: 2022-11-23T07:00:00+08:00
draft: false
categories: ["Web"]
tags: ["Hugo", "Comments"]
author: "Me"
showToc: true
TocOpen: false
hidemeta: false
comments: true
description: "為你的部落格加上留言回應功能吧！"
canonicalURL: "https://noracami.github.io/posts/utterances"
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
Summary: 為你的部落格加上留言回應功能吧！
---

## 情境需求

- 身為一個部落格可以留言是必須的吧
- Hugo 作為靜態網頁框架，串接第三方服務可以省去管理會員註冊登入、留言資料存放的問題

## 方案

Hugo 官網已有提供多種[解決方案](https://gohugo.io/content-management/comments/)

預設推薦你使用 Disqus

其中有些需要付費使用

有些有 open source 版本可以自建服務使用 [Commento](https://gitlab.com/commento/commento)、[Talkyard](https://github.com/debiki/talkyard-prod-one)

這兩個是我接下來有空會想嘗試的 [Remark42](https://remark42.com/)、[Cactus Comments](https://cactus.chat/docs/integrations/hugo/)

---

### 最後，我選擇了 [utterances](https://utteranc.es/)

透過串接 GitHub 服務，結合 Issue tracking 與文章留言

優點：

- 使用 GitHub 帳號登入作身份驗證
- 支援原本 Issue 的功能，如 Markdown 語法、可重複編輯(及保存歷史紀錄)、Syntax Highlight
- 輕量，引入 JS 模組及建立一個公開的 Repo 即可

缺點：

- 本質上是建立 Issue，因此只接受 GitHub 登入，不支援匿名留言
- 留言沒有排序功能
- @標註、回覆其中一則留言無法直接達成（可藉由前往該 GitHub Issue 留言來達成）
- 文章列表處無法顯示留言數
- 明暗主題切換時留言區無法跟著切換

## 安裝步驟

1. 建立一個公開的 Repo、用來儲存文章留言，並授權[utterances](https://github.com/apps/utterances)管理該 Repo（只需選擇這一個即可）
2. 選擇對應方法，沒有特別需求的話選擇 `Issue title contains page pathname` 即可
3. （_非必要_）選擇自訂標籤
4. 選擇樣式主題
5. 在 themes/{your_mod}/layouts 下找到合適的檔案，放入產生的 JS code，以目前我使用的 [PaperMod](https://github.com/adityatelange/hugo-PaperMod/wiki/Features#comments) 是放在 `layouts/partials/comments.html`

## 後續

預計先解決 [明暗主題切換](https://github.com/utterance/utterances/issues/427) 的問題，再來嘗試解決顯示留言數的部分，應該可以透過 留言 > 觸發 Issue > **觸發更新留言數** > (GitHub Action)更新網站

如有疑問也歡迎在下方留言
