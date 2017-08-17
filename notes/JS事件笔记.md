## 窗口事件
  * 可视区的尺寸
  * `document.documentElement.clientHeight`
      * 可视区的高
  * `document.documentElenemt.clinetWidth`
      * 可视区的宽
  * Chrome浏览器会认为滚动条在body元素下，兼容如下：
  ```javascript
  var scrollTop= document.documentEvent.scrollTop || document.body.scrollTop;
  ```
  -------------------
  * onresize
      * 当窗口大小发生变化时触发
  * onscroll
      * 当滚动条移动时触发
      * `Element.scrollHeight` 是计量元素内容高度的只读属性
      * `Element.scrollWidth` 是计量元素内容宽度的只读属性
      * `Element.scrollTop`  滚动条拖动的高度
      * `Element.scrollLeft`  滚动条拖动的宽度
      * 监听这个事件可以用来判断用户是否阅读过文档
        * `Element.scrollTop`+`Element.clientHeight`=`Element.scrollHeight`
        * 具体代码如下：
        ```javascript
        function checkReading () {
          if (checkReading.read) {
            return;
          }
          checkReading.read = this.scrollHeight - this.scrollTop === this.clientHeight;
          return checkReading.read;
        }
        //调用：
        Element.onscroll = checkReading;
        checkReading.call(oToBeRead);
        ```
      * 还可以做全屏无缝滚动
----------------------
## 焦点
> 使浏览器能够区分用户输入的对象，当一个元素有焦点的时候，那么他就可以接收用户的输入。我们可以通过一些方式给元素设置焦点

* 点击
* tab
* js

```javascript
  var oText = document.getElementById('text1');
  //onfocus : 当元素获取到焦点的时候触发
  oText.onfocus = function() {
    if ( this.value == '请输入内容' ) {
      this.value = '';
    }
  }

  //onblur : 当元素失去焦点的时候触发
  oText.onblur = function() {
    if ( this.value == '' ) {
      this.value = '请输入内容';
    }
  }
```
* 不是所有元素都能够接收焦点的.`能够响应用户操作的元素才有焦点`
  * `obj.focus()` 给指定的元素设置焦点
  * `obj.blur()` 取消指定元素的焦点
  * `obj.select()` 选择指定元素里面的文本内容
----------------------------------------
## Event对象
> 事件对象 , 当一个事件发生的时候，和当前这个对象发生的这个事件有关的一些详细的信息都会被临时保存到一个指定地方-event对象，供我们在需要的调用。类似于飞机-黑匣子

* 事件对象必须在一个事件调用的函数里面使用才有内容
* `事件函数`：被事件调用的函数。如果一个函数是被事件调用的，那么这个函数定义的`第一个参数`就是事件对象
* 兼容性：
    * `ie/chrome` : event是一个内置全局对象。
    * `标准下` : 事件对象是通过`事件函数`的第一个参数传入
* 兼容方式：`ev=ev||event;`
* IE8及其以下不支持部分属性，如`target`
* `ev.clientX[Y]` : 当一个事件发生的时候，鼠标到页面`可视区`的距离
* 做`全屏鼠标事件`的时候注意存在滚动条的情况，需要加上`scrollTop/scrollHeight`。
```javascript
var oDiv=document.getElementById('s1');
  document.onmousemove=function(ev){
  	ev=ev||event;
  	var l=ev.clientX-oDiv.clientWidth/2;
  	var t=ev.clientY-oDiv.clientHeight/2;
  	var scrollTop=document.documentElement.scrollTop||document.body.scrollTop;
  	var scrollLeft=document.documentElement.scrollLeft||document.body.scrollLeft;
  	if(l<0) l=0;
  	if(t<0) t=0;
  	if(l>document.documentElement.clientWidth-oDiv.clientWidth) l=document.documentElement.clientWidth-oDiv.clientWidth;
  	if(t>document.documentElement.clientHeight-oDiv.clientHeight) t=document.documentElement.clientHeight-oDiv.clientHeight;
  	oDiv.style.left=l+scrollLeft+'px';
  	oDiv.style.top=t+scrollTop+'px';
  }
```

| 属性/方法 | 类型 | 读/写 | 说明 |
| ------ | ------ | ------ | ------ |
| bubbles | Boolean | 只读 | 事件是否冒泡 |
| cancelable |	Boolean	| 只读	| 是否可以取消事件的默认行为 |
|currentTarget|	Element|	只读|	事件处理程序当前处理元素|
|detail|	Integer|	只读|	与事件相关细节信息|
|eventPhase|	Integer	|只读|	事件处理程序阶段：1 捕获阶段，2 处于目标阶段，3 冒泡阶段|
|preventDefault()|	Function|	只读|	取消事件默认行为|
|stopPropagation()|	Function|	只读|	取消事件进一步捕获或冒泡|
|target|	Element	|只读|	事件的目标元素|
|type|	String	|只读|	被触发的事件类型|
|view|	AbstractView| 只读|与事件关联的抽象视图，等同于发生事件的window对象|

