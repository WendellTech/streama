Streama uses the [Grails Mail plugin](http://gpc.github.io/grails-mail/guide/index.html)

By default the plugin assumes an unsecured mail server configured on port 25, getting the SMTP host name from the environment variable SMTP_HOST. 

You can also add mail server configuration in the application.yml.


For unencrypted SMTP:
```
environments:
   production:
     #See the sample application.yml (https://github.com/dularion/streama/blob/master/docs/sample_application.yml) for reference
grails:
   mail:
     host: smtp.domain.com
     port: 25
     username: youracount@domain.com
     password: yourpassword
     default:
        from: yourfromemail@domain.com
```

For Gmail:

```
environments:
   production:
     #See the sample application.yml (https://github.com/dularion/streama/blob/master/docs/sample_application.yml) 
grails:
   mail:
     host: smtp.gmail.com
     port: 465
     username: youracount@gmail.com
     password: yourpassword
     props: {mail.smtp.auth: true,
             mail.smtp.socketFactory.port: 465,
             mail.smtp.socketFactory.class: javax.net.ssl.SSLSocketFactory,
             mail.smtp.socketFactory.fallback: false}
     default:
        from: yourfromemail@gmail.com

```


For Hotmail:

```
environments:
   production:
     #See the sample application.yml (https://github.com/dularion/streama/blob/master/docs/sample_application.yml) 
grails:
   mail:
     host: smtp.live.com
     port: 587
     username: youracount@live.com
     password: yourpassword
     props: {mail.smtp.starttls.enable: true,
             mail.smtp.port: 587}
     deafult:
        from: yourfromemail@live.com

```