> ### 关于this

* 方法中默认返回的this是原生的this，若要使用Jquery 的方法，需要转换成Jquery对象：$(this).

> ### html()、css()、val()、attr() 的共同特点

* css()、val()、attr()，当不带参数或只有一个参数时是取值操作，两个参数的时候是赋值操作。
* 当获取的是一个集合时，默认只获取第一个子集的值。
* html() 不带参数时是取值操作，带参数是赋值操作。

> ### 加载

* `$(function(){});` ：DOM节点加载完后执行。
* `window.onload=function(){};` ：整个页面加载完后执行。

> ### 属性选择

* $('div'[id=123]) 表示选择ID=111 的DIV元素。
* [^=]  检索开头，$('div'[id^=12]) 表示选择 ID以 12 开头 的DIV元素。
* [$=]  检索结尾，$('div'[id$=12]) 表示选择 ID 以 12 结尾的DIV元素。
* [\*=]  检索包含，$('div'[id\*=12]) 表示选择 ID中含有 12 的DIV元素。
* 引号问题： 当检索值中含有空格时必须用引号括起来，如 $('div'[class^="box box1"])

> ### 链式操作（一般用于设置属性的时候）

* `$('#div').html('aaa').css('background','red').click(function(){alert($(this).html())})`

> ### 命名规范

* 用于区分原生对象和jQuery对象。
* 原生变量默认驼峰规则。
* jQuer对象默认变量名或对象名以 $开头。如： var $userName =$('#userName')

> ### jQuery 的容错处理

* 如：当变量出现未定义就赋值的情况不会报错，不影响后续代码的继续执行。
* 缺点：难以定位出错代码位置。

> ### jQuery 获取到的都是集合

* 获取集合的长度：size() 或 .length   注意这个length没有括号
* 当 .length 为 0 的时候，说明元素没有找到，出错了。用这个来判断元素是否存在。

> ### class 的操作

* addClass() : 添加class
* removeClass() : 移除class
* toggleClass() : 有就移除，没有就添加class。

> ### 显示/隐藏

* show():显示
* hide()：隐藏
* 与 css()的区别：不用考虑目标元素的显示方式。如 block,inline

> ### 兄弟节点的操作

* prev():上一个兄弟节点。
* next(): 下一个兄弟节点。
* prevAll():之前的所有兄弟节点。
* nextAll():之后的所有兄弟节点。
* siblings():除此节点之外的所有与兄弟节点。
* 上述方法的参数可以作为检索特定选项，检索方法与$()一致。
* 可以利用参数来修复查找节点问题：
```javascript
$('input').nextAll('ul');
// 只要ul标签在input之后，无论他们之间隔着多少兄弟节点，都可以找到。
```

> ### eq() : jQuery 集合的下标

* $('div').eq(0) 第一个DIV。

> ### 子节点的操作

* first() : 查找第一个子节点。
* last() : 查找最后一个子节点。
* slice() : 截取集合
    * 第一个参数为起始位置，第二个参数为结束下标，但不包含结束下标。
    * 若只有一个参数，默认截取到末尾。
* children() : 查找子节点，参数可以用来筛选特定节点。筛选方式与$()方式一致。
* find() : 在集合中查找特定元素。
    * 与 children() 的区别：find() 可以查找到子孙节点及其以后的选项，children() 仅查找到子节点就结束。
    * 为避免繁杂的选择器操作，尽量使用find()。（因为CSS选择器都是从右往左查找节点的，如 #div ul 它会先找到所有的 ul，再寻找父节点为 #div 的 ul，性能损耗很大。）

> ### 父节点的操作

* parent() : 返回元素父节点。
* parents() : 返回元素的所有祖先节点。参数可作筛选项，但若有多个祖先节点均符合要求，则都会被选中。
* closest() : 返回一个最靠近自身的、符合要求的祖先节点（包括自身）。
    * 必须带参数，否则会报错。
    * 只返回唯一一个元素。
    * 若自身也符合，直接返回自身。

> ### 插入节点

