---
title: "為什麼使用 Walrus Operator 需要正確的方式"
date: 2025-02-05
draft: false
url: "/python/walrus-operator"
---

### 前言：

在處理重複運算的場景中，我們常需要先運算結果再使用它。Python 3.8 引入的 Walrus Operator (:=) 讓我們能在賦值的同時使用這個值，特別適合在迴圈中使用。讓我們通過檔案讀取這個典型案例來看看如何改善代碼結構。

#### 以檔案讀取為例，傳統的寫法需要在迴圈前和迴圈尾都進行賦值：

```python
# (x) walrus operator -> 犧牲可讀性
with open("test.txt", "r") as file:
    while (line := file.readline().strip()) != "":
        print(line)

# (✓) walrus operator
with open("test.txt", "r") as file:
    while line := file.readline().strip():
        if line != "":
            print(line)

# 有經驗開發者可能會這樣寫 -> 空字串本身是 falsy value
with open("test.txt", "r") as file:
    while line := file.readline().strip():
        print(line)

```

### 總結：

Walrus Operator (:=) 的引入目的是簡化代碼結構，但需要正確的使用方式：

適用場景：

- 迴圈中的賦值和條件判斷
- 避免重複運算
- 簡化流程控制

使用原則：

- 保持代碼清晰為優先
- 避免複雜的條件判斷
- 利用 Python 的特性（如 falsy values）

從範例可見，好的使用方式是讓代碼：

- 更簡潔（避免重複賦值）
- 更易讀（邏輯清晰）
- 更 Pythonic（符合 Python 設計哲學）
