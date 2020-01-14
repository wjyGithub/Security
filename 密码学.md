# java 密码学相关知识

# 数字证书
### 简介
 数字证书主要是用于对公钥进行数字签名,以验证公钥的准确性。证书的签发主要由数字证书颁发认证机构(CA)进行签发  

### 场景
例如,Tom和Jack在网络中进行通信，Tom用自己的私钥对要传输的数据进行加密，Jack用Tom给的公钥进行解密。  
但是，Jack要怎么知道，自己的公钥就是Tom给自己的公钥呢。因此，Tom先去CA机构对自身的公钥进行认证，  
CA用自身的私钥对Tom的公钥以及其他信息进行加密，并生成一个数字证书。在传输的过程中，  
Tom将加密的数据以及数字证书一同发送给Jack,由于CA的公钥是公开的，因此，Jack利用CA的公钥，解开数字证书，并获取到Tom的公钥。

 国际权威数字证书颁发机构：  
 VerSign(http://www.versign.com)  
 GeoTrust(http://www.geotrust.com)  
 Thawte(http://www.thawte.com)

### 浏览器对证书的认证
浏览器内置了国际CA机构签发的数字证书


### https原理
