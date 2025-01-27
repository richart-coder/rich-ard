---
title: "list comprehension 不一定真的那麽 comprehension"
date: 2024-01-27
draft: false
url: "/python/lambda"
---

### 前言:

list comprehension 非常有意思，把一個可能需要寫好幾行的程式縮成一行，我不知道有沒有 comprehension，見仁見智，很多語法可能就只是那個語言的風格並非是解決問題的語法，可有可無，喜歡就可以用，不喜歡就用原本的也行，我們來看程式碼吧

### 程式碼範例

實際上開發的時候很常會遇到三個元素

1. data
2. for
3. logic(可省略)

```python
  # here: 傳統方式
  data = ["James", "John", "David", "Jojo", "Amy"]

  # here: 傳統方式(一致的資料流向由上往下)
  lengthNames = []
  for name in data
    if len(name) >= 5
      lengthNames.append(name)

  # here: pythonic way(使用 if 導致資料流方向不一致)
  lengthNames  = [ name for name in data if len(name) ]
```

### 為什麼傳統方式比較好讀？

一致的資料流向，如果我們追蹤 name 這個變數就明白了，但 pythonic way 資料流向不一致
特別是使用了 if，因為如果使用 if 可能會使變醋更難追蹤，但這也是個選擇，如果你覺得對你來說沒影響那 OK，如果對你有影響的話有用到 if 就不用，使用傳統方式，畢竟不是所有的 case 都需要引入邏輯

### 總結:

list comprehension 就像是把程式碼變魔術一樣，把好幾行程式變成一行。有點像是把漢堡的麵包、肉排、生菜分開煮，再組合成漢堡(傳統方式)，或是直接買一個現成的漢堡(list comprehension)。
最有趣的是用 if 的時候，就像是點漢堡時還要加一堆客製化要求，可能反而讓事情變複雜了。所以 if 就像是加配料，沒配料的漢堡(單純的 list comprehension)反而更好消化。
重點不在於用哪種方式，而是選擇適合你的方式。就像有人喜歡自己做漢堡，有人喜歡買現成的，都 OK！
