# meta标签
## 定义
&emsp;META标签是HTML标记HEAD区的一个关键标签，它通常位于HTML文档的<head>和<title>之间。它提供的信息虽然用户不可见，但却是文档的最基本的元信息。<meta>除了提供文档字符集、使用语言、作者等基本信息外，还涉及对关键词和网页等级的设定。
>所以有关搜索引擎注册、搜索引擎优化排名等网络营销方法内容中，通常都要谈论META标签的作用，我们甚至可以说，META标签的内容设计对于搜索引擎营销来说是至关重要的一个因素，合理利用 Meta 标签的 Description 和Keywords 属性，加入网站的关键字或者网页的关键字，可使网站更加贴近用户体验。
## 属性
1. content属性
* content 属性提供了名称/值对中的值。该值可以是任何有效的字符串。
* content 属性始终要和 name 属性或 http-equiv 属性一起使用。

2. http-equiv 属性
&emsp;http-equiv 属性为名称/值对提供了名称。并指示服务器在发送实际的文档之前，在将要传送给浏览器的 MIME 文档头部包含名称/值对。
<br>
&emsp;当服务器向浏览器发送文档时，会先发送许多名称/值对。虽然有些服务器会发送许多这种名称/值对，但是所有服务器都至少要发送一个：content-type:text/html。这将告诉浏览器准备接受一个 HTML 文档。
<br>
&emsp;使用带有 http-equiv 属性的 <meta> 标签时，服务器将把名称/值对添加到发送给浏览器的内容头部。例如，添加：
```html
<meta http-equiv="charset" content="iso-8859-1">
<meta http-equiv="expires" content="31 Dec 2008">
```
这样发送到浏览器的头部就应该包含：
```
content-type: text/html
charset:iso-8859-1
expires:31 Dec 2008
```
&emsp;当然，只有浏览器可以接受这些附加的头部字段，并能以适当的方式使用它们时，这些字段才有意义。


3. name 属性

&emsp;name 属性提供了名称/值对中的名称。HTML 和 XHTML 标签都没有指定任何预先定义的 <meta> 名称。通常情况下，您可以自由使用对自己和源文档的读者来说富有意义的名称。
<br>
&emsp;"keywords" 是一个经常被用到的名称。它为文档定义了一组关键字。某些搜索引擎在遇到这些关键字时，会用这些关键字对文档进行分类。
<br>
&emsp;类似这样的 meta 标签可能对于进入搜索引擎的索引有帮助：
```html
<meta name="keywords" content="HTML,ASP,PHP,SQL">
```
&emsp;如果没有提供 name 属性，那么名称/值对中的名称会采用 http-equiv 属性的值。

4. scheme 属性(html5不支持)

