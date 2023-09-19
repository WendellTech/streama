This is a step-by-step to migrate from a previous version to current.

## 1. Determine if you're using MySQL or the H2 Java database.
We can tell this a few different ways. If you see "streama.mv.db" and "streama.trace.db" files, prior to downloading the newer version, you're likely using the Java H2 database type (and NOT MySQL).

However if you do not see those files in the same directory as streama, then run:
`mysql -u <USER> -p <PASSWORD>`
to log into MySQL console. From there, run:
`show databases` and you should see a streama database. So that means you're using MySQL.

## 2. Download the newest Streama version AND the application.yml file. 

## 3. Configure application.yml
Configure the application.yml file to use either the flat files (streama.mv.db, etc) OR the MySQL option. If you do need to use MySQL, you must comment out the "org.h2.Driver" section. If you use the MySQL section, you must put in the login and password for Streama. 

## 4. Run Streama.

## 5. Check settings were ported
Go to the web interface at [IP address]:8080 , and log in. Now check that your settings are appropriately ported. If they are not, stop Streama and verify that the application.yml is pointing at the correct type of Database, and that it can log in. 


***

:information_source: If something didn't work or you want to improve the workflow, check the [the dedicated FAQs Page](https://github.com/dularion/streama/wiki/FAQs). 

***