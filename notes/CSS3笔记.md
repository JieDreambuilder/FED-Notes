## CSS3笔记
> ### transition 过渡

* `transition-duration` 过渡时间
* `transition-property` 过渡的属性
    * `all`（默认）
    * `none`
    * `[attr]` 多个属性时用逗号隔开：
    ```css
    div{
        transition: 1s width,2s height;
    }
    ```
* `transition-delay` 延迟时间（第二个时间是延时时间）
    ```css
    div{
        transition: 1s .5s width,2s 1s height;
    }
    ```
* `transition-timing-function` 运动的形式
    * `ease` : （逐渐变慢）默认值
    * `linear` : 匀速
    * `ease-in` : 加速
    * `ease-out` : 减速
    * `ease-in-out` : 先加速再减速
    * `cubic-bezier` : 贝塞尔曲线（x1,y1,x2,y2）
        * [贝塞尔曲线工具](http://matthewlein.com/ceaser/)
    ```css
    div{
        transition: 3s all ease;
    }
    ```
----------------

> ### transform 变换

* 该属性要带浏览器引擎前缀：
    * 标准下 ：不用带前缀
    * webkit : -webkit-
    * firefox : -moz-
    * opera : -o-
* `rotateX()`: 绕X轴旋转
* `rotateY()`: 绕Y轴旋转
* `rotateZ()`: 绕Z轴旋转
* `translateZ()`: 绕Z轴移动（近大远小）
* `transform-style:preserve-3d;` : 建立3D场景
* `perspective` : 景深（值为像素或其他单位）
* `perspective-origin` : 景深基点（值为:left/right/top/bottom）
* `transform-origin` : 变换基点 （值为:left/right/top/bottom）

> ### 属性选择

* #### [attr]
    * 表示带有以 attr 命名的属性的元素。
* #### [attr=value]
    * 表示带有以 attr 命名的，且值为"value"的属性的元素。
* #### [attr~=value]
    *　表示带有以 attr 命名的属性的元素，并且该属性是一个以空格作为分隔的值列表，其中至少一个值为"value"。

    ```html
    <ul>
	    <li att="a b c">abc</li>
	    <li att="b c">bc</li>
	    <li att="c">c</li>
	</ul>
    ```
    
    ```css
    li[att~="b"]{background: red;}
    
    /*
     * abc,bc 被选择并颜色变红
     */
    ```
    
* #### [attr|=value]
    * 表示带有以 attr 命名的属性的元素，属性值为“value”或是以“value-”为前缀开头。
* #### [attr^=value]
    * 表示带有以 attr 命名的，且值是以"value"开头的属性的元素。
* #### [attr$=value]
    * 表示带有以 attr 命名的，且值是以"value"结尾的属性的元素。
* #### [attr\*=value]
    * 表示带有以 attr 命名的，且值包含有"value"的属性的元素。


> ### 结构伪类

* #### E:nth-child(n)
    * 表示E父元素中的第n个字节点。
    * p:nth-child(odd)：匹配奇数行。
    * p:nth-child(even)：匹配偶数行。
    * p:nth-child(3n)：匹配表达式中的计算后的行。
    * 注意
        * 参数 n 是从1开始的。
    * 存在问题：
        * 若目标元素是奇数行但不是 p 元素，则 p:nth-child(odd) 不起作用。
    * 解决方法：
        * parent * :nth-child(odd) 
        * 只匹配奇数行的子元素，不关心是什么标签。
* #### E:nth-last-child(n)
    * 表示E父元素中倒数第n个子节点。
* #### E:nth-of-type(n)
    * 表示E父元素中的第n个类型为E的子节点。
* #### E:nth-last-of-type(n)
    * 表示E父元素中倒数第n个类型为E的子节点。
* #### E:empty
    * 内容为空的子节点。
    * 注意：子节点包含文本节点（换行也算）。
* #### E:first-child
    * 表示E元素中的第一个子节点。
* #### E:last-child 
    * 表示E元素中的最后一个子节点。
* #### E:first-of-type
    * 表示E父元素中类型为E的第一个子节点。
* #### E:last-of-type
    * 表示E父元素中类型为E的最后一个子节点。
* #### E:only-child
    * 表示E父元素中只有一个子节点（不包含文本节点）。
* #### E:only-of-type
    * 表示E父元素中只有一个类型为E的子节点（不包含文本节点）。

> ### 伪类

* #### E:target 
    * 表示当前的URL片段的元素类型，这个元素必须是E

    ```css
    div{
        width:300px;
        height:200px;background:#000;
        font:50px/200px "微软雅黑";
        color:#fff;
        text-align:center;
        display:none;
    }
    div:target{
        display:block;
    }
    ```
    ```html
    <a href="#div1">div1</a>
    <a href="#div2">div2</a>
    <a href="#div3">div3</a>
    <div id="div1">div1</div>
    <div id="div2">div2</div>
    <div id="div3">div3</div>
    ```
* #### E:disabled 
    * 表示不可点击的表单控件。
* #### E:enabled 
    * 表示可点击的表单控件。
* #### E:checked 
    * 表示已选中的checkbox或radio。
* #### E:first-line 
    * 表示E元素中的第一行。
* #### E:first-letter 
    * 表示E元素中的第一个字符。
* #### E::selection
    * 表示E元素在用户选中文字时。
* #### E::before 
    * 生成内容在E元素前。
    * 通常配合 content 属性一起使用。
* #### E::after 
    * 生成内容在E元素后。
    * 通常配合 content 属性一起使用。

    ```css
    p:before{
        content:"something";
        display:block;
        border:1px solid #000;
    }
    p:after{ 
        content:"something"; 
        display:block; 
        border:1px solid #000;
    }
    ```
* #### E:not(s)
    * 表示E元素不被匹配。

    ```css
    h1:not(.h2){ 
        background:Red;
    }
    ```
    ```html
    <h1>h1</h1>
    <h1 class="h2">h1</h1>
    <h1>h1</h1>
    <!-- 第二个h1不变色 -->
    ```
* #### E~F
    * 表示E元素之后的F兄弟元素。

    ```css
    p~div{color: red;}
    ```
    ```html
    <p>前端</p>
    <div>前端开发</div>
    <div>前端开发</div>
    <p>前端</p>
    <div>前端开发</div>
    <!-- div都变红色 -->
    ```

> ### rgba()

* 新增的颜色模式
* 参数：
    * r : 红（0-255）
    * g : 绿（0-255）
    * b : 蓝（0-255）
    * a : Alpha（透明度）（0-1）
* 使用场景
    * 可以准确设定透明的对象。
        * 给color设置rgba可使文字透明。
        * 给background设置rgba可使背景透明。
        * 给border设置rgba可使边框透明。
            * 边框透明会显示背景。

> ### 文字阴影

* text-shadow：x y blur color, …
* 参数
    * x	：横向偏移
    * y	：纵向偏移
    * blur ：模糊距离
    * color	：阴影颜色（可使用rgba添加透明度）
* 嵌套多层用逗号隔开，层数太多会很卡。。很卡。。

```css
h1{ 
    font:100px/200px "微软雅黑";
    text-align:center; 
    color:#000; 
    text-shadow:0 0 0 rgba(0,0,0,1); 
    border:1px solid #000; 
    transition:1s;
}
h1:hover{
    color:rgba(0,0,0,0);
    text-shadow:0 0 100px rgba(0,0,0,0.5);
}
```
```html
<h1>Hey,man!</h1>
```

> ### 文字描边

* -webkit-text-stroke:3px red;

```css
h1{ 
    font:100px/200px "微软雅黑"; 
    text-align:center; 
    color:#000;
    -webkit-text-stroke:3px red;
}
```
```html
<h1>Hey,man!</h1>
```

> ### 文字排版

* direction
    * 参数
        * Rtl：从右向左排列
        * Ltr：从右向左排列
    * 注意要配合unicode-bidi 一块使用

```css
p{
    width:300px;
    border:1px solid #000;
    font:14px/30px "宋体";
    direction:rtl;
    unicode-bidi:bidi-override;
}
```

* Text-overflow 
    * 定义省略文本的处理方式
    * 参数
        * clip ：无省略号
        * ellipsis ：省略号（注意加上溢出隐藏和强制不换行）  
            * overflow:hidden;
            * white-space:nowrap; 

    ```css
    p{
        width:300px;
        border:1px solid #000;
        font:14px/30px "宋体";
        white-space:nowrap; 
        overflow:hidden; 
        text-overflow:ellipsis;
    }
    ```

* 自定义字体
    * [转换字体格式生成兼容代码](http://www.fontsquirrel.com/fontface/generator)
