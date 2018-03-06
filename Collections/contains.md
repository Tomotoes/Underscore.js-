# contains_.contains(list, value, [fromIndex]) Alias: includes 
> 如果list包含指定的value则返回true。
> 如果list 是数组，内部使用indexOf判断。
> 使用fromIndex来给定开始检索的索引位置。

### Eg:
```js
_.contains([1, 2, 3], 3);
=> true
```

### sourceCode
```js
_.contains = _.includes = _.include = function(obj, item, fromIndex, guard) {
  /* 如果是对象 返回 value组成的数组 */
  if (!isArrayLike(obj)) obj = _.values(obj);

  /* formIndex 表示 查询起始位置，如果没有指定该参数，则默认从 0 开始 */
  if (typeof fromIndex != 'number' || guard) fromIndex = 0;
  return _.indexOf(obj, item, fromIndex) >= 0;
};
```