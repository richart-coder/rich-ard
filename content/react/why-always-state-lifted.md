---
title: "一個關鍵:為什麼我們總該提升狀態"
date: 2025-03-20
draft: false
url: "/react/why-always-state-lifted"
---

## 前提:

前端狀態管理是主要核心工作，從原生到框架，原生元素都是基本單元，管理多個組件的狀態基本上是框架要做的事，我們稱為 components，這些組件自身的狀態不太需要理他，因為你會繼承它的狀態，除非你要重新造輪胎，今天我們重新造個輪胎來說明為什麼提升狀態是必要的，Lets go

### pseudoclass 是一個元素的內部狀態

```jsx
import { useState, forwardRef, useRef } from "react";

const Button = forwardRef((props, ref) => {
	const [hovering, setHovering] = useState(false);

	const handleMouseEnter = () => {
		setHovering(true);
	};

	const handleMouseLeave = () => {
		setHovering(false);
	};

	return (
		<button
			ref={ref}
			className={hovering ? `${props.name}-hovered` : ""}
			onMouseEnter={handleMouseEnter}
			onMouseLeave={handleMouseLeave}
		>
			Hover me
		</button>
	);
});

export default function App() {
	const btnRef = useRef(null);
	return <Button ref={btnRef} name="primary" />;
}
```

```css
/* 這是原生 CSS 的語法  */
.primary:hover {
	color: red;
}
/* 這是 React 自定義的語法  */
.primary-hovered {
	color: red;
}
```

## 總結

瀏覽器原生已經處理了許多 UI 互動狀態，React 專注於處理跨組件共享的狀態，區分職責導致更清晰的代碼，基本上大多只需要兩層(container, view)，供應商層除外，有更多的時間再切得更細(大 view -> 小 view)，畢竟 deadline 就在那，區分優先度就顯得夠更重要了，也體現了軟體設計就是權衡
