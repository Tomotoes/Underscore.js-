# keys_.keys(object) 
> 检索object拥有的所有可枚举属性的名称。

### Eg:
```js
_.keys({one: 1, two: 2, three: 3});
=> ["one", "two", "three"]
```

### sourceCode
```js
_.keys = function(obj) {

  if (!_.isObject(obj)) return [];

  if (nativeKeys) return nativeKeys(obj);

  var keys = [];

  for (var key in obj){
    if (_.has(obj, key)) keys.push(key);

    if (hasEnumBug) collectNonEnumProps(obj, keys);
  }

  return keys;
};
```