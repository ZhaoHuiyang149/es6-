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
如果传入undefined，将触发该参数等于默认值，null则没有这个效果。**

* rest参数：rest参数（形式为...变量名）搭配的变量是一个数组，该变量将多余的参数放入数组中，代替arguments对象

```
function add(...values) {
  let sum = 0;

  for (var val of values) {
    sum += val;
  }

  return sum;
}

add(2, 5, 3) // 10
```
```
下面是一个 rest 参数代替arguments变量的例子。
// arguments变量的写法
function sortNumbers() {
  return Array.prototype.slice.call(arguments).sort();
}

// rest参数的写法
const sortNumbers = (...numbers) => numbers.sort();
```
**rest 参数之后不能再有其他参数（即只能是最后一个参数），否则会报错**
```
function f(a, ...b, c) {       // 报错
  // ...
}
```

* name属性：返回该函数的函数名。
* 箭头函数：使用“箭头”（ => ）定义函数
```
var sum = (num1, num2) => { return num1 + num2 };
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};
```
**箭头函数使用需要注意：**
**1.函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。**
```
function Timer() {
  this.s1 = 0;
  this.s2 = 0;
  setInterval(() => this.s1++, 1000);     // 箭头函数，this绑定定义时所在的作用域（即Timer函数）
  setInterval(function () {       // 普通函数，this绑定的是运行时所在的作用域（即全局对象）
    this.s2++;
  }, 1000);
}

var timer = new Timer();

setTimeout(() => console.log('s1: ', timer.s1), 3100);
setTimeout(() => console.log('s2: ', timer.s2), 3100);
// s1: 3
// s2: 0
```
2.不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
3.不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。   
4.不可以使用yield命令，因此箭头函数不能用作 Generator 函数。
```
生成器函数（generator function）和 yield 通常会被用来演示生成斐波那契数列（前两个数字都是 1 ，除此之外任何数字都是前两个数之和的数列）
function* fab(max) {                  // function 后跟上「*」即为生成器函数
    var count = 0, last = 0, current = 1;

    while(max > count++) {
        yield current;              // 生成器函数通常和 yield 关键字同时使用。函数执行到每个 yield 时都会中断并返回 yield 的右值（通过 next 方法返回对象中的 value 字段）。下次调用 next，函数会从 yield 的下一个语句继续执行。等到整个函数执行完，next 方法返回的 done 字段会变成 true, 若函数未执行完，则next方法返回的 done 字段为 false
        var tmp = current;
        current += last;
        last = tmp;
    }
}

var o = fab(10), ret, result = [];

while(!(ret = o.next()).done) {
    result.push(ret.value);
}

console.log(result); // [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```
* 双冒号运算符：函数绑定运算符是并排的两个冒号（::），双冒号左边是一个对象，右边是一个函数。该运算符会自动将左边的对象，作为上下文环境（即this对象），绑定到右边的函数上面。用来取代call、apply、bind调用。
```
foo::bar;
// 等同于
bar.bind(foo);

foo::bar(...arguments);
// 等同于
bar.apply(foo, arguments);

var method = obj::obj.foo;
// 等同于
var method = ::obj.foo;
```
* 尾调用优化：指某个函数的最后一步是调用另一个函数
```
function f(x){
  return g(x);
}
```
* 尾递归：尾调用自身，就称为尾递归
```
正常递归（复杂度 O(n) ）：
function factorial(n) {
  if (n === 1) return 1;
  return n * factorial(n - 1);
}

factorial(5) // 120

尾递归（复杂度 O(1)）：
function factorial(n, total) {
  if (n === 1) return total;
  return factorial(n - 1, n * total);
}

factorial(5, 1) // 120
```
```
尾递归优化过的 Fibonacci 数列实现如下：
function Fibonacci2 (n , ac1 = 1 , ac2 = 1) {
  if( n <= 1 ) {return ac2};

  return Fibonacci2 (n - 1, ac2, ac1 + ac2);
}

Fibonacci2(100) // 573147844013817200000
Fibonacci2(1000) // 7.0330367711422765e+208
```

8.数组的扩展

* 扩展运算符（spread）：三个点（...）。它好比rest参数的逆运算，将一个数组转为用逗号分隔的参数序列。能够替代函数的apply方法；
```
function add(x, y) {
  return x + y;
}

const numbers = [4, 38];
add(...numbers) // 42
```
```
// ES5 的写法
Math.max.apply(null, [14, 3, 77])

// ES6 的写法
Math.max(...[14, 3, 77])

// 等同于
Math.max(14, 3, 77);

// ES6 的写法
let arr1 = [0, 1, 2];
let arr2 = [3, 4, 5];
arr1.push(...arr2);
```
* 扩展运算符的应用:
```
复制数组： 
const a1 = [1, 2];
// 写法一
const a2 = [...a1];
// 写法二
const [...a2] = a1;

合并数组：
var arr1 = ['a', 'b'];
var arr2 = ['c'];
[...arr1, ...arr2]
// [ 'a', 'b', 'c']

与解构赋值结合：（扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错）
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]

const [...butLast, last] = [1, 2, 3, 4, 5];
// 报错

字符串：扩展运算符还可以将字符串转为真正的数组。
[...'hello']
// [ "h", "e", "l", "l", "o" ]

Map 和 Set 结构，Generator 函数：
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

let arr = [...map.keys()]; // [1, 2, 3]

const go = function*(){
  yield 1;
  yield 2;
  yield 3;
};

[...go()] // [1, 2, 3]
```
* Array.of()：将一组值，转换为数组；弥补数组构造函数Array()的不足。
```
Array.of(3, 11, 8) // [3,11,8]
Array(3) // [, , ,]
Array(3, 11, 8) // [3, 11, 8]
* 只有当参数个数不少于 2 个时，Array()才会返回由参数组成的新数组。参数个数只有一个时，实际上是指定数组的长度。
```
* copyWithin()：在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。使用这个方法，会修改当前数组。
它接受三个参数：
-target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
-start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示倒数。
-end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示倒数。
```
[1, 2, 3, 4, 5].copyWithin(0, 3)
// [4, 5, 3, 4, 5]
```