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