#目录
* [Cookie](#Cookie)
    * [简介](#什么是Cookie)
    * [常用属性](#Cookie属性)
    * [不可跨域名性](#Cookie的不可跨域名性)

* [Session](#Sesion)
    * [简介](#什么是Session)
    * [生命周期](#Session的生命周期)
    * [有效期](#Session的有效期)
    * [方法](#Session的常用方法)
    * [Session&浏览器](#Session&浏览器)
    * [URL地址重写](#URL地址重写)
    * [Session中禁止使用Cookie](#Session中禁止使用Cookie)

## Cookie 
### Cookie 机制

&emsp;在程序中，会话跟踪是很重要的事情。理论上，一个用户的所有请求操作都应该属于同一个会话，而另一个用户的所有请求操作则应该属于另一个会话，二者不能混淆。例如，用户A在超市购买的任何商品都应该放在A的购物车内，不论是用户A什么时间购买的，这都是属于同一个会话的，不能放入用户B或用户C的购物车内，这不属于同一个会话。<br>

&emsp;而Web应用程序是使用HTTP协议传输数据的。HTTP协议是无状态的协议。一旦数据交换完毕，客户端与服务器端的连接就会关闭，再次交换数据需要建立新的连接。这就意味着服务器无法从连接上跟踪会话。即用户A购买了一件商品放入购物车内，当再次购买商品时服务器已经无法判断该购买行为是属于用户A的会话还是用户B的会话了。要跟踪该会话，必须引入一种机制。<br>

&emsp;Cookie就是这样的一种机制。它可以弥补HTTP协议无状态的不足。在Session出现之前，基本上所有的网站都采用Cookie来跟踪会话。<br>

### 什么是Cookie
&emsp;Cookie，有时也用其复数形式Cookies，指某些网站为了辨别用户身份、进行session跟踪而存储在用户本地终端上的数据。（通常经过加密，也可以称之为浏览器缓存）<br>
&emsp;由于HTTP是一种无状态的协议，服务器单从网络连接上无从知道客户身份。怎么办呢？就给客户端们颁发一个通行证吧，每人一个，无论谁访问都必须携带自己通行证。这样服务器就能从通行证上确认客户身份了。这就是Cookie的工作原理。<br>
&emsp;Cookie实际上是一小段的文本信息。客户端请求服务器，如果服务器需要记录该用户状态，就使用response向客户端浏览器颁发一个Cookie。客户端浏览器会把Cookie保存起来。当浏览器再请求该网站时，浏览器把请求的网址连同该Cookie一同提交给服务器。服务器检查该Cookie，以此来辨认用户状态。服务器还可以根据需要修改Cookie的内容。<br>
>Cookie总是保存在客户端中，按照存储位置可分为内存Cookie和硬盘Cookie。内存Cookie由浏览器维护，浏览器关闭就会消失。硬盘cookie保存在硬盘中，除非用户手动清理或过了有效期，硬盘Cookie不会被清除。

### Cookie属性
* name(string)<br>
&emsp;该Cookie的名称。Cookie一旦创建，名称便不可更改。
* value(object)<br>
&emsp;该Cookie的值。如果值为Unicode字符，需要为字符编码。如果值为二进制数据，则需要使用BASE64编码。
* Domain(string)<br>
&emsp;域，表示当前Cookie属于那个域或者子域下面<br>
&emsp;对于服务器返回的Set-Cookie中，如果没有指定Domain的值，那么其Domain的值是默认为当前所提交的http的请求所对应的主域名的。比如访问 `http://www.example.com`，返回一个cookie，没有指名domain值，那么其为值为默认的www.example.com。
* Path(string)<br>
&emsp;表示Cookie的所属路径<br>
* Expire time/Max-age(int)<br>
&emsp;表示Cookie的有效期<br>
&emsp;expire的值是一个时间，过了这个时间Cookie就会失效。max-age用来指定经过多长时间后失效。如果服务器返回的Cookie没有指定expire time，则表明此Cookie有效期为当前的session，session回话结束后就会失效。当关闭该页面时，此Cookie就会被浏览器删除。
* secure(boolean)<br>
&emsp;表示该Cookie只能用HTTPS传输。一般用于包含认证信息的Cookie应该用HTTPS传输。
* httponly<br>
&emsp;表示该Cookie只能用于HTTP或HTTPS传输。这意味着浏览器脚本（如JavaScript是不允许访问操作此Cookie）
* comment(string)<br>
&emsp;该Cookie用处说明。浏览器显示Cookie信息时显示该说明。
* version(int)<br>
&emsp;版本号。0表示遵循Netscape的Cookie规范，1表示遵循W3C的RFC 2109规范。

### Cookie的不可跨域名性
* 很多网站都会使用Cookie。例如，Google会向客户端颁发Cookie，Baidu也会向客户端颁发Cookie。那浏览器访问Google会不会也携带上Baidu颁发的Cookie呢？或者Google能不能修改Baidu颁发的Cookie呢？答案是否定的。<br>

`Cookie具有不可跨域名性。根据Cookie规范，浏览器访问Google只会携带Google的Cookie，而不会携带Baidu的Cookie。Google也只能操作Google的Cookie，而不能操作Baidu的Cookie。`

* Cookie在客户端是由浏览器来管理的。浏览器能够保证Google只会操作Google的Cookie而不会操作Baidu的Cookie，从而保证用户的隐私安全。浏览器判断一个网站是否能操作另一个网站Cookie的依据是域名。Google与Baidu的域名不一样，因此Google不能操作Baidu的Cookie。<br>

* 需要注意的是，虽然网站images.google.com与网站www.google.com同属于Google，但是域名不一样，二者同样不能互相操作彼此的Cookie。<br>
>注意：用户登录网站www.google.com之后会发现访问images.google.com时登录信息仍然有效，而普通的Cookie是做不到的。这是因为Google做了特殊处理。

## Session
### Session 机制
&emsp;除了使用Cookie，Web应用程序中还经常使用Session来记录客户端状态。`Session是服务器端使用的一种记录客户端状态的机制`，使用上比Cookie简单一些，相应的也`增加了服务器的存储压力`。
### 什么是Session
&emsp;Session是另一种记录客户状态的机制，不同的是Cookie保存在客户端浏览器中，而Session保存在服务器上。当用户通过浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上，这就是Session。客户端浏览器再次访问时只需要从该Session中查找该用户的状态就可以了。<br>
>如果说Cookie是通过检查客户的“通行证”来确认身份的话，Session就是通过检查保存在服务器上的“客户明细表”来核对客户身份。Session相当于程序在服务器上建立的一份客户档案，客户来访的时候只需要查询客户档案就可以了。
<br>
&emsp;当多个客户端执行程序时，服务器会保存多个客户端的Session。获取Session的时候也不需要声明获取谁的Session。Session机制决定了当前客户只会获取到自己的Session，各客户的Session也彼此独立，互不可见。
### Session的生命周期
&emsp;为了获取更高的存取速度，服务器一般把Session存在内存中，每个用户都有一个独立的Session。如果Session过于复杂，当大量用户访问服务器时可能会导致内存溢出，因此Session信息应该尽量精简。<br>

&emsp;Session在用户第一次访问服务器时自动创建。需要注意只有在访问JSP、Servlet等程序时才会创建，只访问HTML、IMAGE等静态资源时并不会创建。如果尚未生成Session，可以使用request.getSession(true)强制生成Session。<br>

&emsp;Session生成后，只要用户继续访问，服务器就会更新用户的最后访问时间，并维护该Session。用户每访问服务器一次，无论是否读写Session，服务器都认为该用户的Session“活跃(active)了”一次。
### Session的有效期
&emsp;由于会有越来越多的用户访问服务器，因此Session也会越来越多。为防止内存溢出，服务器会把长时间内没有活跃的Session从内存删除。这个时间就是Session的超时时间。如果超过了超时时间没访问过服务器，Session就自动失效了。
<br>

&emsp;Session的超时时间为maxInactiveInterval属性，可以通过对应的getMaxInactiveInterval()获取，通过setMaxInactiveInterval(longinterval)修改。<br>

&emsp;Session的超时时间也可以在web.xml中修改。另外，通过调用Session的invalidate()方法可以使Session失效。
### Session的常用方法
|方法名|描述|
|---|---|
|setAttribute(attribute,value)|设置Session属性。value参数可以为任何Java Object。通常为Java Bean。value信息不宜过大|
|getAttribute(attribute)|返回Session属性|
|getAttributeNames()|返回Session中存在的属性名|
|removeAttribute(attribute)|移除Session属性|
|getId()|返回Session的ID。该ID由服务器自动创建，不会重复|
|getCreationTime()|返回Session创建日期。返回类型为long,常被转化为Date类型，例如：Date createTime = new Date(session.get CreationTime())|
|getLastAccessedTime()|返回Session的最后活跃时间。返回类型为long|
|getMaxInactiveInterval()|返回Session的超时时间。单位为秒。超过该时间没有访问，服务器认为该Session失效|
|isNew()|返回该Session是否是新创建的|
| invalidate()|使该Session失效|
Tomcat中Session的默认超时时间为20分钟。通过setMaxInactiveInterval()修改超时时间。可以修改web.xml改变Session的默认超时时间。例如修改为60分钟：
```xml
<session-config>

   <session-timeout>60</session-timeout>      <!-- 单位：分钟 -->

</session-config>
```
>注意：<session-timeout>参数的单位为分钟，而setMaxInactiveInterval()单位为秒。
### Session&浏览器
&emsp;虽然Session保存在服务器，对客户端是透明的，它的正常运行仍然需要客户端浏览器的支持。这是因为Session需要使用Cookie作为识别标志。HTTP协议是无状态的，Session不能依据HTTP连接来判断是否为同一客户，因此服务器向客户端浏览器发送一个名为JSESSIONID的Cookie，它的值为该Session的id（也就是HttpSession.getId()的返回值）。Session依据该Cookie来识别是否为同一用户。<br>

&emsp;该Cookie为服务器自动生成的，它的maxAge属性一般为–1，表示仅当前浏览器内有效，并且各浏览器窗口间不共享，关闭浏览器就会失效。<br>

&emsp;因此同一机器的两个浏览器窗口访问服务器时，会生成两个不同的Session。但是由浏览器窗口内的链接、脚本等打开的新窗口（也就是说不是双击桌面浏览器图标等打开的窗口）除外。这类子窗口会共享父窗口的Cookie，因此会共享一个Session。<br>

>注意：新开的浏览器窗口会生成新的Session，但子窗口除外。子窗口会共用父窗口的Session。例如，在链接上右击，在弹出的快捷菜单中选择“在新窗口中打开”时，子窗口便可以访问父窗口的Session。<br>
如果客户端浏览器将Cookie功能禁用，或者不支持Cookie怎么办？例如，绝大多数的手机浏览器都不支持Cookie。Java Web提供了另一种解决方案：URL地址重写。
### URL地址重写
&emsp;URL地址重写是对客户端不支持Cookie的解决方案。URL地址重写的原理是将该用户Session的id信息重写到URL地址中。服务器能够解析重写后的URL获取Session的id。这样即使客户端不支持Cookie，也可以使用Session来记录用户状态。HttpServletResponse类提供了encodeURL(Stringurl)实现URL地址重写，例如：
```Java Web
<td>
    <a href="<%=response.encodeURL("index.jsp?c=1&wd=Java") %>">
    Homepage</a>
</td>
```
&emsp;该方法会自动判断客户端是否支持Cookie。如果客户端支持Cookie，会将URL原封不动地输出来。如果客户端不支持Cookie，则会将用户Session的id重写到URL中。重写后的输出可能是这样的：
```Java Web
<td>
    <a href="index.jsp;jsessionid=0CCD096E7F8D97B0BE608AFDC3E1931E?c=1&wd=Java">Homepage</a>
</td>
```
&emsp;即在文件名的后面，在URL参数的前面添加了字符串“;jsessionid=XXX”。其中XXX为Session的id。分析一下可以知道，增添的jsessionid字符串既不会影响请求的文件名，也不会影响提交的地址栏参数。用户单击这个链接的时候会把Session的id通过URL提交到服务器上，服务器通过解析URL地址获得Session的id。

&emsp;如果是页面重定向（Redirection），URL地址重写可以这样写：
```Java Web
<%
    if(“administrator”.equals(userName))
    {
       response.sendRedirect(response.encodeRedirectURL(“administrator.jsp”));
        return;
    }
%>
```
&emsp;效果跟response.encodeURL()是一样的：如果客户端支持Cookie，生成原URL地址，如果不支持Cookie，传回重写后的带有jsessionid字符串的地址。<br>

&emsp;对于WAP程序，由于大部分的手机浏览器都不支持Cookie，WAP程序都会采用URL地址重写来跟踪用户会话。

>注意：TOMCAT判断客户端浏览器是否支持Cookie的依据是请求中是否含有Cookie。尽管客户端可能会支持Cookie，但是由于第一次请求时不会携带任何Cookie（因为并无任何Cookie可以携带），URL地址重写后的地址中仍然会带有jsessionid。当第二次访问时服务器已经在浏览器中写入Cookie了，因此URL地址重写后的地址中就不会带有jsessionid了。
### Session中禁止使用Cookie
&emsp;既然WAP上大部分的客户浏览器都不支持Cookie，索性禁止Session使用Cookie，统一使用URL地址重写会更好一些。Java Web规范支持通过配置的方式禁用Cookie。下面举例说一下怎样通过配置禁止使用Cookie。<br>

&emsp;打开项目sessionWeb的WebRoot目录下的META-INF文件夹（跟WEB-INF文件夹同级，如果没有则创建），打开context.xml（如果没有则创建），编辑内容如下：
```xml
<?xml version='1.0' encoding='UTF-8'?>
<Context path="/sessionWeb"cookies="false">
</Context>
```
或者修改Tomcat全局的conf/context.xml，修改内容如下：
```xml
<!-- The contents of this file will be loaded for eachweb application -->
<Context cookies="false">
    <!-- ... 中间代码略 -->
</Context>
```
&emsp;部署后TOMCAT便不会自动生成名JSESSIONID的Cookie，Session也不会以Cookie为识别标志，而仅仅以重写后的URL地址为识别标志了。<br>
>注意：该配置只是禁止Session使用Cookie作为识别标志，并不能阻止其他的Cookie读写。也就是说服务器不会自动维护名为JSESSIONID的Cookie了，但是程序中仍然可以读写其他的Cookie。