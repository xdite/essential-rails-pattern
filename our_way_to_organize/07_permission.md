!SLIDE

# Permission

!SLIDE

# CanCan

<span class="filename"> app/model/ability.rb </span>
<div class="small">
  <pre class="sh_ruby">
  class Ability
  include CanCan::Ability

  def initialize(user)
    if user.has_role?(:admin)
      admin_permissions(user)
    elsif user.has_role?(:editor)
      editor_permissions(user)
    elsif user.has_role?(:author)
      author_permissions(user)
    elsif user.has_role?(:marketing)
      marketing_permissions(user)
    elsif user.has_role?(:contributor)
      cannot :manage, :all
    else
      cannot :manage, :all
    end
  end
</pre>
</div>