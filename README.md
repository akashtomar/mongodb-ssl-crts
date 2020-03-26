# mongodb-ssl-crts
commands and cnf files to create TLS/SSL certs for mongodb

### Generate key file for CA:
```
openssl genrsa -out mongodb-test-ca.key 4096
```
### Generate crt file for CA:
```
openssl req -new -x509 -days 1826 -key mongodb-test-ca.key -out mongodb-test-ca.crt -config openssl-test-ca.cnf
```
### Generate key file for IA:
```
openssl genrsa -out mongodb-test-ia.key 4096
```
### Generate csr file for IA:
```
openssl req -new -key mongodb-test-ia.key -out mongodb-test-ia.csr -config openssl-test-ca.cnf
```
### Generate crt file for IA:
```
openssl x509 -sha256 -req -days 730 -in mongodb-test-ia.csr -CA mongodb-test-ca.crt -CAkey mongodb-test-ca.key -set_serial 01 -out mongodb-test-ia.crt -extfile openssl-test-ca.cnf -extensions v3_ca
```
### Merge CA and IA files into a pem file:
```
cat mongodb-test-ca.crt mongodb-test-ia.crt  > test-ca.pem
```
### Generate key file for server:
```
openssl genrsa -out mongodb-test-server1.key 4096
```
### Generate csr file for server:
```
openssl req -new -key mongodb-test-server1.key -out mongodb-test-server1.csr -config openssl-test-server.cnf
```
### Generate crt file for server:
```
openssl x509 -sha256 -req -days 365 -in mongodb-test-server1.csr -CA mongodb-test-ia.crt -CAkey mongodb-test-ia.key -CAcreateserial -out mongodb-test-server1.crt -extfile openssl-test-server.cnf -extensions v3_req
```
### Merge crt and key files into pem file:
```
cat mongodb-test-server1.crt mongodb-test-server1.key > test-server1.pem
```
### Generate client key file:
```
openssl genrsa -out mongodb-test-client.key 4096
```
### Generate client csr file:
```
openssl req -new -key mongodb-test-client.key -out mongodb-test-client.csr -config openssl-test-client.cnf
```
### Generate client crt file:
```
openssl x509 -sha256 -req -days 365 -in mongodb-test-client.csr -CA mongodb-test-ia.crt -CAkey mongodb-test-ia.key -CAcreateserial -out mongodb-test-client.crt -extfile openssl-test-client.cnf -extensions v3_req
```
### Merge crt and key files into pem file:
```
cat mongodb-test-client.crt mongodb-test-client.key > test-client.pem
```