> IE8及其以下不支持部分属性，如`target`，需要替换成`srcElement`

-----------------------------------------

## 事件冒泡
> 当一个元素接收到事件的时候，会把他接收到的所有传播给他的父级，一直到顶层window。叫做事件冒泡机制。

* 阻止冒泡：
  * IE8及以下，在当前要阻止冒泡的事件函数中调用 `event.cancelBubble = true;`
  * 标准下，`ev.stopPropagation()`。
  * 只能阻止`当前对象`的`当前事件`的冒泡
  * 标准下阻止同名事件的所有冒泡：`ev.stopImmediatePropagation()`
  * 兼容：
  ```javascript
    if( ev.stopPropagation ){
        ev.stopPropagation();
    }
    else{
        ev.cancelBubble = true;
    }
  ```
* 利用事件冒泡机制可以少些很多代码(`将事件处理程序交给父级`)：
* `safari`浏览器下，`document` `body` 元素无法触发`click`事件，所以要事件委托函数不能绑到他们身上。
```html
    <div id="div1">
    	<div id="div2">分享到</div>
    </div>
```
```javascript
var oDiv = document.getElementById('div1');
oDiv.onmouseover = function() {
	this.style.left = '0px';
}
oDiv.onmouseout = function() {
	this.style.left = '-100px';
}
```
------------------------------
## call、apply
* 相同点：都是用于函数调用、传参、传递`this`
* 不同点：
  * call 的第二个参数开始，即为原函数的第一个参数
  * apply 的第二个参数是一个`数组`，作为原函数的 `arguments`
* 注意： 当第一个参数为`null`时，call、apply对原函数中的this`不做任何处理`
---------------------------
## 事件绑定
* 给标签属性指定事件处理函数:   `<a onclick='alert(1)'>click</a>`
* 通过赋值的形式绑定处理函数:  `document.onclick=function(){alert(1)}`
* 通过绑定事件处理函数：
  * IE下：`obj.attachEvent('onclick',fn1)`
    * 1.没有捕获
    * 2.事件名称有on
    * 3.事件函数执行的顺序：标准ie->正序，非标准ie->倒序
    * 4.this指向window
  * 标准下：`obj.addEventListener('click','fn1',false)`
    * 1.有捕获
      * 第三个参数为`true`的时候在`捕获`的时候执行事件函数
      * 第三个参数为`false`的时候在`冒泡`的时候执行事件函数
    * 2.事件名称没有on
    * 3.事件执行的顺序是正序
    * 4.this触发该事件的对象
* 兼容处理：
  ```javascript
  function bind(obj, evname, fn) {
  	if (obj.addEventListener) {
  		obj.addEventListener(evname, fn, false);
  	} else {
  		obj.attachEvent('on' + evname, function() {
  			fn.call(obj);
  		});
  	}
  }
  ```
>存在问题：因为IE下绑定的是匿名函数，故无法移除。

>JQuery创始人John Resig是这样做的:

```javascript
function addEvent(node, type, handler) {
if (!node) return false;
if (node.addEventListener) {
    node.addEventListener(type, handler, false);
    return true;
}
else if (node.attachEvent) {
    node['e' + type + handler] = handler;
    node[type + handler] = function() {
        node['e' + type + handler](window.event);
    };
    node.attachEvent('on' + type, node[type + handler]);
    return true;
}
return false;
}
```
----------------------------
## 事件取消
* 标准下：`obj.removeEventListener(eventType,functionName,false)`
    * 第三个参数为`true`时，取消`捕获`时期的事件处理函数。
* 低版本IE：`obj.detachEvent(eventType,functionName)`
    * 低版本IE不支持事件捕获
* 二者都无法取消匿名的事件处理函数
* 跨浏览器兼容：
    ```javascript
    function removeEvent(node, type, handler) {
        if (!node) return false;
        if (node.removeEventListener) {
            node.removeEventListener(type, handler, false);
            return true;
        }
        else if (node.detachEvent) {
            node.detachEvent('on' + type, node[type + handler]);
            node[type + handler] = null;
        }
        return false;
    }
    ```
------------------------------
## 键盘事件
### `onkeydown` 
* 当键盘按键`按下`的时候触发
* 如果按下不抬起，那么会连续触发
### `onkeyup`: 
* 当键盘按键`抬起`的时候触发
### `event.keyCode` :
* 是一个数字类型的键盘按键的`ASCII`值
* `ev.ctrlKey`,`ev.shiftKey`,`ev.altKey` 是`ev`下的布尔值属性。
    * 如果`ctrl` || `shift` || `alt` 是按下的状态，返回true，否则返回false。
> 在做键盘事件的时候，一般在`按键抬起`的时候做事件处理，确保按键事件已经发生（如改变`input`表单的`value`值：`onkeydown`-->`value`值改变-->`onkeyup`）

