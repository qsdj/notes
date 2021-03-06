---
title: 数组
date: 2012-12-11
category: grammar
layout: page
modifiedOn: 2013-11-24
---

## 概述

### 数组的定义

数组（array）是按次序排列的一组值，单个值称为元素，它们的位置都有编号（从0开始）。整个数组用方括号表示。

```javascript

var arr = ['a', 'b', 'c'];

```

上面代码中的a、b、c就构成一个数组，两端的方括号是数组的标志，a是0号位置，b是1号位置，c是2号位置。

除了在定义时赋值，数组也可以先定义后赋值。

```javascript

var arr = [];

arr[0] = 'a';
arr[1] = 'b';
arr[2] = 'c';

```

上面的代码与前一段代码是等价的。

任意一种类型的数据，都可以放入数组。

```javascript

var arr = [{a:1}, [1,2,3],function (){return true;}];

arr[0] // Object {a: 1}
arr[1] // [1, 2, 3]
arr[2] // function (){return true;}

```

上面数组的3个成员分别是对象、数组、函数。

如果数组的元素还是数组，就形成了多维数组。

```javascript

var a = [[1,2],[3,4]];
a[0][1] // 2
a[1][1] // 4

```

### 数组与对象的关系

本质上，数组也属于对象，是字典结构（dictionary）的一个变种。

```javascript

var arr = [1, 2, 3];

typeof arr // "object"

```

上面代码表明，由于数组只是一种特殊的对象，所以typeof运算符返回数组的类型是object。

数组的特殊性体现在，它的键都是整数，但是对象内部是以字符串来识别一个键，因此以数值或字符串作为键名，都能读取数组的成员。

```javascript

var arr = ['a', 'b', 'c'];

arr['0'] // 'a'
arr[0] // 'a'

```

上面代码分别用数值和字符串作为键名，结果都能读取数组。

另外，由于数组的键默认是按次序排列的整数（0，1，2...），所以数组不用为每个元素指定键名，而对象的每个值都必须指定键名。

上一节说过，对象有两种读取成员的方法：“点”结构（object.key）和方括号结构（object[key]）。但是，对于数字的键名，不能使用点结构，arr.0的写法不合法，因为单独的数字不能作为标识符（identifier）。所以，数组成员只能用方括号arr[0]表示。

### length属性

数组的length属性，返回数组的成员数量。

```javascript

['a', 'b', 'c'].length // 3

```

JavaScript使用一个32位整数，保存数组的元素个数。这意味着，数组成员最多只有4294967295个（2<sup>32</sup>-1）个，也就是说length属性的最大值就是4294967295。

数组与对象的length属性，也有区别。对象可以有也可以没有length属性，但是数组一定有。而且，数组的length属性是一个动态的值，等于键名中的最大整数加上1。

```javascript

var arr = ['a', 'b'];
arr.length // 2

arr[2] = 'c';
arr.length // 3

arr[9] = 'd';
arr.length // 10

arr[1000] = 'e';
arr.length // 1001

```

上面代码表示，数组的数字键不需要连续，length属性的值总是比最大的那个整数键大出1。

length属性是可写的。如果人为设置一个小于当前成员个数的值，该数组的成员会自动减少到length设置的值。

```javascript

var arr = [ 'a', 'b', 'c' ];
arr.length // 3

arr.length = 2;
arr // ["a", "b"]

```

上面代码表示，当数组的length属性设为2，即最大的整数键只能是1，那么整数键2（值为c）就已经不在数组中了，被自动删除了。

将数组清空的一个有效方法，就是将length属性设为0。

```javascript

var arr = [ 'a', 'b', 'c' ];

arr.length = 0;
arr // []

```

如果人为设置length大于当前元素个数，则数组的成员数量会增加到这个值，新增的位置填入空元素。

```javascript

var a = ['a'];

a.length = 3;
a // ["a", undefined × 2]

```

上面代码表示，当length属性设为大于数组个数时，新增的位置都填充为undefined。

如果人为设置length为不合法的值，JavaScript会报错。

```javascript

// 设置负值
[].length = -1
// RangeError: Invalid array length

// 数组元素个数大于等于2的32次方
[].length = Math.pow(2,32)
// RangeError: Invalid array length

// 设置字符串
[].length = 'abc'
// RangeError: Invalid array length

```

值得注意的是，由于数组本质上是对象的一种，所以我们可以为数组添加属性，但是这不影响length属性的值。

```javascript

var a = [];

a["p"] = "abc";
a.length // 0

a[2.1] = "abc";
a.length // 0

```

上面代码将数组的键分别设为字符串和小数，结果都不影响length属性。因为，length属性的值就是等于最大的数字键加1，而这个数组没有整数键，所以length属性保持为0。

