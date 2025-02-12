---
title: "為什麼 forloop 寫在 local 效能較好"
date: 2024-02-12
draft: false
url: "/python/forloop_performance"
---

## 前言:

在 Python 中，同樣的 for 迴圈程式碼，究竟是寫在函數內（local scope）還是直接寫在全域（global scope）效能比較好？這個問題看似簡單，但實際測試結果可能會讓你大吃一驚。
在這篇文章中，我們將通過實際的效能測試，來探討 Python 中變數作用域（scope）對程式執行效率的影響，並解釋背後的原理。

## 讓我們先看看效能對比：

![local forloop](/images/local_forloop.png)

![global forloop](/images/global_forloop.png)

### Local 版本：

- CPU total: 126 毫秒
- Wall time: 144 毫秒

### Global 版本：

- CPU total: 236 毫秒
- Wall time: 272 毫秒

### Local 版本明顯較快：

- CPU 時間快了約 110 毫秒（約快 46%）
- Wall time 快了約 128 毫秒（約快 47%）

## 在 Python 中，變數查找（variable lookup）的機制是造成這個效能差異的主要原因。讓我們透過 Python 的 ByteCode 來深入了解：

### 變數的載入和儲存機制

Python 使用不同的操作碼（OpCode）來處理不同範疇的變數：

#### 1. Local 變數（較快）：

- LOAD_FAST：本地變數表中載入變數
- STORE_FAST：變數存入本地變數表

#### 2. Global 變數（較慢）：

- LOAD_NAME：需要在全局命名空間中查找變數
- STORE_NAME：需要在全局命名空間中儲存變數

## 總結:

從測試結果可以明確看出，Local 變數比 Global 變數的存取效率高出許多。這主要是因為 Python 在存取 Local 變數時使用了更高效的 FAST 操作碼，而 Global 變數則需要額外的命名空間查找步驟。因此，在編寫需要大量迭代的程式碼時，建議將常用變數定義在函數內部（Local scope），以獲得更好的執行效能。
