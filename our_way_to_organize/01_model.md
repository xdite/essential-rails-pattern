!SLIDE

# Model

!SLIDE code

### Model

<div class="correct smallest">
  <pre class="sh_ruby">
  # 一律先把需要的 lib 都加入
  require "nokogiri"

  class Post < ActiveRecord::Base
    # 先把需要的 module 都加入
    include Techbang::Goodies

    # 宣告 class accessor、類別變數、常數
    cattr_reader :per_page
    @@per_page = 10
    DEFAULT_AUTHOR = "you"

    # 宣告 instance accessor、序列化的資料欄位、可以存取的欄位
  
    attr_reader :headline_image
    serialize :exif
    attr_accessible :title, :excerpt, :description

    # 宣告各式 validation 和 validation filter
    validates_presence_of :title, :published_at
    before_validation :ensure_title

    # 宣告 has_many、belongs_to、has_and_belongs_to_many 等 association 關係
    belongs_to :user, :counter_cache => true
    has_many :comments, :as => :resource
  </pre>
</div>

!SLIDE code

## 續
<div class="correct smallest">
  <pre class="sh_ruby">
    # 如果有 delegate 則寫在那個 association 下
    has_one :profile
    accepts_nested_attributes_for :profile
    delegate :resume, :blog_url, :facebook_url, :plurk_url, :twitter_url, :to => :profile

    # 宣告 gem 的 class method，通常我會把這個 gem 相關的 method 放一起
    has_attached_file :image
    validates_attachment_content_type :image
    validates_attachment_size :image
    after_image_post_process  :get_exif
    delegate :url, :to => :image

    acts_as_taggable_on :tags
    chinese_permalink :title

    define_index do
      indexes title
      indexes description
    end

    # 宣告各式 filter/callback
    before_save :build_profile, :if => "profile.nil?"
    after_save :save_profile

!SLIDE code

## 續
<div class="correct smallest">
  <pre class="sh_ruby">
 
    # 宣告各式 scope 定義
    scope :recent, lambda { |amount| order(""published_at DESC"").limit(amount || 5) }

    # 宣告 class method
    def self.published(post_types=[self.to_s])
      where(:type => post_types).where(:aasm_state => "published").where(["published_at <= ?", Time.zone.now]).order("published_at DESC")
    end

    # 宣告 instance method
    def is_published?
      aasm_state == "published" && published_at <= Time.zone.now
    end
    
    
    protected
    
    # ....
    
    private
    
    # ....

end
</pre>
</div>

!SLIDE

## Annotate

<div class="smaller">
  <pre class="sh_ruby">
  # == Schema Information
  #
  # Table name: categories
  #
  #  id            :integer(4)      not null, primary key
  #  name          :string(255)
  #  label_id      :integer(4)
  #  is_list       :boolean(1)      default(TRUE)
  #  for_marketing :boolean(1)      default(FALSE)
  #  for_admin     :boolean(1)      default(FALSE)
  #  created_at    :datetime
  #  updated_at    :datetime

</pre>
</div>