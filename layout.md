### 水平垂直居中

### (1)absolute + transform
```html
  <div class="parent">
    <div class="child"></div>
  </div>
```
```css
.parent{
  position: relative;
}
.child{
  positon: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
}
```
* 脱离文档流，不会对后续元素的布局造成影响
* transform为css3属性，存在兼容性问题
### (2)table-cell + vertical-align + text-align + inline-block
```css
.parent{
  display: table-cell;
  text-align: center;
  vertical-align: middle;
}
.child{
  display: inline-block;
}
```
### (3)flex + justify-content + align-content
```css
.parent{
  display: flex;
  justify-content: center;
  align-content: center;
}
```
* 只需要对父元素进行样式的设置
* 兼容性存在较大的问题

## 一列定宽，一列自适应
```html
<div class="parent">
  <div class="left"></div>
  <div class="right"></div>
</div>
```
### (1)float + margin
```css
.left{
  width: 100px;
  float: left;
}
.right{
  margin-left: 100px;
}
```
此方法在IE6中会有3像素的BUG，可通过在.left中加入margin-left: -3px，进行解决
### (2)float + overfloat
```css
.left{
  width: 100px;
  float: left;
}
.right{
  overfloat: hidden;
}
```
overloat: hidden会触发BFC模式块级上下文，但不支持IE6
### (3)table
```css
.parent{
  display: table;
  width: 100%;
  table-layout: fixed;
}
.left{
  display: table-cell;
  width: 100px;
}
.right{
  display: table-cell;
}
```
不可以设置margin，但是可以通过padding来设置间距
### (4)flex
```css
.parent{
  display: flex;
}
.left{
  width: 100px;
}
.right{
  flex: 1;
}
```
* 兼容性不好
* 性能不好，不适合大范围的布局

## 等高布局
### (1) table
```css
.parent{
  display: table;
  width: 100%;
  table-layout: fixed;
}
.left{
  display: table-cell;
  width: 100px;
}
.right{
  display: table-cell;
}
```
### (2)flex
```css
.parent{
  display: fix;
}
.left{
  width: 100px;
}
.right{
  flex: 1;
}
```
注意这里实际上使用了align-items: stretch;flex默认的align-items的值为stretch
### (3)float
```css
.parent{
  overfloat: hidden;
}
.left,
.right{
  padding-bottom: 9999px
  margin-bottom: -9999px;
}
.left{
  float: left;
  width: 100px;
  margin-right: 20px;
}
.right{
  overfloat: hidden;
}
```
此方法为伪等高（只有背景显示高度相等），**兼容性较好**
