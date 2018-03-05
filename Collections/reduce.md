# reduce_.reduce(list, iteratee, [memo], [context]) Aliases: inject, foldl 

> 别名为 inject 和 foldl, reduce方法把list中元素归结为一个单独的数值。
> Memo是reduce函数的初始值，会被每一次成功调用iteratee函数的返回值所取代 。
> 这个迭代传递4个参数：memo,value 和 迭代的index（或者 key）和最后一个引用的整个 list。

如果没有memo传递给reduce的初始调用，iteratee不会被列表中的第一个元素调用。
第一个元素将取代memo参数传递给列表中下一个元素调用的iteratee函数。

Eg:
```js
var sum = _.reduce([1, 2, 3], function(memo, num){ return memo + num; }, 0);
=> 6
```
# reduceRight_.reduceRight(list, iteratee, memo, [context]) Alias: foldr 

> reducRight是从右侧开始组合元素的reduce函数， Foldr在 JavaScript 中不像其它有惰性求值的语言那么有用

Eg:
```js
var list = [[0, 1], [2, 3], [4, 5]];
var flat = _.reduceRight(list, function(a, b) { return a.concat(b); }, []);
=> [4, 5, 2, 3, 0, 1]
```

```js
/* 数组最大的元素数量 */
const MAX_ARRAY_INDEX = Math.pow(2, 53) - 1;

/* 获取属性函数 */
const property = function(key) {
  return function(obj) {
    return obj == null ? void 0 : obj[key];
  };
};
const getLength = property('length');
const isArrayLike = function(collection) {
  /* 获取 元素的length属性 做以下判断，确定是否为 集合 */
  const length = getLength(collection);
  return typeof length == 'number' && length >= 0 && length <= MAX_ARRAY_INDEX;
};

const optimizeCb = function(func, context, argCount) {
  if (context === void 0) return func;
  switch (argCount == null ? 3 : argCount) {// 默认三个参数
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

/* 一般而言，两种方法的不同形式会传递布尔型参数，但作者很巧妙的传递了 1 与 -1 ，作为了遍历集合的自增值 */
_.reduce = _.foldl = _.inject = createReduce(1);

_.reduceRight = _.foldr = createReduce(-1);

var createReduce = function(dir) {

  var reducer = function(obj, iteratee, memo, initial) {
    /* 获取对象的可遍历集合 */
    var keys = !isArrayLike(obj) && _.keys(obj),
        length = (keys || obj).length,
        /* 利用巧妙参数 dir 判断初始索引 */
        index = dir > 0 ? 0 : length - 1;
    if (!initial) {
      /* 如果没有传递 memo 参数，则 memo的初始值为 数组的第一项或者最后一项 */
      memo = obj[keys ? keys[index] : index];

      /* 改变遍历数组的初始索引 */
      index += dir;
    }
    for (; index >= 0 && index < length; index += dir) {
      var currentKey = keys ? keys[index] : index;
      memo = iteratee(memo, obj[currentKey], currentKey, obj);
    }
    return memo;
  };

  return function(obj, iteratee, memo, context) {
    /* 判断是否传入初始值 */
    var initial = arguments.length >= 3;
    return reducer(obj, optimizeCb(iteratee, context, 4), memo, initial);
  };
};
```