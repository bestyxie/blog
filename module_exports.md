### export和module.export的区别
在node.js中，通常都是用module.exports和exports进行模块的暴露的，在实际的开发过程中，
发现module.exports和exports的用处好像差不多，于是研究了一下两者的区别。

在node的js模块中，可以直接调用module和exports两个全局变量，但是exports只是module.exports的
一个引用。

> * module.exports的初始值是{}，exports和module.exports指向同一个对象，故exports的初始值也是该{}
> * require引用模块后，返回的是module.exports而不是exports
> * 给exports重新赋值（即：exports=...），这时exports和module.exports指向的就不是同一个对象了，故调用
模块的时候不能访问exports的对象及属性
> * exports.xxx 相当于在导出对象上挂属性，该属性对调用模块直接可见

```javascript
//a.js
var a = 1;
module.exports = a;
exports.a = 2;

//main.js
var a = require('a');

console.log(a) //1
```
现在应该就不难理解为什么打印出来的a是1而不是2了吧

```javascript
//view.js
var view = function (){
  console.log("1");
}
module.exports.view = view;//同exports.view = view;

//main.js
var view = require("view");

view.view();
```
通过给module.exports添加属性的暴露方法，在引用的时候view方法只是该模块的一个方法，如果想直接
通过view()的方式进行调用，可以将view方法当做整个模块赋值给module.exports（module.exports = view）

**所以推荐使用module.exports，尽量少用export来暴露模块方法，这样可以避免一些不必要的问题**