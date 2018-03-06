### sourceCode
```js

_.isEmpty = function(obj) {
  if (obj == null) return true;
  if (isArrayLike(obj) && (_.isArray(obj) || _.isString(obj) || _.isArguments(obj))) return obj.length === 0;
  return _.keys(obj).length === 0;
};

_.isElement = function(obj) {
  return !!(obj && obj.nodeType === 1);
};

_.isArray = nativeIsArray || function(obj) {
  return toString.call(obj) === '[object Array]';
};

_.isObject = function(obj) {
  var type = typeof obj;
  return type === 'function' || type === 'object' && !!obj;
};

_.each(['Arguments', 'Function', 'String', 'Number', 'Date', 'RegExp', 'Error', 'Symbol', 'Map', 'WeakMap', 'Set', 'WeakSet'], function(name) {
  _['is' + name] = function(obj) {
    return toString.call(obj) === '[object ' + name + ']';
  };
});

/* 判断 Arguments 依靠是否存在 callee 属性 */
if (!_.isArguments(arguments)) {
  _.isArguments = function(obj) {
    return _.has(obj, 'callee');
  };
}

var nodelist = root.document && root.document.childNodes;
if (typeof /./ != 'function' && typeof Int8Array != 'object' && typeof nodelist != 'function') {
  _.isFunction = function(obj) {
    return typeof obj == 'function' || false;
  };
}

_.isFinite = function(obj) {
  return !_.isSymbol(obj) && isFinite(obj) && !isNaN(parseFloat(obj));
};

_.isNaN = function(obj) {
  return _.isNumber(obj) && isNaN(obj);
};

_.isBoolean = function(obj) {
  return obj === true || obj === false || toString.call(obj) === '[object Boolean]';
};

_.isNull = function(obj) {
  return obj === null;
};

_.isUndefined = function(obj) {
  return obj === void 0;
};


```