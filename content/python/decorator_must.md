---
title: "裝飾器改變了思考模式"
date: 2024-02-21
draft: false
url: "/python/decorator_must"
---

## 前言:

我非常愛裝飾器，我曾在 javascript 提及它，javascript 當前版本沒有這個特性，但作為首先愛的程式語言來說，我還是透過它來說明什麼是裝飾器，有沒有使用裝飾器是沒差的，但使用它帶來了是讓我們重新思考如何組合代碼

### 裝飾器的本質: Make Wrapper

這種模式讓我們可以：

```python
 def my_decorator(fn):
    # 製造一個 wrapper function
    @wraps(fn)
    def wrapper(*args, **kwargs):
        # 在執行前做些事
        print("Before")

        # 執行原函數
        result = fn(*args, **kwargs)

        # 在執行後做些事
        print("After")
        return result

    return wrapper  # 回傳 wrapper
```

這個包裝的過程讓我們可以：

- 在不修改原函數的情況下新增功能
- 把通用的邏輯抽出來重複使用
- 讓代碼更容易維護和擴展

### 可是包裝後的函數會和原始函數一樣？

```python
  @my_decorator
  def hello():
    """
      hello: 我是原始函數
    """
    print("Hello")

  # here: 如果沒有 @wraps 會輸出 { name }
  print(hello.__name__)  # name=wrapper
  print(hello.__doc__)  # name=None

```

## 總結:

使用裝飾器的重點在於：

1. 裝飾器本質就是製造 wrapper function
2. wrapper 是一個全新的函數，會丟失原始函數的資訊
3. 使用 @wraps(fn) 來保留原始函數的資訊
4. 這種模式讓我們可以優雅地擴展功能，而不修改原始程式碼

這就是為什麼說裝飾器改變了我們的思考模式 - 它讓我們開始思考如何通過包裝來組合程式碼，而不是直接修改它。
