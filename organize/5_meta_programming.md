!SLIDE

### 名稱類似、作用也類似的 method 在同一 class 出現三次請注意

!SLIDE bullets left

# Meta Programming

*  define_method

<div class="smaller">
  <pre class="sh_ruby">
    ["maple", "wow", "diablo3"].each do |key,value|
      define_method("#{key}_game") do
        find_by_name(value)
      end
    end
  </pre>
</div>

*  method_missing

<div class="smaller">
  <pre class="sh_ruby">
    find_by_post_id_and_user_id
    find_or_initialize_by_ooo_and_xxx
  </div>
</div>