---
title: "Symbol：獨特在於身份而非描述"
date: 2025-03-05
draft: false
url: "/javascript/symbol"
---

## 前言:

可以想像有人描述不同人長得很高，如何來辨識不同的兩個人？
對人類來說就看他身份啊，但對機器來說可就難了，機器在內部很難辨識不同身份，只能靠位置來區分身份，JavaScript 的設計者，設計名為 Symbol 的標識符，它的設計保證在程式運行期間的穩定性和可靠性。

### 1. 唯一性: 每個 Symbol 創建後都是唯一的，永遠不會改變或相等於其他 Symbol，除非全域註冊

```js
// 緊緊貼著變數
const id = Symbol("這是一個 id");
const id2 = Symbol("這是一個 id");
```

### 2.描述不可變: Symbol 的描述在創建後不能被修改：

```js
const id = Symbol("id");
// (x) id.description = "id2";
// 因為 'description' 為唯讀屬性，所以無法指派至 'description'。
```

### 3.不可轉換為其他類型: 不能將 Symbol 自動或隱式轉換為字符串或數字：

```js
const number = Symbol("2");
number + "1"; // Uncaught TypeError: Cannot convert a Symbol value to a string
Number(number); // Uncaught TypeError: Cannot convert a Symbol value to a number
```

### 4.屬性特性: 當 Symbol 用作對象屬性鍵時，可以結合 Object.defineProperty() 創建不可變的屬性：

```js
const id = Symbol("id");
const user = {};
Object.defineProperty(user, id, {
	value: 123,
	writable: false,
	configurable: false,
});
// before {Symbol(id): 123}
console.log(user);
user[id] = 246;
// after {Symbol(id): 123}
console.log(user);
```

### 5.不參與序列化 - Symbol 作為鍵的屬性不會被 JSON.stringify() 序列化，適合用於內部實現而非數據傳輸

## 總結:

javascript 從 ES6 開始陸續增加了非常棒的概念，當然都是在 web 開發背景下設計的，Symbol 相對少人使用，但引入它增加了程序運行時的穩定，可讀性上升(因為可以描述 identifier)，使用變數而不是使用字串減少拼錯字的機會，因為無法序列化，我們不用管運輸儲存問題，關注於內部實現，實現關注點分離，總而言之，引入 Symbol 是有價值的
