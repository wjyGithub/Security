# https相关原理

1. 简介  

2. https通信过程  
a) 客户端(浏览器)向服务器发起请求(随机值1和客户端支持的加密算法)  
b) server接收请求，并响应客户端(随机值2和加密算法)  
c) server发送自身的数字证书给客户端
d) 客户端验证证书，并生成一个随机值(预主秘钥)  
e) 证书验证通过,生成会话密钥(随机值1、随机值2以及预主秘钥组装而成),将会话密钥通过证书中的公钥加密  
f) 传输会话密钥至server,

3. 客户端(浏览器)验证SSL证书流程   
(1) 首先浏览器读取证书中的证书所有者、有效期等信息进行一一校验  
(2) 浏览器开始查找操作系统中已内置的受信任的证书发布机构CA，与服务器发来的证书中的颁发者CA比对，用于校验证书是否为合法机构颁发  
(3) 如果找不到，浏览器就会报错，说明服务器发来的证书是不可信任的。  
(4) 如果找到，那么浏览器就会从操作系统中取出 颁发者CA 的公钥，然后对服务器发来的证书里面的签名进行解密  
(5) 浏览器使用相同的hash算法计算出服务器发来的证书的hash值，将这个计算的hash值与证书中签名做对比  
(6) 对比结果一致，则证明服务器发来的证书合法，没有被冒充  
(7) 此时浏览器就可以读取证书中的公钥，用于后续加密了  

4. 证书链(证书合并)  
 RootCA.crt（rCA，被信任的根证书）  
 IntermediateCA.crt（mCA，某些厂商有多个中间证书）  
 server.crt（sCA，通过CSR签下来的证书） 
 
服务器端证书链的配置 
```text
-----BEGIN CERTIFICATE-----  
...... sCA ......  
------END CERTIFICATE------  
-----BEGIN CERTIFICATE-----  
...... mCA (lower) ......  
------END CERTIFICATE------  
-----BEGIN CERTIFICATE-----  
...... mCA (upper) ......  
------END CERTIFICATE------ 
-----BEGIN CERTIFICATE-----
...... rCA  ......    
-----END CERTIFICATE----- 
```
客户端(浏览器)配置证书链(浏览器内置了根证书(rCA),因此,无需配置根证书)
```text
-----BEGIN CERTIFICATE-----
...... sCA ......
------END CERTIFICATE------
-----BEGIN CERTIFICATE-----
...... mCA (lower) ......
------END CERTIFICATE------
-----BEGIN CERTIFICATE-----
...... mCA (upper) ......
------END CERTIFICATE------
```
