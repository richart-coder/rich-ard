---
title: "function 是編程的核心"
date: 2024-01-28
draft: false
url: "/python/function"
---

### 前言:

函數是圖靈完備語言的一個核心特性，擁有 控制流 、狀態管理 、 計算能力，今天就要針對這三項能力解開函數的神秘面紗。

1. 控制流
   函數提供了跳轉與返回機制

```python
def login_view(request):
  try:
  password = request.get("password", "")
  token = request.get("token", "")

  # 跳轉
  verify_login(password, token)
  # 返回控制權

  expect (AuthError, ValidationError) as e:
     error_type = e.__class__.__name__
        error_handler = errorhandler.get(error_type)
        if error_handler:
            return error_handler(request)


login({ "password": "123456", "token": ""})
```

2. 狀態管理
   我們實作 react 狀態管理

```python
hooks = []
current_index = 0
class Hook:
  def __init__(self, type, *args):
    self.type = type;
    self.values = list(args)


def useState(initial_value):
  global current_index

  index = current_index
  try:
     hook = hooks[index]
     value = hook.values[0]
  except IndexError:
     hooks.append(Hook("state", initial_value))

  current_index += 1
  def setState(new_value):

      if value != new_value:
          hook.values[0] = new_value
          render()

  return [value, setState]

def Counter():
  [ count, setCount ] = useState(0)
```

3. 計算能力
   結果不依賴任何外部的變數，只依賴自定義的變數(參數)

```python
    def fib(n):
        if n <= 1:
            return n
        return fib(n-1) + fib(n-2)

```

### 總結：

函式就像是程式語言中的多功能工具，它不僅僅是一段可重複執行的程式碼，更是連結人類邏輯與電腦世界的橋樑。透過控制流程、狀態管理和運算能力，函式賦予了程式設計師用簡潔而強大的方式塑造複雜邏輯的能力，彷彿是一位可以隨心所欲編織程式碼故事的嚮導。無論是處理例外、管理狀態還是執行複雜的運算，函式都展現出令人驚嘆的靈活性和表現力。
