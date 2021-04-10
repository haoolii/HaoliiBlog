+++
title = "Azure 誤刪 Storage Account 救援以及防範"
date = 2021-04-10T15:50:01+08:00
tags = ["Azure"]
categories = ["Azure"]
draft = false
description = "神智不清下不小心誤刪 Storage Account 的災後檢討以及防範。"
+++

<!--more-->
---

## 情境敘述

事情發生在正在準備建立展示用的`Storage Account`，用以部署靜態前端網站。在看到
某些測試用的 `Storage Account` 殘存在列表上，相當不順眼，因此舉手之勞的將無用
到的都刪除。

當下我執行的動作也就是，在多選的情況下，選擇並刪除。
![](/images/2021-04-10-170142.png)

在多選的情況下，僅輸入`yes`就可以刪除該資源。
![](/images/2021-04-10-170735.png)

就是因為在刪除輸入`yes`的情況下，誤刪了該`Storage Account`，而且還是正式環境。
最後發現資源誤刪還是在客戶反映網站有異之後，才驚覺可能自己誤刪了。

## 責任歸屬

在客戶反應後，確認資源群組`Resource Group`，已經發現`Storage Account`確實已經不存在，
因此這時候要追蹤最近時間`Resource Group`內的資源，發生甚麼變動，在最短時間內確定發生事件
以及執行者，追蹤責任歸屬。

進入`Resource Group`並查看`Activity Log`。
![](/images/2021-04-10-171822.png)

可以看出`Delete`是在五分鐘前被`unnhao@gmail.com`刪除的。
![](/images/2021-04-10-171823.png)

確認責任歸屬後，就該進行回覆及搶救。

## 災難恢復

根據官方文件 [Recover a deleted storage account](https://docs.microsoft.com/zh-tw/azure/storage/common/storage-account-recover) 說明，
想要恢復已刪除`Storage Account`只要至同一資源群組的`Storage Account`，並選擇`Recover deleted account`選單內即可看到刪除14天內的`Storage Account`了。

> 如果沒有同資源群組沒有`Storage Account`，馬上建立一個即可，不要選擇相同名稱，會造成覆蓋原始`Storage Account`

![](/images/2021-04-10-172702.png)

還有其他方式是請求微軟協助，但其實也是差不多限制以及方式。

## 防範措施

### 刪除時的習慣

盡量不要選擇使用多選，僅輸入`yes`容易在未確認的狀況下執行刪除動作。

進入該資源進行單一刪除，需要輸入該資源名稱，確認後才能執行刪除，較為安全。
![](/images/2021-04-10-174806.png)

### 使用`Lock`並鎖定該`Storage Account`

選擇`Locks`選項，並進入建立`Lock`
![](/images/2021-04-10-173924.png)

可以選擇`Read-only`或`Delete`，看是否鎖住編輯與刪除，或是僅有刪除。
![](/images/2021-04-10-174106.png)

再次刪除時，就會發現無法刪除。
![](/images/2021-04-10-174506.png)


### 使用`Lock`並鎖定整個`Resource Group`

同理鎖定單一資源，鎖定整個`Resource Group`可使該資源群組下所有資源(包含本身)都無法被刪除。


## 注意事項

* 刪除並恢復之`Storage Account`並不保證可以完整恢復。
* 僅有刪除後14天內可以恢復。
* 刪除後不要嘗試建立相同名稱，會導致無法使用`Azure Portal`介面直接恢復，被新的`Storage Account`覆蓋。
* 僅能拯救刪除`Storage Account`資源，刪除`Resource Group`請自求多福。

## 結論

有`Azure`真幸福，讓我保住了我的工作。