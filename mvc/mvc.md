!SLIDE


# Rails => MVC
## 不要隨意亂扔原始碼

!SLIDE bullets incremental left

# MVC

* Model
  - 用於封裝與 「業務邏輯相關的資料」 以及 「對資料的處理方法」
* Controller
  - 控制應用程式的流程
* View
  - 負責資料與介面的呈現

!SLIDE

## View 只負責資料的呈現
### 不要將 UI 邏輯與基礎行為混在一起

!SLIDE bullets incremental left

# Antipatterns

* LOGIC IN VIEW
* 在 Controller 裡處理資料
* 在 Model 裡寫 view code

!SLIDE code
# LOGIC IN VIEW

!SLIDE
# 在 Controller 裡處理資料

!SLIDE

# 在 Model 裡寫 view code


!SLIDE

### 道德底線： Maintainability （維護性，容易找到問題）