+++
title = "Angular FormatDate基本使用"
date = 2020-10-24T14:24:16+08:00
description = "Angular 時間格式的小工具"
tags = ["Angular", "筆記"]
categories = ["Angular"]
draft = false
+++

<!--more-->
---

# Angular formatDate

## 困境
目前在沒有`momonent.js`或各類第三方時間套件的輔助下，完美快速處理時間相關的格式轉換或時區問題。

### 主要問題
* 時區問題

![](https://i.imgur.com/1R4Vwhd.png)

在不同瀏覽器的時間格式如果沒有特別嚴謹，可能導致時區錯誤。

* 時間格式

![](https://i.imgur.com/YMabgAb.png)

各種時間格式轉換。


## formatDate

```typescript=
import { formatDate } from '@angular/common';
```

此函式是在`Angular6`出現的，其實也就是`Date pipe`內使用的函式，


### 時間格式

時間格式其實就是跟`Date Pipe`格式設定方式一樣，雖然根據Angular官方站寫可參照`ISO date-time string`，但其實格式似乎有差異，像是`YYYY-MM-DD (eg 1997-07-16)`，在`formatDate`是無法使用的。

![](https://i.imgur.com/w0dEhme.png)

#### 使用
```typescript=
const date = new Date();
formatDate(date, 'YYYY-MM-DD', 'zh-Hant-TW'); // YYYY-10-DD ISO格式部分無法
formatDate(date, 'mm:ss', 'zh-Hant-TW');  // 57:09
formatDate(date, 'yyyy-MM-dd', 'zh-Hant-TW'); // 2020-10-24
formatDate(date, 'shortTime', 'zh-Hant-TW'); // 下午1:57
```

那`Angular`的時間格式就是`Date Pipe`的文件囉！
https://angular.io/api/common/DatePipe

```typescript=
const date = new Date();
formatDate(date, 'full', 'zh-Hant-TW', '+0400'); // 2020年10月24日 星期六 上午10:16:54 [GMT+04:00]
formatDate(date, 'full', 'zh-Hant-TW'); // 2020年10月24日 星期六 下午2:16:54 [GMT+08:00]
```

#### 時區
調整時區也就
![](https://i.imgur.com/UgaQQRY.png)


### i18n
時間格式不外乎就是跟地區性有關，`Angular`已經具備`i18n`相關功能，其實`formatDate`也是位於`i18n`範圍內。預設`Angular`有`en_US`這`locale`。

![](https://i.imgur.com/myxl0Do.png)
`locale`其實就是區域代碼，預設是沒有`zh-Hant-TW`，這是自行在模組內註冊。

```typescript=
import { registerLocaleData } from '@angular/common';
import zhHant from '@angular/common/locales/zh-Hant';
registerLocaleData(zhHant, 'zh-Hant-TW');
```

而設定區域代碼對應的物件其實就是各區域習慣的`時間格式`, `貨幣`, `星期格式`等等，都是依些格式上的地區分別，也會提供該地區習慣的`時間格式`。


## 參考
[DatePipe](https://angular.io/api/common/DatePipe)
[FormatDate](https://angular.io/api/common/formatDate)


