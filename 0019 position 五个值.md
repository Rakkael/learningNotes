<a name="cal8R"></a>
### static
position 的默认值为 static。<br />含义：当前元素在文档流中。
<a name="Mkl4U"></a>
### relative
含义：元素仍然在文档流中，但视觉效果有一定的偏移。<br />即实际位置还在原位，只是看起来偏移了。

1. 使用场景
- 用于实现位移。但有其他更好属性实现位移，目前很少使用。
- 配合 absolute 使用。
2. 配合 z-index 使用

z-index 在使用时需要这个元素有 `position:relative;` 属性。
> 注意：index 不要写 9999。

<a name="hyC9r"></a>
### absolute
含义：脱离文档，另起一层。以祖先元素里，**最近**的一个 **position 不是 static 的元素**进行定位。<br />`white-space: nowarp;` 文字不换行。<br />注意：

- 使用`position: absolute;`定位的元素，一定要写上`top/left/bottom/right`属性中的任意**两个**。

例如：
```css
.item{
	position: absolute;
  top: 0;
  right: 0;
}
```
在一些浏览器中，缺失 top/right 会导致定位出错。

- 善用 100%

`left:100%;`可以将元素定位至最右侧。

- 善用 `left: 50%`+负`margin`

实现元素居中显示。也可以用 `transform: translateX(-50%);`替换负 margin，不需要自行计算 margin 的偏移值。
<a name="CkyHr"></a>
### fixed
含义：脱离文档流，以当前视口进行定位。

1. 使用场景
- 广告
- 返回顶部按钮
- 配合 z-index
> 手机上尽量不要使用这个属性，bug 很多。

注意：某些新属性，例如 transform 会导致它不以视口定位，而出现 bug，语义不够正交。<br />例如：[https://jsbin.com/qiqalavofa/1/edit?html,css,output](https://jsbin.com/qiqalavofa/1/edit?html,css,output)
```html
<div class="container">
	<div class="fixed"></div>
</div>
```
```css
*{
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
.container{
	width: 100vw;
	height:50vh;
	border: 1px red solid;
}
.fixed{
	border: 1px gray solid;
	width: 50px;
	height: 50px;
	background-color: gray;
	position: fixed;
	left:10px;
	bottom:10px;
}
.container{
	transform: scale(0.9);  /* transform 属性会使 fixed 相对包裹它的第一个 transform 属性生效的元素进行定位 */
}
```
<a name="fMPCa"></a>
### sticky
粘滞定位。使一个元素即将超出视口时，会粘滞在视口的边缘。<br />兼容性极差，基本上不用。
