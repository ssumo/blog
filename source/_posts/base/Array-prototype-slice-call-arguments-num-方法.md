---
title: 'Array.prototype.slice.call(arguments, num)方法'
date: 2018-09-18 11:56:04
tags: JS
categories: JS
---


每次看见这个方法都不知道干什么用的, 也没有仔细研究下。今天来写写它的用法吧,也是给自己加深一下印象。
从这个字面意思看,其实还是挺好理解的。
- slice 的用法说白了是截取数组用的，之后会返回一个新数组, 原始数组不变.用法为arrayObj.slice(start, [end]);
- call 从字面意思理解为截取出来, 用法为call(this, arg1, arg2);
- arguments 并不是真正的数组对象，只是与数组类似而已，所以它并没有slice这个方法，而Array.prototype.slice.call(arguments, 1)可以理解成是让 arguments 转换成一个数组对象，让 arguments 具有 slice() 方法。要是直接写arguments.slice(1)会报错。

```javascript
// slice截取, 第一个传值为位置, 第二个传值为个数
var a = [1,2,3,4,5]
console.log(a.slice(0,2)) // [1,2]
console.log(a) //[1,2,3,4,5]
```

#### 简单回顾下call的用法: 

我们经常拿call, apply来改变原有的this指向,
call(this, arg1, arg2)
apply(this, [arg1, arg2]), 当然还有bind也可以改变this的指向。

call和apply比较:

1.接受参数不同,前者是参数值, 后者是数组。

2.call 需要把参数按顺序传递进去，而 apply 则是把参数放在数组里。

> JavaScript 中，某个函数的参数数量是不固定的，因此要说适用条件的话，当你的参数是明确知道数量时用 call 。
而不确定的时候用 apply，然后把参数 push 进数组传递进去。当参数数量不确定时，函数内部也可以通过 arguments 这个伪数组来遍历所有的参数。

```javascript
// 改变this指向
function fruits() {}

fruits.prototype = {
    color: "red",
    say: function() {
        console.log("My color is " + this.color);
    }
}
var apple = new fruits;
apple.say();    //My color is red

banana = {
    color: "yellow"
}
apple.say.call(banana);     //My color is yellow
apple.say.apply(banana);    //My color is yellow
```


```javascript
// num没有最大值, Math方法有最大值方法
var  numbers = [5, 458 , 120 , -215 ]; 
var maxInNumbers = Math.max.apply(Math, numbers),   //458
    maxInNumbers = Math.max.call(Math,5, 458 , 120 , -215); //458

```

3.验证是否是数组（前提是toString()方法没有被重写过）

```javascript
// 验证是否是数组
functionisArray(obj){ 
    return Object.prototype.toString.call(obj) === '[object Array]' ;
}
```

4.类（伪）数组使用数组方法(我们今天的主角)

```javascript
var domNodes = Array.prototype.slice.call(document.getElementsByTagName("*"));
```
> Javascript中存在一种名为伪数组的对象结构。比较特别的是 arguments 对象，还有像调用 getElementsByTagName , document.childNodes 之类的，它们返回NodeList对象都属于伪数组。不能应用 Array 下的 push , pop 等方法。
但是我们能通过 Array.prototype.slice.call() 转换为真正的数组的带有 length 属性的对象，这样 domNodes 就可以应用 Array 下的所有方法了。


```javascript
//定义一个 log 方法，让它可以代理 console.log 方法，每一个 log 消息添加一个"(app)"的前辍, 解决方法是：
function log(){
  var args = Array.prototype.slice.call(arguments);
  args.unshift('(app)');
  console.log.apply(console, args);
};
```

#### 简单回顾下arguments对象的用法:

- 无需明确指出参数名, 就可以访问
- 检测参数个数

```javascript
// 检测参数个数
function howManyArgs() {
  console.log(arguments.length);
}

howManyArgs("string", 45);  // 2
howManyArgs(); // 0
howManyArgs(12); // 1
```

- 模拟函数重载

```javascript
// 模拟函数重载
function doAdd() {
  if(arguments.length == 1) {
    console.log(arguments[0] + 5);
  } else if(arguments.length == 2) {
    console.log(arguments[0] + arguments[1]);
  }
}

doAdd(10);	// 15
doAdd(40, 20);	// 60
```

#### 戏太长了,让我们回到Array.prototype.slice.call()

```javascript
// 检测取值
function test(a,b,c,d) { 
    var arg = Array.prototype.slice.call(arguments, 1); 
    console.log(arg); 
} 
test("a","b","c","d"); //[b", "c", "d"]
```

```javascript
// 对比下面的用法(能将具有length属性的对象转成数组)

// length长度和值对应的情况下,转化为数组
var a = {
	length: 2,
	0: 'abc',
	1: 'qwe'
}
console.log(Array.prototype.slice.call(a,0))  // ['abc', 'qwe']
console.log(Array.prototype.slice.call(a,1))  // ['qwe']

// 没有length长度的时候为空数组
var a = {
	length: 0,
	0: 'abc',
	1: 'qwe'
}
console.log(Array.prototype.slice.call(a,0))  // []
console.log(Array.prototype.slice.call(a,1))  // []

var a = {
	0: 'abc',
	1: 'qwe'
}
console.log(Array.prototype.slice.call(a,0))  // []
console.log(Array.prototype.slice.call(a,1))  // []

// 没有参数值的时候为有长度的数组
var a = {
	length: 2,
}
console.log(Array.prototype.slice.call(a,0))  // [empty*2] length为2
console.log(Array.prototype.slice.call(a,1))  // [empty] length为1
```





```javascript
// bind函数的封装
function bind(func, thisArg) {
    var nativeBind = Function.prototype.bind;
    var slice = Array.prototype.slice;
    if (nativeBind && func.bind === nativeBind) {
        return nativeBind.apply(func, slice.call(arguments, 1));
    }

    var args = slice.call(arguments, 2);
    return function () {
        return func.apply(thisArg, args.concat(slice.call(arguments)));
    };
}
```


#### 将函数的实际参数转换成数组的方法

- 方法一：var args = Array.prototype.slice.call(arguments);
- 方法二：var args = [].slice.call(arguments, 0);
- 方法三：

```javascript
var args = []; 
for (var i = 1; i < arguments.length; i++) { 
    args.push(arguments[i]);
}
```

```javascript
// 通用写法
var toArray = function(s){
    try{
        return Array.prototype.slice.call(s);
    } catch(e){
        var arr = [];
        for(var i = 0,len = s.length; i < len; i++){
            //arr.push(s[i]);
            arr[i] = s[i];  //据说这样比push快
        }
         return arr;
    }
}
```








