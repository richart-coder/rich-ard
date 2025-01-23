---
title: "eventloop 如何協調 I/O 密集型任務"
date: 2024-01-23
draft: false
url: "/javascript/eventloop"
---

## 前言:

event 是一個很廣泛的詞語，可以代表任何會發生的事，例如: 點擊或輸入(使用者)，網路請求或渲染(瀏覽器)，某一時間點(計時器)，當事件發生時就會觸發相對應的處理器 ready
這些等待都不需要 CPU， CPU 控制權需要轉移，充分利用 CPU，當中 event loop 扮演了這樣的角色

## 程式碼範例

```javascript
const element = document.getElementById("dom-element");
element.addEventListener("click", () => {
	// 不確定使用者何時會點擊，我們該傻傻等待？
	console.log("click handler");
	setTimeout(() => {
		// 依賴著使用者點擊
		console.log("timeout handler");
	});
});

async function getUsers() {
	// 我們該浪費三秒？ 一個請求一個請求慢慢發出？
	await sleep(3000);
	return {
		data: [
			{ id: 1, name: "John" },
			{ id: 2, name: "David" },
		],
	};
}
```

## event loop 靜態示意圖

![event loop 靜態示意圖](/images/event-loop.png)

事件觸發前並不會去排隊，就好比號碼還沒到時，也不會先排隊，你可以先做自己的事，等到事件觸發，例如前面剩下 2 人時，事件觸發準備中(進入 queue)，這在現實世界很常見，減少等待的時間，增加顧客的體驗

## 總結

Event Loop 是 JavaScript 解決 I/O 密集型任務的核心機制。從程式碼範例可以看到，不論是使用者互動、計時器或網路請求，都不需要同步等待。事件觸發時，callback 會進入對應的隊列，Event Loop 負責在主線程空閒時將它們移入執行，有效利用了 CPU 資源。
