##### 倒计时

```js
import numtwo from './numtwo';

/**
 * @typedef {Object} CountDownResult
 * @property {string} day 天
 * @property {string} hour 时
 * @property {string} minute 分
 * @property {string} second 秒
 * @property {string} millisecond 毫秒
 */
/**
 * 倒计时函数
 * @param {number} ms 毫秒值
 * @param {Function} updateCallback 更新回调函数
 * @param {Function} endCallback 结束回调函数
 * @return {number}
 */
function countDown(ms, updateCallback, endCallback) {
    const speed = 100;
    const format = (milliseconds) => {
        let day,
            hour,
            minute,
            second;
        if (milliseconds >= 0) {
            day = numtwo(Math.floor(milliseconds / 1000 / 60 / 60 / 24));
            hour = numtwo(Math.floor((milliseconds / 1000 / 60 / 60) % 24));
            minute = numtwo(Math.floor((milliseconds / 1000 / 60) % 60));
            second = numtwo(Math.floor((milliseconds / 1000) % 60));
            return {
                day,
                hour,
                minute,
                second,
                millisecond: milliseconds % 1000
            };
        } else {
            return {
                day: '00',
                hour: '00',
                minute: '00',
                second: '00',
                millisecond: '00'
            };
        }
    };
    let interval;
    updateCallback && updateCallback(format(ms));
    interval = setInterval(() => {
        let nextTime = ms - speed,
            end = false;
        if (nextTime <= 0) {
            nextTime = 0;
            clearInterval(interval);
            end = true;
        }
        ms = nextTime;
        if (updateCallback instanceof Function) {
            updateCallback(format(nextTime));
        }
        if (end && endCallback instanceof Function) {
            endCallback();
        }
    }, speed);
    return interval;
}

export default countDown;

```



##### 将手机号中间四位加密

```js
import isTelPhone from './isTelPhone';

/**
 * [encryMobile 将手机号中间四位加密]
 *
 * @param   {String}    str     [手机号]
 * @returns {String}            [加密后的手机号]
 */
function encryMobile(str) {
    // 非手机号照原字符串输出
    if (!isTelPhone(str)) {
        return str;
    }
    return str.replace(/(\d{3})\d{4}(\d{4})/, '$1****$2');
}

export default encryMobile;

```



##### 查找url的参数中的某个字段是否存在，若存在则返回对应值

```js
/**
 * [getQueryString 查找url的参数中的某个字段是否存在，若存在则返回对应值]
 *
 * @param   {String}       url         [目标url]
 * @param   {String}       key         [需要查找的字段]
 * @returns {String}                   [查找字段的值，如不存在则返回 null]
 */
function getQueryString(url, key) {
    let reg = new RegExp('(^|&)' + key + '=([^&]*)(&|$)');
    const splitedArray = url.split('?');
    let queryString = splitedArray[1];
    if (!queryString) {
        return null;
    }
    let r = queryString.match(reg);
    if (r !== null) return decodeURI(r[2]);
    return null;
}

export default getQueryString;

```



#####获取地址栏参数，并以对象的形式返回参数

```js


/**
 * [parseQueryString 获取地址栏参数，并以对象的形式返回参数]
 *
 * @param   {String}    url     [输入url]
 * @returns {Object}            [参数对象]
 */
function parseQueryString(url) {
    let regUrl = /\?([\w\W]+)$/,
        arrUrl = regUrl.exec(url),
        result = {};
    if (arrUrl && arrUrl[1]) {
        let paramString = arrUrl[1],
            params = paramString.split('&');
        for (let i = 0; i < params.length; i++) {
            let param = params[i].split('=');
            if (param.length > 2) {
                param[1] += '=';
            }
            result[param[0]] = param[1];
        }
        return result;
    }
    return result;
}

export default parseQueryString;

```



##### 检测字符串是图片格式

```js
/**
 * [getSuffixName 检测字符窜是图片格式 .jpg .bmp .png .gif]
 *
 * @param    {String}    str        [输入字符串]
 * @returns  {Boolean}              [后缀存在返回true，否则返回false]
 */
function getSuffixName(str) {
    let suffixName = str.substr(str.lastIndexOf('.')).toLowerCase(),
        suffixNameArr = ['.jpg', '.bmp', '.png', '.gif'];
    return suffixNameArr.indexOf(suffixName) > -1;
}

export default getSuffixName;
```



