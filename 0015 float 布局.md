<a name="pnQyf"></a>
### 用好 float 布局只需两步 
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
<a name="FnA5H"></a>
### 实战 float 布局

<br />在 Jsbin 中的实现：

1. ​[用 float 实现双栏布局](https://jsbin.com/wunukipoqu/1/edit?html,css,output)

知识点：

   - `float: right;` 实现右对齐布局。

​<br />

2. ​[用 float 实现三栏布局](https://jsbin.com/bunesirawe/1/edit?html,css,output)

知识点：

   - 当`border-box: box-sizing;`时，border 会占用宽度。因此类名为`content`的元素的实际宽度为 400px - 1px -1px。这种情况下，使用`outline`代替`border`进行调试，`outline`不会占用边框宽度。
   - 当一个元素的内容区（content）宽度为固定时，为这个元素加上`margin: 0 auto;`样式，即可实现居中对齐。但这种方法会覆盖 margin-top 与 margin-bottom，因此使用`margin-left: auto;`与`margin-right: auto;`更好一些。
> 对于 CSS 不该写的代码不要写，只写起决定作用的样式，不写多余的内容。

​<br />

3. 用 float 实现四栏布局

与三栏布局类似，只需四个 float 元素 + clearfix...<br />

4. ​[用 float 实现平均布局](https://jsbin.com/limuluhilu/2/edit?html,css,output)

知识点：

   - 使用负 margin 使 div 正好包裹住内容的 4 个元素。

对于负 margin 个人浅显的理解：<br />负 margin 其实可以理解为增加 div 宽度的加法。例如对于一个**居中**且宽度为 400px 的 div，给它一个`margin-right: -8px;`，那么这个 div 会在位置不变的情况下，右侧宽度增加 8px，总宽度为 408px。<br />因此，当内容区的 4 个元素宽为 94px，右边距为 8px 时，408px 的容器 div 正好可以容纳下这些元素，而不用换行。<br />
<br />​<br />
