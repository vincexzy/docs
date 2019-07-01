# JavaScript

一，注释

原则：

As short as possible（如无必要，勿增注释）：尽量提高代码本身的清晰性、可读性。
As long as necessary（如有必要，尽量详尽）：合理的注释、空行排版等，可以让代码更易阅读、更具美感。
1. 单行注释

必须独占一行。// 后跟一个空格，缩进与下一行被注释说明的代码一致。

2. 多行注释

避免使用 /*...*/ 这样的多行注释。有多行注释内容时，使用多个单行注释。

3. 函数/方法注释

函数/方法注释必须包含函数说明，有参数和返回值时必须使用注释标识。；
参数和返回值注释必须包含类型信息和说明；
当函数是内部函数，外部不可访问时，可以使用 @inner 标识；
```
/**
 * 函数描述
 *
 * @param {string} p1 参数1的说明
 * @param {string} p2 参数2的说明，比较长
 *     那就换行了.
 * @param {number=} p3 参数3的说明（可选）
 * @return {Object} 返回值描述
 */
function foo(p1, p2, p3) {
    var p3 = p3 || 10;
    return {
        p1: p1,
        p2: p2,
        p3: p3
    };
}
```
4. 文件注释

文件注释用于告诉不熟悉这段代码的读者这个文件中包含哪些东西。 应该提供文件的大体内容, 它的作者, 依赖关系和兼容性信息。如下:

```
/**
 * @fileoverview Description of file, its uses and information
 * about its dependencies.
 * @author user@meizu.com (Firstname Lastname)
 * Copyright 2009 Meizu Inc. All Rights Reserved.
 */
```
二，命名

变量

变量使用‘驼峰’命名法
私有属性、变量和方法以下划线 _ 开头。
常量, 使用全部字母大写，单词间下划线分隔的命名方式。
函数

　　函数：

函数, 使用 Camel 命名法。
函数的参数, 使用 Camel 命名法。
　　类

类, 使用 Pascal 命名法
类的 方法 / 属性, 使用 Camel 命名法
　　枚举属性

枚举变量 使用 Pascal 命名法。
枚举的属性， 使用全部字母大写，单词间下划线分隔的命名方式。
　　由多个单词组成的 缩写词，在命名中，根据当前命名法和出现的位置，所有字母的大小写与首字母的大小写保持一致。

三，命名语法
类名，使用名词。
函数名，使用动宾短语。
boolean 类型的变量使用 is 或 has 开头。
Promise 对象用动宾短语的进行时表达。



### JavaScript
#### Performance(性能)
> Favor readability, correctness and expressiveness over performance. JavaScript will basically never be your performance bottleneck. Optimize things like image compression, network access and DOM reflows instead. If you remember just one guideline from this document, choose this one.

基于性能的基础上，需要考虑代码可读性、正确性和表现性。`JavaScript`从根本上来说绝对不会成为你性能上的瓶颈。优化一些事物，例如图片压缩、网络介入以及`DOM`回流。如果在这篇文档中你只想记住这一个指导方针，那就就记住这个吧。

```
// bad (albeit way faster)
const arr = [1, 2, 3, 4];
const len = arr.length;
var i = -1;
var result = [];
while (++i < len) {
  var n = arr[i];
  if (n % 2 > 0) continue;
  result.push(n * n);
}

// good
const arr = [1, 2, 3, 4];
const isEven = n => n % 2 == 0;
const square = n => n * n;

const result = arr.filter(isEven).map(square);
```

#### Statelessness(无状态)
> Try to keep your functions pure. All functions should ideally produce no side-effects, use no outside data and return new objects instead of mutating existing ones.

请尽量保持你函数的纯洁性。所有的函数理想状态下都应该无副作用，请不要使用外部数据，同时返回的应该是一个新的对象而不是改变已有的。

```
// bad
const merge = (target, ...sources) => Object.assign(target, ...sources);
merge({ foo: "foo" }, { bar: "bar" }); // => { foo: "foo", bar: "bar" }

// good
const merge = (...sources) => Object.assign({}, ...sources);
merge({ foo: "foo" }, { bar: "bar" }); // => { foo: "foo", bar: "bar" }
```

#### Natives(原生)
> Rely on native methods as much as possible.

尽可能地使用JavaScript的原生方法。

```
// bad
const toArray = obj => [].slice.call(obj);

// good
const toArray = (() =>
  Array.from ? Array.from : obj => [].slice.call(obj)
)();
```

#### Coercion(类型隐式转换)
> Embrace implicit coercion when it makes sense. Avoid it otherwise. Don't cargo-cult.

当类型隐式转换是有意义的时候，请放心的使用它。如果没有意义，就别使用它。不要盲目地迷权威！

```
// bad
if (x === undefined || x === null) { ... }

// good
if (x == undefined) { ... }
```

#### Loops(循环)
> Don't use loops as they force you to use mutable objects. Rely on array.prototype methods.

当你不得不使用易变的对象的时候，不要使用循环。请考虑使用` array.prototype `方法，即数组原生方法。

```
// bad
const sum = arr => {
  var sum = 0;
  var i = -1;
  for (;arr[++i];) {
    sum += arr[i];
  }
  return sum;
};

sum([1, 2, 3]); // => 6

// good
const sum = arr =>
  arr.reduce((x, y) => x + y);

sum([1, 2, 3]); // => 6
```

> If you can't, or if using array.prototype methods is arguably abusive, use recursion.

如果你不想使用数组方法，或者说你觉得使用数组方法令你很不爽，那么你可以考虑使用递归。
```
// bad
const createDivs = howMany => {
  while (howMany--) {
    document.body.insertAdjacentHTML("beforeend", "<div></div>");
  }
};
createDivs(5);

// bad
const createDivs = howMany =>
  [...Array(howMany)].forEach(() =>
    document.body.insertAdjacentHTML("beforeend", "<div></div>")
  );
createDivs(5);

// good
const createDivs = howMany => {
  if (!howMany) return;
  document.body.insertAdjacentHTML("beforeend", "<div></div>");
  return createDivs(howMany - 1);
};
createDivs(5);
```

> Here's a [generic loop function](https://gist.github.com/bendc/6cb2db4a44ec30208e86) making recursion easier to use.

