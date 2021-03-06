title: 松鼠书读书笔记（七）——HTTPS
date: 2013-09-24 10:52
categories: web 
---
https是web安全的一个大招
<!--more-->

# HTTPS模型 

![](http://dl.iteye.com/upload/attachment/0079/6189/a62bfcc7-b833-3563-80f9-ea6ccc061b19.jpg)

和HTTP的区别，就在于HTTP和TCP之间多了一层SSL或者叫TLS层。这样一来，HTTP报文就不是明文发送到TCP层了，而是经过安全层做了加密，从而保证了HTTP的安全 

像细节的安全握手，密钥交换，编码解码，都是在安全层完成的。所以HTTPS和HTTP的区别，也就在这一层 

# 加密、解密、密钥 

看完这一章，我总算知道什么是密钥了 

原来的报文叫明文，经过编码以后，就得到了密文。然后密文要还原成明文，这个过程就叫做解码 

在HTTPS里，编码也就是“加密”的过程；解码就是“解密”的过程 

在另外一些场合，也有编码和解码的过程。比如说字符串存储成字节，也是编码（encode）；字节还原成字符串，也叫解码（decode）。但是这个是公开的，所以不能叫“加密”、“解密”。类似的，HTTP的基本认证，也会把“username:password”用BASE64编码后发送，这个也不能算加密 

“加密”和“解密”都需要算法。比如ROT3加密算法，就很简单 

这种算法是没有密钥的，也就是说，一段明文，用这个算法加密以后，得到的密文是一定的 

另一种高端的算法，是有密钥的，根据不同的密钥，加密的结果也不一样。比如ROT-N加密算法：对于N=3密钥，明文ABC会变成DEF；对于N=5密钥，明文ABC会变成FGH 

对于这种高端加密算法，算法是公开的，而且还开放源代码让人学习；私密保存的是密钥而已 

# 对称密钥、非对称密钥 

本来早期的加密算法，大多数是对称密钥算法。也就是说，加密用的密钥，和解密用的密钥是一样的 

这就有2个问题： 

1、client和server要通过HTTPS通信，就必须要交换这个密钥才行，密钥在网络上传播，本身就不安全了，所以就无法保证后续通信的安全性 

2、如果server和100个client通信，就得有100个密钥；如果100台机器，互相通信，那大概需要10000个密钥。实际上internet上的通信规模，远不止这个数量级。所以密钥的管理是一个大麻烦 

后来就进化出了非对称密钥算法。加密（编码）用的密钥是一个，解密（解码）用的密钥是另一个。加密用的密钥称为“公钥”，所有客户端都可以通过某种方式得到这个公钥；解密用的密钥称为“私钥”，只有服务端有这个私钥 

这样，私钥就不在网络上传输了，只有服务端才有。安全性比对称密钥算法要高了很多。另外，也解决了密钥太多难管理的问题，现在不管有多少client，都只有一对公钥/私钥 

# 数字签名、数字证书 

这就没什么好说的了，都是上面讲的加密和解密的原理的应用 

利用密钥，对报文的摘要来编码，解码，证明client和server的身份、保护报文内容等等