---
title: "一個渲染策略: useDeferredValue"
date: 2025-03-24
draft: false
url: "/react/useDeferredValue"
---

## 前言:

React 是個建構 UI 流行的框架，使團隊擁有一致的代碼，提供 API 減少樣板代碼，讓我們專注於商業相關的思考與使用體驗上，這一篇文章提供了後者的訊息，傳達了基本概念，React 是如何思考渲染這件事

## 渲染的黃金準則：呼叫 setState

我先 mock 我們可能會遇到的情況(不透過 server):

1. 假設網站的產品我們都載到了使用者內存中(可能上萬個)
2. 使用者 query 他們想要的特定商品

想像一下有兩個區域:

1. 輸入區域
2. 瀏覽區域

```js
const SearchContainer = () => {
	const [query, setQuery] = useState("");

	return (
		<div>
			<SearchInput value={query} onChange={(e) => setQuery(e.target.value)} />
			<SearchResult query={query} />
		</div>
	);
};
```

上面 query 一但改變 SearchInput SearchResult 同時會改，造成有點卡卡的
主要是 SearchResult 渲染的優先度與 SearchInput 是一樣的，勢必得把 SearchResult 渲染的優先度降低，使用 useDeferedValue 告訴 react 這個值延遲更新，讓整個畫面不會輸入一個字就渲染，當然我是舉所有產品我們都載到了使用者內存中，還有其他使用場景就先不提，都一樣的概念

```js
const SearchContainer = () => {
	const [query, setQuery] = useState("");
	const deferredQuery = useDeferredValue(query);
	return (
		<div>
			<SearchInput value={query} onChange={(e) => setQuery(e.target.value)} />
			<SearchResult query={deferredQuery} />
		</div>
	);
};
```

Note:
這個場景不適合使用 useTransition 會導致反模式

### 總結:

這個感覺其實有點類似 throttle，但技術上不一樣，透過在元素/組件 mark 優先度，react 會先跳過低優先度，直到內部觸發更新事件，但是這是最後手段(last resort)，畢竟不需要就別用了是吧
