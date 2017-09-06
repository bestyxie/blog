### IOS下click事件不触发
最近做移动端的项目遇到一个天大的坑，点击图片全屏显示，在安卓上一切正常，在ios上却没有反应,后来一查，才知道ios上好似没有click事件，它是touch事件
所以有两种 解决方案：
#### 1、给元素加上pointer: cursor;
#### 2、同时绑定click事件和touchstart事件
```javascript
$(element).on("click touchstart", function () {});
```
