## 图解http

### 通信数据转发程序

1、代理

代理就是客户端和服务器之间的中间人，接收客户端发送的请求并转发给服务器，同时也接受服务器返回的响应并转发给客户端

![image-20210411133948086](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411133948086.png)

代理不会改变URI，会直接发送给前方持有资源的目标服务器

持有资源实体的服务器被称为源服务器，在HTTP通信过程中，可以级联多台代理服务器，转发时，需要附加Via首部字段以标记出经过的主机信息

![image-20210411134253093](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411134253093.png)

代理的分类：

（1）、缓存代理

代理转发响应时，缓存代理会预先将资源的副本（缓存）保存在代理服务器，当代理再次接收到相同的请求时，就可以直接响应，不需要再次向源服务器请求。

（2）、透明代理

转发请求或者响应时，不对报文做任何加工的代理类型

2、网关

网关是转发其他服务器通信数据的服务器，接收客户端的请求时，他就像自己拥有资源一样对请求进行处理，有时客户端都不会察觉。

网关的工作机制与代理类似，而网关能够使通信线路上的服务器提供非HTTP协议的服务

![image-20210411134908441](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411134908441.png)

3、隧道

是在相隔甚远的客户端和服务器两者之间进行中转，并保持双方通信连接的应用程序

隧道本身不会去解析HTTP请求，即保持原样中转给之后的服务器，隧道会在双方的通信断开后结束。

![image-20210411135240486](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411135240486.png)

4、保存资源的缓存

缓存指的是代理服务器或者客户端本地磁盘里面保存的资源副本，利用缓存可以减少对服务器的访问，节省通信流量和通信时间

![image-20210411135647066](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411135647066.png)

缓存存在的问题：当源服务器内容更新时，再访问缓存的内容就不行了，因此缓存有有限期

![image-20210411135836067](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411135836067.png)

客户端的缓存：

缓存还可以存在于客户端，例如IE浏览器中，缓存的临时网络文件，当浏览器缓存有效时就可以直接访问，失效再向服务器申请资源。

### http首部

![image-20210411142118289](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411142118289.png)

HTTP协议的请求和响应报文中必定包含HTTP首部，首部内容为客户端和服务器分别处理请求和响应提供所需要的信息。

**HTTP请求报文**

![image-20210411142711057](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411142711057.png)

以下是访问http://hackr.jp时，请求报文的首部信息

![image-20210411142756833](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411142756833.png)

GET--方法

HTTP/1.1--http版本

**HTTP响应报文**

![image-20210411142923024](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411142923024.png)

以下是访问http://hackr.jp时，响应报文的首部信息

![image-20210411142957544](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411142957544.png)

**HTTP首部字段**

http首部字段是由首部字段名和字段值构成的，中间用冒号分隔

格式：首部字段名：字段值

例如：

```c++
Content-Type:text/html
上述字段表示报文主体的对象类型
```

字段值对应单个HTTP首部字段可以有多个值，例如

```c++
Keep-Alive:timeout=15,max=100
```

当首部字段重复时，一般情况下浏览器会优先处理第一次出现的首部字段

**4种HTTP首部字段类型**

1、通用首部字段--请求报文和响应报文都会使用

![image-20210411144254596](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411144254596.png)

2、请求首部字段

从客户端向服务器端发送请求报文时使用的首部，补充了请求的附加内容、客户端信息、响应内容相关优先级等信息

![image-20210411144614410](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411144614410.png)

3、响应首部字段

从服务器向客户端返回响应报文时使用的首部，补充了响应的附加内容，也会要求客户端附加额外的内容信息

![image-20210411144625392](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411144625392.png)

4、实体首部字段

![image-20210411144635063](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411144635063.png)

### http缺点

通信使用明文（不加密），内容可能会被窃听

不验证通信对方的身份，因此有可能遭遇伪装

无法证明报文的完整性，所以有可能已经篡改

HTTPS

通过将HTTP与SSL（安全套接层）或TLS（安全层传输协议）的组合使用，与SSL组合使用的HTTP称为HTTPS（超文本传输安全协议）

http一般不验证通信方的身份，只要有请求就响应

![image-20210411163739463](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411163739463.png)

解决：使用SSL不仅提供加密处理，而且还是用一种称为证书的手段，可用于确定对方

证书由值得信赖的第三方机构颁发，用以证明客户端和服务器是实即存在的，另外，伪造证书在技术角度很困难

![image-20210411164035889](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411164035889.png)

**无法证明报文完整性，可能已经遭遇篡改**

所谓完整性：指信息的准确度

![image-20210411164233847](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411164233847.png)

以下是中间人攻击

![image-20210411164254462](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411164254462.png)

#### HTTP+加密+认证+完整性保护=HTTPS

当使用https访问网站时，前面会出现一个锁的标志

![image-20210411165245836](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411165245836.png)

HTTPS并非应用层的一种新协议，只是HTTP通信接口部分用SSL和TLS协议代替而已

通常HTTP和TCP直接通信，当使用SSL，HTTP先和SSL通信，再由SSL和TCP通信

![image-20210411165446888](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411165446888.png)

#### 相互交换密钥的公开密钥加密技术

加密和解密都会用到密钥，任何人只要有密钥就可以解密，当密钥被攻击者获得，加密就失去意义。

1、共享密钥加密的困境

加密和解密用同一个密钥的方式称为共享密钥加密，也称为对称密钥加密

![image-20210411170037855](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411170037855.png)

解决办法：使用两把密钥的公开密钥加密

![image-20210411170237077](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411170237077.png)

如上图所示：bob有一把公钥，一把私钥，公钥可以在互联网上随便传播，当alice向bob发送信息时，先用bob公钥对文件加密，bob得到后再利用自己的私钥解密，其他攻击者只能获得加密文件和bob的公钥，但是当前的技术仍然不能解密，所以还是安全的。

HTTPS使用混合加密机制，公开密钥与共享密钥相比，处理速度慢

**证明公开密钥正确性的证书**

公开密钥存在的问题：无法证明公开密钥本身是货真价实的公开密钥，有可能公开密钥被攻击者换了

解决办法：数字认证机构--处于客户端和服务器双方都信赖的第三方机构

![image-20210411171549156](C:\Users\tz\AppData\Roaming\Typora\typora-user-images\image-20210411171549156.png)

注意：HTTPS虽然安全，但是速度比HTTP慢2到100倍

所以在访问时，非敏感信息一般使用HTTP，只有在包含个人信息等敏感数据时，才利用HTTPS加密通信

### 确认访问用户身份的认证

认证原因：计算机无法判断在电脑面前的使用者的身份

核对的内容：

密码：只有本人才知道的字符串信息

动态令牌：仅限本人持有的设备内显示的一次性密码

数字证书：仅限本人（终端）持有的信息

生物认证：指纹和虹膜等本人的生理信息

IC卡：仅限本人持有的信息

