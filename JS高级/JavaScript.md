# JavaScriptProject

[TOC]

## 1. JavaScript 面向对象编程

### 1.1 面向对象介绍

#### 1.1.1 什么是对象

> Everything is object （万物皆对象）

对象到底是什么，我们可以从两次层次来理解。

**(1) 对象是单个事物的抽象。**

一本书、一辆汽车、一个人都可以是对象，一个数据库、一张网页、一个与远程服务器的连接也可以是对象。当实物被抽象成对象，实物之间的关系就变成了对象之间的关系，从而就可以模拟现实情况，针对对象进行编程。

**(2) 对象是一个容器，封装了属性（property）和方法（method）。**

属性是对象的状态，方法是对象的行为（完成某种任务）。比如，我们可以把动物抽象为animal对象，使用“属性”记录具体是那一种动物，使用“方法”表示动物的某种行为（奔跑、捕猎、休息等等）。

在实际开发中，对象是一个抽象的概念，可以将其简单理解为：**数据集或功能集**。

ECMAScript-262 把对象定义为：**无序属性的集合，其属性可以包含基本值、对象或者函数**。
严格来讲，这就相当于说对象是一组没有特定顺序的值。对象的每个属性或方法都有一个名字，而每个名字都
映射到一个值。

#### 1.1.2 什么是面向对象

> 面向对象不是新的东西，它只是过程式代码的**一种高度封装，目的在于提高代码的开发效率和可维护性**。

面向对象编程 —— Object Oriented Programming，简称 OOP ，**是一种编程开发思想**。
它将真实世界各种复杂的关系，抽象为一个个对象，然后由对象之间的分工与合作，完成对真实世界的模拟。

在面向对象程序开发思想中，每一个对象都是功能中心，具有明确分工，可以完成接受信息、处理数据、发出信息等任务。
因此，**面向对象编程具有灵活、代码可复用、高度模块化等特点，容易维护和开发**，比起由一系列函数或指令组成的传统的过程式编程（procedural programming），更适合多人合作的大型软件项目。

面向对象与面向过程：

- 面向过程就是亲力亲为，事无巨细，面面俱到，步步紧跟，有条不紊
- 面向对象就是找一个对象，指挥得结果
- 面向对象将执行者转变成指挥者
- 面向对象不是面向过程的替代，而是面向过程的封装

面向对象的特性：

- **封装性**
- **继承性**
- **[多态性]** /JavaScript没有多态

扩展阅读：

