---
title: "Install Hugo"
date: 2022-07-30T20:06:44+08:00
# weight: 1
# aliases: ["/first"]
categories: ["Web"]
tags: ["Hugo"]
author: "Me"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "第一篇先談如何安裝 Hugo"
canonicalURL: "https://noracami.github.io/posts/install-hugo"
# disableHLJS: true # to disable highlightjs
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
cover:
  image: "<image path/url>" # image path/url
  alt: "<alt text>" # alt text
  caption: "<text>" # display caption under cover
  relative: false # when using page bundles set this to true
  hidden: true # only hide on current single page
editPost:
  URL: "https://github.com/noracami/hugo-hq/blob/main/content"
  Text: "Suggest Changes" # edit text
  appendFilePath: true # to append file path to Edit link
---

## 安裝 Hugo

### 環境

```shell
# (wsl)
# Distributor ID: Ubuntu
# Description:    Ubuntu 20.04.4 LTS
# Release:        20.04
# Codename:       focal
```

### 下載

```shell
$ wget https://github.com/gohugoio/hugo/releases/download/v0.101.0/hugo_0.101.0_Linux-64bit.deb
$ wget https://github.com/gohugoio/hugo/releases/download/v0.101.0/hugo_extended_0.101.0_Linux-64bit.deb
```

### 安裝

```shell
$ sudo dpkg -i hugo_0.101.0_Linux-64bit.deb
$ sudo dpkg -i hugo_extended_0.101.0_Linux-64bit.deb
```

### 檢查是否成功

```shell
$ hugo version
hugo v0.101.0-466fa43c16709b4483689930a4f9ac8add5c9f66+extended linux/amd64 BuildDate=2022-06-16T07:09:16Z VendorInfo=gohugoio

$ hugo new site {project_name}
$ cd {project_name}
$ hugo server
```

### 安裝佈景主題 (use [PaperMod](https://github.com/adityatelange/hugo-PaperMod/))

```shell
# in {project_name} root
$ git init
$ git clone https://github.com/adityatelange/hugo-PaperMod themes/PaperMod --depth=1
```

## 輸出 (上傳至 GitHub 為例)

```shell
# 產生靜態檔案
$ hugo

# 移出去
$ cp -r public ../
$ cd ../public
$ git init

# GitHub 開專案 名稱為 {你的GitHub ID}.github.io

# 新增遠端
$ git remote add origin https://github.com/{你的GitHub ID}/{你的GitHub ID}.github.io.git

# 推送
$ git push origin main
```

你的部落格就會在 `www.{ID}.github.io` 啦

##### ref: https://mark-weng.com/posts/hugo-setup/
