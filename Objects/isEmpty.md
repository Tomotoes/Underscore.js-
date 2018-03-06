# isEmpty_.isEmpty(object) 
> 如果object 不包含任何值(没有可枚举的属性)，返回true。 
> 对于字符串和类数组（array-like）对象，如果length属性为0，那么_.isEmpty检查返回true。

### Eg:
```js
_.isEmpty([1, 2, 3]);
=> false
_.isEmpty({});
=> true
```

### sourceCode
```js
_.isEmpty = function(obj) {
  if (obj == null) return true;

  // 如果是数组、类数组、或者字符串 , 根据 length 属性判断是否为空
  if (isArrayLike(obj) && (_.isArray(obj) || _.isString(obj) || _.isArguments(obj))) return obj.length === 0;

  /* 如果是对象，根据 keys数量进行判断，但是如果有不可遍历的属性呢 */
  return _.keys(obj).length === 0;
};
```