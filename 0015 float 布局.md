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
在 IE6/7 中，margin 存在双倍 BUG；即 `margin-left: 10px` 会被认为 20px。

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
<a name="Uk3j4"></a>
### 居中
对于一个块元素，如果左边 margin 为 auto，右边 margin 也为 auto，那么这个块元素将会居中。
```css
/* 可能会覆盖 margin-top，margin-bottom */
.block {
	margin: 0 auto;
}

/* 不会覆盖 margin-top，margin-bottom */
.block {
	margin-left: auto;
 	margin-right: auto;
}
```
在 Jsbin 中的实现：

1. ​[用 float 实现双栏布局](https://jsbin.com/wunukipoqu/1/edit?html,css,output)

知识点：

- `float: right;` 实现右对齐布局。
2. ​[用 float 实现三栏布局](https://jsbin.com/bunesirawe/1/edit?html,css,output)

知识点：

- 当`border-box: box-sizing;`时，border 会占用宽度。因此类名为`content`的元素的实际宽度为 400px - 1px -1px。这种情况下，使用`outline`代替`border`进行调试，`outline`不会占用边框宽度。
3. 用 float 实现四栏布局



4. 用 float 实现平均布局

​

​<br />
