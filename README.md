PROBLEM: run the same role with different parameters
----------------------------------------------------

TESTED:  ansible 2.4, 2.5, 2.6

# playbook.yml

__METHOD__: `include_tasks` run with `with_items` loop **inside playbook**.  

Notice the modification time. Only `1-touch_to_reload` was touched, the second
`3-touch_to_reload` was not !  

__PROBLEM__: Handler is run only for first iteration of loop


    $ ./playbook.yml
    
    -rw-r--r--  1 mmitucha  staff     0B Sep 10 16:15 1-touch_to_reload
    drwxrwx---  2 mmitucha  wheel    64B Sep 10 15:56 1_app_v1.0.2
    -rw-r--r--  1 mmitucha  staff     0B Sep 10 15:56 3-touch_to_reload
    drwxrwx---  2 mmitucha  wheel    64B Sep 10 15:56 3_app_v4.0.7



# playbook_v2.yml

__METHOD__: `include_tasks` with `with_items` loop **inside role task**.  

Notice none of `1-touch_to_reload`, `3-touch_to_reload` files were touched, new
one `-touch_to_reload` was touched, because variable was not passed.  

__PROBLEM__: Handler is run with default value from "Defaults".

    $ ./playbook_v2.yml
    
    -rw-r--r--  1 mmitucha  wheel     0B Sep 10 16:17 -touch_to_reload
    -rw-r--r--  1 mmitucha  staff     0B Sep 10 15:55 1-touch_to_reload
    drwxrwx---  2 mmitucha  wheel    64B Sep 10 15:55 1_app_v1.0.2
    -rw-r--r--  1 mmitucha  staff     0B Sep 10 15:55 3-touch_to_reload
    drwxrwx---  2 mmitucha  wheel    64B Sep 10 15:55 3_app_v4.0.7

