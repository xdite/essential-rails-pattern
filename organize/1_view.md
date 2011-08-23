!SLIDE

# View
## 超過 2.5 頁請注意

!SLIDE

## 常用的 html 使用 partial 整理

<div class="correct smallest">
  <pre>
      &lt;div id=&quot;header&quot;&gt;
        
        &lt;div class=&quot;group&quot;&gt;
          
          &lt;%= render :partial =&gt; &quot;common/user_navigation&quot;  %&gt;
          
          &lt;%= render :partial =&gt; &quot;common/header&quot; %&gt;
        
        &lt;/div&gt;
        
        &lt;%= render :partial =&gt; &quot;common/network&quot; %&gt;
        
        &lt;%= render :partial =&gt; &quot;common/man_navigation&quot; %&gt;
        
        &lt;aside&gt;&lt;%= ads_in_layout_top %&gt;&lt;/aside&gt;
      
      &lt;/div&gt;
</pre>
</div>

### （全站 HEADER）

!SLIDE code

## 重複用到的 html 使用 partial 整理

<div class="correct smallest">
  <pre>
    &lt;article class=&quot;articles&quot;&gt;
    
      &lt;%= render :partial =&gt; &quot;posts/s_post&quot;, 
          :collection =&gt; @new_posts, :as =&gt; :post  %&gt;
      
      &lt;%= render :partial =&gt; &quot;posts/apost&quot;,
          :collection =&gt; @advertise_posts :as =&gt; :post  %&gt;
          
    &lt;/article&gt;
  </pre>
</div>

### （文章列表）

!SLIDE

# View

## 同樣用途的 helper 出現第三次請注意

!SLIDE code

## BAD SMELL
<div class="wrong smaller">
  <pre>
    &lt;span&gt;&#x767c;&#x8868;&#x65bc; &lt;%=  l(@post.published_at, :format => :long) %&gt;&lt;/span&gt;
  </pre>
</div>

## GOOD SMELL

<div class="correct smaller">
  <pre>
     &lt;span&gt;&#x767c;&#x8868;&#x65bc; &lt;%= render_published_at(@post) %&gt;&lt;/span&gt;
  </pre>
</div>
