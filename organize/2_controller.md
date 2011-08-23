!SLIDE

### 類似的 code 在同一個 controller 出現第3次請注意

!SLIDE code

## 使用 before_filter 整理

<div class="correct smaller">
  <pre>
      class PostController &lt; ApplicationController
        before_filter :find_category
        
        def show
          # xxxx
        end
        
        def edit
          # xxxx
        end
        
        protected
        
          def find_category
            @category = Category.find(params[:category_id])
          end
      end
  </pre>
</div>

!SLIDE

### 類似的 method 在不同 controller 出現第二次出現請注意 

!SLIDE

## 搬到 application controller 

<div class="correct smaller">
  <pre>
      class ApplicationController < ActionController::Base
        def find_user
          @user = current_user
        end
      end
      
      class UserFavoritesController &lt; ApplicationController
        before_filter :find_user
      end
      
      class UserProfileController &lt; ApplicationController
        before_filter :find_user
      end
  </pre>
</div>

!SLIDE 

### 類似形式的 controller 出現第二次請注意

!SLIDE

## 使用 NameSpace 整理 controller

### BAD SMELL

<span class="filename">app/controller/user_favorites_controller.rb</span>
<div class="wrong smaller">
  <pre>
      class UserFavoritesController &lt; ApplicationController
    
      class UserProfileController &lt; ApplicationController
      
  </pre>
</div>

### GOOD SMELL
<span class="filename">app/controller/user/favorites_controller.rb</span>
<div class="correct smaller">
  <pre>
      class User::FavoritesController &lt; ApplicationController
      
      class User::ProfileController &lt; ApplicationController
      
  </pre>
</div>

!SLIDE

## 單一 method 超過 15 行請注意

!SLIDE bullets left

## Refactor

* refactor 到 model
   - 「業務邏輯相關的資料」 
   - 「對資料的處理方法」
* refactor 到 presentor
  - 不是什麼都需要扔 model
  - Model - View - Presenter

!SLIDE

# MVP

## TODO : Explain
