1.基于Token实现的后台认证机制

几种常用的认证方式

**  a.HTTP Basic Auth**

HTTP Basic Auth简单点说明就是每次请求API时都提供用户的username和password，简言之，Basic Auth是配合RESTful API 使用的最简单的认证方式，只需提供用户名密码即可，但由于有把用户名密码暴露给第三方客户端的风险，在生产环境下被使用的越来越少。因此，在开发对外开放的RESTful API时，尽量避免采用HTTP Basic Auth。

** b.OAuth（开放授权）**

是一个开放的授权标准，允许用户让第三方应用访问该用户在某一web服务上存储的私密的资源（如照片，视频，联系人列表），而无需将用户名和密码提供给第三方应用。

OAuth允许用户提供一个令牌，而不是用户名和密码来访问他们存放在特定服务提供者的数据。每一个令牌授权一个特定的第三方系统（例如，视频编辑网站\)在特定的时段（例如，接下来的2小时内）内访问特定的资源（例如仅仅是某一相册中的视频）。这样，OAuth让用户可以授权第三方网站访问他们存储在另外服务提供者的某些特定信息，而非所有内容。

![](/assets/import2.png)

**c.cookie Auth**

Cookie认证机制就是为一次请求认证在服务端创建一个Session对象，同时在客户端的浏览器端创建了一个Cookie对象；通过客户端带上来Cookie对象来与服务器端的session对象匹配来实现状态管理的。默认的，当我们关闭浏览器的时候，cookie会被删除。但可以通过修改cookie 的expire time使cookie在一定时间内有效；

关于cookie 和session的区别

因为HTTP协议是无状态的，即每次用户请求到达服务器时，HTTP服务器并不知道这个用户是谁、是否登录过等。现在的服务器之所以知道我们是否已经登录，是因为服务器在登录时设置了浏览器的Cookie！Session则是借由Cookie而实现的更高层的服务器与浏览器之间的会话。

Cookie和Session是为了在无状态的HTTP协议之上维护会话状态，使得服务器可以知道当前是和哪个客户在打交道。

Session是在服务端保存的一个数据结构，用来跟踪用户的状态，这个数据可以保存在集群、数据库、文件中；



Cookie是**客户端**保存用户信息的一种机制，用来记录用户的一些信息，也是实现Session的一种方式。

**Cookie的传递流程**：

1.浏览器向某个URL发起HTTP请求（可以是任何请求，比如GET一个页面、POST一个登录表单等）

2.对应的服务器收到该HTTP请求，并计算应当返回给浏览器的HTTP响应。（HTTP响应包括请求头和请求体两部分）

3.在响应头加入Set-Cookie字段，它的值是要设置的Cookie。

4.浏览器收到来自服务器的HTTP响应。

5.浏览器在响应头中发现Set-Cookie字段，就会将该字段的值保存在内存或者硬盘中。（Set-Cookie字段的值可以是很多项Cookie，每一项都可以指定过期时间Expires。 默认的过期时间是用户关闭浏览器时。）

6.浏览器下次给该服务器发送HTTP请求时， 会将服务器设置的Cookie附加在HTTP请求的头字段Cookie中。（浏览器可以存储多个域名下的Cookie，但只发送当前请求的域名曾经指定的Cookie， 这个域名也可以在Set-Cookie字段中指定）。）

7.服务器收到这个HTTP请求，发现请求头中有Cookie字段， 便知道之前就和这个用户打过交道了.

8.过期的Cookie会被浏览器删除。

总之，服务器通过Set-Cookie响应头字段来指示浏览器保存Cookie， 浏览器通过Cookie请求头字段来告诉服务器之前的状态。 Cookie中包含若干个键值对，每个键值对可以设置过期时间。

Session 是存储在服务器端的，避免了在客户端Cookie中存储敏感数据。 Session 可以存储在HTTP服务器的内存中，也可以存在内存数据库（如redis）中， 对于重量级的应用甚至可以存储在数据库中。

**存储在redis中的Session为例，考察如何验证用户登录状态的问题。**

1.用户提交包含用户名和密码的表单，发送HTTP请求。

2.服务器验证用户发来的用户名密码。

3.如果正确则把当前用户名（通常是用户对象）存储到redis中，并生成它在redis中的ID。

