### webpack

>���ȵ�Ȼ�ǰ�װ��~~~
`npm install webpack --save-dev`

>Ȼ�����ǰ�װjQuery
`npm install jQuery --save`
��ĿĿ¼
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