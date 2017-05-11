# RMWebView
=====

学习native和web的交互，再上一各项目中我使用的WKWebView，在这片中，我使用传统的UIWebView，一次想成对比
-----

>随着H5的强大，hybrid app已经成为当前互联网的大方向，单纯的native app和web app在某些方面显得就很劣势。关于H5的发展史，这里有一篇文章推荐给大家，今天我们来学习最基础的基于iOS系统的OC与JS之间是如何进行交互的，本文介绍的是基于UIWebView"协议拦截"实现的交互方式，当然后面还会循序渐进的介绍其他的交互方式。这里的说到的JS指的是广义上JS，并不是单纯的javascript，你可以理解为web前端的三件套(html+css+javascript)；这里说的OC指的是iOS的系统语言Objective-C，为什么叫做OC与JS交互而不是iOS与JS交互或者其他名字，这个不是重点，也有叫web交互，H5交互的

1、OC与JS交互是双向的，一方面是OC向JS发送消息，另一方面是JS向OC发送消息。代码上的表现形式就是方法的相互调用，分为两种：

##一、OC调用JS方法

>- (nullable NSString *)stringByEvaluatingJavaScriptFromString:(NSString *)script;

调用这个方法需要在网页加载完成之后，因为这个时候整个html页面包括js/css已经注入到webView中，此时调用方法才会有响应，相反网页加载完成之前调用界面不会有任何响应

这是UIWebView在加载之前或者网页进行重定向的时候调用的一个方法，而我们JS调用OC采用协议拦截方式实现的细节就是在这个方法里面完成的

有了上面的方法后，很显然，想要JS调用OC我们就可以采用在按钮点击的后重定向一个URL，这个URL携带OC的方法名及参数信息，然后在这个方法中拿到对应的URL，对URL进行解析，提取对应的方法名和参数信息，调用OC相应的方法，从而实现了交互的可能，下面是示例中的URL

>rrcc://showSendNumber_msg_?13300001111&go climbing this weekend

在这个URL中前面的"rrcc://"是URL的scheme，通过这个来提取我们关心的URL，对其他URL不做任何处理，后面就是OC方法和参数的信息了，这里用"?"来分割分割方法名和参数，"&"来分割多个参数，"_"用作OC方法名中冒号的替换。如果你愿意，可以使用任何几个字符来定义这个规则，这里采用的URL中经常会见到的字符。下面贴出部分示例代码
