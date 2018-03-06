# toArray_.toArray(list) 
> 把list(任何可以迭代的对象)转换成一个数组，在转换 arguments 对象时非常有用。

### Eg:
```js
(function(){ return _.toArray(arguments).slice(1); })(1, 2, 3, 4);
=> [2, 3, 4]
```

### sourceCode
```js
var property = function(key) {
  return function(obj) {
    return obj == null ? void 0 : obj[key];
  };
};

var MAX_ARRAY_INDEX = Math.pow(2, 53) - 1;
var getLength = property('length');

/* 判断 collection 是否有 length 属性 */
var isArrayLike = function(collection) {
  var length = getLength(collection);
  return typeof length == 'number' && length >= 0 && length <= MAX_ARRAY_INDEX;
};

 _.isArray = nativeIsArray || function(obj) {
  return toString.call(obj) === '[object Array]';
};

// 去掉数组中所有的假值
// 返回数组副本
// JavaScript 中的假值包括 false、null、undefined、''、NaN、0
 _.identity = function(value) {
  return value;
};

_.values = function(obj) {
  var keys = _.keys(obj);
  var length = keys.length;
  var values = Array(length);
  for (var i = 0; i < length; i++) {
    values[i] = obj[keys[i]];
  }
  return values;
};

_.toArray = function(obj) {
  if (!obj) return [];
  /* 数组返回数组 */
  if (_.isArray(obj)) return slice.call(obj);
  
  /* 集合返回一个全新的数组，并经过 identity闭包过滤，返回一个全新的值  */
  if (isArrayLike(obj)) return _.map(obj, _.identity);
  
  /* 对象返回属性值组成的数组 */
  return _.values(obj);
};
```