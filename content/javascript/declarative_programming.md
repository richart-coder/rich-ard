---
title: "宣告式編程：概念與實踐"
date: 2025-02-04
draft: false
url: "/python/declarative-programming"
---

### 前言:

宣告與命令編程都有適合的地方，命令在底層是非常好的編程模式，但當來到頂層，我們看到了風景，什麼東西長的樣子，不是很清楚？我們來看看有哪些風景是宣告式

1. HTML:

```html
<article>
	<h1>文章標題</h1>
	<p>這是一個段落，包含一個 <a href="/">連結</a></p>
	<ul>
		<li>列表項目 1</li>
		<li>列表項目 2</li>
	</ul>
</article>
```

2. CSS:

```css
/* here：當滑鼠懸停在這個按鈕上時，它應該是什麼樣子 */
.button:hover {
	background-color: darkblue;
}
```

等等...這不是編程語言啊
你是對的！HTML 和 CSS 嚴格來說不是程式語言（programming languages，但它們確實體現了宣告式的思維方式;所以更準確地說：宣告式的概念不僅存在於程式語言中，也存在於其他類型的電腦語言中。

### 我們來簡單實現如何使用 js 實現宣告式的概念

```js
const createLinkRef = (id, active = false) => ({ id, active });
let linkRefs = [
	createLinkRef("product_link", true),
	createLinkRef("about_link"),
	createLinkRef("contact_link"),
];

// here: 這是命令式非宣告式 -> UI 操作層（處理實際的 DOM 更新）
const render = () => {
	const links = document.querySelectorAll("nav a");

	links.forEach((link) => {
		const linkRef = linkRefs.find((btn) => btn.id === link.id);

		link.classList.toggle("active", linkRef.active);
	});
};

const setActive = (id) => {
	linkRefs = linkRefs.map((linkRef) => ({
		...linkRef,
		active: linkRef.id === id,
	}));
	render();
};

const nav = document.querySelector("nav");
nav?.addEventListener("click", (e) => {
	if (e.target.tagName === "A") {
		e.preventDefault();
		setActive(e.target.id);
	}
});

render();
```

### 總結：

宣告式編程的核心在於描述「想要什麼」（What）而非「如何做」（How）。從我們的探討中可以看到：

宣告式思維存在於不同類型的電腦語言中：

- HTML 描述文檔結構
- CSS 描述樣式規則
- 程式語言中的宣告式寫法

在程式架構中，宣告式最適合用於資料狀態層，用來描述：

- 資料的結構
- 資料的轉換規則
- 狀態的改變

而命令式則適合用於底層實作，處理具體的 UI 操作和 DOM 更新。
這種分層方式讓代碼更容易維護和理解，也是現代前端框架設計的核心思路。
