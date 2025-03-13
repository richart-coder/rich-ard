---
title: "配置驅動開發: 美麗的錯誤"
date: 2025-03-13
draft: false
url: "/other/config-driven-development"
---

## 前言:

配置驅動開發帶來了很多優勢，例如：有彈性，好擴展，非開發人員也能使用，我不否認但那是建立在一致的模式，我們在一致模式下作擴展，要實現這樣需要找到相同模式並且為它起個契約，這個契約不管誰使用都要遵守，不然使用錯誤很難發現，因為這模式很難追蹤錯誤，如何找出相同模式並訂一個契約就是一個議題了

### 1. 配置是 for 功能的

配置是為了某特定功能，設定是為了整個專案，至少是目錄層級，我們來看個例子

```ts
const isEligibleForDiscount =
	(eligibilityConfig: DiscountEligibilityConfig) =>
	(product: Product): boolean => {
		const [field, condition] = Object.entries(eligibilityConfig)[0];
		const [operator, threshold] = Object.entries(condition)[0];

		if (field in product) {
			const value = product[field as keyof Product] as number;
			return getComparisonPredicateFn(value)[
				operator as "$gte" | "$lt" | "$lte" | "$eq" | "$in"
			](threshold);
		}

		return false;
	};
```

eligibilityConfig 描述 isEligibleForDiscount 的模樣
我們可以針對某一領域的特定的商業模式做開發，這也被稱為 DSL(domain specific language)
配置讓一個功能可以**基於相同模式**產生多種變化，HTML 你不用知道 browser 如何渲染元素的，你只需要什麼元素需要被渲染就好，看起來很能適應變化，But...

### 2. 除錯是惡夢

試想 HTML 好除錯? 他不會噴任何錯誤，但是想想真的有必要噴？
如果都照同一模式而我們照那份契約使用，理論上來說是不會錯的
因此如果擴展照同一模式，基本上是非常好的，你可以享受自由度又不用除錯，但是如果沒有，除錯會變成你的惡夢

### 3. 何時使用

- 同一模式
- 未來需要擴展

## 總結

配置驅動開發基本上都是限定某個功能，這個功能需要高度擴展並且可以辨識每個配置都遵循相同模式，這樣才有意義，要不然硬編碼是更好的選擇，有時候添加了一層抽象沒有比較好，因為在前期你得花更多的時間，但是沒在後期受益，那是沒有必要的
