---
title: JS同源限制
cover : https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=3693300169,869948815&fm=26&gp=0.jpg
---
# js同源限制
所谓同源指的是“三个相同”
#### .协议相同
#### .域名相同
#### .端口相同
举例来说，`http://www.example.com/dir/page.html`这个网址，协议是http://，域名是`www.example.com`，端口是`80`（默认端口可以省略），它的同源情况如下。

`http://www.example.com/dir2/other.html：同源`
`http://example.com/dir/other.html：不同源（域名不同）`
`http://v2.www.example.com/dir/other.html：不同源（域名不同）`
`http://www.example.com:81/dir/other.html：不同源（端口不同）`
`https://www.example.com/dir/page.html：不同源（协议不同）`
# s目的
同源政策的目的是为了保证用户信息的安全。防止恶意的网站窃取数据
设想这样一种情况：A 网站是一家银行，用户登录以后，A 网站在用户的机器上设置了一个 Cookie，包含了一些隐私信息（比如存款总额）。用户离开 A 网站以后，又去访问 B 网站，如果没有同源限制，B 网站可以读取 A 网站的 Cookie，那么隐私信息就会泄漏。更可怕的是，Cookie 往往用来保存用户的登录状态，如果用户没有退出登录，其他网站就可以冒充用户，为所欲为。因为浏览器同时还规定，提交表单不受同源政策的限制。

由此可见，同源政策是必需的，否则 Cookie 可以共享，互联网就毫无安全可言了。



随着互联网的发展，同源政策越来越严格。目前，如果非同源，共有三种行为受到限制。

1.  `无法读取非同源网页的 Cookie、LocalStorage 和 IndexedDB。`
2. ` 无法接触非同源网页的 DOM。`
3.  `无法向非同源地址发送 AJAX 请求（可以发送，但浏览器会拒绝接收响应）`

另外，通过 JavaScript 脚本可以拿到其他窗口的window对象。如果是非同源的网页，目前允许一个窗口可以接触其他网页的window对象的九个属性和四个方法：window.closed、window.frames、window.length、window.location、window.opener、window.parent、window.self、window.top、window.window、window.blur()、window.close()、window.focus()、window.postMessage()。

上面的九个属性之中，只有`window.location`是可读写的，其他八个全部都是只读。而且，即使是location对象，非同源的情况下，也只允许调用```location.replace```方法和写入```location.href```属性。

虽然这些限制是必要的，但是有时很不方便，合理的用途也受到影响。下面介绍如何规避上面的限制。
# cookie
`Cookie` 是服务器写入浏览器的一小段信息，只有同源的网页才能共享。如果两个网页一级域名相同，只是次级域名不同，浏览器允许通过设置```document.domain```共享 ```Cookie。```

举例来说，A 网页的网址是`http://w1.example.com/a.html`，B 网页的网址是`http://w2.example.com/b.html`，那么只要设置相同的document.domain，两个网页就可以共享 Cookie。因为浏览器通过document.domain属性来检查是否同源。

``` html
// 两个网页都需要设置
document.domain = 'example.com';
```
注意，A 和 B 两个网页都需要设置document.domain属性，才能达到同源的目的。因为设置document.domain的同时，会把端口重置为null，因此如果只设置一个网页的document.domain，会导致两个网址的端口不同，还是达不到同源的目的。
现在，A 网页通过脚本设置一个 Cookie。

```HTML
document.cookie = "test1=hello";

```
B 网页就可以读到这个 Cookie。
```html
var allCookie = document.cookie;

```
注意，这种方法只适用于 Cookie 和 iframe 窗口，LocalStorage 和 IndexedDB 无法通过这种方法，规避同源政策，而要使用PostMessage API。

另外，服务器也可以在设置 Cookie 的时候，指定 Cookie 的所属域名为一级域名，比如.example.com。
```html
Set-Cookie: key=value; domain=.example.com; path=/

```
这样的话，二级域名和三级域名不用做任何设置，都可以读取这个 Cookie。
#iframe 和多窗口通信
`iframe`元素可以在当前网页之中，嵌入其他网页。每个```iframe```元素形成自己的窗口，即有自己的```window```对象。```iframe```窗口之中的脚本，可以获得父窗口和子窗口。但是，只有在同源的情况下，父窗口和子窗口才能通信；如果跨域，就无法拿到对方的 ```DOM```。

比如，父窗口运行下面的命令，如果```iframe```窗口不是同源，就会报错。
```html
document.getElementById("myIFrame").contentWindow.document
// Uncaught DOMException: Blocked a frame from accessing a cross-origin frame.
```
上面命令中，父窗口想获取子窗口的 DOM，因为跨域导致报错。

反之亦然，子窗口获取主窗口的 DOM 也会报错。
```html
window.parent.document.body // 报错

```
这种情况不仅适用于iframe窗口，还适用于window.open方法打开的窗口，只要跨域，父窗口与子窗口之间就无法通信。

如果两个窗口一级域名相同，只是二级域名不同，那么设置document.domain属性，就可以规避同源政策，拿到 DOM。

对于完全不同源的网站，目前有两种方法，可以解决跨域窗口的通信问题。

片段识别符（fragment identifier）
跨文档通信API（Cross-document messaging）
片段标识符指的是，URL 的#号后面的部分，比如http://example.com/x.html#fragment的#fragment。如果只是改变片段标识符，页面不会重新刷新。

父窗口可以把信息，写入子窗口的片段标识符。

```html
var src = originURL + '#' + data;
document.getElementById('myIFrame').src = src;

```
上面代码中，父窗口把所要传递的信息，写入iframe窗口的片段标识符。

子窗口通过监听hashchange事件得到通知。
```html
window.onhashchange = checkMessage;

function checkMessage() {
  var message = window.location.hash;
  // ...
}
```
同样的，子窗口也可以改变父窗口的片段标识符。
```html

parent.location.href = target + '#' + hash;

```
# window.postMessage()
上面的这种方法属于破解，HTML5 为了解决这个问题，引入了一个全新的API：跨文档通信 API（Cross-document messaging）。

这个 API 为window对象新增了一个window.postMessage方法，允许跨窗口通信，不论这两个窗口是否同源。举例来说，父窗口aaa.com向子窗口bbb.com发消息，调用postMessage方法就可以了。
```html
// 父窗口打开一个子窗口
var popup = window.open('http://bbb.com', 'title');
// 父窗口向子窗口发消息
popup.postMessage('Hello World!', 'http://bbb.com');

```
`postMessage`方法的第一个参数是具体的信息内容，第二个参数是接收消息的窗口的源（origin），即“协议 + 域名 + 端口”。也可以设为*，表示不限制域名，向所有窗口发送。
子窗口向父窗口发送消息的写法类似。
```html
// 子窗口向父窗口发消息
window.opener.postMessage('Nice to see you', 'http://aaa.com');

```
父窗口和子窗口都可以通过message事件，监听对方的消息。
```html
// 父窗口和子窗口都可以用下面的代码，
// 监听 message 消息
window.addEventListener('message', function (e) {
  console.log(e.data);
},false);
```

