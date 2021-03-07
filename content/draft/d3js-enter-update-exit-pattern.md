+++
title = "D3js Enter Update Exit Pattern"
date = 2021-01-09T21:21:04+08:00
tags = ["d3js"]
categories = ["筆記"]
draft = false
description = "d3js強大的Enter, Update, Exit，同步資料與畫面。"
+++

<!--more-->
---

# 簡介 Enter、Update、Exit Pattern

`d3js`在處理資料`新增`、`更新`、`刪除`時，提供了幾個API，讓開發者更容易同步資料與畫面。

以下是官方列出的API

* *selection*.data - 綁定元素至資料
* *selection*.join - 根據資料 enter, update 或 exit
* *selection*.enter - 取得enter selection (資料缺少對應的元素, 資料 > 元素)
* *selection*.exit - 取得exit selection (元素缺少對應的資料, 元素 > 資料)
* *selection*.datum - 取得或設定元素的資料

`selection.join`其實就是整合三個API產生出來比較方便使用的API。

```
.data(data, key)
```

