# GeneralKnowledge
some knowledge about web


会话（Session）跟踪是Web程序中常用的技术，用来跟踪用户的整个会话。常用的会话跟踪技术是Cookie与Session。
Cookie通过在客户端记录信息确定用户身份，Session通过在服务器端记录信息确定用户身份。

## Cookie
理论上，一个用户的所有请求操作都应该属于同一个会话（session）,而另一个用户的说有请求则应该属于另一个会话，二者不能混淆。
而Web应用程序是实用HTTP协议传输数据。HTTP协议是无状态的协议。一旦数据交换完毕，客户端与服务端的连接就会关闭，再次交换数据
需要建立新的链接。这就意味着服务器无法从连接上跟踪会话。需要出现一种机制来跟踪会话，Cookie就是这样的一种机制。它可以弥补HTTP
协议无状态的不足。在Session出现之前，网站都是用Cookie来跟踪会话的。

- 什么是Cookie
由于HTTP是一种无状态的协议，服务器单从网络连接上无从知道客户身份。怎么办呢？就给客户端们颁发一个通行证吧，每人一个，
无论谁访问都必须携带自己通行证。这样服务器就能从通行证上确认客户身份了。这就是Cookie的工作原理。
客户端请求服务器，如果服务器需要记录该用户状态，就使用response向客户端浏览器颁发一个Cookie。
客户端浏览器会把Cookie保存起来。当浏览器再请求该网站时，浏览器把请求的网址连同该Cookie一同提交给服务器。
服务器检查该Cookie，以此来辨认用户状态。服务器还可以根据需要修改Cookie的内容。
ps：查看某个网站颁发的Cookie很简单。在浏览器地址栏输入javascript:alert (document. cookie)就可以了（需要有网才能查看）

  注意：Cookie功能需要浏览器的支持。如果浏览器不支持cookie（如大部分手机浏览器）或者把cookie禁用来，cookie就会失效。
不同的浏览器采用不同的方式保存cookie。

- Cookie的不可跨域名性
需要注意的是，虽然网站images.google.com与网站www.google.com同属于Google，但是域名不一样，二者同样不能互相操作彼此的Cookie。

  注意：用户登录网站www.google.com之后会发现访问images.google.com时登录信息仍然有效，而普通的Cookie是做不到的。
这是因为Google做了特殊处理。本章后面也会对Cookie做类似的处理。

- Unicode编码：保存中文
中文与英文字符不同，中文属于Unicode字符，在内存中占4个字符，而英文属于ASCII字符，内存中只占2个字节。
Cookie中使用Unicode字符时需要对Unicode字符进行编码，否则会乱码。

- BASE64编码：保存二进制图片
Cookie不仅可以使用ASCII字符与Unicode字符，还可以使用二进制数据。例如在Cookie中使用数字证书，提供安全度。
使用二进制数据时也需要进行编码。

  注意：本程序仅用于展示Cookie中可以存储二进制内容，并不实用。由于浏览器每次请求服务器都会携带Cookie，
因此Cookie内容不宜过多，否则影响速度。Cookie的内容应该少而精。

- Cookie的有效期
该Cookie失效的时间，单位秒。如果为正数，则该Cookie在maxAge秒之后失效。如果为负数，该Cookie为临时Cookie，
关闭浏览器即失效，浏览器也不会以任何形式保存该Cookie。如果为0，表示删除该Cookie。默认为–1
如果maxAge为负数，则表示该Cookie仅在本浏览器窗口以及本窗口打开的子窗口内有效，关闭窗口后该Cookie即失效。
maxAge为负数的Cookie，为临时性Cookie，不会被持久化，不会被写到Cookie文件中。Cookie信息保存在浏览器内存中，
因此关闭浏览器该Cookie就消失了。Cookie默认的maxAge值为–1。
如果maxAge为0，则表示删除该Cookie。Cookie机制没有提供删除Cookie的方法，因此通过设置该Cookie即时失效实现删除Cookie的效果。
失效的Cookie会被浏览器从Cookie文件或者内存中删除。
response对象提供的Cookie操作方法只有一个添加操作add(Cookie cookie)。
要想修改Cookie只能使用一个同名的Cookie来覆盖原来的Cookie，达到修改的目的。删除时只需要把maxAge修改为0即可。

  注意：从客户端读取Cookie时，包括maxAge在内的其他属性都是不可读的，也不会被提交。浏览器提交Cookie时只会提交name与value属性。
