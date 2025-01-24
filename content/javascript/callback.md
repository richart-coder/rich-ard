---
title: "函數執行在哪裡？"
date: 2024-01-22
draft: false
url: "/javascript/callback"
---

### 前言:

Callback 函數是開發者和執行環境互動的核心機制。當我們使用框架時，並非所有代碼都由我們控制 - 我們定義特定行為，而框架決定何時執行這些行為。以 React 為例，讓我們看看這種互動模式如何運作。

### 代碼範例

```javascript
function Counter() {
  const [count, setCount] = useState(0)

	const customCodeBlock = () => {
		// here: 你定義的 -> 某個狀態下該做什麼
    console.log(`目前數了${count}次`)
	};

  const clickHandler = () => {
    // here:`你定義的 -> 何時該更新狀態
     setCount((prevCount) => prevCount + 1);
  }
  // useEffect: react 開發團隊定義 -> 何時調用你代碼
  useEffect(customCodeBlock, [count]);
	return (
		<div>
			<button onClick=clickHandler>{{ count }}</button>
		</div>
	);
}
```

### 總結:

Callback 函數是一段由開發者定義但由其他代碼控制執行的代碼。這種模式讓開發者只需專注於「做什麼」，而將「何時做」的控制權交給框架。以 React 為例，開發者定義狀態變化時的行為，但實際執行時機由 React 管理，這種控制反轉使代碼更容易維護和測試。
