# find_.find(list, predicate, [context]) Alias: detect 
> 在list中逐项查找，返回第一个通过predicate迭代函数真值检测的元素值，如果没有元素通过检测则返回 undefined。 
> 如果找到匹配的元素，函数将立即返回，不会遍历整个list。

Eg: 
```js
var even = _.find([1, 2, 3, 4, 5, 6], function(num){ return num % 2 == 0; });
=> 2
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

var createPredicateIndexFinder = function(dir) {
  return function(array, predicate, context) {
    predicate = cb(predicate, context);
    var length = getLength(array);
    var index = dir > 0 ? 0 : length - 1;
    for (; index >= 0 && index < length; index += dir) {
      if (predicate(array[index], index, array)) return index;
    }
    return -1;
  };
};

// Returns the first index on an array-like that passes a predicate test.
_.findIndex = createPredicateIndexFinder(1);
_.findLastIndex = createPredicateIndexFinder(-1);

_.findKey = function(obj, predicate, context) {
  predicate = cb(predicate, context);
  var keys = _.keys(obj), key;
  for (var i = 0, length = keys.length; i < length; i++) {
    /* key 获取当前的遍历的属性名 */
    key = keys[i];
    if (predicate(obj[key], key, obj)) return key;
  }
};
// Return the first value which passes a truth test. Aliased as `detect`.
_.find = _.detect = function(obj, predicate, context) {
  var keyFinder = isArrayLike(obj) ? _.findIndex : _.findKey;

  /* 返回的是属性名 */
  var key = keyFinder(obj, predicate, context);
  
  if (key !== void 0 && key !== -1) return obj[key];
};

```