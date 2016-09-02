# cross-domain-request-3
跨域请求解决方案三 html5-postMessage
## postMessage介绍
window.postMessage 是一个安全的跨源通信的方法。一般情况下，当且仅当执行脚本的页面使用相同的协议（通常都是 http）、相同的端口（http默认使用80端口）和相同的 host（两个页面的 document.domain 的值相同）时，才允许不同页面上的脚本互相访问。 window.postMessage 提供了一个可控的机制来安全地绕过这一限制，当其在正确使用的情况下。
##语法

    otherWindow.postMessage(message, targetOrigin);
otherWindow

其他窗口的一个引用，比如iframe的contentWindow属性、执行window.open返回的窗口对象、或者是命名过或数值索引的window.frames。

message

将要发送到其他 window的数据。将会被结构化克隆算法序列化。这意味着你可不受什么限制的安全传送数据对象给目标窗口而无需自己序列化。

targetOrigin

通过窗口的origin属性来指定哪些窗口能接收到消息事件，其值可以是字符串"\*"（表示无限制）或者一个URI。在发送消息的时候，如果目标窗
口的协议、主机地址或端口这三者的任意一项不匹配targetOrigin提供的值，那么消息就不会被发送；只有三者完全匹配，消息才会被发送。这
个机制用来控制消息可以发送到哪些窗口；例如，当用postMessage传送密码时，这个参数就显得尤为重要，必须保证它的值与这条包含密码的
信息的预期接受者的orign属性完全一致，来防止密码被恶意的第三方截获。如果你明确的知道消息应该发送到哪个窗口，那么请始终提供一个
有确切值的targetOrigin，而不是*。不提供确切的目标将导致数据泄露到任何对数据感兴趣的恶意站点。

transfer 可选

Is a sequence of Transferable objects that are transferred with the message. The ownership of these objects is given to 
the destination side and they are no longer usable on the sending side.

执行如下代码, 其他window可以监听派遣的message:

    window.addEventListener("message", receiveMessage, false);
    
    function receiveMessage(event)
    {
    // For Chrome, the origin property is in the event.originalEvent
    // object.
    var origin = event.origin || event.originalEvent.origin; 
    if (event.origin !== "http://example.org:8080")
      return;
  
    // ...
        }
信息的属性包括以下几个：

data

The object passed from the other window.

origin

The origin of the window that sent the message at the time postMessage was called. This string is
the concatenation of the protocol and "://", the host name if one exists, and ":" followed by a port
number if a port is present and differs from the default port for the given protocol. Examples of typical
origins are https://example.org (implying port 443), http://example.net (implying port 80), 
and http://example.com:8080. Note that this origin is not guaranteed to be the current or future origin
of that window, which might have been navigated to a different location since postMessage was called.

source

A reference to the window object that sent the message; you can use this to establish two-way communication 
between two windows with different origins.

##DEMO说明
localhost:3000/app.html向localhost:4000/data.html发送信息
