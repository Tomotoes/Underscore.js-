# each_.each(list, iteratee, [context]) Alias: forEach 

> 遍历list中的所有元素，按顺序用每个元素当做参数调用 iteratee 函数。
> 如果传递了context参数，则把iteratee绑定到context对象上。

> 如果list是个数组，iteratee的参数是 (element, index, list)。
> 如果list是个对象，iteratee的参数是 (value, key, list))。

> 返回list以方便链式调用。

Eg:
```js
_.each([1, 2, 3], alert);
=> alerts 1 2 3

_.each({one: 1, two: 2, three: 3}, alert);
=> alerts 1 2 3
```

#### 注意：集合函数能在数组，对象，和类数组对象，比如arguments, NodeList和类似的数据类型上正常工作。 
但是它通过鸭子类型工作，所以要避免传递带有一个数值类型 length 属性的对象。
每个循环不能被破坏 - 打破， 使用_.find代替，这也是很好的注意。

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

const optimizeCb = function(func, context, argCount) {
  if (context === void 0) return func;
  switch (argCount == null ? 3 : argCount) {// 默认三个参数
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

/**
 * @param  {遍历的对象} obj
 * @param  {操作函数} iteratee
 * @param  {函数执行上下文} context
 * @returns {返回对象本身以便于链式调用} obj
 */
_.each = _.forEach = function(obj, iteratee, context) {
  iteratee = optimizeCb(iteratee, context);
  let i, length;
  if (isArrayLike(obj)) {
    for (i = 0, length = obj.length; i < length; i++) {
      iteratee(obj[i], i, obj);
    }
  } else {
    var keys = _.keys(obj);
    for (i = 0, length = keys.length; i < length; i++) {
      iteratee(obj[keys[i]], keys[i], obj);
    }
  }
  return obj;
};
```