# JavaScript基础(运算符&数据类型&Math对象)

[TOC]

## 变量的命名
规则:
1. 必须以字母，下划线_  \$ 开头，后面接任意的字母,下划线, $ ，数字
2. 不能使用js关键字(已经被js作者事先赋予的特殊的含义)

规范: 
1. 起名要有意义，最好用名词
2. 使用驼峰命名法：第一个单词首字母小写，后面的单词首字母大写

## 算术运算符
- 数学算术运算: 
   +：加
   -：减
    *：乘
    /：除
    %：求余数（求模）
   
## 复合算术运算符
- 复合算术运算符:算术运算的一种简写形式
    += :自身的基础上加多少
    -= : 自身的基础上减多少
    *= :自身的基础上乘多少
    /= :自身的基础上除多少
    %=:自身的基础上模多少

## 关系运算符
1.关系运算符：（比较运算符）  比较两个数据之间的关系（某种条件是否成立）
    \> >= < <=  ==(相等) !=  ===（全等）  !==

2.关系表达式：由关系运算符组成的式子 例如 1 > 0

3.关系表达式的结果一定是布尔类型的数据（true成立  false不成立）

## 逻辑运算符
   &&：逻辑与   并且
    ||:逻辑或   或者
    ! : 逻辑非   取反
    
  - 逻辑与表达式：一假则假(false)
  - 逻辑或表达式：一真则真(true)
  - 逻辑非表达式：取反   真变假  假变真

## 五种基本数据类型
1. string(字符串)
2. number(数字)
3. boolean(布尔)
4. undefined(未定义)
5. null(空值)

ndefined与null的区别
```javascript
console.log ( null == undefined );//true  他们的值都是空
    console.log ( null === undefined );//false  他们的值相等 但是数据类型不同

```

## Math对象：高级数学计算
```javascript
 //1.圆周率
    console.log ( Math.PI );//3.141592653589793
    //2.绝对值:一个数字离坐标原点的距离
    console.log ( Math.abs ( - 2 ) );//2
    //3.天花板函数:向上取整
    console.log ( Math.ceil ( 2.3 ) );//3
    //4.地板函数：向下取整
    console.log ( Math.floor ( 2.3 ) );//2
    //5.四舍五入：
    console.log ( Math.round ( 4.5 ) );//5
    //6.求一组数字最大值
    console.log ( Math.max ( 10, 50, - 10, 99 ) );//99
    //7.求一组数字最小值
    console.log ( Math.min ( 10, 50, - 10, 99 ) );//-10
    //8.求平方（幂运算）  Math.pow(x,y)  求x的y次方
    console.log ( Math.pow ( 2, 3 ) );//8
    //9.求随机数  0-1之间的随机小数
    console.log ( Math.random () );

```
随机生成1-100之间的整数
```javascript
    //思路  0-1的小数放大100倍  0-100
    //0-100向上取整  1-100之间的整数
    var num = Math.random () * 100;//0-100的小数
    console.log ( Math.ceil(num) );
```

## NaN与isNaN
- NaN: not a number 不是一个数字,是数据错误计算的结果

```javascript
 var num = '张三' - 100;//当某个表达式无法计算（计算错误时），会得到NaN
    console.log ( num );//NaN
    console.log ( typeof  NaN );//number

    //2.NaN与任何数字都不等，包含它本身
    console.log ( NaN == 0 );//false
    console.log ( NaN == NaN );//false

    //3.NaN与任何数字计算得到的都是NaN
    console.log ( NaN + 100 );//NaN

    //4.isNaN(数据)：检测一个数据是不是NaN  得到的结果是布尔类型
    console.log ( isNaN ( NaN ) );//true
    console.log ( isNaN ( 123 ) );//false
    //如果检测的数据不是number类型，js编译器会尝试着将这个数据转化为number类型，然后再判断
    //这种数据类型转换称为隐式转换：下一小节讲解
    console.log ( isNaN ( "abc" ) );//true

    //5.number浮点数（小数）精度丢失
    //小数在进行数学计算时，会有一定的误差，这是计算机本身的bug，不仅是js语言，其他语言也有这个问题
    //解决方案：不要让两个小数比较大小，这种情况一般不会影响正常开发
    console.log ( 0.1 + 0.2 );//0.300000000000004
    console.log ( 0.1 + 0.2 == 0.3 );//false
    console.log ( 0.4 + 0.5 );//0.9
    console.log ( 1.1 - 0.2 );//0.9000000000000001
```

## 数据类型转换
1. 转换成number类型
```javascript
/**转换成number
     * 第一种方式：`parseInt()`
     *          * 作用：转换整数
     *          * 从左往右解析，遇到非数字结束，将解析好的整数返回
     *          * 如果第一个字符不是数字或者符号就返回NaN
     * 第二种方式：`parseFloat()`
     *          * 作用：转换小数
     *          * 与parseInt()最大的区别就是可以解析字符串的第一个小数点
     * 第三种方式: `Number()`
     *         * 可以把任意值转换成数值，如果要转换的字符串只要有一个不是数字，返回NaN
     */
    console.log ( Number ( "123" ) );//123
    console.log ( Number ( "123.1.1abc" ) );//NaN  只要有一个字符不是数字，得到就是NaN
    console.log ( parseInt ( "123.1.1abc" ) );//123  从左往右解析，遇到非数字结束
    console.log ( parseFloat ( "123.1.1abc" ) );//123.1  与parseInt唯一的区别就是可以识别第一个小数点

    //一般数字字符串使用parseInt和parseFloat，其他数据类型转数字使用Number（）
    console.log ( Number ( true ) );//1    布尔类型转换number会得到数字0(false)和1（true）
    console.log ( Number ( "" ) );//0  空字符串转number会得到0
```

2. 转换string类型
```javascript
  /**转换成字符串string
     * 第一种方式：`变量名.toString()`
     *      * 如果变量的值为undefined或者null，则会报错
     *  第二种方式：`String(变量名)`
     *      * 与第一种方式的唯一区别就是如果变量的值为undefined或者null不会报错，会得到undefined或者null
     *   第三种方式：`变量名 + ”“ `
            * 原理：当 + 两边一个操作符是字符串类型，一个操作符是其它类型的时候，会先把其它类型转换成字符串再进行字符串拼接，返回字符串
            * 技巧：让一个变量与一个空字符串相加，这个变量就会变成字符串
            * 这种方式可以识别undefined和null
     */
    var num = 20//number类型
    var n
    var str = num.toString ()//转换成字符串
    console.log ( num )//数据类型转换不会改变原有的变量的值
    console.log ( str )//而是得到一个新的变量
    //console.log ( n.toString() );//程序会报错，因为此时n的值为undefined
    console.log ( String ( n ) )//不会报错，因为String()转换可以识别undefined和null
    console.log ( n + "" )//undefined
    console.log ( num + "" )//20 字符串类型
```


3. 转换成boolean类型

- 只有一种方式: Boolean(变量名)
- 有八种情况得到的false，其他一切均为true：0、-0、null、false、NaN、undefined、ocument.all

```javascript
/**转换成boolean
     * 只有一种方式:  `Boolean(变量名)`
     *      * 除数字 0、undefined、null 为假之外，其他所有一切皆为真
     */

    console.log ( Boolean ( 0 ) );//false
    console.log ( Boolean ( "0" ) );//true
    console.log ( Boolean ( null ) );//false
    console.log ( Boolean ( undefined ) );//false
    console.log ( Boolean ( 1 ) );//true
```