## 生成SSL自签泛域名证书

```shell
# 生成私钥
openssl genrsa -out tls.key 4096
# 生成根证书
openssl req -new -sha256 -out tls.csr -key tls.key -config ssl.conf
# 查看根证书
openssl req -text -noout -in tls.csr
# 生成证书
openssl x509 -req -days 3650 -in tls.csr -signkey tls.key -out tls.crt -extensions req_ext -extfile ssl.conf
# 加入钥匙串
sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain tls.crt
sudo apachectl -k restart

# 加入k8s
kubectl create secret generic traefik-cert --from-file=ssl/tls.crt --from-file=ssl/tls.key -n kube-system
```