maxAge属性只被浏览器用来判断Cookie是否过期。 亦可操作 expires 参数使cookie过期。

  注意：修改、删除Cookie时，新建的Cookie除value、maxAge之外的所有属性，例如name、path、domain等，都要与原Cookie完全一样。
否则，浏览器将视为两个不同的Cookie不予覆盖，导致修改、删除失败。

- Cookie的域名
正常情况下，同一个一级域名下的两个二级域名如www.helloweenvsfei.com和images.helloweenvsfei.com也不能交互使用Cookie，
因为二者的域名并不严格相同。如果想所有helloweenvsfei.com名下的二级域名都可以使用该Cookie，需要设置Cookie的domain参数，
例如：
Cookie cookie = new Cookie("time","20080808"); // 新建Cookie
cookie.setDomain(".helloweenvsfei.com");           // 设置域名
cookie.setPath("/");                              // 设置路径
cookie.setMaxAge(Integer.MAX_VALUE);               // 设置有效期
response.addCookie(cookie);                       // 输出到客户端

  注意：domain参数必须以点(".")开始。另外，name相同但domain不同的两个Cookie是两个不同的Cookie。如果想要两个域名完全不同的
网站共有Cookie，可以生成两个Cookie，domain属性分别为两个域名，输出到客户端

- Cookie的路径
domain属性决定运行访问Cookie的域名，而path属性决定允许访问Cookie的路径（ContextPath）。
例如，如果只允许/sessionWeb/下的程序使用Cookie，可以这么写：
Cookie cookie = new Cookie("time","20080808");     // 新建Cookie
cookie.setPath("/session/");                          // 设置路径
response.addCookie(cookie);                           // 输出到客户端
设置为“/”时允许所有路径使用Cookie。path属性需要使用符号“/”结尾。name相同但domain相同的两个Cookie也是两个不同的Cookie。

  注意：页面只能获取它属于的Path的Cookie。例如/session/test/a.jsp不能获取到路径为/session/abc/的Cookie。使用时一定要注意。

- Cookie的安全属性
如果不希望Cookie在HTTP等非安全协议中传输，可以设置Cookie的secure属性为true。浏览器只会在HTTPS和SSL等安全协议中传输此类Cookie。
  提示：secure属性并不能对Cookie内容加密，因而不能保证绝对的安全性。如果需要高安全性，需要在程序中对Cookie内容加密、解密，以防泄密。

## Session
除了使用Cookie，Web应用程序中还经常使用Session来记录客户端状态。Session是服务器端使用的一种记录客户端状态的机制，
使用上比Cookie简单一些，相应的也增加了服务器的存储压力。

- 什么是Sesson
Session是另一种记录客户状态的机制，不同的是Cookie保存在客户端浏览器中，而Session保存在服务器上。客户端浏览器访问服务器的时候，
服务器把客户端信息以某种形式记录在服务器上。这就是Session。客户端浏览器再次访问时只需要从该Session中查找该客户的状态就可以了。
如果说Cookie机制是通过检查客户身上的“通行证”来确定客户身份的话，那么Session机制就是通过检查服务器上的“客户明细表”来确认客户
身份。Session相当于程序在服务器上建立的一份客户档案，客户来访的时候只需要查询客户档案表就可以了。
Session机制决定了当前客户只会获取到自己的Session，而不会获取到别人的Session。各客户的Session也彼此独立，互不可见。

  提示：Session的使用比Cookie方便，但是过多的Session存储在服务器内存中，会对服务器造成压力。