* parent.append(child) : parent节点的里面的最后添加child元素。
* child.appendTo(parent) : 把child元素添加到parent里面的最后。
```javascript
$('ul').append($('<div>hello</div>'))
//<ul>
//  <li></li>
//  <li></li>  
//  <div>hello</div>
//</ul>
```
* parent.prepend(child) : parent节点的里面的最前面添加child元素。
* child.prependTo(parent) : 把child元素添加到parent的最前。
* old.before(newElement) : old的前面添加newElement元素。
* newElement.insertBefore(old) : 把newElement元素添加到old之前。
* old.after(newElement) : old的后面添加newElement元素。
* newElement.insertAfter(old) : 把newElement元素添加到old节点的后面。
* append 和 appendTo 、 prepend 和 prependTo 、before 和 insertBefore 、after 和 insertAfter 作用上是一样的，使用的时候要注意谁是主动、被动关系。在链式写法的时候，谁写在前面谁就是后续代码的描述对象。如：

```javascript
$('ul').insertBefore('#div').css('background','red') 
//把ul放到#div的前面，并且把ul的背景设为红色。
    
$('#div').before('ul').css('background','red') 
//在#div 的前面添加一个ul，并且把 #div 的背景设为红色。
```

> ### 删除节点

* dom.remove() :直接调用目标节点的remove()方法，目标节点即被删除。

> ### 克隆节点

* 默认的DOM操作都为剪切操作。
* 可先调用clone()方法克隆一份，再进行操作。
* clone()参数设为true的时候，可以克隆该节点的事件行为等。

> ### index()索引

* 返回其在兄弟节点中的下标。
* 当含有筛选参数的时候，返回其在筛选结果中的索引下标。
```html
<div><span></span></div>
<div><span id='sp'></span></div>
<div><span></span></div>
```
```javascript
$('#sp').index()    // 0  因为它没有兄弟节点
$('#sp').index('span')  //1 在span标签中索引为1
```

> ### 链式写法

* 要善于利用筛选参数

```javascript
<div id="tab1">
	<input type="button" name="" value="1" style="background: red;">
	<input type="button" name="" value="2">
	<input type="button" name="" value="3">
	<div class="active">1</div>
	<div>2</div>
	<div>3</div>
</div>
<script>
$('#tab1').find('input').click(function () {
		$(this).css('background','red').siblings('input').css('background','');
		$('#tab1').find('div').eq($(this).index('input')).css('display','block').siblings('div').css('display','none');
	})
</script>
```

* end() 链式后退操作

```javascript
$('div').next().css('background','red').end().css('color','blue');
```

* addBack() 、add() 后退添加链式操作

```javascript
$('div').next().css('background','red').addBack().css('color','blue');
	
$('div').add('span').css('background','red');
```

> ### each() 遍历

* 参数：一个含有两个参数的匿名函数。
* 匿名函数的参数：第一个是下标 i ，第二个是原生的目标元素 element。
    * this 问题：匿名函数中的 `this` 指向 原生的 `element` 参数。
    * 结束遍历操作：匿名函数内 `return false;`
    ```javascript
    $('li').each(function(i,element){
        if(i==3) return false;
        $(this).html(i);
    });
    
    // <li>0</li>
    // <li>1</li>
    // <li>2</li>
    // <li></li>
    ```
    
> ### JQuery 包装对象

* wrap(): 给元素包装一个父级标签

```javascript
<span></span>
<span></span>
<span></span>
$('span').wrap('<div>') 
    
// <div><span></span></div>
// <div><span></span></div>
// <div><span></span></div>
```

* wrapAll():给获取到的集合包装一个共同的父级标签：

```javascript
<span></span>
<span></span>
<span></span>
$('span').wrapAll('<div>') 

//<div>
//  <span></span>
//  <span></span>
//  <span></span>
//</div>
```

* wrapInner():给目标元素包装一个内标签。

```javascript
<span></span>
<span></span>
<span></span>
$('span').wrapInner('<div>') 

// <span><div></div></span>
// <span><div></div></span>
// <span><div></div></span>
```

* unwrap() : 消除父级包装（body标签无法被删除）。

