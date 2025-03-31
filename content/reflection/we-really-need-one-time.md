---
title: "為什麼一次就到位的開發是不求實際的"
date: 2025-03-31
draft: false
url: "/python/we-really-need-one-time"
---

## 引言:

哈摟，各位朋友好，今天我們來談談開發的一些反思，對於我做過的事，通常做完之後，我都會去思考有沒有更好的解決方法，為什麼我會 stuck 在這一段時間呢？真的有這個需求？為什麼引入的 bug 還不太好處理呢？

## 從最熟悉的 html 轉換到不熟悉 library 原語

### 純 HTML + CSS

```js
const NavItem = ({ name, ...props }: NavItemProps) => {
	if (isLink) {
		return (
			<a className="...">
				<span>{name}</span>
				<ChevronRight />
			</a>
		);
	}
	return (
		<div className="...">
			<div>{name}</div>
			<div>{description}</div>
		</div>
	);
};
```

### 使用 Radix UI：

```js
const NavItem = ({ name, ...props }: NavItemProps) => {
	if (isLink) {
		return (
			<NavigationMenu.Link asChild>
				<a className="...">
					<span>{name}</span>
					<ChevronRight />
				</a>
			</NavigationMenu.Link>
		);
	}
	return (
		<NavigationMenu.Item>
			<div className="...">
				<div>{name}</div>
				<div>{description}</div>
			</div>
		</NavigationMenu.Item>
	);
};
```

## 讓我們來看看 radix ui

### 優點:

1. 提供了完整的鍵盤導航
2. 處理了 ARIA 屬性和無障礙性
3. 保持了樣式的完全控制

### 缺點:

1. 要學習 Radix UI 的 API
2. 要理解它的組件結構
3. 要記住它的特殊屬性（如 asChild）
4. 增加了程式碼複雜度

## 總結:

為什麼在同時間搞定一切是不切實際的事，首先開發一開始要敏捷性，結果導入了類型，導入了 library，我不是說類型不好，我很常在用，也常常與他搏鬥，尤其是使用第三方你要引入他的類型時，這些都是負擔，你要考慮很多事，有很多的規則要遵守，他很好，替我們處理很多細節，我們都愛它，但是它不是那階段時間應該做的，甚至做它可能從頭到尾都是錯的，我們該替我們網站提供了鍵盤導航？我們該替我們網站具有無障礙特性？我們該引入類型？我們該引入測試？...下次不妨多想想吧