&emsp;scheme 属性用于指定要用来翻译属性值的方案。此方案应该在由 <head> 标签的 profile 属性指定的概况文件中进行了定义。
## 属性详解
### http-equiv
##### http-equiv相当于HTTP文档的文件头作用，它可以向浏览器传回一些有用的信息，以帮助浏览器正确的显示网页内容。
|值|描述|示例|
|:---|:---|:---|
|content-type|设定页面使用的字符集|`<meta http-equiv="content-Type" content="text/html; charset=utf-8">`<br>GB2312时，代表说明网站是采用的编码是简体中文；<br>ISO-8859-1时，代表说明网站是采用的编码是英文；<br>UTF-8时，代表世界通用的语言编码；<br>PS：html5页面的做法是直接使用`<meta charset="utf-8"/>`|
|X-UA-Compatible|IE8的专用标记，用来指定IE8浏览器去模拟某个特定版本的IE浏览器的渲染方式，以此来解决部分兼容问题。|`<meta http-equiv="X-UA-Compatible" content="IE=8">  `<br>以上代码告诉IE浏览器，IE8/9都会以IE8引擎来渲染页面。  <br>`<meta http-equiv="X-UA-Compatible" content="IE=edge"> ` <br>以上代码告诉IE浏览器，IE8/9及以后的版本都会以最高版本IE来渲染页面。 |
|expires|设定网页的过期时间|`<meta http-equiv="expires"content="Fri,12Jan200118:18:18GMT">`<br>PS：必须使用GMT的时间格式|
|refresh|自动刷新并指向某页面|`<meta http-equiv="Refresh" content="2;URL=https://www.baidu.com">`<br>PS：2代表页面停留2秒后跳转到后面的网址上|
|set-cookie|如果网页过期，那么自动删除本地cookie|`<meta http-equiv="Set-Cookie"content="cookie value=xxx;expires=Friday,12-Jan-200118:18:18GMT；path=/">`<br>PS：必须使用GMT的时间格式。|
|windows-target|强制页面在当前窗口中以独立页面显示，可以防止自己的网页被别人当作一个frame页调用|`<meta http-equiv="Window-target" content="_top">`|
|cache-control|缓存机制|`<meta http-equiv="cache-control" content="no-cache">`<br>Public：指示响应可被任何缓存区缓存。<br>Private：指示对于单个用户的整个或部分响应消息，不能被共享缓存处理。这允许服务器仅仅描述当用户的部分响应消息，此响应消息对于其他用户的请求无效。<br>no-cache：指示请求或响应消息不能缓存。<br>no-store：用于防止重要的信息被无意的发布。在请求消息中发送将使得请求和响应消息都不使用缓存。<br>max-age：指示客户机可以接收生存期不大于指定时间（以秒为单位）的响应。<br>min-fresh：指示客户机可以接收响应时间小于当前时间加上指定时间的响应。<br>max-stale：指示客户机可以接收超出超时期间的响应消息。如果指定max-stale消息的值，那么客户机可以接收超出超时期指定值之内的响应消息。|
### name
##### name属性主要用于描述网页，与之对应的属性值为content，content中的内容主要是便于搜索引擎机器人查找信息和分类信息用的。
|值|描述|示例|
|:---|:---|:---|
|author|标注网页作者|`<meta name="author" content="dashen" />`|
|keywords|关键词，用于被搜索引擎收录|`<meta name="keywords" content="新闻,新闻中心, 新闻频道">`|
|description|描述，用于被搜索引擎收录|`<meta name="description" content="新闻中心,包含有时政新闻、国内新闻、国际新闻、社会新闻、时事评论、新闻图片、新闻专题、新闻论坛、军事、历史、的专业时事报道门户网站">`|
|viewport|用于移动端显示优化，控制页面缩放|`<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">`<br>width: 设置layout viewport  的宽度，为一个正整数，或特殊值"device-width"(单位为缩放为 100% 时的 CSS 的像素)<br>height: 设置layout viewport  的高度，一般设置了宽度，会自动解析出高度，可以不用设置<br>initial-scale: 设置页面的初始缩放值，也即是当页面第一次 load 的时候缩放比例，为一个数字，可以带小数<br>minimum-scale: 允许用户的最小缩放值，为一个数字，可以带小数<br>maximum-scale: 允许用户的最大缩放值，为一个数字，可以带小数<br>user-scalable: 是否允许用户进行缩放，值为 "no" 或 "yes"|
|renderer|指定双核浏览器默认以何种方式渲染页面。|`<meta name="renderer" content="webkit">//默认webkit内核`<br>`<meta name="renderer" content="ie-comp">//默认IE兼容模式`<br>`<meta name="renderer" content="ie-stand">//默认IE标准模式`|
|generator|说明网站的采用的什么软件制作|`<meta name="generator" content="Microsoft"/>`|
|revised|网页文档修改时间|`<meta name="revised" content="设计网, 6/24/2015"/>`|
|robots|用来告诉搜索机器人哪些页面需要索引，哪些页面不需要索引。|`<meta name="robots" content="none"/>`<br>all：文件将被检索，且页面上的链接可以被查询；(默认值)<br>none：文件将不被检索，且页面上的链接不可以被查询；<br>index：文件将被检索；<br>follow：页面上的链接可以被查询；<br>noindex：文件将不被检索，但页面上的链接可以被查询；<br>nofollow：文件将不被检索，页面上的链接可以被查询。|
|copyright|网站版权信息|`<meta name="copyright" content="本页版权XXX所有。All Rights Reserved" />`|