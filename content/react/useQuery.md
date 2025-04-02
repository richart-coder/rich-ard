---
title: "useQuery: 三個核心狀態"
date: 2025-04-02
draft: false
url: "/react/useQuery"
---

# 前言:

在現代 React 應用程式開發中，數據請求管理是前端工程師面臨的重要挑戰之一。React Query (TanStack Query) 作為一個強大的數據請求管理庫，通過 useQuery Hook 為我們提供了簡潔而強大的解決方案。
理解 useQuery 的三個核心狀態 —— isPending、isLoading 和 isFetching，是有效使用這個工具的關鍵。這些狀態看似相似，但各自代表著數據請求生命週期中的不同階段，掌握它們之間的差異和使用場景，能幫助開發者構建更流暢、更直覺的用戶體驗。
本文將深入探討這三個狀態的定義、區別及適用場景，並通過實例展示如何在實際開發中靈活運用它們來優化應用程式的數據加載體驗。

## 為什麼需要三種狀態呢

### 查詢有三個視角

start -> process -> end

```js
// 初始化階段
let isLoading = false;
let isPending = false;
let isFetching = false;
let isFirstRequest = true;
// 渲染階段
const todos = cache("todos");
if (todos === undefined) {
	isPending = true;
}

// 掛載階段/查詢操作階段
if (!enable) return;
try {
	// 設置 isFetching 狀態
	isFetching = true;

	// 只有在緩存中沒有數據且是首次請求時，才設置 isLoading = true
	if (todos === undefined && isFirstRequest) {
		isLoading = true;
		afterFirstFetch();
	}

	const data = await queryFn(queryContext);
	// 處理數據...
} catch (err) {
	// 錯誤處理...
} finally {
	// 重置狀態
	isLoading = false;
	isFetching = false;

	// 如果已有數據，isPending 應該是 false
	if (todos !== undefined) {
		isPending = false;
	}
}
```

## 簡潔定義

- isPending: 關注緩存的數據，不關注查詢
- isLoading: 關注查詢也關注緩存
- isFetching: 只關注查詢

## 詳細說明

### isPending: 表示查詢沒有數據，無論是首次還是後續請求，只要沒有數據就為 true。

- 當緩存中沒有數據時為 true
- 當有數據時為 false
- 不受是否為首次請求的影響

### isLoading: 指首次請求且沒有緩存數據的加載狀態。

- 必須同時滿足「沒有緩存數據」和「正在首次請求中」兩個條件
- 只在第一次請求時觸發，後續重新獲取數據時不會設為 true
- 可以視為 isPending && isFetching && 首次請求

**特別注意：在 enabled: false 的手動控制模式下，isLoading 只會在手動調用 refetch() 且無緩存數據時才會變為 true**

### isFetching: 表示任何網絡請求正在進行中，無論是首次還是重新獲取。

- 任何時候發起網絡請求都會設為 true
- 請求完成後設為 false
- 不受緩存狀態影響

# 總結:

選擇正確的狀態來管理 UI 反饋對於提供良好的用戶體驗至關重要。理解這三個狀態的微妙差異可以讓我們更精確地控制應用在不同數據加載階段的行為表現。