- [维基百科 - 面向对象程序设计](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1)
- [知乎：如何用一句话说明什么是面向对象思想？](https://www.zhihu.com/question/19854505)
- [知乎：什么是面向对象编程思想？](https://www.zhihu.com/question/31021366)

#### 1.1.3 程序中面向对象的基本体现

在 JavaScript 中，所有数据类型都可以视为对象，当然也可以自定义对象。
自定义的对象数据类型就是面向对象中的类（ Class ）的概念。

我们以一个例子来说明面向过程和面向对象在程序流程上的不同之处。

假设我们要处理学生的成绩表，为了表示一个学生的成绩，面向过程的程序可以用一个对象表示：

```javascript
var std1 = { name: 'Michael', score: 98 }
var std2 = { name: 'Bob', score: 81 }
```

而处理学生成绩可以通过函数实现，比如打印学生的成绩：

```javascript
function printScore (student) {
  console.log('姓名：' + student.name + '  ' + '成绩：' + student.score)
}
```

如果采用面向对象的程序设计思想，我们首选思考的不是程序的执行流程，
而是 `Student` 这种数据类型应该被视为一个对象，这个对象拥有 `name` 和 `score` 这两个属性（Property）。
如果要打印一个学生的成绩，首先必须创建出这个学生对应的对象，然后，给对象发一个 `printScore` 消息，让对象自己把自己的数据打印出来。

抽象数据行为模板（Class）：

```javascript
function Student (name, score) {
  this.name = name
  this.score = score
}

Student.prototype.printScore = function () {
  console.log('姓名：' + this.name + '  ' + '成绩：' + this.score)
}
```

根据模板创建具体实例对象（Instance）：

```javascript
var std1 = new Student('Michael', 98)
var std2 = new Student('Bob', 81)
```

实例对象具有自己的具体行为（给对象发消息）：

```javascript
std1.printScore() // => 姓名：Michael  成绩：98
std2.printScore() // => 姓名：Bob  成绩 81
```

面向对象的设计思想是从自然界中来的，因为在自然界中，类（Class）和实例（Instance）的概念是很自然的。
Class 是一种抽象概念，比如我们定义的 Class——Student ，是指学生这个概念，
而实例（Instance）则是一个个具体的 Student ，比如， Michael 和 Bob 是两个具体的 Student 。

所以，面向对象的设计思想是：

- 抽象出 Class
- 根据 Class 创建 Instance
- 指挥 Instance 得结果

面向对象的抽象程度又比函数要高，因为一个 Class 既包含数据，又包含操作数据的方法。

### 1.2 创建对象方式

#### 1.2.1 简单方式

我们可以直接通过 `new Object()` 创建：

```javascript
var person = new Object()
person.name = 'Jack'
person.age = 18

person.sayName = function () {
  console.log(this.name)
}
```

每次创建通过 `new Object()` 比较麻烦，所以可以通过它的简写形式对象字面量来创建：

```javascript
var person = {
  name: 'Jack',
  age: 18,
  sayName: function () {
    console.log(this.name)
  }
}
```

对于上面的写法固然没有问题，但是假如我们要生成两个 `person` 实例对象呢？

```javascript
var person1 = {
  name: 'Jack',
  age: 18,
  sayName: function () {
    console.log(this.name)
  }
}

var person2 = {
  name: 'Mike',
  age: 16,
  sayName: function () {
    console.log(this.name)
  }
}
```

通过上面的代码我们不难看出，这样写的代码太过冗余，重复性太高。

#### 1.2.2 简单方式的改进：工厂函数

我们可以写一个函数，解决代码重复问题：

```javascript
function createPerson (name, age) {
  return {
    name: name,
    age: age,
    sayName: function () {
      console.log(this.name)
    }
  }
}
```

然后生成实例对象：

```javascript
var p1 = createPerson('Jack', 18)
var p2 = createPerson('Mike', 18)
```

这样封装确实爽多了，通过工厂模式我们解决了创建多个相似对象代码冗余的问题，
但却没有解决对象识别的问题（即怎样知道一个对象的类型）。

### 1.3 构造函数

内容引导：

- 构造函数语法
- 分析构造函数
- 构造函数和实例对象的关系
  - 实例的 constructor 属性
  - instanceof 操作符
- 普通函数调用和构造函数调用的区别
- 构造函数的返回值
- 构造函数的静态成员和实例成员
  - 函数也是对象
  - 实例成员
  - 静态成员
- 构造函数的问题

#### 1.3.1 更优雅的工厂函数：构造函数

一种更优雅的工厂函数就是下面这样，构造函数：

```javascript
function Person (name, age) {
  this.name = name
  this.age = age
  this.sayName = function () {
    console.log(this.name)
  }
}

var p1 = new Person('Jack', 18)
p1.sayName() // => Jack

var p2 = new Person('Mike', 23)
p2.sayName() // => Mike
```

#### 1.3.2 解析构造函数代码的执行(new关键字)

在上面的示例中，`Person()` 函数取代了 `createPerson()` 函数，但是实现效果是一样的。
这是为什么呢？

我们注意到，`Person()` 中的代码与 `createPerson()` 有以下几点不同之处：

- 没有显示的创建对象
- 直接将属性和方法赋给了 `this` 对象
- 没有 `return` 语句
- 函数名使用的是大写的 `Person`

而要创建 `Person` 实例，则必须使用 `new` 操作符。
以这种方式调用构造函数会经历以下 4 个步骤：

1. **创建一个新对象**
2. **将构造函数的作用域赋给新对象（因此 this 就指向了这个新对象）**
3. **执行构造函数中的代码**
4. **返回新对象**

下面是具体的伪代码：

```javascript
function Person (name, age) {
  // 当使用 new 操作符调用 Person() 的时候，实际上这里会先创建一个对象
  // var instance = {}
  // 然后让内部的 this 指向 instance 对象
  // this = instance
  // 接下来所有针对 this 的操作实际上操作的就是 instance

  this.name = name
  this.age = age
  this.sayName = function () {
    console.log(this.name)
  }

  // 在函数的结尾处会将 this 返回，也就是 instance
  // return this
}
```

#### 1.3.3 构造函数和实例对象的关系

使用构造函数的好处不仅仅在于代码的简洁性，更重要的是我们可以识别对象的具体类型了。
在每一个实例对象中同时有一个 `constructor` 属性，该属性指向创建该实例的构造函数：

```javascript
console.log(p1.constructor === Person) // => true
console.log(p2.constructor === Person) // => true
console.log(p1.constructor === p2.constructor) // => true
```

对象的 `constructor` 属性最初是用来标识对象类型的，
但是，如果要检测对象的类型，还是使用 `instanceof` 操作符更可靠一些：

```javascript
console.log(p1 instanceof Person) // => true
console.log(p2 instanceof Person) // => true
```

总结：

- 构造函数是根据具体的事物抽象出来的抽象模板
- 实例对象是根据抽象的构造函数模板得到的具体实例对象
- 每一个实例对象都具有一个 `constructor` 属性，指向创建该实例的构造函数
  - 注意： `constructor` 是实例的属性的说法不严谨，具体后面的原型会讲到
- 可以通过实例的 `constructor` 属性判断实例和构造函数之间的关系
  - 注意：这种方式不严谨，推荐使用 `instanceof` 操作符，后面学原型会解释为什么

#### 1.3.4 构造函数的问题

使用构造函数带来的最大的好处就是创建对象更方便了，但是其本身也存在一个浪费内存的问题：

```javascript
function Person (name, age) {
  this.name = name
  this.age = age
  this.type = 'human'
  this.sayHello = function () {
    console.log('hello ' + this.name)
  }
}

var p1 = new Person('lpz', 18)
var p2 = new Person('Jack', 16)
```

在该示例中，从表面上好像没什么问题，但是实际上这样做，有一个很大的弊端。
那就是对于每一个实例对象，`type` 和 `sayHello` 都是一模一样的内容，
每一次生成一个实例，都必须为重复的内容，多占用一些内存，如果实例对象很多，会造成极大的内存浪费。

```javascript
console.log(p1.sayHello === p2.sayHello) // => false
```

对于这种问题我们可以把需要共享的函数定义到构造函数外部：

```javascript
function sayHello = function () {
  console.log('hello ' + this.name)
}

function Person (name, age) {
  this.name = name
  this.age = age
  this.type = 'human'
  this.sayHello = sayHello
}

var p1 = new Person('lpz', 18)
var p2 = new Person('Jack', 16)

console.log(p1.sayHello === p2.sayHello) // => true
```

这样确实可以了，但是如果有多个需要共享的函数的话就会造成全局命名空间冲突的问题。

你肯定想到了可以把多个函数放到一个对象中用来避免全局命名空间冲突的问题：

```javascript
var fns = {
  sayHello: function () {
    console.log('hello ' + this.name)
  },
  sayAge: function () {
    console.log(this.age)
  }
}

function Person (name, age) {
  this.name = name
  this.age = age
  this.type = 'human'
  this.sayHello = fns.sayHello
  this.sayAge = fns.sayAge
}

var p1 = new Person('lpz', 18)
var p2 = new Person('Jack', 16)

console.log(p1.sayHello === p2.sayHello) // => true
console.log(p1.sayAge === p2.sayAge) // => true
```

至此，我们利用自己的方式基本上解决了构造函数的内存浪费问题。
但是代码看起来还是那么的格格不入，那有没有更好的方式呢？

#### 1.3.5 总结

- 构造函数语法
- 分析构造函数
- 构造函数和实例对象的关系
  - 实例的 constructor 属性
  - instanceof 操作符
- 构造函数的问题

## 2. 原型

### 2.1 什么是原型

1. 原型:不管是那个构造函数被创建,系统都会自动的帮我们生成一个和这个构造函数对应的对象,这个对象就是原型.
2. 如何访问原型: 构造函数名.prototype.
3. 原型是一个对象,就可以往原型中添加属性和方法: 构造函数实例化的对象们共有的数据.
4. 原型中的成员(属性和方法)谁可以访问: 原型对应的构造函数实例化的对象们

### 2.2 原型需要注意的地方

1. 构造函数实例化的对象们共有的数据.才可以存进原型中.
2. 构造函数名.prototype. 的方式来访问原型(给原型添加成员).
3. 构造函数实例化的对象们 访问属性和方法的规则: 先看自己的,自己有就用自己的,自己没有就看原型有没有......
4. 原型可以修改: 创建对象的代码是在修改原型之前还是修改原型之后.

### 2.3 原型基本使用

```javascript
function People(name, age) {
  this.name = name;
  this.age = age;
}
People.prototype.sayHi = function () {
  console.log('我的名字是:' + this.name + '---年龄:' + this.age)
}
let p1 = new People('mickey',18)
p1.sayHi()  // 我的名字是:mickey---年龄:18
```

### 2.4 `__proto__`属性 

`__proto__`这个属性是属于对象的, 这个属性指向实例化他的构造函数对应的原型. 

可以用代码验证一下  

```javascript
function People(name, age) {
  this.name = name;
  this.age = age;
}
People.prototype.sayHi = function () {
  console.log('我的名字是:' + this.name + '---年龄:' + this.age)
}
let p1 = new People('mickey',18)
p1.sayHi()  // 我的名字是:mickey---年龄:18
console.log(p1.__proto__ === People.prototype)  // true
```

注意: 

`__proto__`不是标准的w3c的属性 

实际开发不推荐使用`__proto__`访问原型 

虽然在实际开发生产环境中不推荐用,但是平时学习,平时开发测试可以用. 

### 2.5 `constructor`属性

属于原型, 指向他对应的那个构造函数. 

```javascript
function People(name, age) {
  this.name = name;
  this.age = age;
}
People.prototype.sayHi = function () {
  console.log('我的名字是:' + this.name + '---年龄:' + this.age)
}
let p1 = new People('mickey',18)
p1.sayHi()  // 我的名字是:mickey---年龄:18
console.log(p1.__proto__ === People.prototype)  // true   __proto__
console.log(People.prototype.constructor === People); //true  constructor
```

原型可以修改.修改了原型后原型中的constructor指向就丢失了. 

但是可以找回:

```javascript
People.prototype = {
constructor:People
}
console.log(People.prototype.constructor === People);  // true
```

### 2.6 原型对象使用建议

- 私有成员（一般就是非函数成员）放到构造函数中
- 共享成员（一般就是函数）放到原型对象中
- 如果重置了 `prototype` 记得修正 `constructor` 的指向

## 3. 继承的方式

### 3.1 构造函数的属性继承：借用构造函数

```javascript
<script>
  function People(name, age) {
    this.name = name;
    this.age = age;
    this.type = '男'
  }
  People.prototype.sayHi = function () {
    console.log('名字:' + this.name + '---年龄:' + this.age + '---性别:' + this.type)
  }
  function Student(name, age) {
    // call的作用是将this指向People函数   借用构造函数继承属性
    People.call(this, name, age)
  }
  let s1 = new Student('mickey', 18) // mickey 18 男
  console.log(s1.name, s1.age, s1.type) // 名字:mickey---年龄:18---性别:男
  s1.sayHi()
</script>
```

### 3.2 构造函数的原型方法继承：拷贝继承（for-in）

```javascript
<script>
  function People(name, age) {
    this.name = name;
    this.age = age;
    this.type = '男'
  }
  People.prototype.sayHi = function () {
    console.log('名字:' + this.name + '---年龄:' + this.age + '---性别:' + this.type)
  }
  function Student(name, age) {
    // call的作用是将this指向People函数   借用构造函数继承属性
    People.call(this, name, age)
  }
  //  原型对象拷贝继承原型对象成员
  for (const key in People.prototype) {
    Student.prototype[key] = People.prototype[key]
  }
  let s1 = new Student('mickey', 18) // mickey 18 男
  console.log(s1.name, s1.age, s1.type) // 名字:mickey---年龄:18---性别:男
  s1.sayHi()
</script>
```

### 3.3 原型继承

```javascript
<script>
  function People(name, age) {
    this.name = name;
    this.age = age;
    this.type = '男'
  }
  People.prototype.sayHi = function () {
    console.log('名字:' + this.name + '---年龄:' + this.age + '---性别:' + this.type)
  }

  function Student(name, age) {
    // call的作用是将this指向People函数   借用构造函数继承属性
    People.call(this, name, age)
  }
  // 利用原型的特性实现继承 这样可以继承到People的方法
  Student.prototype = new People()
  let s1 = new Student('mickey', 18)  // mickey 18 男
  console.log(s1.name, s1.age, s1.type)  // 名字:mickey---年龄:18---性别:男
  s1.sayHi()
</script>
```

### 3.4 静态成员和实例成员

由实例化对象点出来的 就是实例成员. 

由构造函数直接点出来的,就是静态成员. 

构造函数.prototype              静态成员 

实例化对象.`__proto__`         实例成员 

```javascript
<script>
  //1.静态成员和实例成员
  function Person(name){
    this.name = name;
    this.sayHi = function () {
      console.log("你好,哈哈哈");
    }
  }

  //这句话是在给Person对象添加一个sayHi方法.
  Person.sayHi = function () {
    console.log("呵呵呵....");
  }

  //实例化对象
  var p1 = new Person('Mickey');
  p1.sayHi();//像这样,由实例化对象点出来的 就是实例成员.
  Person.sayHi();//像这样,由构造函数直接点出来的,就是静态成员.

  //-------------------------------
  //构造函数.prototype            静态成员
  //实例化对象.__proto__          实例成员
  //arr1.push();      实例成员
  //Math.random();    静态成员
</script>
```

## 4. 闭包

### 4.1 什么是闭包

闭包就是能够读取其他函数内部变量的函数，
由于在 Javascript 语言中，只有函数内部的子函数才能读取局部变量，
因此可以把闭包简单理解成 “定义在一个函数内部的函数”。
所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

闭包的用途：

- 可以在函数外部读取函数内部成员
- 让函数内成员始终存活在内存中

闭包的作用:

- 提升变量的生命周期. 

  ```javascript
  function outer(){
      var num2 = 20;
      function inner(){
          console.log(num2);
      }
      return inner;
  }
  var fn = outer(); //执行完了outer()函数,num2并不会被回收,因为还有一个inner函数在引用他.
  fn();  // 20
  ```

- 提供有限的访问权限. 

### 4.2 沙箱(闭包应用)

避免全局变量污染, 模块化开发. 

```javascript
(function (window) {
  //食物
  var list = [];
  function sayHi() {
      console.log('大家好~我在沙箱里面')
  }
  window.sayHi = sayHi;
}(window));
sayHi();  // 大家好~我在沙箱里面
```

## 5. call、apply、bind

### 5.1 call

`call()` 方法调用一个函数, 其具有一个指定的 `this` 值和分别地提供的参数(参数的列表)。

  注意：该方法的作用和 `apply()` 方法类似，只有一个区别，就是 `call()` 方法接受的是若干个参数的列表，而 `apply()` 方法接受的是一个包含多个参数的数组。

语法：

```javascript
fun.call(thisArg[, arg1[, arg2[, ...]]])
```

参数：

- `thisArg`
  - 在 fun 函数运行时指定的 this 值
  - 如果指定了 null 或者 undefined 则内部 this 指向 window
- `arg1, arg2, ...`
  - 指定的参数列表

### 5.2 apply

`apply()` 方法调用一个函数, 其具有一个指定的 `this` 值，以及作为一个数组（或类似数组的对象）提供的参数。

<p class="danger">
  注意：该方法的作用和 `call()` 方法类似，只有一个区别，就是 `call()` 方法接受的是若干个参数的列表，而 `apply()` 方法接受的是一个包含多个参数的数组。
</p>

语法：

```javascript
fun.apply(thisArg, [argsArray])
```

参数：

- `thisArg`
- `argsArray`

`apply()` 与 `call()` 非常相似，不同之处在于提供参数的方式。
`apply()` 使用参数数组而不是一组参数列表。例如：

```javascript
fun.apply(this, ['eat', 'bananas'])
```

### 5.3 bind

bind() 函数会创建一个新函数（称为绑定函数），新函数与被调函数（绑定函数的目标函数）具有相同的函数体（在 ECMAScript 5 规范中内置的call属性）。
当目标函数被调用时 this 值绑定到 bind() 的第一个参数，该参数不能被重写。绑定函数被调用时，bind() 也接受预设的参数提供给原函数。
一个绑定函数也能使用new操作符创建对象：这种行为就像把原函数当成构造器。提供的 this 值被忽略，同时调用时的参数被提供给模拟函数。

语法：

```javascript
fun.bind(thisArg[, arg1[, arg2[, ...]]])
```

参数：

- thisArg
  - 当绑定函数被调用时，该参数会作为原函数运行时的 this 指向。当使用new 操作符调用绑定函数时，该参数无效。
- arg1, arg2, ...
  - 当绑定函数被调用时，这些参数将置于实参之前传递给被绑定的方法。

返回值：

返回由指定的this值和初始化参数改造的原函数拷贝。

示例1：

```javascript
this.x = 9; 
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 返回 81

var retrieveX = module.getX;
retrieveX(); // 返回 9, 在这种情况下，"this"指向全局作用域

// 创建一个新函数，将"this"绑定到module对象
// 新手可能会被全局的x变量和module里的属性x所迷惑
var boundGetX = retrieveX.bind(module);
boundGetX(); // 返回 81
```

示例2：

```javascript
function LateBloomer() {
  this.petalCount = Math.ceil(Math.random() * 12) + 1;
}

// Declare bloom after a delay of 1 second
LateBloomer.prototype.bloom = function() {
  window.setTimeout(this.declare.bind(this), 1000);
};

LateBloomer.prototype.declare = function() {
  console.log('I am a beautiful flower with ' +
    this.petalCount + ' petals!');
};

var flower = new LateBloomer();
flower.bloom();  // 一秒钟后, 调用'declare'方法
```

#### 小结

- call 和 apply 特性一样
  - 都是用来调用函数，而且是立即调用
  - 但是可以在调用函数的同时，通过第一个参数指定函数内部 `this` 的指向
  - call 调用的时候，参数必须以参数列表的形式进行传递，也就是以逗号分隔的方式依次传递即可
  - apply 调用的时候，参数必须是一个数组，然后在执行的时候，会将数组内部的元素一个一个拿出来，与形参一一对应进行传递
  - 如果第一个参数指定了 `null` 或者 `undefined` 则内部 this 指向 window
- bind
  - 可以用来指定内部 this 的指向，然后生成一个改变了 this 指向的新的函数
  - 它和 call、apply 最大的区别是：bind 不会调用
  - bind 支持传递参数，它的传参方式比较特殊，一共有两个位置可以传递
    - 1. 在 bind 的同时，以参数列表的形式进行传递
    - 1. 在调用的时候，以参数列表的形式进行传递
    - 那到底以谁 bind 的时候传递的参数为准呢还是以调用的时候传递的参数为准
    - 两者合并：bind 的时候传递的参数和调用的时候传递的参数会合并到一起，传递到函数内部