```javascript
<div>
  <span></span>
  <span></span>
  <span></span>
</div>
$('span').unwrap()
//<span></span>
//<span></span>
//<span></span>
```

> ### get()
 
* 将JQuery 对象转换成原生JS对象。
* JQuery默认获取到的是一个集合，故get()中的参数作为集合的下标。如：get(0) 表示第一个元素。
* 参数必须有，因为只有一个元素也是一个集合。
```javascript
$('#div').get(0).scrollHeight
```

> ### 获取元素宽高

* width() : 获取或设置元素宽度。   (width)
* innerWidth() : 获取或设置元素内部宽度 (width+padding)
* outerWidth() : 获取或设置元素宽度。(width+padding+border)
* outerWidth(true) : 获取或设置元素宽度。(width+padding+border+margin)
* 高度(height)的设置和获取也同上。
* 与原生获取的区别：
    * 原生无法获取隐藏元素（display:none）的宽高。
    * JQuery 可以获取隐藏元素（display:none）的宽高。

> ### 获取可视区宽高

* $(window).height() : 获取可视区的高；
* $(window).width() : 获取可视区的宽；
* $(document).height() : 获取文档的高（包含滚动条）；
* $(document).width() : 获取文档的宽（包含滚动条）；
* 滚动条中，元素进入可视区的判断：

    ```html
    <div><img _src="img/1.jpg"></div>
    <div><img _src="img/2.jpg"></div>
    <div><img _src="img/3.jpg"></div>
    <div><img _src="img/4.jpg"></div>
    <div><img _src="img/5.jpg"></div>
    ```
    ```javascript
    toChange();
    $(window).scroll(toChange);
    function toChange(){
        $('img').each(function(i,elem{
        if( $(elem).offset().top < $(window).height() + $(document).scrollTop() ){
        		$(elem).attr('src' ,$(elem).attr('_src') );
        	}
        });
    }
    ```
    
> ### 获取滚动距离

* scrollTop() : 获取或设置元素纵向滚动距离。
* scrollLeft() :  获取或设置元素横向滚动距离。
* 常用等量关系：
    * 文档高度 - 可视区高度 = 滚动距离
    * $(document).scrollTop()==$(document).height()-$(window).height()

> ### 获取元素位置

* offset() : 获取元素距离文档的距离。
    * offset().left : 获取元素距离文档左侧距离（注意left不能加括号）。
    * offset().top : 获取元素距离文档顶部距离。
* offsetParent() : 获取有定位的祖先元素。
* posistion() : 到有定位的祖先节点的距离（不认margin）。
    * posistion().left :到有定位的祖先节点的`左侧`距离。
    * posistion().top : 到有定位的祖先节点的`顶部`距离。
* 获取有定位的祖先节点的距离：
    * $('#div2').offset().left-$('#div2').offsetParent().offset().left
    * $('#div2').offset().top-$('#div2').offsetParent().offset().top

> ### 事件绑定和取消

* JQuery 事件都是绑定形式的。如：

    ```javascript
    $(document).click(function(){
        alert(1);
    });
    $(document).click(function(){
        alert(2);
    });
    // 会弹出两次，分别为：
    // 1
    // 2
    ```
* on() : 
    * 绑定事件

        ```javascript
        $(document).on('click',function(){
            alert(1);
        });
        ```
    * 支持多事件写法
        ```javascript
        $(document).on('click mouseover',function(){
            alert(1);
        });
        ```
    * on() 绑定事件的命名空间：
        ```javascript
        $(document).on('click.abc',function(){
           alert(1);
        });
        $(document).on('click.edf',function(){
           alert(2);
        });
        // 点击屏幕，两个都会弹出。
        ```
    * 参数：
    ```javascript
    $('#div1').on('click',{name:"hello"},function(ev){
		console.log(ev.data.name);
	});
	
	$('#div1').on('click','span',{name:"hello"},function(ev){
		console.log(ev.data.name);
		//只有span标签能触发事件
		
	});
	
    ```
