---
title: "依賴注射模組"
date: 2024-02-18
draft: false
url: "/javascript/inversion_of_control"
---

## 前言:

代碼部署是我認為最重要的事，因為它會深遠影響後續的維護與理解，減少後續的成本，
讓程式碼在正確的位置工作，這看似簡單的原則實際上需要 careful 的設計。
在長期的專案迭代中，這樣的投資是值得的。

### 1.傳統方式帶來的挑戰

傳統的直接依賴方式會帶來以下問題：

1. **高耦合性**：服務之間緊密綁定
2. **難以測試**：無法輕易模擬（mock）依賴
3. **維護成本高**：修改一處可能需要修改多處

```js
class ProductRepository {
	static create(...) {}
}
function create(...) {
  ProductRepository.create(...);
}
// 之後如果想換不同家的服務呢
```

當我們需要更換服務，例如從 MongoDB 換成 PostgreSQL，或在測試時需要模擬資料庫操作，這種寫法就會帶來很大的困擾。

### 2.依賴注入（Dependency Injection）提供了一個優雅的解決方案：

```js
class DIContainer(service) {
  return (fn) => (...args) => {
    fn(...args, service)
  }
}

const inject = DIContainer(ProductService)
// 一個 service 對應到一個 server
pipe(inject)(server)(req, res)
// decorator 提供更優雅的方式(目前提案在第三階段)
@inject
function(req, res, server) {}
```

如果檔案目錄有設計好，上述我基本上不用改，遵循介面原則:
"Program to an interface, not an implementation"（依賴於抽象，而不是具體實現）這個原則所說，我們應該：

- 定義清晰的介面
- 讓具體實現遵循這些介面
- 在業務代碼中只依賴介面

結論:
依賴注入不僅是一種設計模式，更是一種思維方式。它幫助我們：

- 建立更靈活的系統架構
- 提高代碼的可測試性
- 降低維護成本
- 使系統更容易擴展和修改

在實際開發中，合理使用依賴注入可以讓我們的代碼更加優雅和易於維護。
