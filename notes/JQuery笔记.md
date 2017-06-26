* 关于this
    * 方法中默认返回的this是原生的this，若要使用Jquery 的方法，需要转换成Jquery对象：$(this).
* html()、css()、val()、attr() 的共同特点：
    * css()、val()、attr()，当不带参数或只有一个参数时是取值操作，两个参数的时候是赋值操作。
    * 当获取的是一个集合时，默认只获取第一个子集的值。
    * html() 不带参数时是取值操作，带参数是赋值操作。
* 加载：
    * $(function(){ // }); 的功能与 window.onload=function(){} 类似
* 属性选择：
    * $('div'[id=123]) 表示选择ID=111 的DIV元素。
    * [^=]  检索开头，$('div'[id^=12]) 表示选择 ID以 12 开头 的DIV元素。
    * [$=]  检索结尾，$('div'[id$=12]) 表示选择 ID 以 12 结尾的DIV元素。
    * [\*=]  检索包含，$('div'[id\*=12]) 表示选择 ID中含有 12 的DIV元素。
    * 引号问题： 当检索值中含有空格时必须用引号括起来，如 $('div'[class^="box box1"])
* 链式操作（一般用于设置属性的时候）：
    * $('#div').html('aaa').css('background','red').click(function(){alert($(this).html())})
* 命名规范
    * 用于区分原生对象和jQuery对象。
    * 原生变量默认驼峰规则。
    * jQuer对象默认变量名或对象名以 $开头。如： var $userName =$('#userName')
* jQuery 的容错处理：
    * 如：当变量出现未定义就赋值的情况不会报错，不影响后续代码的继续执行。
    * 缺点：难以定位出错代码位置。
* jQuery 获取到的都是集合
    * 获取集合的长度：size() 或 .length   注意这个length没有括号
    * 当 .length 为 0 的时候，说明元素没有找到，出错了。用这个来判断元素是否存在。
* class 的操作:
    * addClass() : 添加class
    * removeClass() : 移除class
    * toggleClass() : 有就移除，没有就添加class。
* 显示/隐藏：
    * show():显示
    * hide()：隐藏
    * 与 css()的区别：不用考虑目标元素的显示方式。如 block,inline
* 兄弟节点的操作：
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
* eq() : jQuery 集合的下标
    * $('div').eq(0) 第一个DIV。
* 子节点的操作：
    * first() : 查找第一个子节点。
    * last() : 查找最后一个子节点。
    * slice() : 截取集合
        * 第一个参数为起始位置，第二个参数为结束下标，但不包含结束下标。
        * 若只有一个参数，默认截取到末尾。
    * children() : 查找子节点，参数可以用来筛选特定节点。筛选方式与$()方式一致。
    * find() : 在集合中查找特定元素。
        * 与 children() 的区别：find() 可以查找到子孙节点及其以后的选项，children() 仅查找到子节点就结束。
        * 为避免繁杂的选择器操作，尽量使用find()。（因为CSS选择器都是从右往左查找节点的，如 #div ul 它会先找到所有的 ul，再寻找父节点为 #div 的 ul，性能损耗很大。）
* 父节点的操作：
    * parent() : 返回元素父节点。
    * parents() : 返回元素的所有祖先节点。参数可作筛选项，但若有多个祖先节点均符合要求，则都会被选中。
    * closest() : 返回一个最靠近自身的、符合要求的祖先节点（包括自身）。
        * 必须带参数，否则会报错。
        * 只返回唯一一个元素。
        * 若自身也符合，直接返回自身。
* 插入节点：
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
* 删除节点
    * dom.remove()    直接调用目标节点的remove()方法，目标节点即被删除。
* 克隆节点
    * 默认的DOM操作都为剪切操作。
    * 可先调用clone()方法克隆一份，再进行操作。
    * clone()参数设为true的时候，可以克隆该节点的事件行为等。
* index()索引：
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
* 链式写法：
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
* each() 遍历
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
* JQuery 包装对象：
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
* get():
    * 将JQuery 对象转换成原生JS对象。
    * JQuery默认获取到的是一个集合，故get()中的参数作为集合的下标。如：get(0) 表示第一个元素。