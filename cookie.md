### 关于cookie的作用域
最近项目中前端需要操作一下cookie，于是遇到了几个问题（主要是因为对cookie的不熟悉）
主要涉及到根域名与子域名之间的cookie的共享、互相修改和删除
##### 1、设置cookie
**根域名**

根域名下只能设置domain为根域名的cookie，否则设置无效

> 如abc.com能设置domain为abc.com或者.abc.com、www.abc.com，但不能设置为def.abc.com

在不指定domain的情况下domain为当前域名

```javascript
$.cookie("login_id", 1111, {
  path: "/",
  expires: 30/*单位为天*/
}); /* abc.com及其所有子域名都可以读取并修改 */

$.cookie("login_id", 1111, {
  path: "/",
  domain: "abc.com",
  expires: 30/*单位为天*/
}); /* abc.com及其所有子域名都可以读取并修改 */

$.cookie("login_id", 1111, {
  path: "/",
  domain: "www.abc.com",
  expires: 30/*单位为天*/
}); /* abc.com及其所有子域名都可以读取并修改 */

$.cookie("login_id", 1111, {
  path: "/",
  domain: "def.abc.com",
  expires: 30/*单位为天*/
}); /* 设置无效 */

```

**二级域名**

拿def.abc.com为例

```javascript
$.cookie("login_id", 1111, {
  path: "/",
  expires: 30/*单位为天*/
}); /* 仅def.abc.com可以读取并修改 */

$.cookie("login_id", 1111, {
  path: "/",
  domain: "abc.com"
  expires: 30/*单位为天*/
}); /* abc.com及其所有子域名都可以读取并修改 */

$.cookie("login_id", 1111, {
  path: "/",
  domain: "ghi.def.abc.com"
  expires: 30/*单位为天*/
}); /* 设置无效 */

```

> 总的来说，就是只有设置的cookie在当前域名下或者高于当前域名的域名下才会生效

##### 2、读取cookie

> * 二级域名只能读取当前域名下或者是设置了domain为比当前域名更高级的域名下的cookie，不能读取其他二级域名
下的cookie，如果想要和其他域名共享cookie，则需要将cookie的domain设置为顶级域名
> * 顶级域名只能读取domain为当前域名的cookie，不能读取domain为子域名的cookie

##### 3、删除cookie

删除cookie和读取cookie有异曲同工之处

> * 二级域名可以删除domain为当前域名的cookie，如果想要删除更高级的域名的cookie，则需要制定domain
> * 顶级域名则只能删除domain为当前域名的cookie

以def.abc.com为例：
```javascript
$.removeCookie("login_id", {path: "/"})/*删除当前域名下的cookie*/
$.removeCookie("login_id", {path: "/", domain: "abc.com"}); /* 删除abc.com域名下login_id*/
```