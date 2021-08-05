## 生成SSL自签泛域名证书

```shell
# 生成私钥
openssl genrsa -out woaisok8s.key 4096
# 生成根证书
openssl req -new -sha256 -out woaisok8s.csr -key woaisok8s.key -config ssl.conf
# 查看根证书
openssl req -text -noout -in woaisok8s.csr
# 生成证书
openssl x509 -req -days 3650 -in woaisok8s.csr -signkey woaisok8s.key -out woaisok8s.crt -extensions req_ext -extfile ssl.conf
# 加入钥匙串
sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain woaisok8s.crt
sudo apachectl -k restart
```