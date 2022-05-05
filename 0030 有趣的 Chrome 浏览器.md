<a name="quJxC"></a>
### chrome 的线程管理
1. Chrome 中每一个扩展插件会开启一个线程，每一个标签页会开启一个线程。
1. 对于每一个标签页，会开启不同的线程，例如
   1. 渲染引擎线程
   1. JS 引擎线程
3. JS 引擎只占用一个线程，因此 JS 单线程引擎。
3. JS 中修改 CSS 或 HTML 的内容，涉及到 DOM 操作。
3. 之所以有 DOM 操作慢的传闻，是因为`单线程的 JS 引擎，需要与渲染引擎跨线程通信`，才能实现修改样式的操作。
3. 跨线程通信的工具由 Chrome 提供。
<a name="PFcl3"></a>
### JS 引擎

1. Chrome 采用的是 V8 引擎。
1. V8 引擎并不是 JS 引擎的第八代，而是作者为 **8 种不同**语言设计了引擎，JS 恰好是第 8 个。
1. V8 引擎是用 C++ 编写的。
1. 引擎主要的作用是
   1. 编译：把 JS 代码翻译为机器能执行的字节码或机器码
   1. 优化：自动地把代码改写为更高效的写法（因此所有 JS 高效写法都不必相信，引擎已经帮你处理了）
   1. 执行：执行 a. 中的字节码和机器码
   1. 垃圾回收：回收 JS 用过的内存，方便以后使用
<a name="nop1E"></a>
### JS 执行时

1. 准备工作
   1. 提供 API :  window / document /  setTimeout
   1. 这些 API **都不是** JS 自身具备的功能，**是浏览器提供的**
   1. 这些功能环境叫做 `运行环境` `runtime env`
   1. 运行环境总会在执行 JS 前，由浏览器准备好
   1. 一旦把 JS 放入页面或控制台，才会开始执行 JS

<a name="G6WyZ"></a>
### 内存图 - 瓜分内存
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22534335/1650891962338-3a136a1e-1c9e-4ef2-85f1-9539439d8d59.png#clientId=ucb45b1b2-7c6a-4&crop=0&crop=0.0112&crop=0.996&crop=1&from=paste&height=838&id=ub0d3f08d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=842&originWidth=1409&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1162154&status=done&style=none&taskId=u25fba621-cca0-4170-94c5-588e7e2d1e4&title=&width=1403)<br />浏览器需要与操作系统内的其他进程瓜分内存。由于内存瓜分涉及太多内容，这里只讨论一个标签页中的内存分配情况。

1. 每一个标签页需要占据一些内存
1. 被占据的这些内存中，又分别被三个线程瓜分渲染线程、用户界面、JS 引擎
1. JS 引擎首先会把代码放入内存，一部分用于存放代码，一部分存放环境和变量，最后一部分存放数据。
1. 数据区（红色部分）又分为栈区和堆区
1. 栈区数据顺序存放，堆区数据随机存放
> 为什么要这样设计？

为了更加高效的内存管理。分两种情况：

   1. 有栈无堆<br />	如果一个对象的数据，以顺序的方式存储在栈中。给这个对象添加新的值，会使它之后所有的内存都向后顺移，这是十分低效的。<br />而对于无顺序存储的堆，只需要再开辟一块儿空间，并把空间的地址赋给新值就可以了。
   1. 有堆无栈<br />栈使用一级缓存，存取速度很快。如果内存只有堆一种结构，对于需要频繁存取的数据，例如 JS 中的基本类型，就会在栈中存储。（仅针对**局部变量**，原因之后再讲）
<a name="cAjDT"></a>
### Stack 和 Heap 场景举例
```javascript
function test(){
  var a = 1;
  var b = a;
  var person = {
    name:"xxd",
    child:{
      name:"xxr"
    }
  };
  var person2 = person;
}

```

1. `var a = 1;`
   1. 在当前作用域中查找，是否存在变量`a`，如果没有，就会在「不知道什么区」开辟一块儿内存存放变量名 a；
   1. 在栈中存放一个数字 1，并把地址赋给 a；![](https://cdn.nlark.com/yuque/0/2022/webp/22534335/1650897304140-84927ee3-cdc9-48c3-be1b-a30ef55dab18.webp#clientId=ua082f165-2080-4&crop=0&crop=0.0958&crop=1&crop=0.8197&from=paste&height=178&id=ud4094b41&margin=%5Bobject%20Object%5D&originHeight=178&originWidth=720&originalType=url&ratio=1&rotation=0&showTitle=false&status=done&style=none&taskId=u38d7ba40-4ae1-4922-81d2-7ae45bd832e&title=&width=720)
2. `var b = a;`
   1. 在当前作用域中查找，是否存在变量`b`，如果没有，就会在「不知道什么区」开辟一块儿内存存放变量名 `b`；
   1. RHS 查询 a 的值为数字 1；再在栈中开辟一行内存，值为 1，地址赋给变量 `b`；
3. `var person = {...};`
   1. 在当前作用域中查找，是否存在变量`person`，如果没有，就会在「不知道什么区」开辟一块儿内存存放变量名 `person`；
   1. 由于值为一个对象，因此在堆中开辟一块儿内存，存放这个对象。堆内存的地址记录在栈中，把栈地址赋值给变量 `person`
4. `var person2 = person;`
   1. 在当前作用域中查找，是否存在变量`person2`，如果没有，就会在「不知道什么区」开辟一块儿内存存放变量名 `person2`；	
   1. RHS 查询 `person`的值为一个对象；再在栈中开辟一行内存，值为这个对象的地址，栈地址再赋给变量 `person2`；
<a name="GmCDZ"></a>
### 简单好记的规律

1. 数据类型就理解为两种： 对象 和 非对象；对象存地址，非对象直接存值
1. 非对象都在 stack；
1. 对象都存 heap；
1. `=`号只会简单的把右边的内容复制到左边去

