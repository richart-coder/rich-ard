---
title: "程式設計優先"
date: 2024-02-24
draft: false
url: "/python/design_first"
---

## 前言:

在現代軟體開發中，良好的程式設計不僅關於代碼的運作，更是關於系統的可維護性和擴展性。當我們面對一個新的需求時，第一個思考的不應該是「如何實現它」，而是「如何設計它」。從事件系統到不可變數據結構，從介面設計到封裝原則，每一個設計決策都會影響整個系統的品質。本文將探討如何在開發過程中優先考慮設計，以及這種思維方式如何幫助我們建立更穩健的系統。

### 1.介面設計

假設我們要做一個 「browser」 event 系統，我們需要問自己一些問題

1. 誰使用這個介面？
2. 介面需要隨著時間擴充？
3. 如何管理 記憶體/效能？
4. 跨系統的集成？
5. 如何測試？

### 一個簡單的模擬

```python
events = {}


def addEventListener(type, listener, config=None):
    if config is None:
        config = {"once": False}

    if type not in events:
        events[type] = []

    setattr(listener, "meta", config)
    events[type].append(listener)


def removeEventListener(type, listener):
    if type not in events:
        return
    events[type].remove(listener)


# 不可變數據結構
class Mouse:
    __slots__ = ("_x", "_y")

    def __init__(self, x, y):
        self._x = x
        self._y = y

    @property
    def x(self):
        return self._x

    @property
    def y(self):
        return self._y


def move(x, y):
    if "move" not in events:
        return

    for listener in events["move"]:
        listener(Mouse(x, y))
        if listener.meta["once"]:
            removeEventListener("move", listener)


# 開發者視角
def moveListener(event):
    print(event.x, event.y)


addEventListener("move", moveListener)

# 全局事件觸發
move(1, 1)
move(2, 2)
move(3, 3)
```

### 結論

設計是編程一切的根基，雖然 AI 現在很方便，但期許自己還是能保有匠人精神，精湛的技術需要一筆一劃雕刻，畢竟這是寫程式快樂的泉源，特徵構成應用程式的樣子，保持設計優先原則，不僅使代碼更易於理解和維護，還提供了良好的可測試性和擴展性。
