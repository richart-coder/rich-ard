---
title: "模式匹配改變了思考模式"
date: 2024-02-01
draft: false
url: "/python/pattern-matching"
---

### 前言:

Python 3.10 引入的模式匹配（Pattern Matching）是一個強大的功能，它不僅提供了比 if-elif 更優雅的語法，更帶來了嶄新的程式設計思維。

#### 在傳統的狀態管理中，我們常常會遇到這樣的場景：

```python
class Hero:
    def __init__(self):
        self.state = None
        self.weapon = None
        self.pillow = None
        self.world_map = None
        self.current_location = None

    def change_state(self, action_type: str, equipment=None):
        if action_type == "fighting":
            self.weapon = equipment
            self.pillow = None
            self.world_map = None
        elif action_type == "sleeping":
            self.weapon = None
            self.pillow = equipment
            self.world_map = None
        elif action_type == "exploring":
            self.weapon = None
            self.pillow = None
            self.world_map = equipment

    def action(self):
        if self.state == "fighting":
            if self.weapon is None:
                raise ValueError("No weapon!")
            # here: enemy 從哪裡來，設計起來就很麻煩，難道要加到 Hero 欄位?
            self.attack(enemy, self.weapon)

        elif self.state == "sleeping":
            if self.pillow is None:
                raise ValueError("No pillow!")
            self.pillow.regen(self)

        elif self.state == "exploring":
            if self.world_map is None:
                raise ValueError("No map!")
            self.world_map.draw_path(self.current_location, destination)


```

```python
@dataclass
class Hero:
    state: Fighting | Sleeping | Exploring

    def change_state(self, new_state: Fighting | Sleeping | Exploring) -> None:
        self.state = new_state
    def action(self):
        match self.state:
            case Fighting(enemy, weapon):
                self.attack(enemy, weapon)
            case Sleeping(pillow):
                pillow.regen(self)
            case Exploring(world_map, destination):
                self.world_map.draw_path(self.current_location, destination)
```

這種設計的優點：

- 數據和行為的自然綁定
- 戰鬥狀態需要武器和敵人
- 休息狀態需要枕頭
- 探索狀態需要地圖和目的地

#### 總結：

本文透過角色狀態的案例，展示了 Python 中 Pattern Matching 和代數數據類型（ADT）的應用。ADT 讓我們能將狀態和相關數據自然地綁定在一起，避免了傳統方式需要手動管理多個相關屬性的問題。使用 Union 類型和模式匹配，我們可以讓編譯器幫我們確保型別安全，避免了傳統的字串型別檢查可能帶來的錯誤。
在狀態轉換方面，這種設計不需要手動清理舊狀態的數據，狀態轉換時的數據完整性由型別系統保證。同時，這種方式也大大減少了空值檢查和型別檢查的需求，使得程式碼更簡潔、更容易維護，並且能在編譯時就發現潛在的錯誤。
這種模式特別適合複雜的狀態管理系統、需要嚴格型別安全的專案，以及有多個互斥狀態的場景。Pattern Matching 結合 ADT 不僅提供了更優雅的語法，更是一種能幫助我們寫出更好、更安全程式碼的設計模式。