* off() :
    * 取消事件
    * 默认取消元素上的所有事件
    * 参数可以取消指定事件

        ```javascript
        $(document).on('click mouseover',function(){
            alert(1);
            $(this).off('mouseover')
        });
        ```
    * 通过click()等函数直接绑定的事件也可以调用该方法直接取消。
        ```javascript
        $(document).click(function(){
            alert(1);
            $(this).off()
        });
        ```
    * 可以通过先清除事件再绑定事件来避免绑定多个重复事件：
        ```javascript
        $(document).click(function(){
            $('#div').off('click').click(function(){
                alert(1);
            });
            //这样即使写在document点击事件内，每次都先清除再绑定，始终只绑定了一个事件。
        });
        ```
    * 可以通过清除指定命名空间的事件函数：
        ```javascript
        $(document).on('click.abc',function(){
           alert(1);
        });
        $(document).on('click.edf',function(){
           alert(2);
        });
        $(document).off('.abc');  //只弹出2
        ```
        
> ### 事件函数参数

* e.pageX : 相对于文档的鼠标X轴坐标。
* e.pageY : 相对于文档的鼠标Y轴坐标。
* e.clientX : 相对于可视区的鼠标X轴坐标。
* e.clientY : 相对于可视区的鼠标Y轴坐标。
* e.target : 事件源
* e.which : 触发事件的按钮键值。（keydown事件函数中，mousedown事件函数中可获取鼠标键值）
* e.ctrlKdy : 返回布尔值，ctrl 是否被按下。
* e.stopPropagation() : 阻止事件冒泡。
* e.stopImmediatePropagation() : 组织该节点所有事件冒泡，必须写在其他事件前才起作用。
* e.preventDefault() : 阻止浏览器默认事件。
* 在事件处理函数中 return false : 既阻止冒泡，也阻止默认事件。
* e.delegateTarget : 事件委托元素。
* e.originalEvent : 获取原生事件对象。

> ### 事件委托

* delegate() : 绑定委托事件。
    * 第一个参数为事件源
    * 第二个参数为事件类型
    * 第三个参数为事件处理函数
    * 事件处理函数中的 this 指向触发事件的元素
* undelegate() : 解除事件委托
    * 调用者必须是被委托事件的元素（e.delegateTarget）。
    * `$(e.delegateTarget).undelegate()`

> ### 主动触发事件

* trigger() : 主动触发事件，参数为触发的事件类型。
* 可以触发指定命名空间的事件：

    ```javascript
    $(document).on('click.abc',function(){
       alert(1);
    });
    $(document).on('click.edf',function(){
       alert(2);
    });
    $(document).trigger('click.abc');  
    //只弹出2
    ```
    
> ### 工具方法

* $.type() : 返回括号内参数的类型。
* $.isFunction() : 判断是不是函数。
* $.isNumberic() : 判断是不是数字。
    * 注意："123" 也会被认为是数字。
* $.isArray() : 判断是不是数组。
* $.isWindow() : 判断是不是窗口。
* $.isEmptObject() : 判断是不是空对象。
* $.isPlainObject() : 判断是不是自变量对象。如：`new Object()`、`{name:'handsome'}`
* $.extend() : 
    * 对象继承操作
    * 深拷贝操作：
        * 默认是浅拷贝
        * 深拷贝第一个参数要设为true
        * 支持多个对象的拷贝。
            * 浅拷贝从第二个参数以后的参数都拷贝给第一个参数。
            * 深拷贝从第三个参数以后的参数都拷贝给第二个参数。
        
        ```javascript
        var a={name:{age:20}};
        var b={};
        $.extend(true,b,a); 
        b.name.age=30;
        console.log(a.name.age) //20
        ```
        
* $.proxy() : 
    * 修改this的指向。
    * 与call、apply 的区别：
        * 仅改变this指向，不会调用该目标函数。
    * 用法:
        * 第一个参数为目标函数。
        * 第二个参数为this指向。
        * 第三个以后的参数作为目标函数的参数。
        
        ```javascript
        function show(x,y){
            alert(x);
            alert(y);
            alert(this);
        };
        
        //仅改变this
        $.proxy(show,window);
        
        //改变this并调用函数
        $.proxy(show,window,3,4)();
        
        //或者
        $.proxy(show,window)(3,4);
        
        //又或者
        $.proxy(show,window,3)(4);
        ```
        
