# 生成自签名证书

```sh
# 生成 CA 证书，证书文件为 cacert.pem, cakey.pem
openssl req -x509 -config ca.conf -newkey rsa:2048 -sha256 -nodes -out cacert.pem -outform PEM

# 生成证书申请文件 输出文件 httpscert.csr, httpskey.pem
openssl req -config server.conf -newkey rsa:2048 -sha256 -nodes -out httpscert.csr -outform PEM

# 校验 CSR 文件
openssl req -text -noout -verify -in httpscert.csr

# 新建文件用于存储证书颁发信息
touch index.txt
echo '01' > serial.txt

# CA 对 CSR 文件进行签名，输出 httpscert.pem 文件
openssl ca -config ca.conf -policy signing_policy -extensions signing_req -out httpscert.pem -infiles httpscert.csr

# 检查生成的证书
openssl x509 -in httpscert.pem -text -noout
```

__注__: 

1. 证书 issuer 的 Subject 信息写在 ca.conf 中
2. 证书的 Subject 信息和 SAN 信息写在 server.conf 文件中

参考链接: http://blog.ideawand.com/2017/11/22/build-certificate-that-support-Subject-Alternative-Name-SAN/
