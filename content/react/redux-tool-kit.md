---
title: "redux: 一個全局的狀態管理"
date: 2025-03-15
draft: false
url: "/react/redux-tool-kit"
---

## 前言:

哈摟，各位親愛的朋友好，今天聊聊 react redux 狀態管理的概念，系統如何改變他的狀態

### 系統的狀態概念

一個任一系統的狀態是從過去的事件累積而來的，就好比今天你的狀態與昨天一樣？我們都在經歷事件然後從這事件成長，所以這一連串的事件可以從起始狀態推到最後狀態，以下是 redux 想表達的概念

```js
const initState = 0
{type: "transfer", payload: {value: 55000}}
{type: "withdrawal", payload: {value: 2000}}
....
const finalState = 2000
```

完美，透過這些事件我們的系統更透明，這是我們要的，保持前進來看看傳統 redux 與現代 redux 差別

### 傳統 redux

一個在系統內工作的單元: reducer

```js
const reducer = (state, action) => {
	const { type, payload } = action;
	switch (type) {
		case "transfer": {
			// awesome
		}
		case "withdrawal": {
			// awesome
		}
		case "deposit": {
			// awesome
		}
	}
};
// Note: 這還是保持單一責任，這些方法都屬于這個 reducer 的職責
```

### 現代: toolkit

我來重現簡單的 toolkit 代碼，就能知道為什麼了

```js
const createSlice = (config) => {
	const { name, initialState, reducers } = config;
	// 這個會被 dispatch(action) 來填充 store 狀態
	const reducer = (state = initialState, action) => {
		const [sliceName, actionName] = action.type?.split("/") || [];

		if (sliceName === name && reducers[actionName]) {
			return reducers[actionName](state, action.payload);
		}

		return state;
	};
	const actions = Object.keys(reducers).reduce((acc, type) => {
		acc[type] = (payload) => ({ type: `${name}/${type}`, payload });
		return acc;
	}, {});

	return {
		reducer,
		actions,
	};
};
```

這種差異多了一層抽象，甩掉了樣板代碼，其實還不止，他發揮了 useState 最大的優勢只關注一個點，不會明明我的部分沒更新卻發生了重新渲染

## 總結:

現代 redux 雖然多了一層抽象，但經過權衡之後，他們認為還是值得的，只能說我很愛 react 這個框架最根本的原因就是 function 是 singleton 最好的朋友，雙向溝通的特性，使得 props 其中的狀態可以更新，這個更新還是整體，使你可以透明看到一切，react 使用了很多很棒的概念，未來在與大家分享