* $.parseJSON() : 将对象形式的字符串转换成对象。
    * 对象形式的字符串必须是严格模式的（key，value都要带引号）。
* $.merge() : 合并数组。
* $.map() : 遍历修改数组。

    ```javascript
    var arr = ['a','b','c'];
    arr = $.map(arr,function(val,i){
    return val + i;
    });
    ```
* $.grep() : 过滤数组元素。

    ```javascript
    var arr = [1,5,3,8,2];
	
	arr = $.grep(arr,function(val,i){
		return val > 4;
	});
    ```
* $.unique() : 只是针对DOM节点的去重方法

    ```javascript
    var aDiv = $('div').get();
    
	var arr = [];
	
	for(var i=0;i<aDiv.length;i++){
		arr.push(aDiv[i]);
	}
	
	console.log(arr);
	
	arr = $.unique(arr);
    ```

* $.inArray() : 类似于 indexOf()
    ```javascript
    var arr = ['a','b','c','d'];
	
	console.log($.inArray('b',arr));  //1
    ```

* $.makeArray() : 转数组的
    ```javascript
    var aDiv = document.getElementsByTagName('div');
	
	aDiv = $.makeArray(aDiv);
	
	aDiv.push();
	
	console.log(aDiv);
    ```

* $.trim() : 去除字符串首尾空格。
* $.each() : 遍历元素，既可以遍历数组，也可以遍历对象。

```javascript
$('div').each(function(){});

var arr = ['a','b','c'];

var obj = { 'name' : 'hello' , 'age' : '20' };

$.each(obj,function(i,val){
	console.log(i);
	console.log(val);
});
```


> ### 显示和隐藏

* show() : 显示
    * 参数可以直接写运动完成的时间，单位为毫秒。
    * 参数还可以写 fast、 normal、 slow。
    * 其中fast=200ms、normal=400ms、slow=600ms
* hide() : 隐藏
    * 参数的设置同上
* toggle() :切换状态
    * 参数的设置同上

> ### 淡入和淡出

* fadeIn()　 淡入
* fadeOut()　 淡出
* fadeToggle()　 切换状态

> ### 上滑和下滑

* slideUp()　: 上滑
* slideDown()　: 下滑
* slideToggle() : 切换状态

> ### animate() 运动

* 参数的设置：
    * 第一个参数：{}，配置需要改变的属性和值。
    * 第二个参数 : 配置时间，默认是400。
    * 第三个参数：设置运动形式，只有两种 swing(默认 : 缓冲 : 慢快慢)  linear(匀速的)。
    * 第四个参数：回调函数。
* 参数的第二种设置形式：
    * 第一个参数：{}，配置需要改变的属性和值，该参数不能为一个空对象。
    * 第二个参数：{}。
        * 配置运动时间（duration）
        * 运动形式（easing）
        * 回调函数（complete）
        * step ()：
            * step 的第一个参数为定时器每一次变动的属性变化值。
            * step 的第二个参数为一个Tween对象，其中pos属性表示运动起点与运动终点的比例。
            
    ```javascript
    $('#div1').animate({
	    width : 300
	},{
	    duration : 1000,
	    easing : 'linear',
	    complete : function(){
	    	//alert(123);
	    },
	    step : function(a,b){     //可以检测我们定时器的每一次变化
	   	//console.log(a);
	    	console.log(b.pos);   //运动过程中的比例值(0~1)
        }
	});
    ```
    
* delay() : 运动的延迟
    * 参数为延迟的时间，单位为毫秒。
* 运动队列
    * 运动是按代码的编写顺序加入运动队列的，前一个运动未结束，后一个运动不会执行。

> ### 结束运动

* stop(): 只会停止当前运动。
* stop(true): 可以停止所有的运动。
* stop(true,true): 可以停止当前运动，并直接到达运动终点。
* finish(): 停止所有运动，并直接到达运动终点。
* 快速触发运动事件导致运动队列过长的问题：
* 先调用stop()清空队列后再添加运动。
`$(this).stop().animate({width:200,height:200});`

