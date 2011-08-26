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

### 相同的 method 在不同 controller 出現第二次出現請注意 

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
* refactor to presentor
  - 不是什麼都需要扔 model
  - Model - View - Presenter

!SLIDE

# MVP
### Model - View - Presenter

!SLIDE bullets left

## MVP 

* UI 高度依賴業務邏輯 
* 業務過於複雜
* 僅在該 View 使用一次，且不屬於 Model 的基礎 method 
* 不應該放在 Controller / Model，而應該放在 Presenter
* 業務邏輯抽出來放在 Presenter，可以隨意改動 UI

!SLIDE smallest

# Presenter

<div class="correct">
  <pre class="sh_ruby">
    class Sites::ShowPresenter
    
      def hottest_topics
        @hottest_topics = @site.topics.hottest.limit(10)
      end

      def recent_topics
        @recent_topics = @site.topics.unhidden.recent(10)
      end
      
    end
  </pre>
</div>

!SLIDE smallest

# Use presenter in controller
<div class="correct">
  <pre class="sh_ruby">
    def show
      @presenter = Sites::ShowPresenter.new(@site)
      @headline_topic = @presenter.headline_topic
      @categories =  @presenter.categories
      @site_alias = @presenter.site_alias

      add_breadcrumb @site.name, site_path(@site)
      seo_meta_desc_keywords(@site)
    end
  </pre>
</div>

!SLIDE 

## 儲存之前、儲存之後需要 do something

!SLIDE code

## BAD SMELL

<div class="wrong">
  <pre>
      if @post.save
        @post.delay.update_search_index
      end
  </pre>
</div>

!SLIDE code

## 善用 model callbacks
<div class="correct">
  <pre>
    class Post &lt; ActiveRecord::Base
      after_save :update_search_index
    end
  </pre>
</div>

## 或者是 Observer

<div class="correct">
  <pre>
    class PostObserver &lt; ActiveRecord::Observer
      observ :post
      
      def after_save(post)
        update_search_index
      end
    end
  </pre>
</div>