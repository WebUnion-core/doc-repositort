
# JavaScript实战技巧 #

## 目录 ##

1. [匹配最外层HTML标签名](#href1)
2. [console总结](#href2)
    1. [普通打印](#href2-1)
    2. [分组打印](#href2-2)
    3. [表格打印](#href2-3)
    4. [计数和计时](#href2-4)
    5. [条件打印和打印跟踪](#href2-5)
3. [浏览器判断原则](#href3)
4. [检查手机号码格式](#href4)
5. [获取当前URL查询字符串参数](#href5)
6. [CRT日期转换](#href6)
7. [使用touchstart代替移动端click事件](#href7)
8. [数组去重](#href8)
9. [获取YYYY-MM-DD格式日期](#href9)
10. [类型获取](#href10)
11. [深拷贝](#href11)
12. [节流](#href12)
13. [防抖](#href13)

## <a name="href1">匹配最外层HTML标签名</a> ##

```js
var rquickExpr = /^<([a-z][^\/\0>:\x20\t\r\n\f]*)[\x20\t\r\n\f]*\/?>(?:<\/\1>|)$/i;
console.log(rquickExpr.exec('<h1><span></span></h1>')[1]); // => "h1"
```

## <a name="href2">console总结</a> ##

### <a name="href2-1">普通打印</a> ###

普通(基本)的打印方法有以下四种，都可接收多个参数:

1. `console.log`: 用得最多的一种，基本上没有任何限制;
2. `console.info`: 打印信息;
3. `console.error`: 打印报错内容;
4. `console.warn`: 打印警告内容。

示例:

```js
console.log('log: ', 'log content.');
console.log('info: ', 'info content.');
console.log('error: ', 'error content.');
console.log('warn: ', 'warn content.');
```

### <a name="href2-2">分组打印</a> ###

分组打印用到的是`console.group()`和`console.groupEnd()`方法，用法如下:

```js
console.group('Group 1: ');
console.log('1. Dva');
console.log('2. Joe');
console.groupEnd();
console.group('Group 2: ');
console.log('1. WJT20');
console.groupEnd();
```

### <a name="href2-3">表格打印</a> ###

表格打印方法`console.table()`可以美观地打印数组和对象:

```js
console.table({
	name: 'WJT20',
	id: 1
});
console.table([
	{ name: 'WJT20', id: 1 },
	{ name: 'Dva', id: 2 }
]);
```

### <a name="href2-4">计数和计时</a> ###

计数即`console.count()`，传入的参数表示字段，每次调用都会统计这个字段调用的次数:

```js
for (var i = 0; i < 5; i++) {
	console.count('counter'); // 共调用5次
}
```

计时常用于计算程序花费的时长，用的是`console.time()`和`console.timeEnd()`:

```js
console.time('test');
for (var i = 0; i < 1000; i++) {} // 1000次循环
console.timeEnd('test');
```

### <a name="href2-5">条件打印和打印跟踪</a> ###

条件打印即`console.assert()`:

```js
for (var i = 0; i < 10; i++) {
	console.assert(i >= 5, { msg: i + ' is less than 5' });
}
```

打印跟踪即`console.trace()`:

```js
console.trace();
```

## <a name="href3">浏览器判断原则</a> ##

代码:

```js
var sUserAgent = navigator.userAgent.toLowerCase();
var browser = {
    bIsIOS: !!navigator.userAgent.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/),
    bIsUc: sUserAgent.match(/ucweb/i) == "ucweb",
    bIsAndroid: sUserAgent.match(/android/i) == "android",
    bIsAndroidVersion: Number(sUserAgent.substr(sUserAgent.indexOf('android') + 8, 3)),
    bIsQQ: sUserAgent.match(/qq/i) == "qq",
    bIsWechat: sUserAgent.match(/micromessenger/i) == "micromessenger",
    bIsWeibo: sUserAgent.match(/weibo/i) == "weibo",
    bIsHuawei: sUserAgent.match(/huawei/i) == "huawei"
};
```

## <a name="href4">检查手机号码格式</a> ##

代码:

```js
function checkPhoneFormat(phone) {
    var pattern = /^0?1[3|4|5|7|8][0-9]\d{8}$/;
    if (pattern.test(phone)) {
        return true;
    } else {
        return false;
    }
};
```

## <a name="href5">获取当前URL查询字符串参数</a> ##

代码:

```js
function parseQueryString() {
    var str = location.search.slice(1);
	var arr = str.split('&');
	var obj = {};

    for (var i = 0; i < arr.length; i++) {
		// 一般参数字符串都经过编码，使用decodeURIComponent()方法将键和值转为原始值
        var item = arr[i].split('=');
		var key = decodeURIComponent(item[0]);
		var value = decodeURIComponent(item[1]);
        obj[key] = value;
    }

    return obj;
}
```

## <a name="href6">CRT日期转换</a> ##

代码:

```js
function translateCRT(CRTDate) {
    var time = CRTDate.replace(new RegExp(' CST', 'gm'), '');
    var now = new Date(time);
    var year = now.getFullYear();
    var month = now.getMonth() + 1;
    var date = now.getDate();
    var hh = now.getHours();
    var mm = now.getMinutes();

    return {
        dataObj: now,
        year: year,
        month: month,
        date: date,
        hour: hh,
        minute: mm
    }
}
```

## <a name="href7">使用touchstart代替移动端click事件</a> ##

代码:

```js
var dialogEl = document.getElementById('dialog-container');
dialogEl.addEventListener('touchstart', function () {
    ...
});
```

解析:

在移动端，touchstart 比 click 要灵敏得多，但是何时该使用 touchstart，何时该使用 click，应视具体场景而定。

## <a name="href8">数组去重</a> ##

代码:

```js
function unique (arr) {
	var newArr = [];
	var isRepeated = false;

	for (var i = 0; i < arr.length; i++) {
		isRepeated = false;

		for (var j = 0; j < newArr.length; j++) {
			if (newArr[j] === arr[i]) {
				isRepeated = true;
				break;
			}
		}

		if (!isRepeated) {
			newArr.push(arr[i]);
		}
	}

	return newArr;
}

console.log(unique([1, 2, 3, 4, undefined, 1, null, undefined, 3, NaN, 5, NaN])); // [1, 2, 3, 4, null, 5]
```

解析:

对于 undefined、null、NaN 等特殊值，for 循环会自动忽视这些值，所以不考虑这些值的去重，另外，Object 的相等判断又是另一个复杂知识了，这里也不考虑。

## <a name="href9">获取YYYY-MM-DD格式日期</a> ##

代码:

```js
function getTimestamp(date, spliter) {
    if (!date) {
        date = new Date();
    }

    if (!spliter) {
        spliter = '-';
    }

    function format(target, figure) {
        target = target.toString();
        for (var i = 0; i < (figure - target.length); i++) {
            target = '0' + target;
        }
        return target;
    }

    var y = format(date.getFullYear(), 4);
    var m = format(date.getMonth() + 1, 2);
    var d = format(date.getDate(), 2);

    return y + spliter + m + spliter + d;
}

console.log(getTimestamp());
```

## <a name="href10">类型获取</a> ##

代码:

```js
function checkedType(target) {
    return Object.prototype.toString.call(target).slice(8, -1);
}
```

## <a name="href11">深拷贝</a> ##

代码:

```js
// 定义检测数据类型的功能函数
function checkedType(target) {
    return Object.prototype.toString.call(target).slice(8, -1);
}

// 实现深度克隆---对象/数组
function clone(target) {
    // 判断拷贝的数据类型
    // 初始化变量result 成为最终克隆的数据
    var result;
    var targetType = checkedType(target);

    if (targetType === 'Object') {
        result = {};
    } else if (targetType === 'Array') {
        result = [];
    } else {
        return target;
    }

    // 遍历目标数据
    for (var i in target) {
        // 获取遍历数据结构的每一项值。
        var value = target[i];

        // 判断目标结构里的每一值是否存在对象/数组
        if (checkedType(value) === 'Object' || checkedType(value) === 'Array') {
            // 对象/数组里嵌套了对象/数组
            // 继续遍历获取到value值
            result[i] = clone(value);
        } else {
            // 获取到value值是基本的数据类型或者是函数。
            result[i] = value;
        }
    }
    return result;
}
```

## <a name="href12">节流</a> ##

代码:

```js
function throttle(fn, time) {
    var ifWork = true; // 用闭包保存一个计时器启动标记
    return function () {
        var params = arguments;
        if (ifWork) {
            // 一旦计时器启动，则将标记置为false
            ifWork = false;
            setTimeout(function () {
                // 注意绑定上下文环境
                fn.apply(this, params);
                ifWork = true;
            }, time);
        }
    };
}
```

## <a name="href13">防抖</a> ##

代码:

```js
function debounce(fn, time) {
    var timer = null; // 用闭包把计时器对象保存下来
    return function () {
        var params = arguments;
        if (timer) {
            // 清除掉已存在的计时器
            clearTimeout(timer);
        }
        timer = setTimeout(function () {
            // 注意绑定上下文环境
            fn.apply(this, params);
        }, time);
    };
}
```

---

```
ID         : 71
DATE       : 2018/05/03
AUTHER     : WJT20
TAG        : JavaScript
```
