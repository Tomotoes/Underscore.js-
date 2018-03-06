# size_.size(list) 
> 返回list的长度。

Eg:
```js
_.size({one: 1, two: 2, three: 3});
=> 3
```

```js
_.size = function(obj) {
  if (obj == null) return 0;
  /* 数组直接返回原生 length，对象返回可遍历键名数组的长度，合情合理 */
  return isArrayLike(obj) ? obj.length : _.keys(obj).length;
};
```