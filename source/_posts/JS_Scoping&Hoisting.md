---
title: JavaScript中的作用域和声明提升 Scoping & Hoisting in JavaScript
tags: 
- JavaScript
- Programming
---
**本文参考:
 Nicholas C.Zakas《JavaScript高级程序设计》2012
 Ben Cherry《JavaScript Scoping and Hoisting》2010
 相关链接:
 https://www.nczonline.net/
 http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html**

 首先要理解的是JS的**执行环境(execution context)**。执行环境定义了变量或者函数有权访问的其他数据。
 每一个执行环境都有一个预制关联的变量对象**(variable object)**，全局环境是最外围的执行环境，它的变量对象是window。
 因此所有的全局变量和函数都是作为window对象的属性和方法创建的；当一个执行环境中的所有代码执行完毕后，此环境会被销毁；全局环境在关闭网页/浏览器时销毁。

 代码在一个环境中执行时，会创建一个变量对象的**作用域链(scope chain)**，其作用是保证执行环境有权访问的所有变量和函数的有序访问。
 作用域链由当前环境变量环境向外展开，直到全局执行环境。

 了解了以上概念后，我们再回头来看。
 需要指出的是，我手上的第三版《JavaScript高级程序设计》第三版出版于2012年，因此在此书中依旧写到“JavaScript”没有块级作用域；
 实际上在2015年， ES6标准(Standard ECMA-262 6th Edition)中，为JS增加了“块级作用域”的概念，具体用法在下文表述。

 在C++中，我们可以定义这样一个代码块：
```
  int i = 1;
  cout<<i<<endl; //1
  
  if(true){
  	int i = 2;
  	cout<<i<<endl; //2
  }

  cout<<i<<endl; //1
```
 毫无疑问，它的输出结果是
```
 1 
 2
 1
```
 这是因为C++有块级作用域，i = 2这个赋值只作用于if这个block的花括号里，这不影响i是一个全局变量。

 但在JS中有所不同，请看下面的代码：
```
  var i = 1;
  console.log(i); //1

  if (true){
  	var i = 2;
  	console.log(i); //2
  }

  console.log(i); //2
```
 这段代码的输出结果是
```
 1 
 2
 2
```
 发现区别了吗？
 这是因为ES5及之前的版本中，JavaScript是函数级作用域（function-level-scope），在if、for这样的语句中使用var声明，并不会创建一个新的作用域，只有**函数**才能创建新的作用域。

 如果你必须在函数中创建一个临时的作用域，可以像下面这样做：
```
  var i = 1;
  console.log(i); //1

  if (true){
  	(function(){
  		var i = 2;
  		}());
  }

  console.log(i); //1
```
 这是一个灵活的办法，但是未免有些头痛医头的味道，而且嵌套关系不够简洁，不过在ES6中我们可以避免这样做。

 接下来说声明提升。

 声明提升是指，变量和函数在声明之前即可使用，值为undefined。

 变量提升：
 
```
  console.log(a); //undefined
  var a = 1;
```

 上面这段代码等价于
```
  var a;
  console.log(a); //undefined
  a = 1;
```
 
 划重点，在ES6中，添加了一种更加严谨的变量声明方法:使用let声明变量
 * let声明的变量拥有块级作用域。也就是说用let声明的变量的作用域只是外层块，而不是整个外层函数。
 * let声明的全局变量不是全局对象的属性。这就意味着，你不可以通过window.变量名的方式访问这些变量。它们只存在于一个不可见的块的作用域中，这个块理论上是Web页面中运行的所有JS代码的外层块。
 这是let声明方式带来的最显著的两个变化，其他的高级特性请参考：http://es6-features.org

 所以，以上两段代码如果改用let声明:
```
  console.log(a);
  let a = 1;
```
 就会输出
```
  a is not defined
```
 下面的代码可以看到对比：
```
  function doSth() {
  if (true) {
    var a = 4; // 作用域为 foo 函数体内
    let b = 5; // 作用域为 if{} 内
    console.log(a); // 4
    console.log(b); // 5
    }
  }
  console.log(a); // 4
  console.log(b); // 无法访问
```
 
 ES6中，let不再像var那样具有变量提升的特性了，如果代码块内存在let声明的变量，凡在声明语句前引用该变量，就会返回引用错误，因为从代码块内起始处至该let声明处，这个变量就会处于一个特殊区域，叫做:“临时死区”（temporal dead zone）
```
  if (true) {
    console.log(a); // ReferenceError
    let a = 3;
  }
```

 当然，不仅var声明的变量会提升，函数也存在这一现象。但不同的函数声明方式产生不同的结果。

 1.函数声明：

```
  doSth(); //1
  function doSth(){
  	console.log(1); 
  }
  doSth(); //1
```
 使用函数声明后，整个函数体都会提升

 2.函数表达式：
```
  doSth(); //doSth is not a function
  var doSth = function(){
  	console.log(1);
  }
  doSth(); //1
```
 函数表达式只有声明的名称被提升了，赋值部分并没有被提升

 3.函数构造法：
```
  doSth(233); //doSth is not a function
  var doSth = new Function("a","console.log(a)");
  doSth(666); //666
```
 这种方法不会产生变量提升。

 最后看一个例子：
```
  var doSth = function (){
  	console.log(1);
  }
  function doSth(){
  	console.log(2);
  }
  doSth();
```
 等价于：
```
  function doSth(){
    console.log(2);
  }
  var doSth;
  doSth = function(){
  	console.log(1);
  }
  doSth();
```
 所以最后输出结果为
```
  1
```

 综上，为了避免bug，建议在ES6版本下出全局变量外声明均使用let。


