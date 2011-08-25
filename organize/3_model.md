!SLIDE

# Model 超過 2.5 頁請注意

!SLIDE code

## Extract to Module

<div class="correct">
  <pre>
    class Post &lt; ActiveRecord::Base
      include Techbang::PostBase
      
      # ... some bussiness logic method here
    end
  </pre>
</div>

!SLIDE code

### Common Behavior

<div class="correct smallest">
  <pre>
    module Techbang
      module PostBase
      extend ActiveSupport::Concern
      included do
        included AASM
        include ExcerptMethods
        include ContentMethods
        include PermalinkMethods

        aasm_state :submitted, :enter => :notifiy_reviewer
        aasm_state :published
        
        before_save :randomize_excerpt_image_file_name
        
      end
      
      module ClassMethods
      end
      
      module InstanceMethods

        def is_scheduled?
          aasm_state == "published" && published_at > Time.zone.now
        end
      end
    end
  </pre>
</div>

!SLIDE left

## 使用 STI / NameSpace

### Single Table Inheritance

<div class="correct smaller">
  <pre>
     class TechbangPost &lt; Post
     end
    
     class AdvertisePost &lt; Post
     end
  </pre>
</div>

### NameSpace

<div class="correct smaller">
  <pre>
     class User::Profile &lt; ActiveRecord::Base
     end
    
     class User::Preference &lt; ActiveRecord::Base
     end
  </pre>
</div>

!SLIDE code

## Extract to composed class

<div class="correct smallest">
  <pre>
      class Customer &lt; ActiveRecord::Base
        composed_of :address, :mapping => [ %w(address_street street),
                                            %w(address_city city)]
      end
      
      class Address
        attr_reader :street
        def initialize(street)
          @street, @city = street, city
        end
        
        def close_to?(other_address)
          city = other_address.city
        end
      end
  </pre>
      
</div>

!SLIDE

## 類似作用與名稱的 Model 超過 2 個請注意

!SLIDE code

## Polymorphic Association

### Bad Smell

<div class="wrong smaller">
  <pre>
    class PostComment &lt; ActiveRecord::Base
    class PhotoCommenct &lt; ActiveRecord::Base
  </pre>
</div>
### Good Smell
<div class="correct smaller">
  <pre>
    class Comment &lt; ActiveRecord::Base
      belongs_to :commentable, :polymorphic => true
    end
    
    class Article &lt; ActiveRecord::Base
      has_many :comments, :as => :commentable
    end

    class Photo &lt; ActiveRecord::Base
      has_many :comments, :as => :commentable
    end
  </pre>
</div>

!SLIDE

## column 超過 10 個請注意

!SLIDE

## Virtual Attribute

<div class="correct smaller">
  <pre>
    
    &lt;%= f.text_field :full_name %&gt;
    
    def full_name
      [first_name, last_name].join(' ')
    end

    def full_name=(name)
      split = name.split(' ', 2)
      self.first_name = split.first
      self.last_name = split.last
    end
  </pre>
</div>

### (用 getter / setter 造出 virtual attribute)

!SLIDE

## Serialize

<div class="correct smallest">
  <pre>
    class User &lt; ActiveRecord::Base
      serialize :preferences
    end

    user = User.create(:preferences => { "background" => "black",
        "display" => large })
    User.find(user.id).preferences # => { "background" => "black",
        "display" => large }
  </pre>
</div>

### (把小資料存在同一個 column 內)