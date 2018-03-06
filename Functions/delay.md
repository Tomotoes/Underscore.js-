# delay_.delay(function, wait, *arguments) 
> 类似setTimeout，等待wait毫秒后调用function。
> 如果传递可选的参数arguments，当函数function执行时， arguments 会作为参数传入。

### Eg:
```js
var log = _.bind(console.log, console);
_.delay(log, 1000, 'logged later');
=> 'logged later' // Appears after one second.
```

### sourceCode
```js
 _.delay = function(func, wait) {
   /* 获取方法所需的参数 */
  var args = slice.call(arguments, 2);
  
  return setTimeout(function(){
    /* 返回已执行后需要调用的函数的结果 */
    return func.apply(null, args);
  }, wait);
};
```