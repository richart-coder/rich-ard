---
title: "enum type 的核心價值"
date: 2024-02-02
draft: false
url: "/python/enum"
---

### 前言:

在 Python 的程式設計世界中，枚舉型別（Enum）是一個強大且富有表現力的工具，為開發者提供了一種優雅且類型安全的方式來定義一組有限的、可預測的常數集合。不同於傳統的常數定義，Enum 型別不僅僅是簡單的靜態值，它還提供了豐富的語義和結構化的方法，使得程式碼更加清晰、可讀和可維護。
Python 的枚舉型別特別有趣，因為 Python 本身並沒有內建的常數值機制。Enum 彌補了這一空白，為開發者提供了一種建立不可變、具有意義的常數集合的方法。它完美地融入了物件導向的編程風格，允許開發者創建具有豐富行為和意義的常數集合，而不僅僅是簡單的數值或字串。

#### 1. 傳統的建立常數值:

```python
# 先思考傳統如何建立常數的？
BAD_REQUEST = 400
FORBIDDEN = 403
NOT_FOUND = 404
```

##### 使用全大寫變數定義常數存在一些問題，而 Enum 提供了更優秀的解決方案：

1. 類型不安全性

```python
# 可以被重新賦值
BAD_REQUEST = 400
BAD_REQUEST = 500

# 甚至可以是不同類型
BAD_REQUEST = 400
BAD_REQUEST = "error"
```

2. 缺乏命名空間與組織性

```python
# 不同的常數定義可能會混雜在一起
BAD_REQUEST = 400
FORBIDDEN = 403
DATABASE_CONNECTION_TIMEOUT = 5000
```

3. 無法阻止意外的比較和操作

```python

BAD_REQUEST = 400
FORBIDDEN = 403
print(BAD_REQUEST + FORBIDDEN)
```

### 基於上述這些問題，我們如何使用 Enum 解決上述問題？

```python

# 定義 namespace
class HttpStatus(Enum):
  # 定義狀態碼(不可變性)
    OK = 200
    CREATED = 201
    NO_CONTENT = 204

    MOVED_PERMANENTLY = 301
    FOUND = 302
    NOT_MODIFIED = 304

    BAD_REQUEST = 400
    FORBIDDEN = 403
    NOT_FOUND = 404

    INTERNAL_SERVER_ERROR = 500
    SERVICE_UNAVAILABLE = 503


# @dataclass 簡化錯誤類型的定義
@dataclass
class ClientError:
    code: HttpStatus


@dataclass
class ServerError:
    code: HttpStatus


@dataclass
class Response:
    error: Optional[Union[ClientError, ServerError]] = None
    ok: bool = False

    @classmethod
    def create(cls, status: HttpStatus):
        if 200 <= status.value < 300:
            return cls(ok=True)

        if 300 <= status.value < 400:
            return cls()

        if 400 <= status.value < 500:
            return cls(error=ClientError(status))

        if 500 <= status.value < 600:
            return cls(error=ServerError(status))

        raise ValueError(f"Unsupported HTTP status code: {status}")
```

### 總結:

#### 通過採用 Enum 來實現 HTTP 狀態碼的處理，我們獲得了以下優勢：

1. 型別安全與不可變性

- 狀態碼一旦定義就無法被修改
- 確保狀態碼的型別一致性
- 防止意外的值修改和型別混淆

2. 命名空間管理

- 相關的狀態碼被邏輯性地組織在一起
- 避免全局命名空間污染
- 代碼結構更清晰明確

3. 錯誤處理更加優雅

- 清晰區分客戶端錯誤和服務器錯誤
- 統一的回應格式
- 更容易進行錯誤追蹤和除錯

3. 可維護性與擴展性

- 容易添加新的狀態碼
- IDE 支援更好的代碼提示
- 方便未來進行功能擴展

這種設計方式不僅解決了傳統常數定義的問題，還為 HTTP 狀態碼的處理提供了一個更加完整和專業的解決方案。透過現代 Python 特性的運用，我們能夠寫出更加穩健、易維護且優雅的程式碼。
