---
title: "context manager 簡化了流程的管理"
date: 2025-01-29
draft: false
url: "/python/context-manager"
---

### 前言:

想像你要去健身房運動：

- 進門要刷卡（開始）
- 運動（做你的事）
- 離開要關櫃子拿包包（結束）

Context Manager 就像是幫你處理"進門刷卡"和"離開拿包"這些例行公事，讓你專注在運動（主要工作）上！

### 1. 當你在管理檔案時:

```python
f = open('readme.txt', 'w')
f.write('context manager is awesome')
f.close()
```

上述的流程可以分成 3 個 open -> write -> close, 這三個行為都圍繞在 file，但主要做的事是哪個？不言而喻，我們如何把它簡化呢

```python
# 進入 context 會自動打開檔案(start)
with open('readme.txt', w) as f:
  f.write('context manager is awesome')
# 離開 context 會自動關閉檔案(end)

```

#### 我們這時會想內部如何實作的?

```python
class open:
  def __init__(self, name, mode='r'):
    self.name = name
    self.mode = mode

  def __enter__(self):
    f = open(self.name, self.mode)
    self.file = f
    return f

  def __exit__(self, exc_type, exc_value, traceback):
    self.file.close()
```

### 2. 例外的測試(exception handling)

```python

  class assertRaises(exception):
    def __init__(self, exception):
      self.exception = exception

    def __enter__(self):
      return self

    def __exit__(self, exc_type, exc_value, traceback):

      if exc_type is None:
        raise AssertionError(f"期望拋出異常 {self.exception.__name__}")


   with assertRaises(ZeroDivisionError):
    # 期望與實際符合，錯誤繼續傳播
      1 / 0

   with assertRaises(IndexError):
    # 期望與實際不符合，錯誤停止傳播，出現 "期望拋出異常 ZeroDivisionError"

```

### 總結:

Context Manager 就像是一個貼心的管家，在你工作前後幫你處理那些重要但繁瑣的事情，讓你可以專注在真正重要的工作上！
