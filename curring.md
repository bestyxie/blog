### 柯里化函数
----
面试啥的经常都会问到函数柯里化，以前只知道原理，认为很简单就没有真正写过，今天就来看看，到底应该怎么实现。
```javascript
function add(){
  var args = [].slice.call(arguments);
  var adder = function(){
    var _adder = function(){
      var _args = [].push.call(args,arguments);
      return _adder;
    }
    // 利用隐式转换的特性，当最后执行时隐式转换，并计算最终的值返回
    _adder.toString = function(){
      return _args.reduce(function(a,b){
        return a + b;
      });
    }
  }
  return adder.apply(null,args);
}
```
这里需要说明的是在adder方法里面重写了_adder的toString方法，其主要目的是为了计算，以及在_adder方法内部返回其本身的时候，既希望是结果，又希望是方法的目的。
后面就可以这样随意组合实现加法的运算了：
```javascript
add(1,2,3,4,5); //15
add(1,2,3,4)(5); //15
add(1)(2)(3)(4)(5); //15
```
