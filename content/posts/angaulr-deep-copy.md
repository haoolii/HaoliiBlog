+++
title = "Angaulr 開發工具內的深拷貝 Deep Copy"
date = 2020-10-03T21:37:48+08:00
tags = ["Angular"]
categories = ["筆記"]
draft = false
+++

<!--more-->
---

## 使用場景
深拷貝使用多半在深層結構物件傳入`@Input`或是各種`function`傳遞深層結構物件時，防止在眾多`Component`以及`function`，未注意`reference`而不小心更動到結構內的資料。

## 替代方案
當然也有許多套件都有提供深拷貝的`function`，自己撰寫也不困難。
常見套件：
* lodash - `cloneDeep`
* ramda - `clone`

如果物件未含`function`也可以直接使用
```javascript
JSON.parse(JSON.stringify(object));
```

## 工具位置

除了`deepCopy`還有許多有趣的`function`針對陣列或物件，適合尋寶。

```typescript
import { deepCopy } from '@angular-devkit/core/src/utils/object';

//...

const obj1 = {};

const obj2 = deepCopy(obj1);

console.log(obj1 === obj2); // false
//...
```



## Immutable.js

`Immutable.js`在`react`比較常聽到，但在`Angular`似乎比較少見，不過在`OnPush`策略可能有各種搭配對比`React PureComponent`的情況。


## 參考
[angular2-with-immutablejs](https://blog.scottlogic.com/2016/01/05/angular2-with-immutablejs.html)