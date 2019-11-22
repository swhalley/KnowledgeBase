# Java Keystore
For java keystore, see the Java document

# Troubleshooting Certificates
A certificate is a chain of one or more certificates. The browser isn't always the best at showing 
you all of the certificates in a chain. To see all this information OpenSSL is great. 

`$> openssl s_client -showcerts --connect foodomain.com:2456`

This will dump out all of the cert information along with the issuer certificates. copy all the lines including
```
-----BEGIN CERTIFICATE-----
certificate contents
-----END CERTIFICATE-----
```

copy these each into their own files. There may be multiple.

The run the following commands against each file in order to see the information about each certificate in the chain

`$> openssl x509 -enddate -issuer -subject -noout -in /path/to/saved/cert.pem`

This will print out some of the contents of the certificate