##### 数据超过4位数时的处理

```js
/**
 * numtoFixed 数据超过4位数时的处理
 *
 * numtoFixed(10000) // "1万"
 * numtoFixed(10000, 'w') // 1w
 *
 * @param    {Number|String}        data        数据
 * @param    {String}               unit        单位
 * @returns  {String}                           处理后的数据
 */
function numtoFixed(data, unit = '万') {
    let num = Number(data);
    if (!num) return num;
    num = Math.ceil(num); // 保证数据为整数
    return num.toString().length <= 4 ? num : (num / 10000).toFixed(1) + unit;
}

export default numtoFixed;

```


##### 个位数字两位显示的处理

```js
/**
 * [numtwo 个位数字两位显示的处理]
 *
 * @param    {Number}    num        [数字]
 * @returns  {String}               [格式化后的数字]
 */
function numtwo(num) {
    let numStr = num.toString();
    return numStr.length === 1 ? (0 + numStr) : numStr;
}

export default numtwo;


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



#####删除数组指定index的元素，返回被删除后的数组

```js
/**
 * [removeArrayIndex 删除数组指定index的元素，返回被删除后的数组]
 *
 * @param   {Array}     arr         输入的数组
 * @param   {Number}    index       指定元素的index
 * @returns {Array}                 删除指定index的元素后的数组
 */
function removeArrayIndex(arr, index) {
    return arr.filter((item, i) => i !== index);
}

export default removeArrayIndex;

```



##### 时间戳格式化成时分秒

```js
/**
 * [secondsFormat 秒格式化成 h:mm:ss]
 *
 * @param       {String|Number}     seconds         时间（单位：秒）
 * @param       {Number}            type            转化类型(0: 分的符号为 ', 秒的符号为 '')
 * @returns     {String}                            格式化后的字符串
 */
function secondsFormat(seconds, type) { // 时间秒格式化成 h:mm:ss
    let min = Math.floor(seconds / 60),
        second = seconds % 60,
        hour, newMin, time;
    if (min >= 60) {
        hour = Math.floor(min / 60);
        newMin = min % 60;
    }
    if (second < 10) {
        second = '0' + second;
    }
    if (min < 10) {
        min = '0' + min;
    }
    if (newMin < 10) {
        newMin = '0' + newMin;
    }
    if (type === 0) {
        time = hour ? (hour + ':' + newMin + '\'' + second + '\'\'') : (min + '\'' + second + '\'\'');
    } else {
        time = hour ? (hour + ':' + newMin + ':' + second) : (min + ':' + second);
    }
    return time;
}

export default secondsFormat;

```



##### 时间戳格式化成各种类型

```js
import numtwo from './numtwo';

/**
 * [timestampToFormat 时间戳格式化成各种类型]
 *
 * @param       {String|Number}        timestamp    [时间]
 * @param       {String}        format       [格式]
 * @returns     {String}                            [格式化后的时间]
 */
function timestampToFormat(timestamp, format) {
    let date = new Date(timestamp),
        year = date.getFullYear(),
        month = date.getMonth() + 1,
        day = date.getDate(),
        hour = date.getHours(),
        minute = date.getMinutes(),
        second = date.getSeconds(),
        numtwoMonth = numtwo(month),
        numtowDay = numtwo(day),
        numtwoHour = numtwo(hour),
        numtwoMinute = numtwo(minute),
        numtwoSecond = numtwo(second);
    if (typeof format === 'string') {
        let newDateString = format;
        newDateString = newDateString.replace(/yyyy/g, year);
        newDateString = newDateString.replace(/MM/g, numtwoMonth);
        newDateString = newDateString.replace(/dd/g, numtowDay);
        newDateString = newDateString.replace(/M/g, month);
        newDateString = newDateString.replace(/d/g, day);
        newDateString = newDateString.replace(/hh/g, numtwoHour);
        newDateString = newDateString.replace(/h/g, hour);
        newDateString = newDateString.replace(/mm/g, numtwoMinute);
        newDateString = newDateString.replace(/m/g, minute);
        newDateString = newDateString.replace(/ss/g, numtwoSecond);
        newDateString = newDateString.replace(/s/g, second);
        return newDateString;
    } else {
        return year + '-' + numtwoMonth + '-' + numtowDay + ' ' + numtwoHour + ':' + numtwoMinute + ':' + numtwoSecond;
    }
}

export default timestampToFormat;

```

