---
title: "lambda 使程式碼更簡潔"
date: 2025-01-26
draft: false
url: "/python/lambda"
---

### 前言:

lambda 在數學上有一個符號 λ，用於表示線性代數中的特徵值。只在乎輸入與輸出的關係，這與 context 有很大的不同，考慮的範圍不一樣，在實務上，我們是如何應用的？

### 程式碼範例:

##### 假設我們要做一個 todolist

```python
class Manager(object):

  def __init__(self):
    self.table = []

  def create(self, *, **kwargs):
    self.table({**kwargs})

  def filter(self, predicateFn):
    newTable = []
    for row in self.table
        if predicateFn(row):
          newTable.append(row)

    return newTable`

class Todo {
  objects = Manager()
}

Todo.objects.create(title="python-lambda", due_at="2025-1-26")
Todo.objects.create(title="python-list_comprehension", due_at="2025-1-27")

# here: lamdba 應用
tasks_due_today = Todo.objects.filter(due_at=timezone.now())
```

### 重新思考:

1. 從上述來說只要符合一行代碼能完成都能使用 lambda function
2. 不重用邏輯(特定的商業邏輯)
3. 與可讀性無關

- 為什麼與可讀性無關呢
  只需要符合第一個就能使用，但是 one line 真的都很好讀？或者這樣說，比較緊揍的程式碼就比較好讀？可能因人而異
  舉個例子:

  ```python
  yesno = "yes" if age >= 18 else "no"
  ```

  上述的程式碼也是一行但有人覺得不好讀 使用 regular function 當然 ok
  這只是一個選擇，你也可以選擇比較好讀的寫法，舉個例子:

  ```python
  if age >= 18:
    return "yes"
  else:
    return "no"
  ```

  觀察會發現這兩個寫法有些微差異
  第一個關於 expression(lambda expression)(不需要 return)
  第二個關於 statement(regular function)(需要 return)

### 總結:

Lambda 是個簡單的小函數，就像快速寫便條紙一樣，只記錄輸入跟輸出。它最適合用在簡單的一行程式，比如篩選資料時。
不過它不適合寫複雜的業務邏輯，因為 Lambda 更像是個計算公式，而不是詳細的操作步驟。要用它主要看場合是否合適，不用太在意程式碼看起來漂不漂亮。
Lambda 的好處是讓某些程式碼可以寫得更簡潔。不過同樣的事情用一般函數也能做到，選哪種方式就看你覺得哪種寫法更順手了。
