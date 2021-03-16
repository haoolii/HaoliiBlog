+++
title = "Angular Material CDK 自定義 Dialog"
date = 2021-03-16T17:27:01+08:00
tags = ["Angular", "Angular Material CDK", "筆記"]
categories = ["Angular", "Angular Material"]
draft = false
description = "使用Angular Material CDK 快速開發自定義的Dialog"
+++

<!--more-->
---

## 使用情境
遇到需客製化自定義Dialog時，也許只是簡單的幾行文字或是按鈕，但很可能會遇到一些需要自己Handle的狀況。

1. Dialog位置
2. Dialog背景事件阻擋
3. Component或Template載入流程

以上情況或是沒有想到的事件，CDK也許都已經準備好了，因此直接使用現成lib最安全，也最快速。

## CDK 與 Dialog 的交織
CDK中會使用到於Dialog中的有。

1. Overlay : 用於定位元件的定位，可能是相黏於某一個DOM元素，或是整覆蓋於頁面中心。Dropdown或是Menu也可以使用此部件。
2. Portal: 用於動態產生Component，並透過Overlay放置在畫面某一位置。

## 實際步驟

1. 安裝`@angular/cdk`
```
yarn add @angular/cdk
```

2. 引入 Overlay, Portal Module
```typescript
// 省略...

import { OverlayModule } from "@angular/cdk/overlay";
import { PortalModule } from "@angular/cdk/portal";

@NgModule({
  imports: [BrowserModule, OverlayModule, PortalModule],
  declarations: [AppComponent, HelloComponent, ModalComponent],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

3. 事件觸發，定位、Overlay設定、動態產生組件

```typescript
// 省略...
export class AppComponent {

  constructor(private overlay: Overlay, private injector: Injector) {}

  showModal() {
    // 定位策略，甚至可以指定定位黏貼於某一DOM
    const strategy = this.overlay
      .position()
      .global()
      .centerHorizontally()
      .centerVertically();

    // Overlay設定，是否有背景，及設定定位策略
    const config = new OverlayConfig({
      hasBackdrop: true,
      backdropClass: "cdk-overlay-dark-backdrop",
      positionStrategy: strategy
    });

    // 使用剛剛建立的Config建立一個OverlayRef
    const overlayRef = this.overlay.create(config);

    // 建立一個Injector，父層是此AppComponent的Injector，並多新增一個Provider，並帶上overlayRef
    // 如此以來彈出的Component可以透過DI取得OverlayRef並關閉視窗。
    const injector = Injector.create({
      parent: this.injector,
      providers: [{ provide: OverlayRef, useValue: overlayRef }]
    });

    // 動態建立Component並給予injector，並將該Component放置於Overlay上。
    overlayRef.attach(new ComponentPortal(ModalComponent, null, injector));
  }
}

// 省略...
```

4. Dialog元件關閉事件。

```typescript
// 省略...
export class ModalComponent implements OnInit {
  // 透過Di的方式取得OverlayRef
  constructor(private overLayRef: OverlayRef) {}

  ngOnInit() {}

  // 即可透過detach關閉該Overlay
  close() {
    this.overLayRef.detach();
  }
}
// 省略...
```

## 樣式
Overlay有樣式需要載入，比如置中或是backdrop的顏色，我們可直接引入CDk Overlay的樣式。

```scss
@import "~@angular/cdk/overlay-prebuilt.css";
```

## Demo
https://stackblitz.com/edit/angular-cdk-modal

## 結論
目前如此簡單的幾行Code，就可以完成我們的自定義Dialog，非常方便。

## 參考
[Angular CDK 火力上陣 - Mike Huang](https://youtu.be/FQ2-lQE2X5A)

[Exploring Angular DOM manipulation techniques using ViewContainerRef](https://indepth.dev/posts/1052/exploring-angular-dom-manipulation-techniques-using-viewcontainerref)


