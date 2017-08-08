### Layout布局

#### 两栏式布局

```css
// 清楚浮动
.fn-clear {
  zoom: 1
}
div {
    box-sizing: border-box;
}
.fn-clear:after {
  content: '';
  display: block;
  visibility: hidden;
  inline-height: 0;
  clear: both;
  overflow: hidden;
}
.a {
  background: lightblue;
  text-align: center;
  font-size: 32px;
  line-height: 200px;
}
.b {
  background: yellowgreen;
  font-size: 32px;
  line-height: 200px;
  text-align: center;
}
```
```html
<div class="fn-clear">
  <div class="left a">left</div>
  <div class="right b">right</div>
</div>
```
左边自适应，右边固定宽度200px
```html
<div class="fn-clear">
  <div class="left a">left</div>
  <div class="right b">right</div>
</div>
```
```css
.left {
  float: left;
  width: 100%;
  padding-right: 200px;
}
.right {
  float: left;
  width: 200px;
  margin-left: -200px;
}
```
右边自适应，左边固定宽200px
```html
// 顺序反一反
<div class="fn-clear">
  <div class="right a">right</div>
  <div class="left b">left</div>
</div>
```
```css
.left {
  float: right;
  width: 200px;
  margin-right: -200px;
}
.right {
  float: right;
  width: 100%;
  padding-left: 200px;
}
```
两边长度固定
```html
<div class="fn-clear" style="min-width: 400px;">
  <div class="left a">left</div>
  <div class="right b">right</div>
</div>
```
```css
.left {
  float: left;
  width: 200px;
}
.right {
  float: right;
  width: 200px;
}
```