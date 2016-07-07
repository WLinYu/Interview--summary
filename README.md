# Interview--summary
###1.页面输入url到页面加载完成显示的过程。

    1.发送一个url请求时，浏览器会开启一个线程来处理这个请求，t同时在远程的DNS服务器上启动一个DSN查询，使浏览器获得请求对应的IP。
    2.浏览器与远程的web服务通过TCP三次握手协商来建立一个TCP/IP连接。握手包括：一个同步报文，一个同步-应答报文，一个应答报文（在浏览器和服务器之间传递）。
    3.TCP/IP连接建立后，浏览器通过该链接向远程服务器发送http的get请求。远程服务器找到资源并使用http响应返回该资源（值为200的http响应表示一个正确的响应）。
    4.web服务器提供资源服务，客户端开始下载资源。
    5.请求返回后，浏览器会解析html生成DOM Tree,其次会根据CSS生成CSS Rule Tree，而javascript可根据DOM API操作DOM。

###2.跨域问题

    1.JSONP：
    原理：动态的插入script标签，通过script标签引入一个js文件，这个js文件载入成功后，会执行我们在url参数中指定的函数，并且会把我们需要的json数据作为参数传入。只支持get请求。（遵循同源策略：域名、协议、端口相同，是一种安全协议）
    2.CORS：
    设置Acess-Control-Allow-Origin来进行的。如果浏览器检测到相应的设置，就可以允许Ajax进行跨域的访问.
    3.document.domain:
    通过修改document.domain来跨子域（子域和主域的document.domain设为同一个主域,主域相同的使用document.domain）.前提条件：这两个域名必须属于同一个基础域名!而且所用的协议，端口都要一致，否则无法利用document.domain进行跨域
    4.Window.name:
    通过window.name来进行跨域，窗口载入的所有页面共享一个window.name，具有读写权限。
    5.window.postMessage
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
###4.性能优化
代码层面：避免使用CSS表达式，避免使用高级选择器，通配选择器。
缓存利用：缓存Ajax，使用CDN，使用外部js和css文件以便缓存，添加Expires头，服务端配置Etag，减少DNC查找等。
请求数量：合并样式脚本，使用css图片精灵，初始首屏之外的图片资源按需加载静态资源延迟加载。
请求带宽：压缩文件，开启GZIP。

**代码层面的优化**：
>   
    1.用hash-table来优化查找
    2.少用全局变量
    3.用innerHTML代替DOM操作，减少DOM操作次数，优化javascript性能
    4.用setTimeout来避免页面失去响应
    5.缓存DOM节点查找的结果
    6.避免使用CSS Expression
    7.避免全局查询
    8.避免使用with（with会创建自己的作用域，增加作用域链长度）
    9.多个变量声明合并
    10.避免图片和iFrame等的空src。空src会重新加载当前页面，影响速度和效率
    11.尽量避免在HTML标签中写Style属性
    
**减少页面加载时间的方法**：
（加载时间是指:感知的时间或者实际加载时间）
>   1.优化图片
    2.图像格式选择（GIF：提供的颜色较少，可用在一些对颜色要求不高的地方）
    3.优化CSS（压缩合并CSS，如margin-top，margin-left...）
    4.网址后加斜杆（如www.***.com/目录,会判断这个“目录是什么文件类型，或者是目录”）
    5.标明高度和宽度（如果浏览器没有找到这2个参数，她需要一边下载图片一边计算大小，如果图片很多，浏览器需要不断地调整页面。
    这不但影响速度，也影响浏览器体验。当浏览器知道了高度和宽度参数后，即使图片暂时无法显示，页面也会腾出图片的空位，然后继续加载后面的内容。从而加载时间快了，浏览器体验更好了）
    6.减少http请求（合并文件，合并图片）

**网站的文件和资源进行优化**：
>   文件合并、文件最小化/文件压缩、使用托管CDN，缓存的使用（多个域名来提供缓存）
