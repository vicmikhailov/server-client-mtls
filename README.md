# Server and client certificates for mTLS
## Server certificate
```shell
openssl genpkey -algorithm RSA -out server.key -pass pass:changeit && \
openssl req -new -key server.key -out server.csr -subj "/CN=localhost" -passin pass:changeit && \
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt -passin pass:changeit && \
openssl pkcs12 -export -out server.p12 -inkey server.key -in server.crt -passin pass:changeit -passout pass:changeit -name springboot-server

```

## Client certificate

```shell
openssl genpkey -algorithm RSA -out client.key -pass pass:changeit && \
openssl req -new -key client.key -out client.csr -subj "/CN=localhost" -passin pass:changeit && \
openssl x509 -req -days 365 -in client.csr -signkey client.key -out client.crt -passin pass:changeit && \
openssl pkcs12 -export -out client.p12 -inkey client.key -in client.crt -passin pass:changeit -passout pass:changeit && \
cat client.key client.crt > client.pem
```

## Server truststore - trust client certificate

```shell
keytool -importcert -alias client -file client.crt -keystore truststore.jks -storepass changeit -noprompt
``` 

## Curl server
```shell
 curl --cert client.pem --key client.key --cacert server.crt  https://localhost:8443/hello

```
