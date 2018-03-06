# filter_.filter(list, predicate, [context]) Alias: select 

> 遍历list中的每个值，返回所有通过predicate真值检测的元素所组成的数组。

### Eg:
```js
var evens = _.filter([1, 2, 3, 4, 5, 6], function(num){ return num % 2 == 0; });
=> [2, 4, 6]
```

### sourceCode
```js

_.each = _.forEach = function(obj, iteratee, context) {
  iteratee = optimizeCb(iteratee, context);
  let i, length;
  /* 如果是集合 */
  if (isArrayLike(obj)) {
    for (i = 0, length = obj.length; i < length; i++) {
      iteratee(obj[i], i, obj);
    }
  } else {
    /* 获取 对象的可遍历键名数组 */
    var keys = _.keys(obj);
    for (i = 0, length = keys.length; i < length; i++) {
      /* obj[keys[i]] 也就是 objs[当前属性名] 即 value*/
      iteratee(obj[keys[i]], keys[i], obj);
    }
  }
  return obj;
};

_.filter = _.select = function(obj, predicate, context) {
  var results = [];
  predicate = cb(predicate, context);
  /* 遍历 obj集合中的各个元素，对每个元素进行 第二个参数函数的包装，而第二个参数中 包含着 我们外部调用的筛选函数 */
  /* 如果符合规则，便存到数组中，以便返回 */
  _.each(obj, function(value, index, list) {
    if (predicate(value, index, list)) results.push(value);
  });
  return results;
};

```