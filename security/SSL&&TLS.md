## Background

### Eavesdroping

窃听风险：第三方可以获知通信内容

### Tampering

篡改风险：第三方可以修改通信内容

### Pretending

冒充风险：第三方可以冒充他人身份参与通信

## Target

### Encrypted

加密传播，第三方无法窃听

### Verified

具有校验机制，一旦被篡改，通信双方会立刻发现。

### Identitied

配备身份证书，防止身份被冒充

## How

### HandShake

    ClientHello: I want you!
    ServerHello: OK. This is my ID card.
    ClientResponse: Ennn. It's you. xxxxxxxxxx(pre-master key)xxxxxx
    ServerResponse: I got it. Let's talk in xxx.

#### ClientHello

    （1） 支持的协议版本，比如TLS 1.0版。
    （2） 一个客户端生成的随机数，稍后用于生成"对话密钥"。
    （3） 支持的加密方法，比如RSA公钥加密。
    （4） 支持的压缩方法。

#### ServerHello

    （1） 确认使用的加密通信协议版本，比如TLS 1.0版本。如果浏览器与服务器支持的版本不一致，服务器关闭加密通信。
    （2） 一个服务器生成的随机数，稍后用于生成"对话密钥"。
    （3） 确认使用的加密方法，比如RSA公钥加密。
    （4） 服务器证书。

#### ClientResponse

    （1） 一个随机数。该随机数用服务器公钥加密，防止被窃听。
    （2） 编码改变通知，表示随后的信息都将用双方商定的加密方法和密钥发送。
    （3） 客户端握手结束通知，表示客户端的握手阶段已经结束。这一项同时也是前面发送的所有内容的hash值，用来供服务器校验。

#### ServerResponse

    （1）编码改变通知，表示随后的信息都将用双方商定的加密方法和密钥发送。
    （2）服务器握手结束通知，表示服务器的握手阶段已经结束。这一项同时也是前面发送的所有内容的hash值，用来供客户端校验。

### Record
开始用上面三个随机数导出的对称加密密钥进行通信。上面握手的核心就在于第三次客户端发给服务端的随机数是否安全，这个随机数是使用非对称密钥来加密解密的。

### CA
CA可以信任其它CA，因此产生了证书链，一般不超过3到4层

根证书是客户端信证的CA的证书。

### Reference

[SSL/TLS协议运行机制的概述](http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)

[图解SSL/TLS协议](http://www.ruanyifeng.com/blog/2014/09/illustration-ssl.html)

[How does SSL/TLS work](https://security.stackexchange.com/questions/20803/how-does-ssl-tls-work)