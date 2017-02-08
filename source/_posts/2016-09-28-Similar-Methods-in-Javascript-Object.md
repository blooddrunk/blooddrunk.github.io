---
title: Javascript Object对象中一些容易混淆的方法
date: 2016-09-28 19:36:54
tags:
  - Javascript
categories:
  - 前端
  - Javascript
---

### `Object.is()` vs `===`

与`===`不同，`Object.is()`认为`+0`和`-0`不相等，而两个`NaN`值相等。

```javascript
Object.is(0, -0); // false
+0 === -0; // true

Object.is(NaN, 3/0); // true
NaN === Number.NaN; // false
```

<!-- more -->

### `Object.keys()` vs `Object.getOwnPropertyNames()`

两者都只会返回自身的属性，不同之处在于`Object.keys()`只会返回可枚举(*enumerable*)的属性，而`Object.getOwnPropertyNames()`还会返回不可枚举的属性。

```javascript 只返回不可枚举的属性
var target = myObject;
var enum_and_nonenum = Object.getOwnPropertyNames(target);
var enum_only = Object.keys(target);
var nonenum_only = enum_and_nonenum.filter(function(key) {
    var indexInEnum = enum_only.indexOf(key);
    if (indexInEnum == -1) {
        // not found in enum_only keys mean the key is non-enumerable,
        // so return true so we keep this in the filter
        return true;
    } else {
        return false;
    }
});
```

### `Object.seal()` vs `Object.freeze()` vs `Object.preventExtension()`

`Object.preventExtension()`: 返回一个新的对象，该对象不能添加新的属性。

`Object.seal()`: 返回一个新的对象，除了不能添加新的属性之外，所有已有的属性都会被标记为“不可配置”的(见[`Object.defineProperty()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty))，也就是说所有的属性都不能被删除，但是如果某个属性的描述符已被配置为`writable`，该属性的值还是可以被修改。

`Object.freeze()`: 同`Object.seal()`一样。除了新返回的对象的所有属性都不能被修改(non-writable)。

> `Object.isExtensible()`, `Object.isSealed()`和`Object.isFrozon()`可分别用于检测一个对象是否由上面的方法生成

> 'sealed'或'frozon'的对象自然也是*non-extensible*的