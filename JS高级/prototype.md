# 原型
[TOC]
### 什么是原型
1. 原型:不管是那个构造函数被创建,系统都会自动的帮我们生成一个和这个构造函数对应的对象,这个对象就是原型.
2. 如何访问原型: 构造函数名.prototype.
3. 原型是一个对象,就可以往原型中添加属性和方法: 构造函数实例化的对象们共有的数据.
4. 原型中的成员(属性和方法)谁可以访问: 原型对应的构造函数实例化的对象们

### 原型需要注意的地方
1. 构造函数实例化的对象们共有的数据.才可以存进原型中.
1. 构造函数名.prototype. 的方式来访问原型(给原型添加成员).
1. 构造函数实例化的对象们 访问属性和方法的规则: 先看自己的,自己有就用自己的,自己没有就看原型有没有......
1. 原型可以修改: 创建对象的代码是在修改原型之前还是修改原型之后.

### 原型基本使用

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

### `__proto__`属性 

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

### `constructor`属性

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

### 原型对象使用建议

- 私有成员（一般就是非函数成员）放到构造函数中
- 共享成员（一般就是函数）放到原型对象中
- 如果重置了 `prototype` 记得修正 `constructor` 的指向

## 继承的方式

### 构造函数的属性继承：借用构造函数

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

### 构造函数的原型方法继承：拷贝继承（for-in）

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

### 原型继承

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

### 静态成员和实例成员 

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