> 解决`onkeydown`连续触发时，第二次触发的延时响应问题：定时器和键盘事件结合。用`onkeydown`和`onkeyup`来开启、关闭定时器执行重复的操作。

```javascript
var oDiv=document.createElement("div");
document.body.appendChild(oDiv);
oDiv.id='div1';
oDiv.style.width='100px';
oDiv.style.height='100px';
oDiv.style.background='red';
oDiv.style.position='absolute';
document.onkeydown=function(ev){
	var e=ev||event;
	switch(e.keyCode){
		case 37 :
		clearInterval(oDiv.timmer);
		oDiv.timmer=setInterval(function(){
			oDiv.style.left=oDiv.offsetLeft-10 +'px';
		},10);
		break;
		case 38 :
		clearInterval(oDiv.timmer);
		oDiv.timmer=setInterval(function(){
			oDiv.style.top=oDiv.offsetTop-10 +'px';
		},10);
		break;
		case 39 :
		clearInterval(oDiv.timmer);
		oDiv.timmer=setInterval(function(){
			oDiv.style.left=oDiv.offsetLeft+10 +'px';
		},10);
		break;
		case 40 :
		clearInterval(oDiv.timmer);
		oDiv.timmer=setInterval(function(){
			oDiv.style.top=oDiv.offsetTop+10 +'px';
		},10);
		break;
	}
	
}
document.onkeyup=function(ev){
	var e=ev||event;
	if(e.keyCode==37||e.keyCode==38||e.keyCode==39||e.keyCode==40){
		clearInterval(oDiv.timmer);
	}
}
```

> 不是所有元素都能够接收键盘事件，能够响应用户输入的元素，能够接收焦点的元素才能够接收键盘事件。通常在做全局动画时，把键盘事件触发放在`document`元素身上，页面打开时`document`默认获得焦点。

------------------------

## 默认事件
> 当一个事件发生的时候浏览器自己会默认做的事情

> 阻止默认事件：

* 事件处理函数中`return false;`：            标准下只能阻止`on`系列事件的默认行为，不能阻止addEventListener()绑定的事件的默认行为。非标准下（IE8及以下）对`attachEvent()`或`obj.event=function(){}`都有效。
* 调用`ev.preventDefault()`：
    * IE8及以下会报错，需要判断后使用:
    ```javascript
    if(ev.preventDefault()){
        ev.preventDefault();
    }
    ```

> 用`addEventListener(`绑定的事件必须使用`ev.preventDefault()`才能阻止浏览器默认行为。

-----------------------

## 右键菜单oncontextmenu
> 右键菜单事件，当右键菜单（环境菜单）显示出来的时候触发。

## 拖拽的原生实现（`onmousedown`+`onmousemove`+`onmouseup`）

>解决鼠标移动太快，移出目标导致无法触发目标的`onmouseup`事件：将`onmousemove`绑定在`document`对象身上。

>解决因目标元素被另一层级DIV覆盖而无法触发目标的`onmouseup`事件：将`onmouseup`绑定在`document`对象身上。

>解决IE8及以下无法阻止浏览器默认拖拽事件的问题：`element.setCapture` , `element.releaseCapture`

```javascript
var oDiv=document.getElementById('s1');
oDiv.onmousedown=function(e){
	var ev=e||event;
	this.move=true;
	
	//用全局捕获阻止IE8及以下浏览器默认拖拽行为
	if ( oDiv.setCapture ) {
		oDiv.setCapture();
	}
	return false;
}
document.onmousemove=function(e){
	var ev=e||event;
	var l=document.documentElement.scrollLeft||document.body.scrollLeft;
	var t=document.documentElement.scrollTop||document.body.scrollTop;
	if(oDiv.move){
		oDiv.style.left=l+ev.clientX-oDiv.clientWidth/2+'px';
		oDiv.style.top=t+ev.clientY-oDiv.clientHeight/2+'px';
	}
}
document.onmouseup=function(e){
	var ev=e||event;
	oDiv.move=false;
	//释放全局捕获 releaseCapture();
	if ( oDiv.releaseCapture ) {
		oDiv.releaseCapture();
	}
	return false;
}
```
------------------------------
## 鼠标滚轮事件

> 在元素上鼠标滚轮滑动或单击触发的事件


> 兼容性

* ie/chrome : onmousewheel
    * `event.wheelDelta`
        * 上：120
		* 下：-120
		
* firefox : `DOMMouseScroll` 必须用`addEventListener`
	* event.detail
		* 上：-3
		* 下：3
* 兼容处理：
    ```javascript
    var b = true;
	if (ev.wheelDelta) {
		b = ev.wheelDelta > 0 ? true : false;
	} else {
		b = ev.detail < 0 ? true : false;
	}
    ```
## 事件的更深入学习：

> 阮一峰博客：[事件模型](http://javascript.ruanyifeng.com/dom/event.html)、[事件种类](http://javascript.ruanyifeng.com/dom/event-type.html)
