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

### 錯誤

<div class="wrong">
  <pre>
  &lt;% if current_user &amp;&amp; current_user == post.user %&gt;
    &lt;%= link_to(&quot;Edit&quot;, edit_post_path(post))%&gt;
  &lt;% end %&gt;
  </pre>
</div>

### 正確

<div class="correct">
  <pre >
    &lt;% if editable?(post) %&gt;
      &lt;%= link_to(&quot;Edit&quot;, edit_post_path(post))%&gt;
    &lt;% end %&gt;
  </pre>
</div>


!SLIDE
## 在 Controller 裡處理資料

### 錯誤

<div class="wrong">
  <pre>
     def checkout
       book = Book.find(params[:id])
       current_user.balance -= book.price
       curremt_user.save!
       redirect_to account_path
     end
  </pre>
</div>

### 正確

<div class="correct">
  <pre>
    def checkout
       book = Book.find(params[:id])
       current_user.purchase(book)
       redirect_to account_path
    end   
  </pre>
</div>

!SLIDE

## 在 Model 裡寫 view code

### 錯誤

<span class="filename"> app/models/post.rb</span>

<div class="wrong smaller">
  <pre>
     def tag_list_for_welcome_page
        tags.maps {|tag| "&lt;span&gt; tag.name &lt;/span&gt;" }.join(" / ")
     end
  </pre>
</div>

### 正確

<span class="filename"> app/helpers/post_helper.rb</span>
<div class="correct smaller">
  <pre>
    def tag_list(tags)
      tags.map { |tag| content_tag(:span, tag.name ) }.join(" / ")
    end   
  </pre>
</div>

!SLIDE

### 道德底線： Maintainability （維護性，容易找到問題）