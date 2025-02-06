---
title: "值缺失盡量少用 None"
date: 2024-02-06
draft: false
url: "/python/missing-value"
---

### 前言:

在程式設計中，我們經常使用 None（或其他語言中的 null）來表示「沒有值」的情況。這個做法在 Java 中曾被其創始人 Tony Hoare 稱為「十億美元的錯誤」，因為它常導致執行時錯誤造成系統崩潰，進而帶來巨大的商業損失。
但更深層的問題是：使用 None 常常代表我們把領域模型設計得太過簡化。以婚姻狀態為例，用 None 來表示「未婚」看似直觀，但這種設計無法承載更豐富的業務資訊，也容易在系統擴展時遇到困難。
現代程式語言提供了更好的工具來處理這類情況：

- Python 的資料類別（dataclass）和 Union 型別
- Rust 的枚舉型別
- Java 的 Optional 類型

#### 這些工具讓我們能更精確地模型化業務概念，而不是簡單地用 None 來表示「沒有值」。讓我們看看如何改進一個使用 None 的設計...

```python
@dataclass
class Person:
  name: str
  spouse: str | None

alice = Person("Alice", None)
bob = Person("Bob", "Alice")  # 只能存名字，沒有其他資訊
```

這種方式的問題：

- 無法記錄婚姻日期等資訊
- 配偶只能用名字表示，缺乏關聯
- 不容易擴展新的狀態或資訊

```python
@dataclass(frozen=True)
class Married:
    spouse: "Person"
    date_of_marriage: date

@dataclass
class Person:
    name: str
    marital_status: Literal["SINGLE"] | Married

# 清晰的業務語義
alice = Person("Alice", "SINGLE")
bob = Person("Bob", Married(alice, date(2024, 2, 5)))
```

改進後的優點：

- 婚姻狀態成為完整的業務概念
- 可以包含更多相關資訊
- 型別安全的狀態表示
- 直接反映領域邏輯

### 總結:

這種演進顯示了如何從單純的技術處理（None 檢查）走向更好的領域模型設計。通過選擇合適的資料結構和型別，我們不僅解決了技術問題，更提升了程式碼的表達能力和可維護性。
