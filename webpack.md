### webpack

##### 1、 **basic usage**
>首先当然是安装啦~~~
```
npm install webpack --save-dev
````

>然后我们安装jQuery
```
npm install jQuery --save
```
项目目录
```
|-webpack-demo
  |-dis
    |-index.html
  |-src
    |-index.js
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
```
webpack --config config.js
```

##### 2、 资源管理(assets management)
webpack不仅可以打包js文件，还可以打包css，图片、字体、json等文件，为了可以在js文件里面引用这些文件，我们需要安装对应的loader

**（1）Loading css**

  为了打包css文件，我们需要安装style-loader和css-loader
```
npm install --save-dev style-loader css-loader
```
webpack.config.js
```javascript
var path = require("path")

module.exports = {
    entry: "./src/index.js",
    output: {
      filename: "bundle.js",
      path: path.resolve(__dirname, "dist")
    },
    module: {
      rules: [
        {
          text: /\.(css|sass|less)$/,
          use: ["style-loader","css-loader"]
        }
      ]
    }
}
```
loader是从右往左执行的，意思是会先执行css-loader再执行style-loader，现在我们可以通过import './style.css'的方式在需要用到该样式的文件里引入css文件了，该模块在执行的时候，
会在<head>标签内生成<style>标签，里面包含已经被序列化（stringified）过的样式

**(2)Loading Image、Json...**

webpack打包图片、json、字体等文件，只需要用到file-loader
```
npm install --save-dev file-loader
```
webpack.config.js
```javscript
var path = require("path")

module.exports = {
    entry: "./src/index.js",
    output: {
      filename: "bundle.js",
      path: path.resolve(__dirname, "dist")
    },
    module: {
      rules: [
        {
          text: /\.(css|sass|less)$/,
          use: ["style-loader","css-loader"]
        },
        {
          text: /\.(jpeg|png|gif|svg)$/,
          use: ["file-loader"]
        }
      ]
    }
}
```
其他资源雷同

#### （3）输出管理（output management）
>在平时的开发中，免不了会在文件名后面再加上hash值作为文件的名称，或者需要增加入口文件，每一次构建资源的名称都会发生改变，
那么再引用了入口文件的html里，就需要做出相应的改变，这就变得特别的麻烦。这时可以用`html-webpack-plugin`为我们解决
问题
```
npm install --save-dev html-webpack-plugin
```
webpack.config.js
```javascript
var path = require("path");
var HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: {
    app: "./src/index.js"
  },
  plugins: [
    new HtmlWebpackPlugin()
  ],
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist")
  }
}
```
该插件会将js资源放到body标签后面，当使用了`extract-text-webpack-plugin`的时候，会将样式通过link
标签放在head标签里

#### (4)开发模式(development)
在前面的配置中，webpack会将我们依赖的所有的js资源都打包到一个bundle里面，当需要调试时，浏览器console
出来的信息指向的也是在bundle里面的位置，很难追踪到代码的原始位置。为了更好的追踪错误和警告，javascript提供了
source map功能，将编译后的代码映射回原始源代码，它可以明确地告诉我们错误来自源代码的哪个文件哪一行。

在webpack中，我们使用`inline-source-map`选项进行配置

webpack.config.js
```javascript
var path = require("path");
var HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: {
    app: "./src/index.js"
  },
  devtool: "inline-source-map",
  plugins: [
    new HtmlWebpackPlugin()
  ],
  output: {
    filename: "[name].bundle.js",
    path: path.resolve(__dirname, "dist")
  }
}
```