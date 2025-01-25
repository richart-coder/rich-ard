---
title: "JavaScript 裝飾器：程式碼的優雅增強"
date: 2024-01-25
draft: false
url: "/javascript/callback"
---

前言:
裝飾器是一種元編程語法，類似於為程式碼元素添加標籤。它們的關鍵特徵是：

- 不改變原始程式碼的核心邏輯
- 提供額外功能而不觸碰原始代碼區塊
- 在編譯或運行時動態增強程式碼行為

```javascript
@memoize
function fib(a, b) {
  // algorithm
}

function maxlength(max) {
  return (target, context) => {
     // here: 一開始 v8 engine 調用
    if(context.kind === "field") {
      return (value) => {
         // here: 設置值的時候調用
         if(value.length > max.length) {
            throw new Error(`Max length ${max} exceeded`);
         }
         return value
      }
    }
  }
}
class User {
  @readonly
  name

  @maxlength(8)
  email
}

class Product {
  #originalPrice

  @off(20) // 直接在靜態階段設置折扣
  #price

  constructor(originalPrice) {
    this.#originalPrice = originalPrice;
  }

  getDiscountedPrice() {
    this.#price = this.#originalPrice;
    return this.#price;
  }
}
```

### 總結

裝飾器提供了一種優雅且強大的方式來擴展和增強程式碼，同時保持原始實現的整潔性。目前提案正逐步成為 JavaScript 的新特性，開發者們對此充滿期待。
它就像是程式碼的客製化貼紙，讓開發者可以用更有創意的方式為程式碼添加功能，同時不需要重寫核心邏輯。想像它是程式開發中的一種精巧工藝，能為代碼增添靈活性和表現力。
