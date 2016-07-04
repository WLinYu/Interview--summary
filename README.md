# Interview--summary
###1.页面输入url到页面加载完成显示的过程。

    1.发送一个url请求时，浏览器会开启一个线程来处理这个请求，t同时在远程的DNS服务器上启动一个DSN查询，使浏览器获得请求对应的IP。
    2.浏览器与远程的web服务通过TCP三次握手协商来建立一个TCP/IP连接。握手包括：一个同步报文，一个同步-应答报文，一个应答报文（在浏览器和服务器之间传递）。
    3.TCP/IP连接建立后，浏览器通过该链接向远程服务器发送http的get请求。远程服务器找到资源并使用http响应返回该资源（值为200的http响应表示一个正确的响应）。
    4.web服务器提供资源服务，客户端开始下载资源。
    5.请求返回后，浏览器会解析html生成DOM Tree,其次会根据CSS生成CSS Rule Tree，而javascript可根据DOM API操作DOM。

###2.跨域问题

    JSONP：
    原理：动态的插入script标签，通过script标签引入一个js文件，这个js文件载入成功后，会执行我们在url参数中指定的函数，并且会把我们需要的json数据作为参数传入。只支持get请求。（遵循同源策略：域名、协议、端口相同，是一种安全协议）
    CORS：
    设置Acess-Control-Allow-Origin来进行的。如果浏览器检测到相应的设置，就可以允许Ajax进行跨域的访问.
    document.domain:
    通过修改document.domain来跨子域（子域和主域的document.domain设为同一个主域,主域相同的使用document.domain）.前提条件：这两个域名必须属于同一个基础域名!而且所用的协议，端口都要一致，否则无法利用document.domain进行跨域
    Window.name:
    通过window.name来进行跨域，窗口载入的所有页面共享一个window.name，具有读写权限。
    window.postMessage
    使用HTML5新引进的window.postMessage方法来进行跨域传送数据。
    
###3.阻止事件冒泡和浏览器默认行为
```javascript
//阻止事件冒泡
    function stopEventBubble(event){
        var e=event || window.event;//事件兼容

        if (e && e.stopPropagation){
            e.stopPropagation();    
        }
        else{
            e.cancelBubble=true;//IE的阻止冒泡事件
        }
    }
//阻止浏览器的默认行为 
    function stopDefault( event ) { 
        var e=event || window.event;//事件兼容
        
        //阻止默认浏览器动作(W3C) 
         if ( e && e.preventDefault ){
            e.preventDefault(); 
        }
        //IE中阻止函数器默认动作的方式 
        else{
            window.event.returnValue = false; 
        }
    return false; 
}
```

    
