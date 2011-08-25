!SLIDE

# Expensive Job

!SLIDE bullets left

## Expensive Jobs

* sending massive newsletters
* image resizing
* http downloads
* updating solr, our search server, after product changes
* batch imports
* spam checks

!SLIDE code

## DelayedJob

<div class="correct smaller">
  <pre class="sh_ruby">
    
  class NewsletterJob < Struct.new(:text, :emails)
    def perform
      emails.each { |e| NewsletterMailer.deliver_text_to_email(text, e) }
    end    
  end 
  </pre>
</div>