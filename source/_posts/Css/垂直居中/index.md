---
title: 垂直居中的几种方法
---

### 绝对定位与margin-top(负外边距）

<iframe height="400" style="width: 100%;" scrolling="no" title="ZELEVdq" src="https://codepen.io/ajn404/embed/ZELEVdq?height=265&theme-id=light&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/ajn404/pen/ZELEVdq'>ZELEVdq</a> by ajn404
  (<a href='https://codepen.io/ajn404'>@ajn404</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

### 绝对定位与transform

<iframe height="400" style="width: 100%;" scrolling="no" title="文字垂直居中2" src="https://codepen.io/ajn404/embed/gOgOqxG?height=265&theme-id=light&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/ajn404/pen/gOgOqxG'>文字垂直居中2</a> by ajn404
  (<a href='https://codepen.io/ajn404'>@ajn404</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>
[tranlate(50%,-50%)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/translate())

### 使用绝对定位结合margin：auto

<iframe height="400" style="width: 100%;" scrolling="no" title="文字垂直水平居中3" src="https://codepen.io/ajn404/embed/oNBNVoE?height=265&theme-id=light&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/ajn404/pen/oNBNVoE'>文字垂直水平居中3</a> by ajn404
  (<a href='https://codepen.io/ajn404'>@ajn404</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>
1. 垂直居中的元素相对于父元素绝对定位
2. top和bottom相等
3. margin：auto

### 对父元素使用padding

<iframe height="400" style="width: 100%;" scrolling="no" title="VwPYPEd" src="https://codepen.io/ajn404/embed/VwPYPEd?height=265&theme-id=light&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/ajn404/pen/VwPYPEd'>VwPYPEd</a> by ajn404
  (<a href='https://codepen.io/ajn404'>@ajn404</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

- 父元素不设置height

- 父元素padding设置为子元素的height

### 使用flex布局

<iframe height="400" style="width: 100%;" scrolling="no" title="垂直居中方法5" src="https://codepen.io/ajn404/embed/GRrpdqX?height=265&theme-id=light&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/ajn404/pen/GRrpdqX'>垂直居中方法5</a> by ajn404
  (<a href='https://codepen.io/ajn404'>@ajn404</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

[flex布局经典教程](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

```css
  display:flex;
  text-align:center;
  align-items:center;
```