> ### 异步请求

* $.ajax(): 
    * 参数为一个对象。
    * 可设置的对象属性：
        * url : 提交请求地址。
        * data : 需要提交的数据。
        * type : 提交的方式。（默认的提交方式为get，一般都使用post）
        * success : 请求成功后的回调函数。
        * error : 请求失败的回调函数。
        * dataType : 设置返回数据类型。常用的是json。
        * async : 是否异步。（默认为异步）
        
    ```javascript
    $.ajax({
        url:'login.phh',
        data:{username:'admin',psd:'admin'},
        dataType:'json',
        success :function(data){
            console.log(data);
        },
        error : function(e){
            console.log(e);
        }
    });
    ```
    
    * $.param() : 将对象参数转为URL参数形式的字符串。
    
    ```javascript
    var obj = { "name":"hello","age":"20" };
	
	obj = $.param( obj );
	
	console.log( obj );// name=hello&age=20
    ```
    
    * serialize(): 获取并拼接表单中的name和value值。
    
    ```javascript
    var a = $('form').serialize();
    console.log( a );  //a=1&b=2&c=3
    ```
    
    * serializeArray(): 获取并拼接表单中的name和value值，拼接成对象数组。
    
    ```javascript
    var a = $('form').serializeArray(); 
    console.log( a );
    //[{ name:"a", value:"1"},{ name:"b", value:"2"}, { name:"c",value:"3"}]
    ```
    
* $.post():
    * 第一个参数为目的 URL。
    * 第二个参数为提交的数据。
    * 第三个参数为请求成功的回调函数。
    * 第四个参数为返回的数据类型。
    

    ```javascript
    $.post('doLogin.php',{username:'admin',psd:'admin'},function(data){
        console.log(data);
    },'json').error(function(e){
        console.log(e);
    });
    ```
    
* $.get():
    * 参数设置同上。
    * 使用方式同上。

* $.getJSON():

    ```javascript
    $.getJSON('data.php?callback=?',function(data){
		console.log(data);
	}).error(function(err){
		console.log(err);
	});   
    ```

* 默认配置：
    * $.ajaxSetup()
    
    ```javascript
    
    $.ajaxSetup({
	    type : 'POST'
	});
	
    ```


> ### 删除节点

* remove()
* detach() : 保留节点原有事件。

> ### 获取文本

* 获取所有文本，不含html标签。

> ### 替换节点

* replaceWidth（）
* replaceAll（）
* 区别：方法调用者不同，便于链式写法。

> ### hover事件

* 参数是两个回调函数，第一个是鼠标移入，第二个是移出。
* 调用的是mouseenter 和 mouseleave 。

> ### focusin()、focusout()

* 与focus ,  blur区别是上面二者能冒泡。

> ### one()

* 只执行一次的事件，绑定方式与on()相同。

> ### triggerHandler()

* 主动触发事件
* 与trigger()的区别：不会触发浏览器的默认事件。

> ### JQ的截止从操作

* nextUntil() ： 下一个兄弟节点，直到符合参数的筛选条件才停止。
* prevUntil() : 上一个兄弟节点，直到符合参数的筛选条件才停止。
* parentsUntil() : 上一个祖先节点，直到符合参数的筛选条件才停止。

> ### 数据缓存

* data() : 将键值对与节点绑定。
    * `$('#div').data('say','hello')`
    * `$('#div').data('say')   //hello`
    * 不会设置行间属性，不会存在内存泄露。
* prop() : 设置节点的自定义属性。
    * 设置行间属性。
    * 源码中调用 `.`和`[]` 直接设置/读取属性。
* attr() : 设置节点的属性。
    * 设置行间属性。
    * 源码中调用 `setAttribute()`和`getAttribute()` 设置/读取属性。
* 删除：
    * removeData()
    * removeProp()
    * removeAttr()

> ### JSON形式的设置

