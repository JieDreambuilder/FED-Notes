## CSS3笔记
> Transition 过渡

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

> Transform 变换

* 该属性要带浏览器引擎前缀：
    * 标准下 ：不用带前缀
    * webkit : -webkit-
    * firefox : -moz-
    * opera : -o-
* `rotateX()`: 绕X轴旋转
* `rotateY()`: 绕Y轴旋转
* `translateZ()`: 绕Z轴移动（近大远小）
* `transform-style:preserve-3d;` : 建立3D场景
* `perspective` : 景深（值为像素或其他单位）
* `perspective-origin` : 景深基点（值为:left/right/top/bottom）
* `transform-origin` : 变换基点 （值为:left/right/top/bottom）

> [Example Click Here](https://github.com/JieDreambuilder/learn-note-fontend/blob/master/CSSTransformTransition.html)
