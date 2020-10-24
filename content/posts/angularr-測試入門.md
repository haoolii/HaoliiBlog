+++
title = "Angular 單元測試筆記"
date = "2019-03-10"
description = "Angular 單元測試簡單筆記"
tags = ["Angular"]
categories = ["筆記"]
+++

<!--more-->
---

## 基本配置介紹

```typescript
import { CoursesService } from './../shared/services/courses.service';
import { async, ComponentFixture, TestBed } from '@angular/core/testing';

import { CoursesComponent } from './courses.component';
import { DebugElement } from '@angular/core';

const coursesServiceStub = {
  all: () => {
    return {
      subscribe: () => {}
    };
  },
  delete: () => {
    return {
      subscribe: () => {}
    };
  },
};

describe('CoursesComponent', () => {
  let component: CoursesComponent;
  let fixture: ComponentFixture<CoursesComponent>;
  let de: DebugElement;
  let coursesService: CoursesService;

  beforeEach(async(() => {
    TestBed.configureTestingModule({
      declarations: [ CoursesComponent ],
      providers: [{
        provide: CoursesService,
        useValue: coursesServiceStub
      }]
    })
    .compileComponents();
  }));

  beforeEach(() => {
    fixture = TestBed.createComponent(CoursesComponent);
    component = fixture.componentInstance;
    de = fixture.debugElement;
    coursesService = de.injector.get(CoursesService);
    fixture.detectChanges();
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  it('should call coursesService on delete ', () => {
    spyOn(coursesService, 'delete').and.callThrough();
    component.deleteCourse(1);
    expect(coursesService.delete).toHaveBeenCalledWith(1);
  });

  it('should render the correct tilte', () => {
    const h1 = de.query(By.css('h1'));
    expect(h1.nativeElement.innerText).toBe('Hello users');
    component.title = 'HEY THERE!';
    fixture.detectChanges();
    expect(h1.nativeElement.innerText).toBe('HEY THERE!');
  });
});

```

* TestBed
  提供一個測試環境，可用來配置測試的`Component`或`Services`。

* ComponentFixture
  是與創建的`Component`以及其對應的`element`互動的工具。

* debugElement
  可以取到`Template`渲染完的畫面，透過`query`, `css`等方式確認`Template`的值。

* beforeEach
  初始化會分成非同步與同步，非同步部分是因為需要初始化相關`Template`相關。其他同步事件都在同步的`beforeEach`。

* spyOn
  模擬物件或方法的行為，可攔截原本的方法呼叫以及回應。