
## Generate Web Service code from WSDL
Take a WSDL and generate a jar (JAX-WS)

`wsimport path/to/wsdl/MyWSDL.wsdl -d outputFolder -clientJar MyWSDLCode.jar`

# Update truststore
By default Java trusts the big name CA's who can sign certificates (the Verisigns of the world). It does not know who you are or what your custom certificates are supposed to be. In order to tell Java to trust your certificates do the following.
* Download the certificate using a browser (Firefox is the easiest to do this in)
* Run the following command `keytool -import -v -file path/to/downloaded/cert/MyCert.crt -alias "my certificate" -keystore path/to/trust/store/customTrustStore`
   * This imports the certificate and creates a custom trust store you can use in your application
   * The keytool will prompt you to enter a password. Java's default is "changeit"
   * Multiple certs can be added to this trust store if needed
* The java application needs to know the location of the trust store when it runs. for that use the following command line arguments
   * -Djavax.net.ssl.trustStore=path/to/trust/store/customTrustStore
   * -djavax.net.ssl.trustStorePassword=changeit
   * if you don't want to use command line parameters, you can also hard code this into your application using `System.setProperty("javax.net.ssl.trustStore", "path/to/trust/store/customTrustStore");
   
# My Batis - Paramatize configuration
Normally it is beter to use the jndi lookup to manage the connection. But if you need to use credentials here is a good way to paramatize them.
*From http://www.mybatis.org/mybatis-3/configuration.html
```mybatis-config.xml
<dataSource type="POOLED">
  <property name="driver" value="${driver}"/>
  <property name="url" value="${url}"/>
  <property name="username" value="${username}"/>
  <property name="password" value="${password}"/>
</dataSource>
```

Then when the application is running you can use the following to make those parameters dynamic
```SessionFactory.java
Properties props = new Properties();
props.setProperty( "username", someField.getValue());
...

SqlSessionFactory factory = sqlSessionFactoryBuilder.build(reader, props);
```

Here is a good guide to using MyBatis https://www.concretepage.com/mybatis-3/mybatis-3-annotation-example-with-select-insert-update-and-delete
