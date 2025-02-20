---
title: "Vue 開發中的依賴注入：靈活管理模擬資料的藝術"
date: 2024-02-20
draft: false
url: "/vue/mockdata"
---

## 前言

在前端開發中，我們經常遇到需要等待後端 API 開發完成的情況。這可能會導致前端開發進度受阻，影響整體專案時程。然而，我們可以透過依賴注入(Dependency Injection)的方式，先使用模擬資料(Mock Data)進行開發，等後端 API 準備就緒後再無縫替換。

這種開發方式有以下優點：

1. 前端開發不需等待後端
2. 可以先確認 UI/UX 的體驗
3. 方便測試不同資料情境
4. 容易切換測試環境和正式環境

在 Vue 專案中，我們可以建立一個模擬資料層，透過依賴注入的方式提供資料給元件使用。這不只加速開發流程，也讓程式碼更容易維護和測試。

## 模擬資料與 API 實作

1. 首先，我們定義資料型別並建立模擬資料和實際 API 的實作：

```typescript
// types/product.ts
interface Product {
	id: string;
	src: string;
	name: string;
	price: number;
	description: string;
}

// mock_getProduct.ts
const getProduct = async (id: string): Promise<Product> => {
	await new Promise((resolve) => setTimeout(resolve, 1000));
	return {
		id,
		src: "/images/product-1.jpg",
		name: "iPhone 15",
		price: 799,
		description: "The latest iPhone with amazing camera and performance.",
	};
};

// api_getProduct.ts
import { API_BASE_URL } from "./setting";

async function getProduct(id: string): Promise<Product> {
	const response = await fetch(`${API_BASE_URL}/products/${id}`);
	return response.json();
}
```

2. API 提供者

建立一個集中管理 API 的提供者：

```typescript
// apiProvider.ts
import fake_getProduct from "./mock_getProduct";
import real_getProduct from "./api_getProduct";

const getProduct =
	import.meta.env.MODE === "development" ? fake_getProduct : real_getProduct;

export default getProduct;
```

3. 在 Vue 組件中使用：

```typescript
import getProduct from "./apiProvider";

onMounted(async () => {
	// 邏輯可以重用拉到 store
	isLoading.value = true;
	try {
		product.value = await getProduct(id);
	} finally {
		isLoading.value = false;
	}
});
```

## 總結:

前端開發中的依賴注入，就像是專案開發的萬能金鑰！它讓開發者擺脫了等待後端 API 的苦惱，用模擬資料快速推進專案。
透過統一的資料介面和環境變數控制，團隊可以靈活地在模擬和真實資料之間切換。這不僅加速了開發節奏，還能提前驗證介面體驗，為專案的順利推進提供了強大的技術支持。
簡單來說，這是一種聰明的開發策略，讓前端開發更加敏捷、高效，同時降低了團隊協作的複雜度。
