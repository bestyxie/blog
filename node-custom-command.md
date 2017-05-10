### NodeJs自定义命令
-------------------

因为现在所在的公司的前端是使用自己开发的脚手架进行前端开发的，于是对这方面提起了兴趣，但是想要建立脚手架，还是得要先从如何建立自定义的node命令说起
#### 目标
我们目标是要列出用户所在位置或者指定位置的所有文件和文件夹，像下面这样：

![Alt data](https://github.com/bestyxie/pics/blob/master/2017.05.09-blog-node-custom-command-1.png)

首先建立文件夹wcj/bin，在bin下面建立ls.js文件，注意，在文件顶部需要加上`#!/usr/bin/env node`，这个命令表示在执行该文件的时候要先去/uer/bin/env环境下去查找node解释器。lf.js：
```javascript
#!/usr/bin/env node
var run = function(obj){
    if(obj[0] == '-v'){
        console.log('version is 1.0.0');
    }else if(obj[0] == '-h'){
        console.log('Usage:');
        console.log('  -v --version [show version]');
    }else{
        fs.readdir(path,function(err,files){
            if(err){
                return console.log(err);
            }
            for(var i = 0;i < files.length; i+=1){
                console.log(files[i]);
            }
        })
    }
}

run(process.argv.slice(2));
```
再回到wcj目录下创建package.json文件：
```json
{
  "name": "lf",
  "version": "1.0.0",
  "description": "ls ----",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "bin": {"lf": "bin/lf.js"},
  "author": "",
  "license": "ISC"
}
```
bin属性表示将命令lf映射到bin/lf.js脚本
#### 安装
在项目目录下执行下面这条命令，整个包就会被安装在本地全局环境下,然后再执行ls命令：
```
npm link
```

![Alt data](https://github.com/bestyxie/pics/blob/master/2017.05.09-blog-node-custom-command-2.png)

此时执行lf.js文件不再需要写长长的node ./bin/lf.js的命令了，只需要一句ls。如果需要卸载，只需要像卸载其他npm库一样执行 `npm uninstall lf -g`就行了，这时再到npm目录下看，已经没有lf这个包的存在了。
当然，你也可以将它发布到npm上，此时你就可以像这样来安装包了：
```
npm install lf -g
```
#### commander
commander是TJ大神写的一个可以帮助我们更加方便地编写命令的npm包，通过该包，我们就不用再写那么原生的复杂的代码了，commander都已经帮我们封装好了。
#### 特性
Commander的方便之处在于：
自记录代码、自动生成帮助、合并短参数`（“ABC”==“-A-B-C”）`、默认选项、强制选项、命令解析、提示符
#### API
* version(): 定义版本
* command(): 定义一个命令名字
* option(): 添加自定义命令参数,设置关键字和描述，关键字包括简写和全写
* parse(): 解析命令行参数argv
* description(): 设置description值
* usage(): 设置usage值
#### Option
commander会为程序自动提供一个 `-h `选项
```javascript
var program = require('commander');
program.version('1.0.0')
     .option('-r| --resume','resume')
     .parse(process.argv);
if(program.resume) console.log('This is my resume!');
```
![Alt data](https://github.com/bestyxie/pics/blob/master/2017.05.09-blog-node-custom-command-1.png)

program.resume为Boolean类型，也可以为String类型。值为真或者不为空，则执行后面的语句。
option接受四个参数：
1. 关键字，包括简写和全写，简写用 -，全写用--，用‘,’，‘|’分隔
2. 描述，会在help信息里展示出来
3. 回调函数
4. 默认值
#### Unknow Option
如果option收到未知参数时，会抛出错误，可以使用allowUnknowOption()
```javascript
program.allowUnknowOption()
    .option('-r,--resume','resume')
    .parse(process.argv);
```
