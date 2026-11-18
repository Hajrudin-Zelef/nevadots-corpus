Go to UniFi base folder
cd /usr/lib/unifi
Copy your private key file and the pem file (single file including the cert, intermediate ca cert and chain to the root if needed) to /usr/lib/unifi/
Convert your private key from PKCS#8 to PKCS#1 if necessary (when you get an error message like java.lang.IllegalStateException: private.key contains an artifact that is not a key pair: org.bouncycastle.asn1.pkcs.PrivateKeyInfo)
# OpenSSL 2:
openssl rsa -in private.key -out transformed-private.key
# OpenSSL 3:
openssl pkey -in private.key -traditional -out transformed-private.key
Stop the UniFi Network application
service unifi stop
Import cert and key to keystore
java -jar lib/ace.jar import_key_cert transformed-private.key certificate.pem
Optionally you can add a CA cert as an additional, last parameter (if it is not already included in the certificate.pem)
Start the UniFi Network application
service unifi start
Clean up and delete your cert and private key files.
That's it.