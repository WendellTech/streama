by [rhulha](https://github.com/rhulha)

Uncomment the ssl connector in `/etc/tomcat7/server.xml`


### generate keystore and key
```
cd /usr/share/tomcat7
keytool -keysize 2048 -genkey -alias tomcat -keyalg RSA -storepass changeit -keystore .keystore
```

Do not enter your name, instead use the domain name. For example mystreama.net
You can leave the rest as unknown.

### create the certificate signing request 
```
keytool -certreq -keyalg RSA -alias tomcat -file csr.csr -storepass changeit -keystore .keystore
```


paste the contents of the csr.csr to startssl, copy the response to a file called response.crt, the ca.crt and the sub ca crt file ( use the class 1 version ).

### Add ca certificates and sign key
```
keytool -import -trustcacerts -alias startcom.ca -file ca.crt -storepass changeit -keystore .keystore
keytool -import -alias startcom.ca.sub -file sub.crt -storepass changeit -keystore .keystore
keytool -import -alias tomcat -file response.crt -storepass changeit -keystore .keystore

service tomcat7 restart
```