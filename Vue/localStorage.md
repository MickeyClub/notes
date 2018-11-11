# 页面关闭后获取localStorage



```javascript
// 读取本地localStorage数据
let todoList = JSON.parse(localStorage.getItem("todoList")) || []
// 注册页面关闭时间,存取localStorage数据
window.onunload = function () {
    localStorage.setItem("todoList",JSON.stringify(todoList))
}
```

