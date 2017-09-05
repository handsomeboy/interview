# Cookie和Session的区别？



## Cookie和Session概述
先来一张图看一下cookie和session:

!!17.png

<br><br>
## Cookie
**什么是Cookie?**
Cookie意为“甜饼”，是由W3C组织提出，最早由Netscape社区发展的一种机制。目前Cookie已经成为标准，所有的主流浏览器如IE、Netscape、Firefox、Opera等都支持Cookie。


由于HTTP是一种无状态的协议，服务器单从网络连接上无从知道客户身份。怎么办呢？就给客户端们颁发一个通行证吧，每人一个，无论谁访问都必须携带自己通行证。这样服务器就能从通行证上确认客户身份了。这就是Cookie的工作原理。

Cookie实际上是一小段的文本信息。客户端请求服务器，如果服务器需要记录该用户状态，就使用response向客户端浏览器颁发一个Cookie。**客户端浏览器会把Cookie保存起来**。当浏览器再请求该网站时，浏览器把请求的网址连同该Cookie一同提交给服务器。服务器检查该Cookie，以此来辨认用户状态。服务器还可以根据需要修改Cookie的内容。

!!18.jpg

<br>
**cookie具体内容是什么？**

```
BIDUPSID: 9D2194F1CB8D1E56272947F6B0E5D47E
PSTM: 1472480791
BAIDUID: 3C64D3C3F1753134D13C33AFD2B38367:FG
ispeed_lsm: 2
MCITY: -131:
pgv_pvi: 3797581824
pgv_si: s9468756992
BDUSS: JhNXVoQmhPYTVENEdIUnQ5S05xcHZMMVY5QzFRNVh5SzZoV0xMVDR6RzV-bEJZSVFBQUFBJCQAAAAAAAAAAAEAAACteXsbYnRfY2hpbGQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAALlxKVi5cSlYZj
BD_HOME: 1
H_PS_PSSID: 1423_21080_17001_21454_21408_21530_21377_21525_21193_21340
BD_UPN: 123253
sug: 3
sugstore: 0
ORIGIN: 0
bdime: 0
```

<br>
**java中cookie的方法**
```java
Cookie cookie = new Cookie("username","helloweenvsfei"); // 新建Cookie
cookie.setMaxAge(Integer.MAX_VALUE); // 设置生命周期为MAX_VALUE
cookie.setPath("/"); // 设置路径
cookie.setDomain(".helloweenvsfei.com"); // 设置域名
response.addCookie(cookie); // 输出到客户端
```
<br>
**Cookie有哪些好处？**
1、Cookie能使站点跟踪特定访问者的访问次数、最后访问时间和访问者进入站点的路径 

2、Cookie能告诉在线广告商广告被点击的次数，从而可以更精确的投放广告 

3、Cookie有效期限未到时，Cookie能使用户在不键入密码和用户名的情况下进入曾经浏览过的一些站点 

4、Cookie能帮助站点统计用户个人资料以实现各种各样的个性化服务 
<br>
**cookie有哪些限制？**
1、必须在HTML文件的内容输出之前设置；

2、不同的浏览器对Cookie的处理不一致，且有时会出现错误的结果。

3、限制是在客户端的。一个浏览器能创建的Cookie数量最多为30个，并且每个不能超过4KB，每个WEB站点能设置的Cookie总数不能超过20个。


<br><br>
## Session
**什么是Session**
Session是另一种记录客户状态的机制，不同的是Cookie保存在客户端浏览器中，而Session**保存在服务器上**。客户端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上。这就是Session。客户端浏览器再次访问时只需要从该Session中查找该客户的状态就可以了。
如果说Cookie机制是通过检查客户身上的“通行证”来确定客户身份的话，那么Session机制就是通过检查服务器上的“客户明细表”来确认客户身份。Session相当于程序在服务器上建立的一份客户档案，客户来访的时候只需要查询客户档案表就可以了。

!!19.jpg
<br>
**java中session有哪些方法？**

void setAttribute(String attribute, Object value)：
设置Session属性。value参数可以为任何Java Object。通常为JavaBean。value信息不宜过大 

String getAttribute(String attribute)：返回Session属性 

Enumeration getAttributeNames()：返回Session中存在的属性名 

void removeAttribute(String attribute)：移除Session属性 

String getId()：返回Session的ID。该ID由服务器自动创建，不会重复 

long getCreationTime()：返回Session的创建日期。返回类型为long，常被转化为Date类型，例如：Date createTime = new Date(session.get >CreationTime()) long getLastAccessedTime()：返回Session的最后活跃时间。返回类型为

long int getMaxInactiveInterval()：返回Session的超时时间。单位为秒。超过该时间没有访问，服务器认为该Session失效(**Tomcat默认20分钟**) 

void setMaxInactiveInterval(int second)：设置Session的超时时间。单位为秒 

boolean isNew()：返回该Session是否是>新创建的 

void invalidate()：使该Session失效


<br>
**Session有什么优点？**

1.如果要在诸多Web页间传递一个变量，那么用Session变量要比通过QueryString传递变量可使问题简化。

2.要使WEb站点具有用户化，可以考虑使用Session变量。你的站点的每位访问者都有用户化的经验，基于此，随着LDAP和诸如MS Site 
Server等的使用，已不必再将所有用户化过程置入Session变量了，而这个用户化是取决于用户喜好的。 

3.你可以在任何想要使用的时候直接使用session变量，而不必事先声明它，这种方式接近于在VB中变量的使用。使用完毕后，也不必考虑将其释放，因为它将自动释放。 


**Session有什么缺点？**
1.Session变量和cookies是同一类型的。如果某用户将浏览器设置为不兼容任何cookie，那么该用户就无法使用这个Session变量！ 

2.当一个用户访问某页面时，每个Session变量的运行环境便自动生成，这些Session变量可在用户离开该页面后仍保留20分钟！（事实上，这些变量一直可保留至“timeout”。“timeout”的时间长短由Web服务器管理员设定。一些站点上的变量仅维持了3分钟，一些则为10分钟，还有一些则保留至默认值20分钟。）所以，如果在Session中置入了较大的对象（如ADO 
recordsets，connections， 等等），那就有麻烦了！随着站点访问量的增大，服务器将会因此而无法正常运行！ 

3.因为创建Session变量有很大的随意性，可随时调用，不需要开发者做精确地处理，所以，过度使用session变量将会导致代码不可读而且不好维护。


<br><br>
## Cookie和Session区别


1、cookie数据存放在客户的浏览器上，session数据放在服务器上。

2、cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗
   考虑到安全应当使用session

3、session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能
   考虑到减轻服务器性能方面，应当使用COOKIE

4、单个cookie在客户端的限制是3K，就是说一个站点在客户端存放的COOKIE不能3K。

5、所以个人建议：
   将登陆信息等重要信息存放为SESSION
   其他信息如果需要保留，可以放在COOKIE中