---
title: "props drilling: 真實世界產生的問題及如何解決"
date: 2025-03-09
draft: false
url: "/python/props-drilling"
---

## 前言:

流言傳來傳去會失真去真實性？透過誰誰誰去傳的訊準確？我們能夠追蹤這些流言？我們如何解決流言產生的問題？這也發生在在編碼世界中也無關什麼框架，但今天我們借 react 的術語使用原生 js 去闡述這個概念

### 傳遞訊息由上往下傳

```js
const processMessage(message) => "聽說你是同性戀"
const 小賴(message) => {
  console.log(message)
  return "幹 誰在亂傳!"
}
const 小華(message) => {
   小賴(processMessage(message))
}
const 小明() {
  const message = "小賴可能是同性戀"
  小華(message)
}
```

上述只用一個中間人，其實現實流言在多個位置傳來傳去，是真是假也不確定，因為沒有追蹤
我們要如何解決呢，創一個共通的位置吧，一但紀錄了就不會改變，除非有人去修改它，代碼會變這樣

```js
let message;
const processMessage(message) => "聽說你是同性戀"
const 小賴(message) => {
  console.log(message)
  return "三小拉! 你才是同性戀，你全家都同性戀"
}
const 小華(message) => {
  console.log(message)
   return "真假啊 小賴是同信戀"
}
const 小明() {
  message = "小賴可能是同性戀"
}
```

## 總結:

建立一個集中的"真相來源"，讓所有組件直接訪問，組件應該只知道它需要知道的內容，通過合理的架構設計和適當的狀態管理策略，我們可以建立更加清晰、可維護的應用程序，就像在現實世界中建立可靠的資訊渠道一樣重要。
