!SLIDE

# View

!SLIDE code

<div>
  <pre class="sh_html">
&lt;!DOCTYPE html&gt;
&lt;html&gt;

&lt;head&gt;
  &lt;meta charset=&quot;utf-8&quot;&gt;
  &lt;%= render_page_title %&gt;
  &lt;%= stylesheet_link_tag :all %&gt;
  &lt;%= csrf_meta_tag %&gt;
&lt;/head&gt;

  </pre>
</div>

!SLIDE code
<div>
  <pre class="sh_html">
&lt;%= render_body_tag %&gt;
    &lt;div id=&quot;header&quot;&gt;
        &lt;%= render :partial =&gt; &quot;common/header&quot;%&gt;
    &lt;/div&gt;

    &lt;div id=&quot;content&quot;&gt;
        &lt;%= render_stickies %&gt;
        &lt;div id=&quot;main&quot;&gt;
            &lt;%= yield %&gt; 
        &lt;/div&gt;
        &lt;div id=&quot;sidebar&quot;&gt;
            &lt;%= yield(:sidebar)%&gt;
        &lt;/div&gt;
    &lt;/div&gt;

  </pre>
</div>

!SLIDE code
<div>
  <pre class="sh_html">
    
    &lt;div id=&quot;footer&quot;&gt;
        &lt;%= render :partial =&gt; &quot;common/footer&quot; %&gt;
    &lt;/div&gt;
    
    &lt;%= javascript_includes_tag :all %&gt;
    &lt;%= yield(:page_specific_javascript)%&gt;

&lt;/body&gt;
&lt;/html&gt;
  </pre>
</div>

!SLIDE

# View

!SLIDE bullets left

* app/views/common
* app/views/sidebar
* app/views/advertises