* on()、css()、attr()

    ```javascript
    $('div').on({
        click:function(){
            //do something
        },
        mouseover:function(){
            //do something
        }
    });
    
    $('div').css({
        width:'100px',
        height:'100px',
        background:'red'
    });
    
    $('div').attr({
        title:'title',
        href:'http://127.0.0.1'
    });
    ```

> ### 回调形式的设置

* addClass()、html()、val()
* 原理：函数的返回值作为参数

```javascript
$('div').addClass(function(i,oldClass){
	console.log(i);
	console.log(oldClass);
	return 'box'+(i+1);
});
```

> ### 防止库之间冲突

* $. noConflict()

```javascript
var J = $.noConflict();                  

J('#div').css('background','red');

J.trim('   hello   ');
```

> ### 队列

* $.queue() : 入队
* $.dequeue() : 出队
* 入队的元素只能是函数。
* 参数：
    * 绑定队列的元素
    * 队列的名字
    * 入队的函数
* 每次出队操作都会执行一次出队的函数。


```javascript
function a(){
	alert(1);
}

function b(){
	alert(2);
}

function c(){
	alert(3);
}*/

$.queue( document , 'miaov' , a );
$.queue( document , 'miaov' , b );
$.queue( document , 'miaov' , c );

$.dequeue( document , 'miaov' );
$.dequeue( document , 'miaov' );
$.dequeue( document , 'miaov' );
```
* 原生中默认队列名为 `fx`
* 还可以这样调用：`$('div').queue()`、`$('div').dequeue()`

```javascript
$('div').animate({width : 200});
//$('div').delay(2000);
$('div').queue('fx',function(){
	setTimeout(function(){
		$('div').dequeue();
		//若没有出队操作，之后的队列元素都不会执行。
		//（跟在售票窗买了票还待在窗口前不走一样，导致后面的人不能买票）
	},2000);
});
$('div').animate({height : 200});
$('div').animate({left : 200});

```

> ### Callbacks 回调对象

* 参数：
    * `$.Callbacks('once');` : 只触发一次。
    * `$.Callbacks('memory');` : 写在 fire() 后面的 add() 也能被添加。
    * `$.Callbacks('unique');` : 去除重复的项。
    * `$.Callbacks('stopOnFalse');` : 函数中 return false 就停止后续执行。
    * 可以混合使用，参数用空格隔开：`$.Callbacks('once unique');`
* add() : 添加
* remove() : 移除
* fire() : 触发


```javascript
function a(){
	alert(1);
}
function b(){
	alert(2);
}
function c(){
	alert(3);
}

var cb = $.Callbacks();

cb.add(a);
cb.add(b);
cb.fire();  //1,2
cb.add(c);
cb.remove(a);
cb.fire();  //2,3

```

* 使用场景：

```javascript
var cb = $.Callbacks();
	
function a(){
	alert(1);
}
(function(){
    function b(){
    	alert(2);
    }
    cb.add(a);
    cb.add(b);
})();

cb.fire();  //1,2

```

> ### $.Deferred() 延迟对象

* 基于Callbacks的衍生对象。
* deferred.done(): 添加事件处理函数，当事件状态为`resolved`时，里面的处理函数会被执行。
* deferred.fail(): 添加事件处理函数，当事件状态为`rejected`时，里面的处理函数会被执行。
* deferred.isRejected() :是否调用过 reject()。
* deferred.isResolved() : 是否调用过 resolve()。

```javascript
var dfd = $.Deferred();

setTimeout(function(){
	
	alert(1);
	//dfd.resolve();   1,2
	dfd.reject();    // 1,3
	
},1000);

dfd.done(function(){
	alert(2);
});
dfd.fail(function(){
	alert(3);
});
```

```javascript
$.ajax('xxx.php').done(function(){}).fail(function(){});
```

> ### 插件的编写

* $.fn.extend() : 扩展静态方法。
* $.extend() : 扩展实例方法。
* 基本格式：
    * 配置参数
    * 方法
    * 自定义事件
* [一个TAB选项卡插件例子](https://github.com/JieDreambuilder/learn-note-fontend/blob/master/examples/JQ-TABS-Extension.html)