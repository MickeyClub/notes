# ES6新增的方法

[TOC]

### some()

some():方法用于检测数组中的元素是否有满足指定条件的，若满足返回true，否则返回false；

```javascript
this.list.some((item,index)=>{
    // 遍历匹配对应的id 删除
    if(item.id == id){
        this.list.splice(index,1)
        return true;   // 如果返回true  立即终止数组循环
    }
})
```

###  

### findIndex()

 findIndex()函数也是查找目标元素，找到就返回元素的位置，找不到就返回-1。 

```javascript
  let index = this.list.findIndex(item => {
      if (item.id == id) {
        return true
        }
      })
  this.list.splice(index, 1)
```



### filter过滤器 

类似于forEach 

```javascript
return this.list.filter(item => {
    // if(item.name.indexOf(keywords) != -1)
	// include 自己
    if (item.name.includes(keywords)) {
        return item
    }

})
```





### padStart() 

作用: 方法会用一个字符串填充当前字符串（如果需要的话则重复填充），返回填充后达到指定长度的字符串。从当前字符串的末尾（左侧）开始填充。 

语法: 

```
str.padEnd(targetLength [, padString])
```

### padEnd() 

作用: 方法会用一个字符串填充当前字符串（如果需要的话则重复填充），返回填充后达到指定长度的字符串。从当前字符串的末尾（右侧）开始填充。 

语法: 

```
str.padEnd(targetLength [, padString])
```

