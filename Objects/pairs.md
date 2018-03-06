# pairs_.pairs(object) 
> 把一个对象转变为一个[key, value]形式的数组。

### Eg:
```js
_.pairs({one: 1, two: 2, three: 3});
=> [["one", 1], ["two", 2], ["three", 3]]
```

### sourceCode
```js
_.pairs = function(obj) {
  /* 获取对象键名组成的数组 */
  var keys = _.keys(obj);
  var length = keys.length;
  var pairs = Array(length);
  for (var i = 0; i < length; i++) {
    pairs[i] = [keys[i], obj[keys[i]]];
  }
  return pairs;
};
```