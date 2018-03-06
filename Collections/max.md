# max_.max(list, [iteratee], [context]) 
> 返回list中的最大值。
> 如果传递iteratee参数，iteratee将作为list中每个值的排序依据。
> 如果list为空，将返回-Infinity，所以你可能需要事先用isEmpty检查 list 。

### Eg:
```js
var stooges = [{name: 'moe', age: 40}, {name: 'larry', age: 50}, {name: 'curly', age: 60}];
_.max(stooges, function(stooge){ return stooge.age; });
=> {name: 'curly', age: 60};
```

### sourceCode
```js
 _.max = function(obj, iteratee, context) {
  var result = -Infinity, lastComputed = -Infinity,
      value, computed;
  if (iteratee == null && obj != null) {
    /* 如果没有传递第二个参数，则 数组单纯求最大值，对象单纯求最大Value */
    obj = isArrayLike(obj) ? obj : _.values(obj);
    for (var i = 0, length = obj.length; i < length; i++) {
      value = obj[i];
      if (value > result) {
        result = value;
      }
    }
  } else {
    iteratee = cb(iteratee, context);
    _.each(obj, function(value, index, list) {
      computed = iteratee(value, index, list);
      if (computed > lastComputed || computed === -Infinity && result === -Infinity) {
        result = value;
        lastComputed = computed;
      }
    });
  }
  return result;
};
```