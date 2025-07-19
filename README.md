
# gmsm
GM SM2/3/4 library based on Golang
=======

[![Build Status](https://travis-ci.com/tjfoc/gmsm.svg?branch=master)](https://travis-ci.com/github/tjfoc/gmsm)


## Feature
 gmsm包含以下主要功能

    SM2: 国密椭圆曲线算法库
        . 支持Generate Key, Sign, Verify基础操作
        . 支持加密和不加密的pem文件格式(加密方法参见RFC5958, 具体实现参加代码)
        . 支持证书的生成，证书的读写(接口兼容rsa和ecdsa的证书)
        . 支持证书链的操作(接口兼容rsa和ecdsa)
        . 支持crypto.Signer接口

    SM3: 国密hash算法库
       . 支持基础的sm3Sum操作
       . 支持hash.Hash接口

    SM4: 国密分组密码算法库
        . 支持Generate Key, Encrypt, Decrypt基础操作
        . 提供Cipher.Block接口
        . 支持加密和不加密的pem文件格式(加密方法为pem block加密, 具体函数为x509.EncryptPEMBlock)

## [Usage 使用说明](./API使用说明.md)

## Communication
tjfoc国密交流 
   
[![Join the chat at https://gitter.im/tjfoc/gmsm](https://badges.gitter.im/tjfoc/gmsm.svg)](https://gitter.im/tjfoc/gmsm?utm_source=badge&utm_medium=badge&utm_campaign=-badge&utm_content=badge)


- 如果你对国密算法开源技术及应用感兴趣，欢迎添加“苏州同济区块链研究院·小助手“微信，回复“国密算法进群”，加入“同济区块链国密算法交流群”。微信二维码如下:  
     ![微信二维码](https://github.com/tjfoc/wutongchian-public/blob/master/wutongchain.png)

- 发送邮件到tj@wutongchain.com
 
 
 ## License
 版权所有 苏州同济区块链研究院有限公司(http://www.wutongchain.com/)
 
 Copyright 2017- Suzhou Tongji Fintech Research Institute. All Rights Reserved.
 Licensed under the Apache License, Version 2.0 (the "License");
 
 you may not use this file except in compliance with the License.
 You may obtain a copy of the License at
      http://www.apache.org/licenses/LICENSE-2.0
 Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 
 See the License for the specific language governing permissions and limitations under the License.
=======

## 国密与openssl的差异
```shell
# 颁发根证书
openssl ecparam -genkey -name sm2 -out sm2.key
openssl req -new -key sm2.key -out sm2.csr -sm3 -nodes -subj "/C=CN/ST=Beijing/L=Beijing/O=Example/OU=IT/CN=example.com"
openssl req -x509 -new -nodes -key sm2.key -sm3 -days 365 -out sm2.crt -subj "/C=CN/ST=Beijing/L=Beijing/O=Example/OU=IT/CN=example.com"
openssl x509 -in sm2.crt -text -noout

# 颁发下级证书
openssl ecparam -genkey -name sm2 -out child.key
openssl req -new -key child.key -out child_new.csr -sm3 -subj "/C=AU/ST=Some-State/O=Internet Widgits Pty Ltd"
openssl x509 -req -in child_new.csr -CA sm2.crt -CAkey sm2.key -out child.crt -days 365 -sm3 -sigopt "distid:1234567812345678"
```
* go-gmssl的实现逻辑是没有填写uid就会填入默认uid：1234567812345678
* openssl的实现逻辑是都需要自己手动填入