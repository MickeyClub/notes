## crawler爬虫框架

官网传送门: https://www.npmjs.com/package/crawler





### 封装: 爬虫下载网络图片到本地

```javascript
// 获取图片到本地  工具函数
function getImg(uri, filename) {
    // 图片爬虫
    var picSpider = new Crawler({
        encoding: null,
        jQuery: false, // set false to suppress warning message.
        callback: function (err, res, done) {
            if (err) {
                console.error(err.stack);
            } else {
                fs.createWriteStream("./imgs/"+res.options.filename).write(res.body);
            }
            done();
        }
    });

    // 获取图片 并保存
    picSpider.queue({
        uri,
        filename
    });
}
```





