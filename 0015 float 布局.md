<a name="pnQyf"></a>
### 步骤
1. 子元素的 CSS 中加入 float: left 和 width
1. 父元素标签上加 `class='clearfix'` 属性。

​

其中 clearfix 为固定写法，写法如下：
```css
clearfix::after {
	content: '';
  display: block;
 	clear: fix; 
}
```
<a name="ria35"></a>
### IE 中存在的一些 BUG
在 IE6/7 中，margin 存在双倍 BUG；即 `margin-left: 10px` 会被认为 20px，

1. 可以通过在 margin 前加下划线解决，如
```css
.class {
	margin-left: 10px;
  _margin-left: 5px;
}
```
对于 IE6/7，`_margin-left:5px` 会被认为是正确语法，并覆盖它上面的 `margin-left: 10px`，同时由于双倍 BUG 的存在，5px 会被认作 10px，因此实现了 margin-left 为 10px 的需求。

2. 再在 CSS 中再加一行 `display: inline-block;`

例如：
```css
.class {
	margin-left: 10px;
	display: inline-block; 
}
```
​<br />
