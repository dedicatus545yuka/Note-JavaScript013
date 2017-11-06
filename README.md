# Note-JavaScript013
事件
## 事件  
### 事件对象   
#### 获取鼠标坐标
- 当HTML页面中标签绑定的事件被触发时，我们还可以通过Event事件对象获取鼠标当前的坐标值。
    - pageX 和 pageY: 表示鼠标在整个页面中的位置。如果页面过大（存在滚动条），部分页面可能存在可视区域之外。
    - clientX 和 clientY: 表示鼠标在整个可视区域中的位置。
    - screenX 和 screenY: 表示鼠标在整个屏幕中的位置。从屏幕（不是浏览器）的左上角开始计算。
    - offsetX 和 offsetY: 表示鼠标相对于定位父元素的位置。
![offsetX和offsetY属性](http://a3.qpic.cn/psb?/V118JuTr0BKcy7/HRmT57ASGn*xm57pHBFOps1xdw77vHYpzRpmNlj2oUU!/m/dPIAAAAAAAAAnull&bo=GwWAAgAAAAADB74!&rf=photolist&t=5)    

#### 鼠标的相对坐标 
```html 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>鼠标的相对坐标值</title>
    <style>
        body {
            margin: 0px;
        }
        #show {
            width: 300px;
            height: 300px;
            background-color: lightcoral;

            position: absolute;
            top: 100px;
            left: 200px;
        }

    </style>
</head>
<body>
<div id="show"></div>
<script>
    var show = document.getElementById('show');
    /*
        鼠标相对页面中某个具体元素时
        * mouseover -> 鼠标悬停在指定元素范围中
          * 只被触发一次
        * mousemove -> 鼠标跟随事件
          * 鼠标在指定元素范围中，移动就会被触发(被触发多次)
        * mouseout -> 鼠标离开指定元素的范围
          * 只被触发一次
      */
    show.addEventListener('mouseover',function(){
        console.log(event);
    });
    show.addEventListener('mousemove',function(event){
        // console.log('yyy');

        var pageX = event.pageX;
        var pageY = event.pageY;

        var offsetX = event.offsetX;
        var offsetY = event.offsetY;

        var style = window.getComputedStyle(show,null);
        var top = parseInt(style.top);
        var left = parseInt(style.left);

        var x = pageX - left;
        var y = pageY - top;

        console.log('offsetX = '+offsetX+',offsetY = '+offsetY,'X = '+x+',Y = '+y);

    });
    show.addEventListener('mouseout',function(){
        console.log('zzz');
    });

</script>
</body>
</html>
```
### 事件周期  
- 根据 W3C 标准事件的发生流程可以分为捕获阶段、触发阶段以及冒泡阶段。
    - 捕获阶段: 事件根据 DOM 树结构从最上层节点向下传播，直到绑定该事件节点为止。
    - 触发阶段: 事件发生，执行对应的处理函数的逻辑代码。
    - 冒泡阶段: 事件根据 DOM 树结构从绑定事件节点向上传播。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>事件周期</title>
    <style>
        #d1 {
            width: 300px;
            height: 300px;
            padding: 50px;
            background-color: lightslategray;

            position: relative;
        }
        #d2 {
            width: 200px;
            height: 200px;
            padding: 50px;
            background-color: lightseagreen;

            position: relative;
        }
        #d3 {
            width: 100px;
            height: 100px;
            padding: 50px;
            background-color: lightcoral;
        }
    </style>
</head>
<body>
<div id="d1">
    <div id="d2">
        <div id="d3"></div>
    </div>
</div>
<script>
    var d1 = document.getElementById('d1');
    var d2 = document.getElementById('d2');
    var d3 = document.getElementById('d3');

    /*
        addEventListener()方法的第三个参数
        * true - 捕获阶段，依次向里传递
        * false - 默认值，冒泡阶段，依次向外传递
     */
    d1.addEventListener('click',function(){
        alert('这是d1元素')
    },true);
    d2.addEventListener('click',function(){
        alert('这是d2元素')
    },true);
    d3.addEventListener('click',function(){
        alert('这是d3元素')
    },true);
