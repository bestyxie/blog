### export��module.export������
��node.js�У�ͨ��������module.exports��exports����ģ��ı�¶�ģ���ʵ�ʵĿ��������У�
����module.exports��exports���ô������࣬�����о���һ�����ߵ�����

��node��jsģ���У�����ֱ�ӵ���module��exports����ȫ�ֱ���������exportsֻ��module.exports��
һ�����á�

> * module.exports�ĳ�ʼֵ��{}��exports��module.exportsָ��ͬһ�����󣬹�exports�ĳ�ʼֵҲ�Ǹ�{}
> * require����ģ��󣬷��ص���module.exports������exports
> * ��exports���¸�ֵ������exports=...������ʱexports��module.exportsָ��ľͲ���ͬһ�������ˣ��ʵ���
ģ���ʱ���ܷ���exports�Ķ�������
> * exports.xxx �൱���ڵ��������Ϲ����ԣ������ԶԵ���ģ��ֱ�ӿɼ�

```javascript
//a.js
var a = 1;
module.exports = a;
exports.a = 2;

//main.js
var a = require('a');

console.log(a) //1
```
����Ӧ�þͲ������Ϊʲô��ӡ������a��1������2�˰�

```javascript
//view.js
var view = function (){
  console.log("1");
}
module.exports.view = view;//ͬexports.view = view;

//main.js
var view = require("view");

view.view();
```
ͨ����module.exports������Եı�¶�����������õ�ʱ��view����ֻ�Ǹ�ģ���һ�������������ֱ��
ͨ��view()�ķ�ʽ���е��ã����Խ�view������������ģ�鸳ֵ��module.exports��module.exports = view��

**�����Ƽ�ʹ��module.exports����������export����¶ģ�鷽�����������Ա���һЩ����Ҫ������**