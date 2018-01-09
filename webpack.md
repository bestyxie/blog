### webpack

##### 1�� **basic usage**
>���ȵ�Ȼ�ǰ�װ��~~~
```
npm install webpack --save-dev
````

>Ȼ�����ǰ�װjQuery
```
npm install jQuery --save
```
��ĿĿ¼
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
����webpack֮�����ǾͿ���������js�ļ��������������İ��Ϳ⣬����Ҫ����html�ļ���ֱ������Щ�ļ���

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
Ȼ��������������ִ��`webpack src/index.js dist/bundle.js`��webpack�ͻ��Զ������Ǵ���������ļ�

һ�����ǵ���Ŀ������ô�򵥣���ô���������д���ͷǳ��Ĳ����㣬����webpack�и������ļ�*webpack.config.js*
�ڸ�Ŀ¼�´���webpack.config.js
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
entryΪ����ļ���·����output�������
�������Ϳ�����������ֱ��ִ��webpack�����ˣ�Ĭ�Ͼ���ִ��webpack.config.js�ļ���Ҳ����ָ�������ļ�`config.js`
```
webpack --config config.js
```

##### 2�� ��Դ����(assets management)
webpack�������Դ��js�ļ��������Դ��css��ͼƬ�����塢json���ļ���Ϊ�˿�����js�ļ�����������Щ�ļ���������Ҫ��װ��Ӧ��loader

**��1��Loading css**

  Ϊ�˴��css�ļ���������Ҫ��װstyle-loader��css-loader
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
loader�Ǵ�������ִ�еģ���˼�ǻ���ִ��css-loader��ִ��style-loader���������ǿ���ͨ��import './style.css'�ķ�ʽ����Ҫ�õ�����ʽ���ļ�������css�ļ��ˣ���ģ����ִ�е�ʱ��
����<head>��ǩ������<style>��ǩ����������Ѿ������л���stringified��������ʽ

**(2)Loading Image��Json...**

webpack���ͼƬ��json��������ļ���ֻ��Ҫ�õ�file-loader
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
������Դ��ͬ

#### ��3���������output management��
>��ƽʱ�Ŀ����У��ⲻ�˻����ļ��������ټ���hashֵ��Ϊ�ļ������ƣ�������Ҫ��������ļ���ÿһ�ι�����Դ�����ƶ��ᷢ���ı䣬
��ô������������ļ���html�����Ҫ������Ӧ�ĸı䣬��ͱ���ر���鷳����ʱ������`html-webpack-plugin`Ϊ���ǽ��
����
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
�ò���Ὣjs��Դ�ŵ�body��ǩ���棬��ʹ����`extract-text-webpack-plugin`��ʱ�򣬻Ὣ��ʽͨ��link
��ǩ����head��ǩ��

#### (4)����ģʽ(development)
��ǰ��������У�webpack�Ὣ�������������е�js��Դ�������һ��bundle���棬����Ҫ����ʱ�������console
��������Ϣָ���Ҳ����bundle�����λ�ã�����׷�ٵ������ԭʼλ�á�Ϊ�˸��õ�׷�ٴ���;��棬javascript�ṩ��
source map���ܣ��������Ĵ���ӳ���ԭʼԴ���룬��������ȷ�ظ������Ǵ�������Դ������ĸ��ļ���һ�С�

��webpack�У�����ʹ��`inline-source-map`ѡ���������

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
**ѡ��һ����������**

ÿ�α�������ʱ����Ҫ�ֶ�ִ��webpack�����ͱ�÷ǳ��鷳����webpack����һ������ѡ����԰������ڴ��뷢���仯��
ʱ���Զ�������룺

1��Webpack's Watch Mode

2��Webpack-dev-server

3��Webpack-dev-middleware

**�۲���ģʽ��Webpack's Watch Mode��**

```
webpack --watch
```
����������νű�����ῴ��webpack������룬ȴ�����˳������У�������Ϊscript�ű����ڹ۲��ļ�

������ȥ�޸�����һ���ļ���ʱ�򣬾��ܵ�����webpack�Զ����±����޸ĺ��ģ�顣

���ַ�ʽΨһ��ȱ����ǣ�Ϊ�˿����޸ĺ��Ч���������ֶ�ˢ���������������Զ�ˢ��������͸����ˣ�
`webpack-dev-server`ǡ���ܹ�ʵ��������Ҫ��Ч����

**webpack-dev-server**

`webpack-dev-server`����Ϊ���ṩһ���򵥵�web�������������ܹ�ʵʱ���¼��أ�live reloading����
����Ҫ��װ����������
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
Ȼ��ִ������
```
webpack-dev-server --open
```
�����ٶ���ش�������޸ĵ�ʱ������Է�����������Զ�ˢ��

**webpack-dev-middleware**

ͨ����������Ҳ�ܷ��֣�����ʵ��һ���м��������ζ�����ǿ������Լ���ķ�������ʵ��ʵʱˢ�£�������webpack�ṩ��web
��������

ͨ��`webpack-dev-middleware`����ʵʱˢ�£���Ҫͨ��express�web������,���м����ͨ��webpack�������ļ����͵��÷�����
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

��Ŀ��Ŀ¼���½�һ��`server.js`�ļ�
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
���ڿ����ڱ���ִ��
```
node server.js
```
���ڿ�����������д�`http://localhost:3000`���Ϳ��Կ������webpackӦ�ó����Ѿ�����,���ҿ������ļ��޸�֮���Զ�ˢ�������
#### ���滻
emmmmmm...�о�������ܹ��ڵ�ҳ��Ŀ����Ƚ���Ҫ�������ʹ��react���п������ο�[react-hot-loader](https://github.com/gaearon/react-hot-loader),
vue��ο�[vue loader](https://github.com/vuejs/vue-loader)
#### Tree Shaking
�ٷ��ĵ�˵tree shaking��һ���������ͨ�����������Ƴ�javascript�����ĵ�δ���ô��루dead-code��

����������һ��ģ���ʱ���п���ֻ�õ����������е�һЩ����������webpack���ǻὫ����û���õ��ķ���һ��������bundle��

Ϊ�˽��������⣬���ǿ������һ���ܹ�ɾ��δ���ô����ѹ�����ߡ���UglifyjsPlugin
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