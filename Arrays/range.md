# range_.range([start], stop, [step]) 
> 一个用来创建整数灵活编号的列表的函数，便于each 和 map循环。
> 如果省略start则默认为 0；step 默认为 1.返回一个从start 到stop的整数的列表，用step来增加 （或减少）独占。
> 值得注意的是，如果stop值在start前面（也就是stop值小于start值），那么值域会被认为是零长度，而不是负增长。
> 如果你要一个负数的值域 ，请使用负数step.

Eg:
```js
_.range(10);
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
_.range(1, 11);
=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
_.range(0, 30, 5);
=> [0, 5, 10, 15, 20, 25]
_.range(0, -10, -1);
=> [0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
_.range(0);
=> []
```

```js
 _.range = function(start, stop, step) {
   /* 如果只传一个值 */
  if (stop == null) {
    /* 修改 stop的值 */
    stop = start || 0;
    start = 0;
  }
  step = step || 1;

  /* 获取数组的长度 */
  var length = Math.max(Math.ceil((stop - start) / step), 0);
  var range = Array(length);

  /* start + = step */
  for (var idx = 0; idx < length; idx++, start += step) {
    range[idx] = start;
  }

  return range;
};
```