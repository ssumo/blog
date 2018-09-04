---
title: repeatify实现复制字符串的几种实现方案
date: 2018-09-04 15:40:11
tags: JS
categories: JS
---


#### 前言

今天偶然遇到一道题,最开始没看清题目意思,以为是需要实现一个函数,输出**console.log('hello', repeatify(3));**能得到结果**hellohellohello**

查阅了半天资料也搞不清楚如何在一个函数体里面调取console的传值参数。

后来找同事帮忙看这道题，同事帮我解了半天也不知道该如何写这个函数，后来同事说我是不看错题。仔仔细细看才知道真的是看错了（另外一个同事求助我发给我的）。


#### 原题
原题：创建 “原生（native）” 方法
在String对象上定义一个repeatify函数。这个函数接受一个整数参数，来明确字符串需要重复几次。这个函数要求字符串重复指定的次数。举个例子：

**console.log('hello'.repeatify(3));**

应该打印出**hellohellohello**


考察的点就是继承及原型(prototype)属性的知识，另外ES6的**String.prototype.repeat()**也可以实现。 当然可扩展的方式很多，想题到解题的整个过程还是比较有趣的，就记下了这道题。


#### 解题

##### 1. 利用空数组的join字符串

```javascript
String.prototype.repeatify = function (n) {
    return (new Array(n + 1)).join(this);
}
console.log('hello'.repeatify(3));
```

##### 2. call调用数组原型join方法，构造类数组对象，此处我们设定的length为非负才行。
```javascript
String.prototype.repeatify = function (n) {
    return Array.prototype.join.call({
        length: n + 1,
    }, this)
}
console.log('hello'.repeatify(3));
```

##### 3. 这里采用闭包，存储一个对象，再使用join方法实现。
```javascript
String.prototype.repeatify = (function () {
    var join = Array.prototype.join, 
        obj = {};
    return function (n) {
        obj.length = n + 1
        return join.call(obj, this)
    }
})()
console.log('hello'.repeatify(3));
```


##### 4. 采用二分法
```javascript
function repeatify(n) {
    var s = this, 
        total = [];
    while (n > 0) {
        if (n % 2 == 1) {
            total[total.length] = s;
            if (n == 1) {
                break
            }
            s += s;
            n = n >> 1; // 等同n/2取商
        }
    }
    return total.join('')
}
console.log('hello'.repeatify(3));
```


##### 5. 此处和第4个作对比，避免创建数据和使用join，不过这种有个缺点是循环中创建的字符串比要求的要长，需要回减得到结果。
```javascript
function repeatify(n) {
    var s = this,
        c = s.length * n;
    do {
        s += s;
    } while (n = n >> 1)
    s = s.substring(0, c)
    return s

}
console.log('hello'.repeatify(3));
```


##### 6. 此处也对比第4个方法，将声明的数组对象设置为string对象，'' + '' 的方式。
```javascript
function repeatify(n) {
    var s = this,
        total = '';
    while (n > 0) {
        if (n % 2 == 1) {
            total += s;
            if (n == 1) {
                break
            }
            s += s;
            n = n >> 1;
        }
    }
    return total
}
console.log('hello'.repeatify(3));


function repeatify(n) {
    var s = this,
        total = '';
    for (var i = 0; i < n; i++) {
        total += s;
    }
    return total;
}
console.log('hello'.repeatify(3));
```


##### 7. 这里采用递归算法，也不失为一个比较好的方法。
```javascript
function repeatify(n) {
    if (n == 1) {
        return this;
    }
    var s = repeatify(Math.floor(n / 2));
    s += s;
    if (n % 2) {
        s += this;
    }
    return s
}
console.log('hello'.repeatify(3));
```


##### 8. 这个方法的优点是代码量较小，但是当n达到一个量级时，这种做法就会劣于已上方案。
```javascript
function repeatify(n) {
    return (n <= 0) ? '' : this.concat(repeatify(--n))
}
console.log('hello'.repeatify(3));
```

##### 9. 使用ES6的写法：String.prototype.repeat()
```javascript
function repeatify(n) {
    var s = this;
    if (n > 0){
        return this.repeat(n);
    } else {
        return '';
    }
}
console.log('hello'.repeatify(3));
```

已上还需要考虑几个问题，这么多种做法，那种做法更好，是所谓的最优解。

> 普遍认为数组join()拼接字符串比字符串+拼接速度要快得多，最新查到一些说法是V8下+拼接字符串，要比数组join()拼接字符串快。

上面这段引用出自一篇博客，我没有考究过，实在是欠缺更深次的研究了。

经过查阅资料：

[https://segmentfault.com/a/1190000007481700](https://note.youdao.com/)

[https://www.zhihu.com/question/19747496](https://note.youdao.com/)
```
1.在旧浏览器（ie7-）下用 join 会更高效。

2.在现代浏览器，尽量用 "+" , 更高效。

3.当然，在少数现代浏览器里 "+" 不一定会比 join 快（如，safari 5.0.5，opera 11.10)

4.本身是字符串数组的，直接 join 会更好。

5.在 "+" 与 concat 之间，当然是优选使用 "+" ，方便又直观又高效。
```


每次回顾一个知识点，越发觉得自己知识匮乏，惶惶不安，还是脚踏实地，静下心来学习吧，共勉。