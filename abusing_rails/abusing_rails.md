!SLIDE

# 濫用 Rails 內建機制
## 隱藏的炸彈

!SLIDE code bullets left

## ABUSE: 濫用 helper 的 magic

<div class="wrong smaller">
  <pre>
     &lt;%= form_for @model %&gt;
     &lt;%= form_for [@post,@comment] %&gt;
     &lt;%= link_to "mode_name", @model %&gt;
  </pre>
</div>

### 實作 STI 時，magic 針對 model type 展開，爆光光...
### 翻修時無法使用 git grep "xxxx_path" 快速找到可能有問題處

!SLIDE code

# ABUSE: ORM 無限串接

<br><br><br>

<div class="wrong">
  <pre>
      post.aa.bb.cc.dd.ee.ff.gg.hh.ii.jj.kk.ll.mm.nn.oo.pp
  </pre>
</div>

<br><br><br>
### Create A LOT OF objects
### Complex JOIN & SLOW query

!SLIDE bullets left

## Solution

* 使用 scope 整理 code
* 使用 method 整理 code
* 使用 query_reviewer 檢查 query
* 減少 join 的機會，使用多個 select 造出 query

!SLIDE code

## ABUSE: 厭惡使用 RESTful 

<div class="wrong smaller">
  <pre>
    # match ':controller(/:action(/:id(.:format)))'
  </pre>
</div>

### 程式碼失去組織性
### !!! 嚴重的 security issue !!!


!SLIDE bullets left

## 避免跨站偽造請求 Cross-site request forgery

* 所有讀取、查詢性質操作，都應該用GET
* 修改或刪除到資料的，則要用POST、PUT或DELETE
* 所有的POST請求，都必須加上一個安全驗證碼


!SLIDE code

## 內建機制
<span class="filename">app/controllers/application_controller.rb</span>
<div class="correct smaller">
  <pre>
     class ApplicationController &lt; ActionController::Base
       protect_from_forgery
     end
  </pre>
</div>

<span class="filename">app/views/layout/application.html.erb</span>
<div class="correct smaller">
  <pre>
    &lt;%= csrf_meta_tag %&gt;
  </pre>
</div>

!SLIDE

<div class="wrong smaller">
  <pre>
    # match ':controller(/:action(/:id(.:format)))'
  </pre>
</div>

## 硬幹就失去天然屏障了，任何 action 都可以 get

!SLIDE center


<div class="wrong smaller">
  <pre>
    &lt;img src=&quot;/posts/delete_all&quot;&gt;
  </pre>
</div>

!SLIDE 
<div class="wrong">
  <h1> !! VERY DANGER !!</h1>
</div>

!SLIDE
## Solution
# RESTful ...... please!!

!SLIDE code

## ABUSE: SQL Query in LOOP
<div class="wrong">
  <pre>
    &lt;% Post.limit(10).each do |post| %&gt;
    &lt;%= post.categories.map{ |c| "&lt;span&gt;#{c.name}&lt;/span&gt;"} =%&gt; 
    &lt;% end %&gt;
  </pre>
</div>

## Some operation is very EXPENSIVE

!SLIDE bullets left

## Solution

* cache expensive operation
  - ex. counter_cache, memorize, cache
* includes
* find\_in\_batch

!SLIDE code
## abusing helper 

<div class="wrong">
  <pre>
     def post_information(post)
      content ||=&quot; &quot;
      content += &quot;&lt;div class=\&quot;information\&quot;&gt;
      content += &quot;&lt;span class=\&quot;author\&quot;&gt;#{ post.author.name} &lt;/span&gt;&quot;
      content += &quot;published at&quot;
      content + = &quot;&lt;span class=\&quot;time\&quot;&gt; #{ l(post.published_at, :format =&gt; short) &lt;/span&gt;&quot;
      content + = &quot;&lt;/div&gt;&quot;
      return content
     end
  </pre>
  
</div>


!SLIDE bullets left

## Solution

* Never write plain html in helper
* use content_tag in your helper
* use "partial" to orgnize your code