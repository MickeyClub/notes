# JavaScript高级01(数据类型&面试题)
[TOC]

### 1. 数据类型
JavaScript 有 5 种简单数据类型：`Undefined、Null、Boolean、Number、String` 和 1 种复杂数据类型 `Object `
- 基本数据类型
	- `string number boolean undefined null`
- 复杂数据类型
	- `Array Object Function Math Date - RegExp(正则表达式) String Number Boolean`

### 2. 数据类型检测

##### 2.1 typeof
- typeof能够检测那些数据类型?
	- 除了null之外的所有的基本数据类型,还可以检测function,
	- 因为null检测出来的是object
- 语法:
	- `typeof 需要检测的内容`
```javascript
var num = 123;
console.log(typeof  num);	//"nubner"
console.log(typeof (typeof  '123'));	//"string"
console.log(typeof (10 + 20));	//"nubner"
function test1(){
  console.log("哈哈,我是test1");
}
console.log(typeof test1);//"function"
var test2 = null;
console.log(typeof test2);	//"object"
```

##### 2.2 检测数组
1.  isArray
```javascript
var arr = [10,20,30];
console.log(typeof arr);//object
console.log(Array.isArray(arr));//true
```
2. instanceof
不严谨
```javascript
var arr = [10,20,30];
console.log(arr instanceof  Array);//true
console.log(arr instanceof  Object);//true
```
检测对象也是true,因为数组也是对象的一种


##### 2.3 NaN
not a number 表示不是一个数字 ,属于number数据类型的.
```javascript
'abc'
isNaN(需要判断的内容) ; 检测一个值是否是NaN
如果你是一个数字,就不是一个NaN,那么isNaN()就是false
console.log(isNaN(10));//false
//如果你不是一个数字,就是一个NaN,那么isNaN()就是true
console.log(isNaN('abc')); //true
//注意:isNaN() 检测的时候会发生隐式类型转换.
console.log(isNaN('123')); //'false'

```

### 3. 值类型&引用类型
- 值类型:  基本数据类型     
	- 存放的是真实的值         
- 引用类型:  复杂的数据类型
	- 存放的是值的地址(引用,指针)    
```javascript
var num = 10; //值类型
var arr = [10,20,30]; //引用类型
```

- 值类型作为函数的参数
```javascript
function test1(num){
  num = 20;
}
var num1 = 10;
test1(num1);
console.log(num1); //10
```

- 引用类型作为函数的参数
```javascript
function test2(arr){
  arr[0] = 100;
}
var arr1 = [10,20,30];
test2(arr1);
console.log(arr1[0]);
```

### 4. 转换成布尔类型的false
undefined null 0 -0 '' false   NaN
非0就是转换成布尔类型的true
如果操作数是一个对象，返回 false ；
如果操作数是一个空字符串，返回 true ；
如果操作数是一个非空字符串，返回 false ；
如果操作数是数值 0，返回 true ；
如果操作数是任意非 0 数值（包括 Infinity ），返回 false ；
如果操作数是 null ，返回 true ；
如果操作数是 NaN ，返回 true ；
如果操作数是 undefined ，返回 true 。



### 5. [] == ![]
全部都为true
```javascript
console.log([] == ![]);	
console.log([] == false);
console.log([].valueOf() == false);
console.log([] == false);
console.log([].toString() == false);
console.log("" == false);
console.log(false == false);
//以上结果都是true
```
### 6. 复杂数据类型和基本数据类型运算

```javascript
// 4.一个复杂的数据类型和一个基本数据类型参与运算,就会先调用复杂数据类型的valueOf()方法得到值来参与运算.
// 如果调用valueOf()方法得到的值不能参与运算,那么就调用这个复杂数据类型的toString()方法得到值再来运算.
var obj = {};
console.log(obj.valueOf()); //{}
console.log(obj.toString()); //'[object Object]'
console.log(obj + 100); //'[object Object]100'

var obj1 = {
  valueOf: function () {
    return 200;
  }
}
console.log(obj1 + 100);//300
```