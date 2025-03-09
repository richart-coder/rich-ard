---
title: "硬編碼的藝術：何時選擇直接編碼而非配置驅動"
date: 2025-02-26
draft: false
url: "/python/harded-code"
---

## 前言

在寫程式的時候，我們常被告誡「不要硬編碼」，彷彿這是什麼大罪一樣。我以前也是這麼想的，直到最近在處理一個訂單系統時，我才發現——有時候直接把邏輯寫死反而是更好的選擇。
這篇文章就來聊聊我的經驗。當我試圖把訂單狀態流程做成「超級靈活可配置」的系統時，卻發現自己掉進了過度設計的陷阱。有時候，簡單明瞭的 if-else 不只執行更快，還讓整個程式碼更容易理解和維護。
來看看為什麼有些時候，硬編碼不是罪過，而是明智之舉吧！

### 我的第一個方案：硬編碼處理流程

最初，我使用了最直接的方法，就是用 if-elif 語句處理狀態轉換：

```python
def process_order_event(order: Order):
    event = yield "訂單已創建但尚未付款"
    while True:
        if order.status == "Created" and event == "商品付款了":
            order.pay()
            event = yield "訂單已付款，等待出貨"

        elif order.status == "Paid" and event == "商品離開了庫存":
            order.ship()
            event = yield "訂單已出貨"

        elif order.status == "Shipped" and event == "商品交給了客戶":
            order.complete()
            event = yield "訂單已完成，等待客戶簽收"

        else:
          event = yield f"事件 '{event}' 不適用於當前狀態 '{order.status}'"
```

這個生成器函數可以接收事件，並根據訂單當前狀態執行相應的操作，然後等待下一個事件。簡單明瞭，很好理解。

### 「進化」：配置驅動的設計

但我當時想，如果將來需要增加新的狀態或修改現有流程怎麼辦？於是我嘗試做了一個更「靈活」的設計：

```python
TRANSITIONS = [
    {
        "event": "商品付款了",
        "source": "Created",
        "target": "Paid",
        "method": "pay",
        "message": "訂單已付款，等待出貨"
    },
    {
        "event": "商品離開了庫存",
        "source": "Paid",
        "target": "Shipped",
        "method": "ship",
        "message": "訂單已出貨"
    },
    {
        "event": "商品交給了客戶",
        "source": "Shipped",
        "target": "Completed",
        "method": "complete",
        "message": "訂單已完成，等待客戶簽收"
    }
]

def process_order_event(order: Order):
    event = yield "訂單已創建但尚未付款"
    while True:
        found_match = False
        for transition in TRANSITIONS:
            if order.status == transition["source"] and event == transition["event"]:
                getattr(order, transition["method"])()
                event = yield transition["message"]
                found_match = True
                break

        if not found_match:
            event = yield f"事件 '{event}' 不適用於當前狀態 '{order.status}'"
```

看起來更靈活了！現在我可以只修改 TRANSITIONS 列表，而不用改變代碼邏輯。

### 問題來了：複雜性與性能

但是，這個「進化」版本帶來了一些問題：

- 執行效率下降：每次收到事件時，都要遍歷整個轉換列表尋找匹配項
- 引入了額外變數：需要 found_match 來追蹤是否找到匹配的轉換
- 程式碼變得更複雜：邏輯不再那麼直接明瞭
- 靜態分析能力減弱：IDE 和分析工具難以追蹤動態類型

最重要的是，我意識到這種「靈活性」其實很少被用到。訂單狀態流程是核心業務邏輯，不會經常變動。即使有變動，通常也需要經過慎重的評估和測試，修改幾行代碼根本不是問題。

什麼時候硬編碼其實更好？
經過這次經驗，我總結出幾點關於何時選擇硬編碼的指導原則：

1. 當邏輯相對穩定時：像訂單狀態這樣的核心業務邏輯不會頻繁變動
2. 當性能很重要時：硬編碼的條件判斷比動態查找更高效
3. 當代碼清晰度很重要時：直接的 if-elif 結構更易於理解和維護
4. 當編譯時檢查有價值時：編譯器可以幫忙檢查類型上的錯誤

## 結論

不要被「硬編碼是反模式」的教條所誤導。軟體設計是關於做出權衡，而不是盲目追求特定模式。在適當的場景下，硬編碼不僅是可接受的，還可能是最佳選擇。
當你下次想要「靈活化」一段代碼時，先問問自己：這種靈活性真的必要嗎？它帶來的複雜性和性能損失是否值得？有時候，最簡單的解決方案反而是最好的。
