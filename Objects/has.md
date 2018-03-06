# has_.has(object, key) 
> 对象是否包含给定的键吗？等同于object.hasOwnProperty(key)，但是使用hasOwnProperty 函数的一个安全引用，以防意外覆盖。

### Eg:
```js
_.has({a: 1, b: 2, c: 3}, "b");
=> true
```
### sourceCode
```js
_.has=function(obj,key){
  return obj != null && hasOwnProperty.call(obj, key);
}
```