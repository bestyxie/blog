### ����cookie��������
�����Ŀ��ǰ����Ҫ����һ��cookie�����������˼������⣨��Ҫ����Ϊ��cookie�Ĳ���Ϥ��
��Ҫ�漰����������������֮���cookie�Ĺ��������޸ĺ�ɾ��
##### 1������cookie
**������**

��������ֻ������domainΪ��������cookie������������Ч

> ��abc.com������domainΪabc.com����.abc.com��www.abc.com������������Ϊdef.abc.com

�ڲ�ָ��domain�������domainΪ��ǰ����

```javascript
$.cookie("login_id", 1111, {
  path: "/",
  expires: 30/*��λΪ��*/
}); /* abc.com�������������������Զ�ȡ���޸� */

$.cookie("login_id", 1111, {
  path: "/",
  domain: "abc.com",
  expires: 30/*��λΪ��*/
}); /* abc.com�������������������Զ�ȡ���޸� */

$.cookie("login_id", 1111, {
  path: "/",
  domain: "www.abc.com",
  expires: 30/*��λΪ��*/
}); /* abc.com�������������������Զ�ȡ���޸� */

$.cookie("login_id", 1111, {
  path: "/",
  domain: "def.abc.com",
  expires: 30/*��λΪ��*/
}); /* ������Ч */

```

**��������**

��def.abc.comΪ��

```javascript
$.cookie("login_id", 1111, {
  path: "/",
  expires: 30/*��λΪ��*/
}); /* ��def.abc.com���Զ�ȡ���޸� */

$.cookie("login_id", 1111, {
  path: "/",
  domain: "abc.com"
  expires: 30/*��λΪ��*/
}); /* abc.com�������������������Զ�ȡ���޸� */

$.cookie("login_id", 1111, {
  path: "/",
  domain: "ghi.def.abc.com"
  expires: 30/*��λΪ��*/
}); /* ������Ч */

```

> �ܵ���˵������ֻ�����õ�cookie�ڵ�ǰ�����»��߸��ڵ�ǰ�����������²Ż���Ч

##### 2����ȡcookie

> * ��������ֻ�ܶ�ȡ��ǰ�����»�����������domainΪ�ȵ�ǰ�������߼��������µ�cookie�����ܶ�ȡ������������
�µ�cookie�������Ҫ��������������cookie������Ҫ��cookie��domain����Ϊ��������
> * ��������ֻ�ܶ�ȡdomainΪ��ǰ������cookie�����ܶ�ȡdomainΪ��������cookie

##### 3��ɾ��cookie

ɾ��cookie�Ͷ�ȡcookie������ͬ��֮��

> * ������������ɾ��domainΪ��ǰ������cookie�������Ҫɾ�����߼���������cookie������Ҫ�ƶ�domain
> * ����������ֻ��ɾ��domainΪ��ǰ������cookie

��def.abc.comΪ����
```javascript
$.removeCookie("login_id", {path: "/"})/*ɾ����ǰ�����µ�cookie*/
$.removeCookie("login_id", {path: "/", domain: "abc.com"}); /* ɾ��abc.com������login_id*/
```