</script>
</body>
</html>
```
![事件周期](http://a3.qpic.cn/psb?/V118JuTr0BKcy7/nCUbPKFqU.UwTdeYF4ojDeTbimkXkfIthAxu*4DdPUg!/m/dGoBAAAAAAAAnull&bo=GwWAAgAAAAADB74!&rf=photolist&t=5)    

> 值得注意的是: IE 8 及之前版本的浏览器不支持捕获阶段。   

#### 阻止冒泡 
- 只触发当前节点的事件，而不继续向上冒泡，我们可以通过 Event 事件对象提供的属性来完成:
    - IE 8 及之前版本的浏览器: cancelBubble 属性
    - IE 9 及之后版本和其他浏览器: stopPropagation() 方法  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>阻止冒泡</title>
    <style>
        #d1 {
            width: 300px;
            height: 300px;
            padding: 50px;
            background-color: lightslategray;

            position: relative;
        }
        #d2 {
            width: 200px;
            height: 200px;
            padding: 50px;
            background-color: lightseagreen;

            position: relative;
        }
        #d3 {
            width: 100px;
            height: 100px;
            padding: 50px;
            background-color: lightcoral;
        }
    </style>
</head>
<body>
<div id="d1">
    <div id="d2">
        <div id="d3"></div>
    </div>
</div>
<script>
    var d1 = document.getElementById('d1');
    var d2 = document.getElementById('d2');
    var d3 = document.getElementById('d3');

    /*
        addEventListener()方法的第三个参数
        * 值为true或false，捕获阶段或冒泡阶段
        * 在当前方法中，没有办法直接既不使用捕获，也不使用冒泡
        W3C组织提供的解决方案
        * 建议调用addEventListener()方法时 -> 使用冒泡阶段
          * 默认值就是冒泡阶段
        * 通过Event事件对象，将冒泡取消
     */
    d1.addEventListener('click',function(){
        alert('这是d1元素')
    },false);
    d2.addEventListener('click',function(event){
        alert('这是d2元素');
        event.stopPropagation();
    },false);
    d3.addEventListener('click',function(){
        alert('这是d3元素')
    },false);
</script>
</body>
</html>
```
### 事件委托  
> 将事件绑定其祖先元素的方式，我们可以称之为事件委托

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>事件委托</title>
</head>
<body>
<button id="add">添加</button>
<ul id="phone">
    <li><a href="#">苹果</a></li>
    <li><a href="#">小米</a></li>
    <li><a href="#">锤子</a></li>
</ul>
<script>
    /*var aElems = document.getElementsByTagName('a');
    for (var i=0;i<aElems.length;i++){
        var aElem = aElems[i];
        aElem.addEventListener('click',function(event){
            console.log(event.target);
            event.preventDefault();
        });
    }
    // 通过添加按钮，动态向无序列表中继续添加选项
    var add = document.getElementById('add');
    var phone = document.getElementById('phone');
    add.addEventListener('click',function(){
        var newLi = document.createElement('li');
        newLi.innerHTML = '<a href="#">新的选项</a>';
        phone.appendChild(newLi);
        // 为新添加的选项，重新绑定click事件
        var a = phone.lastElementChild.firstElementChild;
        a.addEventListener('click',function(event){
            console.log(event.target);
            event.preventDefault();
        });
    });*/

    // 所有<a>标签的祖先元素中，是否存在着共同的祖先元素 -> <ul>
    var ul = document.getElementById('phone');
    ul.addEventListener('click',function(event){
        console.log('click被触发了...');
        var target = event.target;
        if (target.nodeName === 'A'){
            console.log(event.target);
            event.preventDefault();
        }
    });

    var add = document.getElementById('add');
    add.addEventListener('click',function(){
        var newLi = document.createElement('li');
        newLi.innerHTML = '<a href="#">新的选项</a>';
        phone.appendChild(newLi);
    });
</script>
</body>
</html>
```
 



