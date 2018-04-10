es6笔记
=====================================

## 2018年4月2日

1.let命令

* let声明的变量只在它所在的代码块有效，var声明的变量在全局范围内都有效；
* var命令会发生”变量提升“现象，即变量可以在声明之前使用，值为undefined；let所声明的变量一定要在声明后使用，否则报错；
* 如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。
  总之，在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。
* let不允许在相同作用域内，重复声明同一个变量，不能在函数内部重新声明参数。

2.块级作用域

* 避免内层变量覆盖外层变量，避免用来计数的循环变量泄露为全局变量；

3.const命令

* const声明一个只读的常量。一旦声明，常量的值就不能改变；
* 只在声明所在的块级作用域内有效。
* const命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用；
* const声明的常量，也与let一样不可重复声明。

**const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动，至于它指向的数据结构是不是可变的，就完全不能控制了，若真的想将对象冻结，应该使用Object.freeze方法**

4.变量的解构赋值

* 数组的解构：元素是按次序排列的，变量的取值由它的位置决定；模式匹配：等号两边的模式相同，左边的变量就会被赋予对应的值；不完全解构：等号左边的模式，只匹配一部分的等号右边的数组；
* 对象的解构：对象的属性没有次序，变量必须与属性同名，才能取到正确的值；

```
let obj = { first: 'hello', last: 'world' };
let { first: f, last: l } = obj;
let object = {f, l};
console.log(object)        // object = { f: 'hello', l: 'world'}
```
```
解构也可以用于嵌套结构的对象
let obj = {
  p: [
    'Hello',
    { y: 'World' }
  ]
};

let {p: [xx, { y }] } = obj;
console.log('y', y)   // world     对象必须命名一致，数组只需保持一致的位置即可
let q = [xx, { y }]
console.log('q', q)  // q = ['hello', { y: 'World' }]
```

**解构赋值允许指定默认值（数组对象均可以）**

```
let [x = 1, y = x] = [1, 2]; // x=1; y=2
let [x = y, y = 1] = [];     // ReferenceError: y is not defined
{x: y = 3} = {x: 5};    y // 5
```

* 字符串的解构赋值: 字符串被转换成了一个类似数组的对象

```
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"

属性解构赋值：let {length : len} = 'hello';       len // 5
```
* 函数参数的结构赋值

```
[[1, 2], [3, 4]].map(([a, b]) => a + b);
// [ 3, 7 ]
```

5.字符串的扩展

* 确定一个字符串是否包含在另一个字符串中: indexOf(JavaScript)、 includes()：返回布尔值，表示是否找到了参数字符串、startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部、endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部；

**使用第二个参数n时，endsWith的行为与includes()、startsWith()方法有所不同。它针对前n个字符，而其他两个方法针对从第n个位置直到字符串结束**

* repeat()：表示将原字符串重复n次，参数如果是小数，会被取整，参数是负数或者Infinity，会报错；
* padStart()、padEnd()：两个参数，第一个参数用来指定字符串的最小长度，第二个参数是用来补全的字符串；

6.数值的扩展

* parseInt()、parseFloat()、isInteger()
* Math.trunc() 用于去除一个数的小数部分，返回整数部分；
* Math.sign() 用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。
* Math.cbrt() 用于计算一个数的立方根;
* Math.imul() 返回两个数以 32 位带符号整数形式相乘的结果，返回的也是一个 32 位的带符号整数
* Math.hypot() 返回所有参数的平方和的平方根;
* 指数运算符（**）
* 指数运算符可以与等号结合，形成一个新的赋值运算符（**=）

## 2018年4月3号

7.函数的扩展

* 允许为函数的参数设置默认值，即直接写在参数定义的后面

```
function log(x, y = 'World') {
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello
```
* 参数默认值可以与解构赋值的默认值，结合起来使用

```
function foo({x, y = 5}) {
  console.log(x, y);
}

foo({}) // undefined 5
foo({x: 1}) // 1 5
foo({x: 1, y: 2}) // 1 2
foo() // TypeError: Cannot read property 'x' of undefined
```
```
function fetch(url, { body = '', method = 'GET', headers = {} }) {
  console.log(method);
}

fetch('http://example.com', {})
// "GET"

fetch('http://example.com')
// 报错
```
```
function fetch(url, { body = '', method = 'GET', headers = {} } = {}) {
  console.log(method);
}

fetch('http://example.com')
// "GET"
```
* 参数默认值的位置

```
// 例一
function f(x = 1, y) {
  return [x, y];
}

f() // [1, undefined]
f(2) // [2, undefined])
f(, 1) // 报错
f(undefined, 1) // [1, 1]

// 例二
function f(x, y = 5, z) {
  return [x, y, z];
}

f() // [undefined, 5, undefined]
f(1) // [1, 5, undefined]
f(1, ,2) // 报错
f(1, undefined, 2) // [1, 5, 2]      // 若改为null，则没有这个效果，不能触发默认值
```
**上面代码中，有默认值的参数都不是尾参数。这时，无法只省略该参数，而不省略它后面的参数，除非显式输入undefined。
如果传入undefined，将触发该参数等于默认值，null则没有这个效果。
**
