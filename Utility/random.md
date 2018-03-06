# random_.random(min, max) 

> 返回一个min 和 max之间的随机整数。如果你只传递一个参数，那么将返回0和这个参数之间的整数。

### Eg:

```js
_.random(0, 100);
=> 42
-.random(10)
=> 2
```

### sourceCode
```js
.random = function(min, max) {
  if (max == null) {
    max = min;
    min = 0;
  }

  /* 最小值 + 下取整(0 ~ 0.99 * (差值+1) ) */
  /* 最小值 + 0 => 最小值 */

  /* 下取整(0.99 * 差值) => 差值减一 */
  /* 最小值 + 下取整(0.99 * (差值+1) ) */
  return min + Math.floor(Math.random() * (max - min + 1));
};
```