- Session的生命周期
Session保存在服务器端。为了获得更高的存取速度，服务器一般把Session放在内存里。每个用户都会有一个独立的Session。
如果Session内容过于复杂，当大量客户访问服务器时可能会导致内存溢出。因此，Session里的信息应该尽量精简。
Session在用户第一次访问服务器的时候自动创建。Session生成后，只要用户继续访问，服务器就会更新Session的最后访问时间，
并维护该Session。用户每访问服务器一次，无论是否读写Session，服务器都认为该用户的Session“活跃（active）”了一次。
由于会有越来越多的用户访问服务器，因此Session也会越来越多。为防止内存溢出，服务器会把长时间内没有活跃的Session从内存删除。
这个时间就是Session的超时时间。如果超过了超时时间没访问过服务器，Session就自动失效了。
 
-   Session对浏览器的要求
虽然Session保存在服务器，对客户端是透明的，它的正常运行仍然需要客户端浏览器的支持。这是因为Session需要使用Cookie作为识别标志。
HTTP协议是无状态的，Session不能依据HTTP连接来判断是否为同一客户，因此服务器向客户端浏览器发送一个名为JSESSIONID的Cookie，
它的值为该Session的id（也就是HttpSession.getId()的返回值）。Session依据该Cookie来识别是否为同一用户。
该Cookie为服务器自动生成的，它的maxAge属性一般为–1，表示仅当前浏览器内有效，并且各浏览器窗口间不共享，关闭浏览器就会失效。
因此同一机器的两个浏览器窗口访问服务器时，会生成两个不同的Session。但是由浏览器窗口内的链接、脚本等打开的新窗口
（也就是说不是双击桌面浏览器图标等打开的窗口）除外。这类子窗口会共享父窗口的Cookie，因此会共享一个Session。

  注意：新开的浏览器窗口会生成新的Session，但子窗口除外。子窗口会共用父窗口的Session。例如，在链接上右击，在弹出的快捷菜单中选择“在新窗口中打开”时，子窗口便可以访问父窗口的Session。如果客户端浏览器将Cookie功能禁用，或者不支持Cookie怎么办？例如，绝大多数的手机浏览器都不支持Cookie。Java Web提供了另一种解决方案：URL地址重写。服务端操作，在此省略。


### sessionStrage & localStrage

HTML5 提供了两种在客户端存储数据的新方法：
localStorage - 没有时间限制的数据存储
sessionStorage - 针对一个 session 的数据存储
sessionStorage不是一种持久化存储，浏览器关闭之后会随之清除。而localStorage用于持久化的本地存储，除非主动删除数据，否则数据是永远不会过期的。
sessionStorage数据的存储仅特定于某个会话中，也就是说数据只保持到浏览器关闭，当浏览器关闭后重新打开这个页面时，之前的存储已经被清除。而 localStorage 是一个持久化的存储，它并不局限于会话。

### indexDB
indexDB是一种轻量级NOSQL数据库。相比web sql(sqlite)更加高效，包括索引、事务处理和健壮的查询功能。

它的特点包括：
一个网站可能有一个或多个 IndexedDB 数据库，每个数据库必须具有惟一的名称。
一个数据库可包含一个或多个对象存储。一个对象存储（由一个名称惟一标识）是一个记录集合。每个记录有一个键 和一个值。该值是一个对象，可拥有一个或多个属性。键可能基于某个键生成器，从一个键路径衍生出来，或者是显式设置。一个键生成器自动生成惟一的连续正整数。键路径定义了键值的路径。它可以是单个 JavaScript 标识符或多个由句点分隔的标识符。(有点像列数据库的特点)
IndexedDB中，几乎所有的操作都是采用了command->request->result的方式。比如查询一条记录，返回一个request，在request的result中得到查询结果。又比如打开数据库，返回一个request，在request的result中得到返回的数据库引用。
indexedDB需要放到web服务器上才可以运行。










