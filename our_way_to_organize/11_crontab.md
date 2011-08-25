!SLIDE

# Crontab

!SLIDE bullets left smaller

## whenever

* crontab should with projects, not with machines

<div class="smaller">
  <pre class="sh_ruby">
    every 3.hours do
      runner "MyModel.some_process"       
      rake "my:rake:task"                 
      command "/usr/bin/my_great_command"
    end

    every 1.day, :at => '4:30 am' do 
      runner "DB.Backup"
    end

    every :hour do # Many shortcuts available: :hour, :day, :month, :year, :reboot
      runner "SomeModel.ladeeda"
    end

    every :sunday do # Use any day of the week or :weekend, :weekday 
      runner "Task.do_something_great"
    end
</pre>
</div>