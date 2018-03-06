# map_.map(list, iteratee, [context]) Alias: collect 

> 通过对list里的每个元素调用转换函数(iteratee迭代器)生成一个与之相对应的数组。
> iteratee传递三个参数：value，然后是迭代 index 或 key ，最后一个是引用指向整个list。

### Eg:
```js
_.map([1, 2, 3], function(num){ return num * 3; });
=> [3, 6, 9]
_.map({one: 1, two: 2, three: 3}, function(num, key){ return num * 3; });
=> [3, 6, 9]
_.map([[1, 2], [3, 4]], _.first);
=> [1, 3]
```

### sourceCode
```js
const property = function(key) {
  return function(obj) {
    return obj == null ? void 0 : obj[key];
  };
};
var optimizeCb = function(func, context, argCount) {
  if (context === void 0) return func;
  switch (argCount == null ? 3 : argCount) {
    case 1: return function(value) {
      return func.call(context, value);
    };
    case 2: return function(value, other) {
      return func.call(context, value, other);
    };
    case 3: return function(value, index, collection) {
      return func.call(context, value, index, collection);
    };
    case 4: return function(accumulator, value, index, collection) {
      return func.call(context, accumulator, value, index, collection);
    };
  }
  return function() {
    return func.apply(context, arguments);
  };
};

var cb = function(value, context, argCount) {
  if (value == null) return _.identity;
  if (_.isFunction(value)) return optimizeCb(value, context, argCount);
  if (_.isObject(value)) return _.matcher(value);
  return _.property(value);
};
_.map = _.collect = function(obj, iteratee, context) {
  iteratee = cb(iteratee, context);
  /* 如果不是集合 便是对象，获取对象可遍历的键名数组 */
  var keys = !isArrayLike(obj) && _.keys(obj),
      length = (keys || obj).length,//获取 长度，利用短路操作符
      results = Array(length);//定义 返回的数组，参数为 长度，很赞，不浪费内存
  for (var index = 0; index < length; index++) {
    var currentKey = keys ? keys[index] : index;
    results[index] = iteratee(obj[currentKey], currentKey, obj);
  }
  return results;
};
```