这里是一个[通用的循环函数](https://gist.github.com/bendc/6cb2db4a44ec30208e86)，它使得递归使用起来更加容易。

#### Arguments(arguments内置对象)
> Forget about the arguments object. The rest parameter is always a better option because:
 * it's named, so it gives you a better idea of the arguments the function is expecting
 * it's a real array, which makes it easier to use.
 
 请忘了`arguments`对象。额外的参数总会是一个较好的选择，因为：
 * 它已经被命名了，所以它能让你在实现函数的功能上有一个更好的期望，能够为你提供理想的参数。
 * 它是一个数组，会让你使用起来更加方面
 
 #### Apply
> forget about apply(). Use the spread operator instead.

请忘记关于`apply()`的一切，使用操作符去代替。

```
const greet = (first, last) => `Hi ${first} ${last}`;
const person = ["John", "Doe"];

// bad
greet.apply(null, person);

// good
greet(...person);
```

#### Bind
> Don't bind() when there's a more idiomatic approach.

当你有一个更合理的方法的时候，不要使用`bind()`。

```
// bad
["foo", "bar"].forEach(func.bind(this));

// good
["foo", "bar"].forEach(func, this);
```

```
// bad
const person = {
  first: "John",
  last: "Doe",
  greet() {
    const full = function() {
      return `${this.first} ${this.last}`;
    }.bind(this);
    return `Hello ${full()}`;
  }
}

// good
const person = {
  first: "John",
  last: "Doe",
  greet() {
    const full = () => `${this.first} ${this.last}`;
    return `Hello ${full()}`;
  }
}
```

#### Higher-order functions(高阶函数)
> Avoid nesting functions when you don't have to.

当你不是必须使用嵌套函数的时候，请避免它。

```
// bad
[1, 2, 3].map(num => String(num));

// good
[1, 2, 3].map(String);
```

#### Composition(组合函数)
> Avoid multiple nested function calls. Use composition instead.

请避免使用多层嵌套函数调用，使用组合函数去替代。

```
const plus1 = a => a + 1;
const mult2 = a => a * 2;

// bad
mult2(plus1(5)); // => 12

// good
const pipeline = (...funcs) => val => funcs.reduce((a, b) => b(a), val);
const addThenMult = pipeline(plus1, mult2);
addThenMult(5); // => 12
```

#### Caching(缓存)
>  Cache feature tests, large data structures and any expensive operation.

缓存功能测试，大数据结构以及任何高性能损耗的操作。

```
// bad
const contains = (arr, value) =>
  Array.prototype.includes
    ? arr.includes(value)
    : arr.some(el => el === value);
contains(["foo", "bar"], "baz"); // => false

// good
const contains = (() =>
  Array.prototype.includes
    ? (arr, value) => arr.includes(value)
    : (arr, value) => arr.some(el => el === value)
)();
contains(["foo", "bar"], "baz"); // => false
```

#### Variables(变量)
> Favor const over let and let over var.

推荐定义变量使用的优先级顺序，`const` > `let` > `var`。
```
// bad
var me = new Map();
me.set("name", "Ben").set("country", "Belgium");

// good
const me = new Map();
me.set("name", "Ben").set("country", "Belgium");
```

#### Conditions(条件语句)
> Favor IIFE's and return statements over if, else if, else and switch statements.

推荐使用`匿名执行函数`返回语句，代替 `if`,`else if`,`else`,`switch` 语句。

```
// bad
var grade;
if (result < 50)
  grade = "bad";
else if (result < 90)
  grade = "good";
else
  grade = "excellent";

// good
const grade = (() => {
  if (result < 50)
    return "bad";
  if (result < 90)
    return "good";
  return "excellent";
})();
```

#### Object iteration(对象迭代)
> Avoid for...in when you can.

如果可以的话，请不要使用 `for...in`。
```
const shared = { foo: "foo" };
const obj = Object.create(shared, {
  bar: {
    value: "bar",
    enumerable: true
  }
});

// bad
for (var prop in obj) {
  if (obj.hasOwnProperty(prop))
    console.log(prop);
}

// good
Object.keys(obj).forEach(prop => console.log(prop));
```

#### Objects as Maps
> While objects have legitimate use cases, maps are usually a better, more powerful choice. When in doubt, use a Map.

当对象有合理的用例时，`maps`是一个更棒更高效的选择。当有疑问时，请使用`map`!

```
// bad
const me = {
  name: "Ben",
  age: 30
};
var meSize = Object.keys(me).length;
meSize; // => 2
me.country = "Belgium";
meSize++;
meSize; // => 3

// good
const me = new Map();
me.set("name", "Ben");
me.set("age", 30);
me.size; // => 2
me.set("country", "Belgium");
me.size; // => 3
```

#### Curry(柯里化)
> Currying is a powerful but foreign paradigm for many developers. Don't abuse it as its appropriate use cases are fairly unusual.

柯里化的功能是很强大，但是对于很多开发者来说它是陌生的。不要滥用它，但适当地使用还是非常有用的。

```
// bad
const sum = a => b => a + b;
sum(5)(3); // => 8

// good
const sum = (a, b) => a + b;
sum(5, 3); // => 8
```

#### Readability(可读性)
> Don't obfuscate the intent of your code by using seemingly smart tricks.

不要试图通过一些看似聪明的小技巧来模糊混淆你代码本身的真实目的。

```
// bad
foo || doSomething();

// good
if (!foo) doSomething();
```
```
// bad
void function() { /* IIFE */ }();

// good
(function() { /* IIFE */ }());
```
```
// bad
const n = ~~3.14;

// good
const n = Math.floor(3.14);
```
#### Code reuse(代码复用)
> Don't be afraid of creating lots of small, highly composable and reusable functions.

不要害怕创建大量的小的，高组合型和可复用性的函数。

```
// bad
arr[arr.length - 1];

// good
const first = arr => arr[0];
const last = arr => first(arr.slice(-1));
last(arr);
```
```
// bad
const product = (a, b) => a * b;
const triple = n => n * 3;

// good
const product = (a, b) => a * b;
const triple = product.bind(null, 3);
```

#### Dependencies(依赖性)
> Minimize dependencies. Third-party is code you don't know. Don't load an entire library for just a couple of methods easily replicable:

降低代码的依赖性。因为第三方的代码库是你完全不知道的。不要为了几个简单的很容易被复制的方法去加载整个库。

```
// bad
var _ = require("underscore");
_.compact(["foo", 0]));
_.unique(["foo", "foo"]);
_.union(["foo"], ["bar"], ["foo"]);

// good
const compact = arr => arr.filter(el => el);
const unique = arr => [...Set(arr)];
const union = (...arr) => unique([].concat(...arr));

compact(["foo", 0]);
unique(["foo", "foo"]);
union(["foo"], ["bar"], ["foo"]);
```

# [JavaScript Standard Style](https://github.com/standard/standard)

这是 JavaScript [standard](https://github.com/standard/standard) 代码规范的全文。

掌握本规范的最好方法是安装并在自己的代码中使用它。

## 细则

* **使用两个空格**进行缩进。

  eslint: [`indent`](http://eslint.org/docs/rules/indent)

  ```js
  function hello (name) {
    console.log('hi', name)
  }
  ```

* 除需要转义的情况外，**字符串统一使用单引号**。

  eslint: [`quotes`](http://eslint.org/docs/rules/quotes)

  ```js
  console.log('hello there')
  $("<div class='box'>")
  ```

* **不要定义未使用的变量**。

  eslint: [`no-unused-vars`](http://eslint.org/docs/rules/no-unused-vars)

  ```js
  function myFunction () {
    var result = something()   // ✗ avoid
  }
  ```

* **关键字后面加空格**。

  eslint: [`keyword-spacing`](http://eslint.org/docs/rules/keyword-spacing)

  ```js
  if (condition) { ... }   // ✓ ok
  if(condition) { ... }    // ✗ avoid
  ```

* **函数声明时括号与函数名间加空格**。

  eslint: [`space-before-function-paren`](http://eslint.org/docs/rules/space-before-function-paren)

  ```js
  function name (arg) { ... }   // ✓ ok
  function name(arg) { ... }    // ✗ avoid

  run(function () { ... })      // ✓ ok
  run(function() { ... })       // ✗ avoid
  ```

* **始终使用** `===` 替代 `==`。<br>
  例外： `obj == null` 可以用来检查 `null || undefined`。

  eslint: [`eqeqeq`](http://eslint.org/docs/rules/eqeqeq)

  ```js
  if (name === 'John')   // ✓ ok
  if (name == 'John')    // ✗ avoid
  ```

  ```js
  if (name !== 'John')   // ✓ ok
  if (name != 'John')    // ✗ avoid
  ```

* **字符串拼接操作符 (Infix operators)** 之间要留空格。

  eslint: [`space-infix-ops`](http://eslint.org/docs/rules/space-infix-ops)

  ```js
  // ✓ ok
  var x = 2
  var message = 'hello, ' + name + '!'
  ```

  ```js
  // ✗ avoid
  var x=2
  var message = 'hello, '+name+'!'
  ```

* **逗号后面加空格**。

  eslint: [`comma-spacing`](http://eslint.org/docs/rules/comma-spacing)

  ```js
  // ✓ ok
  var list = [1, 2, 3, 4]
  function greet (name, options) { ... }
  ```

  ```js
  // ✗ avoid
  var list = [1,2,3,4]
  function greet (name,options) { ... }
  ```

* **else 关键字要与花括号**保持在同一行。

  eslint: [`brace-style`](http://eslint.org/docs/rules/brace-style)

  ```js
  // ✓ ok
  if (condition) {
    // ...
  } else {
    // ...
  }
  ```

  ```js
  // ✗ avoid
  if (condition)
  {
    // ...
  }
  else
  {
    // ...
  }
  ```

* **多行 if 语句的**的括号不能省。

  eslint: [`curly`](http://eslint.org/docs/rules/curly)

  ```js
  // ✓ ok
  if (options.quiet !== true) console.log('done')
  ```

  ```js
  // ✓ ok
  if (options.quiet !== true) {
    console.log('done')
  }
  ```

  ```js
  // ✗ avoid
  if (options.quiet !== true)
    console.log('done')
  ```

* **不要丢掉**异常处理中`err`参数。

  eslint: [`handle-callback-err`](http://eslint.org/docs/rules/handle-callback-err)
  ```js
  // ✓ ok
  run(function (err) {
    if (err) throw err
    window.alert('done')
  })
  ```

  ```js
  // ✗ avoid
  run(function (err) {
    window.alert('done')
  })
  ```

* **使用浏览器全局变量时加上** `window.` 前缀。<br>
  Exceptions are: `document`, `console` and `navigator`.

  eslint: [`no-undef`](http://eslint.org/docs/rules/no-undef)

  ```js
  window.alert('hi')   // ✓ ok
  ```

* **不允许有连续多行空行**。

  eslint: [`no-multiple-empty-lines`](http://eslint.org/docs/rules/no-multiple-empty-lines)

  ```js
  // ✓ ok
  var value = 'hello world'
  console.log(value)
  ```

  ```js
  // ✗ avoid
  var value = 'hello world'


  console.log(value)
  ```

* **对于三元运算符** `?` 和 `:` 与他们所负责的代码处于同一行

  eslint: [`operator-linebreak`](http://eslint.org/docs/rules/operator-linebreak)

  ```js
  // ✓ ok
  var location = env.development ? 'localhost' : 'www.api.com'

  // ✓ ok
  var location = env.development
    ? 'localhost'
    : 'www.api.com'

  // ✗ avoid
  var location = env.development ?
    'localhost' :
    'www.api.com'
  ```

* **每个 var 关键字**单独声明一个变量。

  eslint: [`one-var`](http://eslint.org/docs/rules/one-var)

  ```js
  // ✓ ok
  var silent = true
  var verbose = true

  // ✗ avoid
  var silent = true, verbose = true

  // ✗ avoid
  var silent = true,
      verbose = true
  ```

* **条件语句中赋值语句**使用括号包起来。这样使得代码更加清晰可读，而不会认为是将条件判断语句的全等号（`===`）错写成了等号（`=`）。

  eslint: [`no-cond-assign`](http://eslint.org/docs/rules/no-cond-assign)

  ```js
  // ✓ ok
  while ((m = text.match(expr))) {
    // ...
  }

  // ✗ avoid
  while (m = text.match(expr)) {
    // ...
  }
  ```

* **单行代码块两边加空格**。

  eslint: [`block-spacing`](http://eslint.org/docs/rules/block-spacing)

  ```js
    function foo () {return true}    // ✗ avoid
    function foo () { return true }  // ✓ ok
  ```

* **对于变量和函数名统一使用驼峰命名法**。

  eslint: [`camelcase`](http://eslint.org/docs/rules/camelcase)

  ```js
    function my_function () { }    // ✗ avoid
    function myFunction () { }     // ✓ ok

    var my_var = 'hello'           // ✗ avoid
    var myVar = 'hello'            // ✓ ok
  ```

* **不允许有多余的行末逗号**。

  eslint: [`comma-dangle`](http://eslint.org/docs/rules/comma-dangle)

  ```js
    var obj = {
      message: 'hello',   // ✗ avoid
    }
  ```

* **始终将逗号置于行末**。

  eslint: [`comma-style`](http://eslint.org/docs/rules/comma-style)

  ```js
    var obj = {
      foo: 'foo'
      ,bar: 'bar'   // ✗ avoid
    }

    var obj = {
      foo: 'foo',
      bar: 'bar'   // ✓ ok
    }
  ```

* **点号操作符须与属性需在同一行**。

  eslint: [`dot-location`](http://eslint.org/docs/rules/dot-location)

  ```js
    console.
      log('hello')  // ✗ avoid

    console
      .log('hello') // ✓ ok
  ```

* **文件末尾留一空行**。

  eslint: [`eol-last`](http://eslint.org/docs/rules/eol-last)

* **函数调用时标识符与括号间不留间隔**。

  eslint: [`func-call-spacing`](http://eslint.org/docs/rules/func-call-spacing)

  ```js
  console.log ('hello') // ✗ avoid
  console.log('hello')  // ✓ ok
  ```

* **键值对当中冒号与值之间要留空白**。

  eslint: [`key-spacing`](http://eslint.org/docs/rules/key-spacing)

  ```js
  var obj = { 'key' : 'value' }    // ✗ avoid
  var obj = { 'key' :'value' }     // ✗ avoid
  var obj = { 'key':'value' }      // ✗ avoid
  var obj = { 'key': 'value' }     // ✓ ok
  ```

* **构造函数要以大写字母开头**。

  eslint: [`new-cap`](http://eslint.org/docs/rules/new-cap)

  ```js
  function animal () {}
  var dog = new animal()    // ✗ avoid

  function Animal () {}
  var dog = new Animal()    // ✓ ok
  ```

* **无参的构造函数调用时要带上括号**。

  eslint: [`new-parens`](http://eslint.org/docs/rules/new-parens)

  ```js
  function Animal () {}
  var dog = new Animal    // ✗ avoid
  var dog = new Animal()  // ✓ ok
  ```

* **对象中定义了存值器，一定要对应的定义取值器**。

  eslint: [`accessor-pairs`](http://eslint.org/docs/rules/accessor-pairs)

  ```js
  var person = {
    set name (value) {    // ✗ avoid
      this._name = value
    }
  }

  var person = {
    set name (value) {
      this._name = value
    },
    get name () {         // ✓ ok
      return this._name
    }
  }
  ```

* **子类的构造器中一定要调用 `super`**

  eslint: [`constructor-super`](http://eslint.org/docs/rules/constructor-super)

  ```js
  class Dog {
    constructor () {
      super()   // ✗ avoid
    }
  }

  class Dog extends Mammal {
    constructor () {
      super()   // ✓ ok
    }
  }
  ```

* **使用数组字面量而不是构造器**。

  eslint: [`no-array-constructor`](http://eslint.org/docs/rules/no-array-constructor)

  ```js
  var nums = new Array(1, 2, 3)   // ✗ avoid
  var nums = [1, 2, 3]            // ✓ ok
  ```

* **避免使用 `arguments.callee` 和 `arguments.caller`**。

  eslint: [`no-caller`](http://eslint.org/docs/rules/no-caller)

  ```js
  function foo (n) {
    if (n <= 0) return

    arguments.callee(n - 1)   // ✗ avoid
  }

  function foo (n) {
    if (n <= 0) return

    foo(n - 1)
  }
  ```

* **避免对类名重新赋值**。

  eslint: [`no-class-assign`](http://eslint.org/docs/rules/no-class-assign)

  ```js
  class Dog {}
  Dog = 'Fido'    // ✗ avoid
  ```

* **避免修改使用 `const` 声明的变量**。

  eslint: [`no-const-assign`](http://eslint.org/docs/rules/no-const-assign)

  ```js
  const score = 100
  score = 125       // ✗ avoid
  ```

* **避免使用常量作为条件表达式的条件（循环语句除外）**。

  eslint: [`no-constant-condition`](http://eslint.org/docs/rules/no-constant-condition)

  ```js
  if (false) {    // ✗ avoid
    // ...
  }

  if (x === 0) {  // ✓ ok
    // ...
  }

  while (true) {  // ✓ ok
    // ...
  }
  ```

* **正则中不要使用控制符**。

  eslint: [`no-control-regex`](http://eslint.org/docs/rules/no-control-regex)

  ```js
  var pattern = /\x1f/    // ✗ avoid
  var pattern = /\x20/    // ✓ ok
  ```

* **不要使用 `debugger`**。

  eslint: [`no-debugger`](http://eslint.org/docs/rules/no-debugger)

  ```js
  function sum (a, b) {
    debugger      // ✗ avoid
    return a + b
  }
  ```

* **不要对变量使用 `delete` 操作**。

  eslint: [`no-delete-var`](http://eslint.org/docs/rules/no-delete-var)

  ```js
  var name
  delete name     // ✗ avoid
  ```

* **不要定义冗余的函数参数**。

  eslint: [`no-dupe-args`](http://eslint.org/docs/rules/no-dupe-args)

  ```js
  function sum (a, b, a) {  // ✗ avoid
    // ...
  }

  function sum (a, b, c) {  // ✓ ok
    // ...
  }
  ```

* **类中不要定义冗余的属性**。

  eslint: [`no-dupe-class-members`](http://eslint.org/docs/rules/no-dupe-class-members)

  ```js
  class Dog {
    bark () {}
    bark () {}    // ✗ avoid
  }
  ```

* **对象字面量中不要定义重复的属性**。

  eslint: [`no-dupe-keys`](http://eslint.org/docs/rules/no-dupe-keys)

  ```js
  var user = {
    name: 'Jane Doe',
    name: 'John Doe'    // ✗ avoid
  }
  ```

* **`switch` 语句中不要定义重复的 `case` 分支**。

  eslint: [`no-duplicate-case`](http://eslint.org/docs/rules/no-duplicate-case)

  ```js
  switch (id) {
    case 1:
      // ...
    case 1:     // ✗ avoid
  }
  ```

* **同一模块有多个导入时一次性写完**。

  eslint: [`no-duplicate-imports`](http://eslint.org/docs/rules/no-duplicate-imports)

  ```js
  import { myFunc1 } from 'module'
  import { myFunc2 } from 'module'          // ✗ avoid

  import { myFunc1, myFunc2 } from 'module' // ✓ ok
  ```

* **正则中不要使用空字符**。

  eslint: [`no-empty-character-class`](http://eslint.org/docs/rules/no-empty-character-class)

  ```js
  const myRegex = /^abc[]/      // ✗ avoid
  const myRegex = /^abc[a-z]/   // ✓ ok
  ```

* **不要解构空值**。

  eslint: [`no-empty-pattern`](http://eslint.org/docs/rules/no-empty-pattern)

  ```js
  const { a: {} } = foo         // ✗ avoid
  const { a: { b } } = foo      // ✓ ok
  ```

* **不要使用 `eval()`**。

  eslint: [`no-eval`](http://eslint.org/docs/rules/no-eval)

  ```js
  eval( "var result = user." + propName ) // ✗ avoid
  var result = user[propName]             // ✓ ok
  ```

* **`catch` 中不要对错误重新赋值**。

  eslint: [`no-ex-assign`](http://eslint.org/docs/rules/no-ex-assign)

  ```js
  try {
    // ...
  } catch (e) {
    e = 'new value'             // ✗ avoid
  }

  try {
    // ...
  } catch (e) {
    const newVal = 'new value'  // ✓ ok
  }
  ```

* **不要扩展原生对象**。

  eslint: [`no-extend-native`](http://eslint.org/docs/rules/no-extend-native)

  ```js
  Object.prototype.age = 21     // ✗ avoid
  ```

* **避免多余的函数上下文绑定**。

  eslint: [`no-extra-bind`](http://eslint.org/docs/rules/no-extra-bind)

  ```js
  const name = function () {
    getName()
  }.bind(user)    // ✗ avoid

  const name = function () {
    this.getName()
  }.bind(user)    // ✓ ok
  ```

* **避免不必要的布尔转换**。

  eslint: [`no-extra-boolean-cast`](http://eslint.org/docs/rules/no-extra-boolean-cast)

  ```js
  const result = true
  if (!!result) {   // ✗ avoid
    // ...
  }

  const result = true
  if (result) {     // ✓ ok
    // ...
  }
  ```

* **不要使用多余的括号包裹函数**。

  eslint: [`no-extra-parens`](http://eslint.org/docs/rules/no-extra-parens)

  ```js
  const myFunc = (function () { })   // ✗ avoid
  const myFunc = function () { }     // ✓ ok
  ```

* **`switch` 一定要使用 `break` 来将条件分支正常中断**。

  eslint: [`no-fallthrough`](http://eslint.org/docs/rules/no-fallthrough)

  ```js
  switch (filter) {
    case 1:
      doSomething()    // ✗ avoid
    case 2:
      doSomethingElse()
  }

  switch (filter) {
    case 1:
      doSomething()
      break           // ✓ ok
    case 2:
      doSomethingElse()
  }

  switch (filter) {
    case 1:
      doSomething()
      // fallthrough  // ✓ ok
    case 2:
      doSomethingElse()
  }
  ```

* **不要省去小数点前面的0**。

  eslint: [`no-floating-decimal`](http://eslint.org/docs/rules/no-floating-decimal)

  ```js
  const discount = .5      // ✗ avoid
  const discount = 0.5     // ✓ ok
  ```

* **避免对声明过的函数重新赋值**。

  eslint: [`no-func-assign`](http://eslint.org/docs/rules/no-func-assign)

  ```js
  function myFunc () { }
  myFunc = myOtherFunc    // ✗ avoid
  ```

* **不要对全局只读对象重新赋值**。

  eslint: [`no-global-assign`](http://eslint.org/docs/rules/no-global-assign)

  ```js
  window = {}     // ✗ avoid
  ```

* **注意隐式的 `eval()`**。

  eslint: [`no-implied-eval`](http://eslint.org/docs/rules/no-implied-eval)

  ```js
  setTimeout("alert('Hello world')")                   // ✗ avoid
  setTimeout(function () { alert('Hello world') })     // ✓ ok
  ```

* **嵌套的代码块中禁止再定义函数**。

  eslint: [`no-inner-declarations`](http://eslint.org/docs/rules/no-inner-declarations)

  ```js
  if (authenticated) {
    function setAuthUser () {}    // ✗ avoid
  }
  ```

* **不要向 `RegExp` 构造器传入非法的正则表达式**。

  eslint: [`no-invalid-regexp`](http://eslint.org/docs/rules/no-invalid-regexp)

  ```js
  RegExp('[a-z')    // ✗ avoid
  RegExp('[a-z]')   // ✓ ok
  ```

* **不要使用非法的空白符**。

  eslint: [`no-irregular-whitespace`](http://eslint.org/docs/rules/no-irregular-whitespace)

  ```js
  function myFunc () /*<NBSP>*/{}   // ✗ avoid
  ```

* **禁止使用 `__iterator__`**。

  eslint: [`no-iterator`](http://eslint.org/docs/rules/no-iterator)

  ```js
  Foo.prototype.__iterator__ = function () {}   // ✗ avoid
  ```

* **外部变量不要与对象属性重名**。

  eslint: [`no-label-var`](http://eslint.org/docs/rules/no-label-var)

  ```js
  var score = 100
  function game () {
    score: while (true) {      // ✗ avoid
      score -= 10
      if (score > 0) continue score
      break
    }
  }
  ```

* **不要使用标签语句**。

  eslint: [`no-labels`](http://eslint.org/docs/rules/no-labels)

  ```js
  label:
    while (true) {
      break label     // ✗ avoid
    }
  ```

* **不要书写不必要的嵌套代码块**。

  eslint: [`no-lone-blocks`](http://eslint.org/docs/rules/no-lone-blocks)

  ```js
  function myFunc () {
    {                   // ✗ avoid
      myOtherFunc()
    }
  }

  function myFunc () {
    myOtherFunc()       // ✓ ok
  }
  ```

* **不要混合使用空格与制表符作为缩进**。

  eslint: [`no-mixed-spaces-and-tabs`](http://eslint.org/docs/rules/no-mixed-spaces-and-tabs)

* **除了缩进，不要使用多个空格**。

  eslint: [`no-multi-spaces`](http://eslint.org/docs/rules/no-multi-spaces)

  ```js
  const id =    1234    // ✗ avoid
  const id = 1234       // ✓ ok
  ```

* **不要使用多行字符串**。

  eslint: [`no-multi-str`](http://eslint.org/docs/rules/no-multi-str)

  ```js
  const message = 'Hello \
                   world'     // ✗ avoid
  ```

* **`new` 创建对象实例后需要赋值给变量**。

  eslint: [`no-new`](http://eslint.org/docs/rules/no-new)

  ```js
  new Character()                     // ✗ avoid
  const character = new Character()   // ✓ ok
  ```

* **禁止使用 `Function` 构造器**。

  eslint: [`no-new-func`](http://eslint.org/docs/rules/no-new-func)

  ```js
  var sum = new Function('a', 'b', 'return a + b')    // ✗ avoid
  ```

* **禁止使用 `Object` 构造器**。

  eslint: [`no-new-object`](http://eslint.org/docs/rules/no-new-object)

  ```js
  let config = new Object()   // ✗ avoid
  ```

* **禁止使用 `new require`**。

  eslint: [`no-new-require`](http://eslint.org/docs/rules/no-new-require)

  ```js
  const myModule = new require('my-module')    // ✗ avoid
  ```

* **禁止使用 `Symbol` 构造器**。

  eslint: [`no-new-symbol`](http://eslint.org/docs/rules/no-new-symbol)

  ```js
  const foo = new Symbol('foo')   // ✗ avoid
  ```

* **禁止使用原始包装器**。

  eslint: [`no-new-wrappers`](http://eslint.org/docs/rules/no-new-wrappers)

  ```js
  const message = new String('hello')   // ✗ avoid
  ```

* **不要将全局对象的属性作为函数调用**。

  eslint: [`no-obj-calls`](http://eslint.org/docs/rules/no-obj-calls)

  ```js
  const math = Math()   // ✗ avoid
  ```

* **不要使用八进制字面量**。

  eslint: [`no-octal`](http://eslint.org/docs/rules/no-octal)

  ```js
  const num = 042     // ✗ avoid
  const num = '042'   // ✓ ok
  ```

* **字符串字面量中也不要使用八进制转义字符**。

  eslint: [`no-octal-escape`](http://eslint.org/docs/rules/no-octal-escape)

  ```js
  const copyright = 'Copyright \251'  // ✗ avoid
  ```

* **使用 `__dirname` 和 `__filename` 时尽量避免使用字符串拼接**。

  eslint: [`no-path-concat`](http://eslint.org/docs/rules/no-path-concat)

  ```js
  const pathToFile = __dirname + '/app.js'            // ✗ avoid
  const pathToFile = path.join(__dirname, 'app.js')   // ✓ ok
  ```

* 使用 `getPrototypeOf` 来替代 **`__proto__`**。

  eslint: [`no-proto`](http://eslint.org/docs/rules/no-proto)

  ```js
  const foo = obj.__proto__               // ✗ avoid
  const foo = Object.getPrototypeOf(obj)  // ✓ ok
  ```

* **不要重复声明变量**。

  eslint: [`no-redeclare`](http://eslint.org/docs/rules/no-redeclare)

  ```js
  let name = 'John'
  let name = 'Jane'     // ✗ avoid

  let name = 'John'
  name = 'Jane'         // ✓ ok
  ```

* **正则中避免使用多个空格**。

  eslint: [`no-regex-spaces`](http://eslint.org/docs/rules/no-regex-spaces)

  ```js
  const regexp = /test   value/   // ✗ avoid

  const regexp = /test {3}value/  // ✓ ok
  const regexp = /test value/     // ✓ ok
  ```

* **return 语句中的赋值必需有括号包裹**。

  eslint: [`no-return-assign`](http://eslint.org/docs/rules/no-return-assign)

  ```js
  function sum (a, b) {
    return result = a + b     // ✗ avoid
  }

  function sum (a, b) {
    return (result = a + b)   // ✓ ok
  }
  ```

* **避免将变量赋值给自己**。

  eslint: [`no-self-assign`](http://eslint.org/docs/rules/no-self-assign)

  ```js
  name = name   // ✗ avoid
  ```

* **避免将变量与自己进行比较操作**。

  esint: [`no-self-compare`](http://eslint.org/docs/rules/no-self-compare)

  ```js
  if (score === score) {}   // ✗ avoid
  ```

* **避免使用逗号操作符**。

  eslint: [`no-sequences`](http://eslint.org/docs/rules/no-sequences)

  ```js
  if (doSomething(), !!test) {}   // ✗ avoid
  ```

* **不要随意更改关键字的值**。

  eslint: [`no-shadow-restricted-names`](http://eslint.org/docs/rules/no-shadow-restricted-names)

  ```js
  let undefined = 'value'     // ✗ avoid
  ```

* **禁止使用稀疏数组（Sparse arrays）**。

  eslint: [`no-sparse-arrays`](http://eslint.org/docs/rules/no-sparse-arrays)

  ```js
  let fruits = ['apple',, 'orange']       // ✗ avoid
  ```

* **不要使用制表符**。

  eslint: [`no-tabs`](http://eslint.org/docs/rules/no-tabs)

* **正确使用 ES6 中的字符串模板**。

  eslint: [`no-template-curly-in-string`](http://eslint.org/docs/rules/no-template-curly-in-string)

  ```js
  const message = 'Hello ${name}'   // ✗ avoid
  const message = `Hello ${name}`   // ✓ ok
  ```

* **使用 `this` 前请确保 `super()` 已调用**。

  eslint: [`no-this-before-super`](http://eslint.org/docs/rules/no-this-before-super)

  ```js
  class Dog extends Animal {
    constructor () {
      this.legs = 4     // ✗ avoid
      super()
    }
  }
  ```

* **用 `throw` 抛错时，抛出 `Error` 对象而不是字符串**。

  eslint: [`no-throw-literal`](http://eslint.org/docs/rules/no-throw-literal)

  ```js
  throw 'error'               // ✗ avoid
  throw new Error('error')    // ✓ ok
  ```

* **行末不留空格**。

  eslint: [`no-trailing-spaces`](http://eslint.org/docs/rules/no-trailing-spaces)

* **不要使用 `undefined` 来初始化变量**。

  eslint: [`no-undef-init`](http://eslint.org/docs/rules/no-undef-init)

  ```js
  let name = undefined    // ✗ avoid

  let name
  name = 'value'          // ✓ ok
  ```

* **循环语句中注意更新循环变量**。

  eslint: [`no-unmodified-loop-condition`](http://eslint.org/docs/rules/no-unmodified-loop-condition)

  ```js
  for (let i = 0; i < items.length; j++) {...}    // ✗ avoid
  for (let i = 0; i < items.length; i++) {...}    // ✓ ok
  ```

* **如果有更好的实现，尽量不要使用三元表达式**。

  eslint: [`no-unneeded-ternary`](http://eslint.org/docs/rules/no-unneeded-ternary)

  ```js
  let score = val ? val : 0     // ✗ avoid
  let score = val || 0          // ✓ ok
  ```

* **`return`，`throw`，`continue` 和 `break` 后不要再跟代码**。

  eslint: [`no-unreachable`](http://eslint.org/docs/rules/no-unreachable)

  ```js
  function doSomething () {
    return true
    console.log('never called')     // ✗ avoid
  }
  ```

* **`finally` 代码块中不要再改变程序执行流程**。

  eslint: [`no-unsafe-finally`](http://eslint.org/docs/rules/no-unsafe-finally)

  ```js
  try {
    // ...
  } catch (e) {
    // ...
  } finally {
    return 42     // ✗ avoid
  }
  ```

* **关系运算符的左值不要做取反操作**。

  eslint: [`no-unsafe-negation`](http://eslint.org/docs/rules/no-unsafe-negation)

  ```js
  if (!key in obj) {}       // ✗ avoid
  ```

* **避免不必要的 `.call()` 和 `.apply()`**。

  eslint: [`no-useless-call`](http://eslint.org/docs/rules/no-useless-call)

  ```js
  sum.call(null, 1, 2, 3)   // ✗ avoid
  ```

* **避免使用不必要的计算值作对象属性**。

  eslint: [`no-useless-computed-key`](http://eslint.org/docs/rules/no-useless-computed-key)

  ```js
  const user = { ['name']: 'John Doe' }   // ✗ avoid
  const user = { name: 'John Doe' }       // ✓ ok
  ```

* **禁止多余的构造器**。

  eslint: [`no-useless-constructor`](http://eslint.org/docs/rules/no-useless-constructor)

  ```js
  class Car {
    constructor () {      // ✗ avoid
    }
  }
  ```

* **禁止不必要的转义**。

  eslint: [`no-useless-escape`](http://eslint.org/docs/rules/no-useless-escape)

  ```js
  let message = 'Hell\o'  // ✗ avoid
  ```

* **import, export 和解构操作中，禁止赋值到同名变量**。

  eslint: [`no-useless-rename`](http://eslint.org/docs/rules/no-useless-rename)

  ```js
  import { config as config } from './config'     // ✗ avoid
  import { config } from './config'               // ✓ ok
  ```

* **属性前面不要加空格**。

  eslint: [`no-whitespace-before-property`](http://eslint.org/docs/rules/no-whitespace-before-property)

  ```js
  user .name      // ✗ avoid
  user.name       // ✓ ok
  ```

* **禁止使用 `with`**。

  eslint: [`no-with`](http://eslint.org/docs/rules/no-with)

  ```js
  with (val) {...}    // ✗ avoid
  ```

* **对象属性换行时注意统一代码风格**。

  eslint: [`object-property-newline`](http://eslint.org/docs/rules/object-property-newline)

  ```js
  const user = {
    name: 'Jane Doe', age: 30,
    username: 'jdoe86'            // ✗ avoid
  }

  const user = { name: 'Jane Doe', age: 30, username: 'jdoe86' }    // ✓ ok

  const user = {
    name: 'Jane Doe',
    age: 30,
    username: 'jdoe86'
  }                                                                 // ✓ ok
  ```

* **代码块中避免多余留白**。

  eslint: [`padded-blocks`](http://eslint.org/docs/rules/padded-blocks)

  ```js
  if (user) {
                              // ✗ avoid
    const name = getName()

  }

  if (user) {
    const name = getName()    // ✓ ok
  }
  ```

* **展开运算符与它的表达式间不要留空白**。

  eslint: [`rest-spread-spacing`](http://eslint.org/docs/rules/rest-spread-spacing)

  ```js
  fn(... args)    // ✗ avoid
  fn(...args)     // ✓ ok
  ```

* **遇到分号时空格要后留前不留**。

  eslint: [`semi-spacing`](http://eslint.org/docs/rules/semi-spacing)

  ```js
  for (let i = 0 ;i < items.length ;i++) {...}    // ✗ avoid
  for (let i = 0; i < items.length; i++) {...}    // ✓ ok
  ```

* **代码块首尾留空格**。

  eslint: [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks)

  ```js
  if (admin){...}     // ✗ avoid
  if (admin) {...}    // ✓ ok
  ```

* **圆括号间不留空格**。

  eslint: [`space-in-parens`](http://eslint.org/docs/rules/space-in-parens)

  ```js
  getName( name )     // ✗ avoid
  getName(name)       // ✓ ok
  ```

* **一元运算符后面跟一个空格**。

  eslint: [`space-unary-ops`](http://eslint.org/docs/rules/space-unary-ops)

  ```js
  typeof!admin        // ✗ avoid
  typeof !admin        // ✓ ok
  ```

* **注释首尾留空格**。

  eslint: [`spaced-comment`](http://eslint.org/docs/rules/spaced-comment)

  ```js
  //comment           // ✗ avoid
  // comment          // ✓ ok

  /*comment*/         // ✗ avoid
  /* comment */       // ✓ ok
  ```

* **模板字符串中变量前后不加空格**。

  eslint: [`template-curly-spacing`](http://eslint.org/docs/rules/template-curly-spacing)

  ```js
  const message = `Hello, ${ name }`    // ✗ avoid
  const message = `Hello, ${name}`      // ✓ ok
  ```

* **检查 `NaN` 的正确姿势是使用 `isNaN()`**。

  eslint: [`use-isnan`](http://eslint.org/docs/rules/use-isnan)

  ```js
  if (price === NaN) { }      // ✗ avoid
  if (isNaN(price)) { }       // ✓ ok
  ```

* **用合法的字符串跟 `typeof` 进行比较操作**。

  eslint: [`valid-typeof`](http://eslint.org/docs/rules/valid-typeof)

  ```js
  typeof name === 'undefimed'     // ✗ avoid
  typeof name === 'undefined'     // ✓ ok
  ```

* **自调用匿名函数 (IIFEs) 使用括号包裹**。

  eslint: [`wrap-iife`](http://eslint.org/docs/rules/wrap-iife)

  ```js
  const getName = function () { }()     // ✗ avoid

  const getName = (function () { }())   // ✓ ok
  const getName = (function () { })()   // ✓ ok
  ```

* **`yield *` 中的 `*` 前后都要有空格**。

  eslint: [`yield-star-spacing`](http://eslint.org/docs/rules/yield-star-spacing)

  ```js
  yield* increment()    // ✗ avoid
  yield * increment()   // ✓ ok
  ```

* **请书写优雅的条件语句（avoid Yoda conditions）**。

  eslint: [`yoda`](http://eslint.org/docs/rules/yoda)

  ```js
  if (42 === age) { }    // ✗ avoid
  if (age === 42) { }    // ✓ ok
  ```

## 关于分号

* 不要使用分号。 (参见：[1](http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding)，[2](http://inimino.org/%7Einimino/blog/javascript_semicolons)，[3](https://www.youtube.com/watch?v=gsfbh17Ax9I))

  eslint: [`semi`](http://eslint.org/docs/rules/semi)

  ```js
  window.alert('hi')   // ✓ ok
  window.alert('hi');  // ✗ avoid
  ```

* 不要使用 `(`, `[`, or `` ` `` 等作为一行的开始。在没有分号的情况下代码压缩后会导致报错，而坚持这一规范则可避免出错。

  eslint: [`no-unexpected-multiline`](http://eslint.org/docs/rules/no-unexpected-multiline)

  ```js
  // ✓ ok
  ;(function () {
    window.alert('ok')
  }())

  // ✗ avoid
  (function () {
    window.alert('ok')
  }())
  ```

  ```js
  // ✓ ok
  ;[1, 2, 3].forEach(bar)

  // ✗ avoid
  [1, 2, 3].forEach(bar)
  ```

  ```js
  // ✓ ok
  ;`hello`.indexOf('o')

  // ✗ avoid
  `hello`.indexOf('o')
  ```

  备注：上面的写法只能说聪明过头了。

  相比更加可读易懂的代码，那些看似投巧的写法是不可取的。

  譬如：

  ```js
  ;[1, 2, 3].forEach(bar)
  ```

  建议的写法是：

  ```js
  var nums = [1, 2, 3]
  nums.forEach(bar)
  ```


## 拓展阅读

- [An Open Letter to JavaScript Leaders Regarding Semicolons][1]
- [JavaScript Semicolon Insertion – Everything you need to know][2]

##### 一个值得观看的视频：

- [JavaScript 中的分号多余吗？- YouTube][3]

当前主流的代码压缩方案都是基于词法（AST-based）进行的，所以在处理无分号的代码时完全没有压力（何况 JavaScript 中分号本来就不是强制的）。

##### 一段摘抄自 *["An Open Letter to JavaScript Leaders Regarding Semicolons"][1]* 这篇文章的内容：

> [自动化插入分号的做法]是安全可依赖的，而且其产出的代码能够在所有浏览器里很好地运行。 Closure compiler, yuicompressor, packer 还有 jsmin 都能正确地对这样的代码进行压缩处理。并没有任何性能相关的问题。
>
> 不得不说，Javascript 社区里的大牛们一直是错误的，并不能教给你最佳实践。真是让人忧伤啊。 我建议先弄清楚 JS 是怎样断句的（还有就是哪些地方看起来断了其实并没有），明白了这个后就可以随心写出漂亮的代码了。
>
> 一般来说， `\n` 表示语句结束，除非：
>   1. 该语句有未闭合的括号， 数组字面量， 对象字面量 或者其他不能正常结束一条语句的情况（譬如，以 `.` 或 `,` 结尾）
>   2. 该语句是 `--` 或者 `++` （它会将后面的内容进行自增或减）
>   3. 该语句是 `for()`，`while()`，`do`，`if()` 或者 `else` 并且没有 `{`
>   4. 下一行以 `[`，`(`，`+`，`*`，`/`，`-`，`,`，`.` 或者其他只会单独出现在两块内容间的二元操作符。
>
> 第一条很容易理解。即使在 JSLint 中，也允许 JSON，构造器的括号中，以及使用 `var` 配合 `,` 结尾来声明多个变量等这些情中包含 `\n`。
>
> 第二条有点奇葩。 我还想不出谁会（除了这里用作讨论外）写出 `i\n++\nj` 这样的代码来，不过，顺便说一下，这种写法最后解析的结果是 `i; ++j`，而不是 `i++; j`。
>
> 第三条也容易理解。 `if (x)\ny()` 等价于 `if (x) { y() }`。解释器会向下寻找到代码块或一条语句为止。
>
> `;` 是条合法的 JavaScript 语句。所以 `if(x);` 等价于 `if(x){}`，表示 “如果 x 为真，什么也不做。” 这种写法在循环里面可以看到，就是当条件判断与条件更新是同一个方法的时候。 不常见，但也不至于没听说过吧。
>
> 第四条就是常见的 “看，说过要加分号！” 的情形。但这些情况可以通过在语句前面加上分号来解决，如果你确定该语句跟前面的没关系的话。举个例子，假如你想这样：
>
> ```js
> foo();
> [1,2,3].forEach(bar);
> ```
>
> 那么完全可以这样来写：
>
> ```js
> foo()
> ;[1,2,3].forEach(bar)
> ```
>
> 后者的好处是分号比较瞩目，一旦习惯后便再也不会看到以 `(` 和 `[` 开头又不带分号的语句了。

[1]: http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding
[2]: http://inimino.org/~inimino/blog/javascript_semicolons
[3]: https://www.youtube.com/watch?v=gsfbh17Ax9I
