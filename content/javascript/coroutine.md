---
title: "Coroutine 讓程式碼執行得更有效率"
date: 2024-01-22
draft: false
url: "/javascript/coroutine"
---

## 前言

Js 代碼在單一執行緒中執行，但這種執行方式有其限制，不論程式碼目的是什麼，他都會一路執行到底，但是如果遇到需要等待，它就會等待，導致 CPU 閒置。
某些任務是不需要 CPU 的(比如網路請求、檔案讀取)，這種原地等待的方式相當沒有效率。
為了解決這個問題，協程（Coroutine）應運而生，它協調瀏覽器的多執行緒來實現更高效的執行方式。

## 程式碼示例

```javascript
const headers = {};
async function getUsers() {
	const baseUrl = `${window.location.protocol}://${window.location.hostname}`;
	// 這裡需要等待，CPU 閒置，需要轉移 CPU 控制權
	const response = await fetch(`${baseUrl}/api/users`, { headers });
	return await response.json();
}
getUsers().then((data) => {
	// here: 這個代碼塊暫時存在別的記憶體中
	console.log(data);
});
console.log("換我執行了");
```

## 總結

協程解決了資源利用沒效率的問題，進一步帶來了許多好處

#### CPU 資源利用

CPU 不再因為等待而閒置，可以在等待期間處理其他任務，實現了真正的非阻塞操作。

#### 併發處理能力

系統能夠同時處理多個異步操作，這種能力在處理 I/O 密集型任務時特別有價值。舉例來說，當需要同時處理多個網路請求時，效率提升尤其明顯。

#### 程式碼品質

相比傳統的 callback 方式，協程提供了更易讀、更直觀的程式碼結構。不僅錯誤處理變得更加直觀，也徹底避免了所謂的 "callback hell" 問題。

#### 系統架構優勢

協程系統完美地配合了瀏覽器的多執行緒架構，透過 Event Loop 提供可靠的任務調度，同時又保持了 JavaScript 單執行緒的簡單特性。
