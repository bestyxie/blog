### webpack

>首先当然是安装啦~~~
`npm install webpack --save-dev`

>然后我们安装jQuery
`npm install jQuery --save`
项目目录
```
--webpack-demo
  |--dis
    |--index.html
  |--src
    |--index.js
```

index.js
```javascript
import $ from "jQuery";

var div_ele = $("<div>Hello World!</div>");
$("body").appendChild(div_ele);
```
用了webpack之后，我们就可以像按需在js文件里面引进依赖的包和库，不需要再在html文件里分别加载这些文件了

index.html
```html
<!DOCTYPE html>
<html>
<head>
<title>webpack-demo</title>
</head>
<body>
<script src="./bundle.js"></script>
</body>
</html>
```
然后我们在命令行执行`webpack src/index.js dist/bundle.js`，webpack就会自动帮我们打包好所有文件

一般我们的项目不会这么简单，那么再在命令行打包就非常的不方便，所以webpack有个配置文件*webpack.config.js*
在根目录下创建webpack.config.js
```javascript
var path = require("path")

module.exports = {
    entry: "./src/index.js",
    output: {
      filename: "bundle.js",
      path: path.join(__dirname, "dist")
    }
}
```
entry为入口文件的路径，output就是输出
接下来就可以在命令行直接执行webpack命令了，默认就是执行webpack.config.js文件，也可以指定其他文件`config.js`