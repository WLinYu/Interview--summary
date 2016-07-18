#关于javascript的相关算法题
1.object prototype
  定义一个函数spacify，将字符串作为参数传入，然后返回这个带空格间隔的字符串
  eg：`spacify('hello world') // => 'h e l l o  w o r l d'`
  除开for循环，答案为：
```
function spacify(str){
  return str.split('').join(' ');
}
```
  如何将这个函数直接作用在字符串对象上：
  eg:`'hello world'.spacify();`
  (这个问题考察了对原型链的理解,直接在原型链上定义属性的危害)
```
  String.prototype.spacify = function(){
    return this.split('').join(' ');
  }
```
(函数声明和函数表达式的区别)

2.Arguments
  定义一个log函数，类似实现一个建党的控制台输出。
  eg：`log('hello world');//=>输出hello world`
  一般会在定义里面直接写`console.log`,不过更优秀的方式是使用apply。
  ```
  function log(str){
    console.log(str);
  }
  ```
  接下来会给出传入的参数不固定，暗示，实际上`console.log`接受多个参数。
  eg：`log('hello','world')`
  现在我们用apply(可能会在apply和call上j困惑)，console上下文传入也是非常重要的。
```
  function log(){
    console.log.apply(console,arguments);
  }
```
  要求输出的字符串前统一加上`(app)`这样的字符串
  eg：`'(app) hello world'`
  面试者应该知道arguments是一个伪数组，需要转换成正常的数组，可以使用`Array.prototype.slice`
```
  function log(){
    var args = Array.prototype.slice.call(arguments);
    args.unshift('(app)');
    
    console.log.apply(console,args);//console.log.apply(this,args);当前环境上下文
  }
```

3.Content
  对于上下文以及this的理解
  给出一下代码，解释count的值
```
  var User = {
    count: 1;
    
    getCount: function(){
      return this.count;
    }
  };
  
  console.log(User.getCount());
  
  var func = User.getCount;
  console.log(func());
```
  正确输出`1`和`undefined`.实际上，func的上下文是`window`，因此失去了`count`属性。
  如何保证`func`的上下文始终都和`User`关联，这样使得输出的答案是`1`.
  正确的答案是使用`Function.prototype.bind`,代码：
```
  var func = User.getCount.bind(User);
  console.log(func());
```
  完善上面的代码。如果老的浏览器不支持该方法，怎么去兼容。(前端工程师应该对apply和call有着较为深刻的理解)
```
  Function.prototype.bind = Function.prototype.bind || function(context){
    var self = this;
    
    return function(){
      return self.apply(context, arguments);
    }
  };
```
内容来自[译](http://www.jackpu.com/-pian-fei-chang-bu-cuo-de-qian-duan-mian-shi-wen-zhang/)
  