### 数组的空位

当数组的某个位置是空元素（比如两个逗号之间没有任何值，或者值为undefined），我们称该数组存在空位（hole）。

```javascript

var a = [1,,1];

a
// [1, undefined, 1]

```

需要注意的是，如果最后一个元素后面有逗号，并不会产生空位。也就是说，有没有这个逗号，结果都是一样的。不过，这一条对IE 8及以下版本不适用。

```javascript

var a = [1,2,3,];

a.length // 3
a // [1, 2, 3]

```

上面代码中，原数组最后一个成员后面有一个逗号，这不影响length属性的值，与没有这个逗号时效果一样。

使用delete命令删除一个值，会形成空位。

```javascript

var a = [1,2,3];

delete a[1];
a
// [1, undefined, 3]

```

需要注意的是，如果使用delete命令删除一个值，不影响length属性。

```javascript

var a = [1,2,3];
a.length // 3

delete a[1];
a // [1, undefined, 3]
a.length // 3

delete a[2];
a // [1, undefined, undefined]
a.length // 3

```

上面代码用delete命令删除了两个键，对length属性没有影响。因为这两个键还在，只是值变为了undefined。也就是说，length属性不过滤undefined的值。

由于length属性不计入非整数键且不过滤undefined，所以使用它进行数组遍历，一定要非常小心。

```javascript

for (var i=0; i<a.length; i++){
	// some code
}

```

上面的写法会遗漏数组对象的非整数键，并且会包括空位。

空位通过空值生成，还是通过显式设为undefined生成，有一个细微的差别。如果通过空值生成，使用数组的forEach方法或者for...in结构进行遍历，空位就会被跳过。

```javascript

var a = [,,,];

a.forEach(function (x, i) { console.log(i+". "+x) })
// 不产生任何输出 

for (var i in a){console.log(i)}
// 不产生任何输出

```

如果空位是通过显式定义undefined生成，遍历的时候就不会被跳过。

```javascript

var a = [undefined,undefined,undefined];

a.forEach(function (x, i) { console.log(i+". "+x) });
// 0. undefined
// 1. undefined
// 2. undefined

for (var i in a){console.log(i)}
// 0
// 1
// 2

```

### in运算符，for...in循环

检查某个键是否存在的运算符in，适用于对象，也适用于数组。

```javascript

2 in [ 'a', 'b', 'c' ]
// true

'2' in [ 'a', 'b', 'c' ]
// true

```

使用for-in循环，可以遍历数组的所有元素。

```javascript

var a = [1,2,3];

for (var i in a){
	console.log(a[i]);
}
// 1
// 2
// 3

```

需要注意的是，for-in会遍历数组所有的键，即使是非数字键。

```javascript

var a = [1,2,3];
a.foo = true;
for (var key in a) { 
	console.log(key); 
}
// 0
// 1
// 2
// foo

```

所以，使用for-in遍历数组的时候，一定要小心。

另一种遍历的做法是用for循环或者while循环结合length属性。

```javascript

var a = [1,2,3];
for(var i = 0; i < a.length; i++){
	console.log(a[i]);
}

// or

var i = 0;
while (i< a.length){
	console.log(a[i]);
	i++;
}

// or

var l = a.length;
while (l--){
	console.log(a[l]);
}

```

上面代码是三种遍历数组的写法。需要注意的是，最后一种写法是逆向遍历，即从最后一个元素向第一个元素遍历。

### Array构造函数

除了直接使用方括号创建，数组还可以使用JavaScript内置的Array构造函数创建。但是，Array构造函数的参数定义不符合直觉，使用一个参数时表示数组的长度，使用一个以上参数时表示数组的定义，所以建议总是直接采用方括号创建数组。

```javascript

var a = new Array();
a // []
a.length // 0

var a = new Array(1);
a // [undefined × 1]
a.length // 1

var a = new Array(2);
a // [undefined × 2]
a.length // 2

var a = new Array(1,2);
a // [1,2]
a.length // 2

```

对Array构造函数的详细介绍，参见《标准库》一章的《Array对象》。

## 参考链接

- Axel Rauschmayer, [Arrays in JavaScript](http://www.2ality.com/2012/12/arrays.html)
- Axel Rauschmayer, [JavaScript: sparse arrays vs. dense arrays](http://www.2ality.com/2012/06/dense-arrays.html)
- Felix Bohm, [What They Didn’t Tell You About ES5′s Array Extras](http://net.tutsplus.com/tutorials/javascript-ajax/what-they-didnt-tell-you-about-es5s-array-extras/)
- Juriy Zaytsev, [How ECMAScript 5 still does not allow to subclass an array](http://perfectionkills.com/how-ecmascript-5-still-does-not-allow-to-subclass-an-array/)
