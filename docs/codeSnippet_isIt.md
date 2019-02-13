##### 判断是否为对象

```js
/**
 * 判断是否是对象
 * @param {*} obj 判断对象
 * @return {boolean}
 */
function isObject(obj) {
    return Object.prototype.toString.call(obj) === '[object Object]';
}
export default isObject;
```



##### 判断是否为数组

```js
/**
 * 判断是否为数组
 *
 * @param {*} obj 输入的对象
 * @returns {boolean} obj是数组返回true，否则返回false
 */
function isArray(obj) {
    return Object.prototype.toString.call(obj) === '[object Array]';
}
export default isArray;
```



##### 判断是否为null 或undefined

```js
/**
 * [isUndefinedOrNull 判断是否为null 或undefined]
 *
 * @param    {*}            value   [输入数据]
 * @returns  {Boolean}              [输入值是 null 或 undefined 返回 true，否则返回 false]
 */
function isUndefinedOrNull(value) {
    return value === null || value === undefined;
}
export default isUndefinedOrNull;
```


##### 判断对象是否为空

```js
import isArray from './isArray';
import isObject from './isObject';

/**
 * [isEmptyObject 判断对象是否为空]
 *
 * @param   {Object}        obj        [输入对象]
 * @returns {Boolean}                  [空数组、null、undefined、{}、0、'' 返回true，其他返回false]
 */
function isEmptyObject(obj) {
    // 空数组返回 true
    if (isArray(obj)) {
        return !obj.length;
    }
    if (isObject(obj)) {
        for (let t in obj) {
            return false;
        }
        return true;
    }
    return !obj;
}

export default isEmptyObject;

```



##### 判断对象是否相等

```js
/**
 * [isObjectValueEqual 判断对象是否相等]
 *
 * @param   {Object}    a    [A对象]
 * @param   {Object}    b    [B对象]
 * @returns {Boolean}        [a,b对象相等返回true，不等返回false]
 */
function isObjectValueEqual(a, b) {
    if ((typeof a === 'number' && typeof b === 'number') ||
        (typeof a === 'string' && typeof b === 'string')) {
        return a === b;
    }
    let aProps = Object.getOwnPropertyNames(a),
        bProps = Object.getOwnPropertyNames(b);

    if (aProps.length !== bProps.length) {
        return false;
    }

    for (let i = 0; i < aProps.length; i++) {
        let propName = aProps[i];
        if (Object.prototype.toString(a[propName]) === '[object Object]' || Object.prototype.toString(b[propName]) === '[object Object]') {
            if (!isObjectValueEqual(a[propName], b[propName])) return false;
        } else if (a[propName] !== b[propName]) {
            return false;
        }
    }
    return true;
}

export default isObjectValueEqual;

```


##### 判断邮箱格式是否正确

```js
/**
 * [isEmail 判断邮箱格式是否正确]
 *
 * @param   {String}    str      [邮箱]
 * @returns {Boolean}            [邮箱格式正确返回true，错误返回false]
 */
function isEmail(str) {
    let reg = /^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+((\.[a-zA-Z0-9_-]{2,3}){1,2})$/;
    return reg.test(str);
}

export default isEmail;

```



##### 判断手机格式是否正确

```js
/**
 * [isTelPhone 判断手机格式是否正确]
 *
 * @param   {String}    str      [手机号]
 * @returns {Boolean}            [手机号格式正确返回true，错误返回false]
 */
function isTelPhone(str) {
    let reg = /^(1[3456789]\d{9})$/;
    return reg.test(str);
}

export default isTelPhone;

```



#