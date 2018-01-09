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
**选择一个开发工具**

每次编译代码的时候都需要手动执行webpack命令，这就变得非常麻烦，在webpack中有一下三种选项可以帮助你在代码发生变化的
时候自动编译代码：

1、Webpack's Watch Mode

2、Webpack-dev-server

3、Webpack-dev-middleware

**观察者模式（Webpack's Watch Mode）**

```
webpack --watch
```
现在运行这段脚本，你会看到webpack编译代码，却不会退出命令行，这是因为script脚本还在观察文件

我们再去修改任意一个文件的时候，就能到看到webpack自动重新编译修改后的模块。

这种方式唯一的缺点就是，为了看到修改后的效果，必须手动刷新浏览器。如果能自动刷新浏览器就更好了，
`webpack-dev-server`恰好能够实现我们想要的效果。

**webpack-dev-server**

`webpack-dev-server`可以为你提供一个简单的web服务器，并且能够实时重新加载（live reloading），
还是要安装啊，哭唧唧
```
npm install --save-dev webpack-dev-server
```
webpack.config.js
```javascript
var path = require("path");
var HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: {
    app: "./src/index.js"
  },
  devtool: "inline-source-map",
  devServer: {
    contentBase: "./dist"
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
然后执行命令
```
webpack-dev-server --open
```
现在再对相关代码进行修改的时候，你可以发现浏览器会自动刷新

**webpack-dev-middleware**

通过名称我们也能发现，这其实是一个中间件，这意味着我们可以在自己搭建的服务器上实现实时刷新，而不是webpack提供的web
服务器。

通过`webpack-dev-middleware`进行实时刷新，需要通过express搭建web服务器,该中间件将通过webpack处理后的文件发送到该服务器
```
npm install --save-dev express webpack-dev-middleware
```
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
    path: path.resolve(__dirname, "dist"),
    publicPath: "/"
  }
}
```

项目根目录下新建一个`server.js`文件
```
|-webpack-demo
  |-dis
    |-index.html
  |-src
    |-index.js
  |-server.js
  |-webpack.config.js
  |-package.json
```
server.js
```javascript
var express = require("express");
var webpack = require("webpack");
var webpackDevMiddleware = require("webpack-dev-middleware");

var app = express();
var config = require("./webpack.config.js");
var compiler = webpack(config);

app.use(webpackDevMiddleware(compiler, {
  publicPath: config.output.publicPath
}));

app.listen(3000, function () {
  console.log("Example app listening on port 3000!\n");
})
```
现在可以在本地执行
```
node server.js
```
现在可以在浏览器中打开`http://localhost:3000`，就可以看到你的webpack应用程序已经运行,并且可以在文件修改之后自动刷新浏览器
#### 热替换
emmmmmm...感觉这个功能归于单页面的开发比较重要，如果是使用react进行开发，参考[react-hot-loader](https://github.com/gaearon/react-hot-loader),
vue则参考[vue loader](https://github.com/vuejs/vue-loader)
#### Tree Shaking
官方文档说tree shaking是一个术语。。。通常用于描述移除javascript上下文的未引用代码（dead-code）

当我们引入一个模块的时候，有可能只用到了他们其中的一些方法，但是webpack还是会将其他没有用到的方法一块儿打包进bundle中

为了解决这个问题，我们可以添加一个能够删除未引用代码的压缩工具——UglifyjsPlugin
```
npm install --save-dev uglifyjs-webpack-plugin
```
webpack.config.js
```javascript
const path = require("path");
const UglifyJsPlugin = require("uglifyjs-webpack-plugin");

module.exports = {
  entry: {
      app: "./src/index.js"
    },
    devtool: "inline-source-map",
    plugins: [
      new HtmlWebpackPlugin(),
      new UglifyJsPlugin()
    ],
    output: {
      filename: "[name].bundle.js",
      path: path.resolve(__dirname, "dist"),
      publicPath: "/"
    }
}
```