这个ID称为Session ID，通过Session ID可以从Redis中取出对应的用户对象， 敏感数据（比如authed=true）都存储在这个用户对象中。

4.设置Cookie为sessionId=xxxxxx\|checksum并发送HTTP响应， 仍然为每一项Cookie都设置签名。

5.用户收到HTTP响应后，便看不到任何敏感数据了。在此后的请求中发送该Cookie给服务器。

6.服务器收到此后的HTTP请求后，发现Cookie中有SessionID，进行放篡改验证。

7.如果通过了验证，根据该ID从Redis中取出对应的用户对象， 查看该对象的状态并继续执行业务逻辑。

实现上述过程，在Web应用中可以直接获得当前用户。 相当于在HTTP协议之上，通过Cookie实现了持久的会话。这个会话便称为Session

**d.Token Auth**

![](/assets/import3.png)

#### Token Auth的优点 {#token-auth的优点}

Token机制相对于Cookie机制又有什么好处呢？

* **支持跨域访问**
  : Cookie是不允许垮域访问的，这一点对Token机制是不存在的，前提是传输的用户认证信息通过HTTP头传输.
* **无状态\(也称：服务端可扩展行\)**
  :Token机制在服务端不需要存储session信息，因为Token 自身包含了所有登录用户的信息，只需要在客户端的cookie或本地介质存储状态信息.
* **更适用CDN**
  : 可以通过内容分发网络请求你服务端的所有资料（如：javascript，HTML,图片等），而你的服务端只要提供API即可.
* **去耦**
  : 不需要绑定到一个特定的身份验证方案。Token可以在任何地方生成，只要在你的API被调用的时候，你可以进行Token生成调用即可.
* **更适用于移动应用**
  : 当你的客户端是一个原生平台（iOS, Android，Windows 8等）时，Cookie是不被支持的（你需要通过Cookie容器进行处理），这时采用Token认证机制就会简单得多。
* **CSRF**
  :因为不再依赖于Cookie，所以你就不需要考虑对CSRF（跨站请求伪造）的防范。
* **性能**
  : 一次网络往返时间（通过数据库查询session信息）总比做一次HMACSHA256计算 的Token验证和解析要费时得多.
* **不需要为登录页面做特殊处理**
  : 如果你使用Protractor 做功能测试的时候，不再需要为登录页面做特殊处理.
* **基于标准化**
  :你的API可以采用标准化的 JSON Web Token \(JWT\). 这个标准已经存在多个后端库（.NET, Ruby, Java,Python, PHP）和多家公司的支持（如：Firebase,Google, Microsoft）.

基于JWT Token 认证机制的流程

#### 登录 {#登录}

* 第一次认证：第一次登录，用户从浏览器输入用户名/密码，提交后到服务器的登录处理的Action层（Login Action）；
* Login Action调用认证服务进行用户名密码认证，如果认证通过，Login Action层调用用户信息服务获取用户信息（包括完整的用户信息及对应权限信息）；
* 返回用户信息后，Login Action从配置文件中获取Token签名生成的秘钥信息，进行Token的生成；
* 生成Token的过程中可以调用第三方的JWT Lib生成签名后的JWT数据；
* 完成JWT数据签名后，将其设置到COOKIE对象中，并重定向到首页，完成登录过程；

       ![](/assets/import4.png)

请求认证

基于Token的认证机制会在每一次请求中都带上完成签名的Token信息，这个Token信息可能在COOKIE

中，也可能在HTTP的Authorization头中；

![](/assets/import5.png)

客户端（APP客户端或浏览器）通过GET或POST请求访问资源（页面或调用API）；

* 认证服务作为一个Middleware HOOK 对请求进行拦截，首先在cookie中查找Token信息，如果没有找到，则在HTTP Authorization Head中查找;
* 如果找到Token信息，则根据配置文件中的签名加密秘钥，调用JWT Lib对Token信息进行解密和解码;
* 完成解码并验证签名通过后，对Token中的exp、nbf、aud等信息进行验证;
* 全部通过后，根据获取的用户的角色权限信息，进行对请求的资源的权限逻辑判断；
* 如果权限逻辑判断通过则通过Response对象返回；否则则返回HTTP 401；





