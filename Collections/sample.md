# sample_.sample(list, [n]) 
> 从 list中产生一个随机样本。
> 传递一个数字表示从list中返回n个随机元素。否则将返回一个单一的随机项。

Eg:
```js
_.sample([1, 2, 3, 4, 5, 6]);
=> 4

_.sample([1, 2, 3, 4, 5, 6], 3);
=> [1, 6, 2]
```

```js
 _.values = function(obj) {
  var keys = _.keys(obj);
  var length = keys.length;
  var values = Array(length);
  for (var i = 0; i < length; i++) {
    values[i] = obj[keys[i]];
  }
  return values;
};
_.random = function(min, max) {
  if (max == null) {
    max = min;
    min = 0;
  }
  return min + Math.floor(Math.random() * (max - min + 1));
};
 _.shuffle = function(obj) {
  var set = isArrayLike(obj) ? obj : _.values(obj);
  var length = set.length;
  var shuffled = Array(length);
  for (var index = 0, rand; index < length; index++) {
    rand = _.random(0, index);
    if (rand !== index){
      shuffled[index] = shuffled[rand];
      shuffled[rand] = set[index];
    }
  }
  return shuffled;
};
_.sample = function(obj, n, guard) {
  if (n == null || guard) {
    /* 如果是对象，先获取 value数组 */
    if (!isArrayLike(obj)) obj = _.values(obj);
    /* 返回随机元素 */
    return obj[_.random(obj.length - 1)];
  }
  /* 返回打乱后并截取的数组 */
  return _.shuffle(obj).slice(0, Math.max(0, n));
};
```