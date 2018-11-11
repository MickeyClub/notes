# JavaScript里的事件委托





HTML代码如下

```html
<body>
    <input type="button" value="点我生成li元素" id="btn">
    <ul id="ul">
        <li>隔壁老王1</li>
        <li id="li2">隔壁老王2</li>
        <li>隔壁老王3</li>
        <li>隔壁老王4</li>
        <li>隔壁老王5</li>
    </ul>
</body>
```

- 通过普通方式不能给动态生成的元素添加点击事事件

  JavaScript代码如下

```javascript
 <script>
     //获取ul元素
    var ul = document.getElementById('ul')
    //获取所有li元素
    var liList = document.getElementsByTagName('li')

    //动态添加li元素
    document.getElementById('btn').onclick = function () {
        var li = document.createElement('li');
        li.innerHTML = "我是新来的";
        ul.appendChild(li);
    }
    
    //使用普通方式不能给动态生成的元素添加点击事件
    for (var i = 0; i < liList.length; i++) {
        liList[i].onclick = function () {
            alert(this.innerHTML);
        }
    }
 </script>
```



通过元素的js事件委托

```javascript
<script>
    //获取ul元素
    var ul = document.getElementById('ul')
    //获取所有li元素
    var liList = document.getElementsByTagName('li')

    //动态添加li元素
    document.getElementById('btn').onclick = function () {
        var li = document.createElement('li');
        li.innerHTML = "我是新来的";
        ul.appendChild(li);
    }

    // 用原生js进行事件委托
    // 给当前元素的父级添加点事件,并监听 target事件源
     ul.addEventListener('click',function (e) {
        alert(e.target.innerHTML)
    })

</script>
```

