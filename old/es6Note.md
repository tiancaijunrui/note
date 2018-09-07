---
title: es6Note
tags: test
categories: test
---
### let,const 和block 作用域
- let 允许创建块级作用域，ES6推荐再函数中使用let，而非var
- 同样在块级作用域有效的令一个变量声明方式是const，他可以声明一个常量，ES6中，const声明的常量类似与指针，他指向某个引用，也就是说这个[常量]并非一成不变的。

```
{
  const ARR = [5,6];
  ARR.push(7);
  console.log(ARR); // [5,6,7]
  ARR = 10; // TypeError
}
```
- 有几个点需要注意：
	- let关键字声明的变量不具备变量提升（hoisting）特性
	- let和const声明只在最靠近的一个块中（花括号内）有效
	- 当使用常量const声明时，请使用大写变量。
	- const在声明时必须被赋值

### 箭头函数
- ES6中，箭头函数就是函数的一种简写形式，使用括号包裹参数，跟随一个 =>,紧接着是函数体：

```
var getPrice = function() {
  return 4.55;
};
 
// Implementation with Arrow Function
var getPrice = () => 4.55;
```
- 函数中this在箭头函数中，指向的是上一个function

```
function Person(){
  this.age = 0;
 
  setInterval(() => {
    // |this| 指向 person 对象
    this.age++;
  }, 1000);
}
 
var person = new Person();
```

### 函数参数默认值
- ES6 中允许你对函数参数设置默认值
```
let getFinalPrice = (price, tax=0.7) => price + price * tax;
getFinalPrice(500); // 850
```
### 对象词法扩展
- ES6 允许声明在对象字面量时使用简写语法，来初始化属性变量和函数的定义方法，并且允许在对象属性中进行计算操作：

```
function getCar(make, model, value) {
  return {
    // 简写变量
    make,  // 等同于 make: make
    model, // 等同于 model: model
    value, // 等同于 value: value
 
    // 属性可以使用表达式计算值
    ['make' + make]: true,
 
    // 忽略 `function` 关键词简写对象函数
    depreciate() {
      this.value -= 2500;
    }
  };
}
 
let car = getCar('Barret', 'Lee', 40000);
 
// output: {
//     make: 'Barret',
//     model:'Lee',
//     value: 40000,
//     makeKia: true,
//     depreciate: function()
// }
```

### 对象超类
- ES6 允许在对象中使用super方法

### 模板语法和分隔符
- ${...} 用来渲染一个变量
- `作为分隔符

### for ... of VS for ... in 
- for...of 用于遍历一个迭代器
```
let nicknames = ['di', 'boo', 'punkeye'];
nicknames.size = 3;
for (let nickname of nicknames) {
  console.log(nickname);
}
// 结果: di, boo, punkeye
```
- for ... in 用来遍历对象中的属性

```
let nicknames = ['di', 'boo', 'punkeye'];
nicknames.size = 3;
for (let nickname in nicknames) {
  console.log(nickname);
}
Result: 0, 1, 2, size
```
### Map 和 WeakMap

- ES6中两种新的数据结构集：map和weakMap。事实上每个对象都可以看作是一个Map。
- WeakMap
	- WeakMap 就是一个Map，只不过他的所有key都是弱应用，意思就是WeakMap中的东西垃圾回收时不考虑，使用它不用担心内存泄漏的问题。
	- 另一个需要注意的点是，WeakMap的所有key必须是对象，它只有四个方法delete(key),has(key),get(key),set(key,val)

### Set 和WeakSet
- Set对象是一组不重复的值，重复的值将被忽略，值类型可以是原始类型和引用类型
- 可以通过forEach和for—。。。of来遍历Set对象
- Set 同样有delete（）和clear（）方法
- WeakSet 类似与WeakMap，WeakSet 对象可以让你在一个集合中保存对象的弱引用，在WeakSet中的对象只允许出现一次
### 迭代器
- 迭代器允许访问数据集合的一个元素，当指针指向数据集合最后一个元素时，迭代器便会退出，他提供了next（）函数来便利一个序列，这个方法返回又给包含done和value 属性的对象。
- ES6中可以通过Symbol.iterator 给对象设置默认的遍历器，无论什么时候对象需要被遍历。执行他的@@iterator方法便可以返回一个用于获取值的迭代器。
- 数组默认就是一个迭代器，你可以通过[Symbol.iterator]()自定义一个对象的迭代器

```
var arr = [11,12,13];
var itr = arr[Symbol.iterator]();
 
itr.next(); // { value: 11, done: false }
itr.next(); // { value: 12, done: false }
itr.next(); // { value: 13, done: false }
 
itr.next(); // { value: undefined, done: true }
```
### Generators
- Generators 函数是ES6的新特性，他允许一个函数返回的可遍历对象生成多个值。在使用中你会看到*语法和一个新的关键字yield：

```
function *infiniteNumbers() {
  var n = 1;
  while (true){
    yield n++;
  }
}
 
var numbers = infiniteNumbers(); // returns an iterable object
 
numbers.next(); // { value: 1, done: false }
numbers.next(); // { value: 2, done: false }
numbers.next(); // { value: 3, done: false }
// 每次执行yield时，返回的值变为迭代器的下一个值。
```

### Promises
- Es6 对Promise有了原生的支持，一个Promise是一个等待被异步执行的对象，当他执行完成后，其状态会变成resolved或者rejected。
- 每一个Promise都有一个.then方法，这个方法接受两个参数，第一个是处理resolved状态的回调，一个是处理rejected状态的回调

```
var p = new Promise(function(resolve, reject) {  
  if (/* condition */) {
    // fulfilled successfully
    resolve(/* value */);  
  } else {
    // error, rejected
    reject(/* reason */);  
  }
});
```
参考连接：http://www.runoob.com/w3cnote/es6-concise-tutorial.html