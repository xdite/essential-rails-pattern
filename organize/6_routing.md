!SLIDE

## CRUD-LIKE action 

!SLIDE

## BAD SMELL

<div class="wrong smallest">
  <pre>
    class UsersController &lt; ApplicationController
      def add_friend
        # ....
      end
      
      def add_to_favorites
        # ....
      end
    end
  </pre>
</div>

!SLIDE

## GOOD SMELL

<div class="correct smallest">
  <pre>
    class User::FriendsController &lt; ApplicationController
      def create
        # ...
      end
    end
    
    class User::FavoritesController &lt; ApplicationController
      def create
        # ...
      end
    end

  </pre>
</div>



