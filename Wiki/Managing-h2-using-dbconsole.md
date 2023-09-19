If you are running streama with the default setting you can manage your h2 using the built-in `/dbconsole` endpoint. 
But first, you got to enable the dbconsole for your production environment (and maybe disable it again after you've done your managing, otherwise your system might be vulnerable). 

Note: only users with admin-rights can access that endpoint. 

## Enable dbconsole 
add the following to your application.yml
```
environments:
  production:
    grails:  
      dbconsole:
        enabled: true
```

Or if you already have a block for environments: production: then add the grails-bit to it, like so: 
![](https://i.imgur.com/mau7V9m.png)

You gotta restart your streama-server for this change to take effect. 

## Accessing dbconsole
Now you can navigate to `//your.streama.com/dbconsole`
![](https://imfascinated.org/img/wxr20vos8rz0teo28d5v.png)

There, make sure to input the correct jdbc connection string (default is `jdbc:h2:./streama;MVCC=TRUE;LOCK_TIMEOUT=10000;DB_CLOSE_ON_EXIT=FALSE`)

This should list all tables on the left and allow you to do some basic sql operations! 

## Moving from h2 to MySQL
Coming soon... 
