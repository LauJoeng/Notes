**HTML标签学习**

- hr 标签添加分割线

- ul和li标签组成无序列表，ul即为unordered list，li即为list

	ex：
	<ul>
		<li>第一行</li>
		<li>第二行</li>
	</ul>
- ol和li组成有序列表，与ul和li相似，ol即为ordered list
- div标签划分区块，可以为div添加id
- table、tbody、tr、th都是表格标签里使用
 - tr代表一行，即table row，th 表格的头部的一个单元格，即table head，表格表头，td，表格数据，即table data,tbody是为了防止表格加载时一格一格显示

	<table>
	  <tbody>
	    <tr>
	      <th>班级</th>
	      <th>学生数</th>
	      <th>平均成绩</th>
	    </tr>
	    <tr>
	      <td>一班</td>
	      <td>30</td>
	      <td>89</td>
	    </tr>
	    <tr>
	      <td>二班</td>
	      <td>35</td>
	      <td>85</td>
	    </tr>
	    <tr>
	        <td>三班</td>
	        <td>23</td>
	        <td>55</td>
	    </tr>
	  </tbody>
	</table>
- a标签 ，超链接
  - 其中a标签里有个target属性，分别代表在什么窗口打开，_blank -- 在新窗口中打开链接 ，_parent -- 在父窗体中打开链接 ，_self -- 在当前窗体打开链接,此为默认值 ，_top -- 在当前窗体打开链接，并替换当前的整个窗体(框架页) 一个对应的框架页的名称 -- 在对应框架页中打开。

- img标签，插入图片
```
<img src="图片地址" alt="下载失败时的替换文本" title = "提示文本">
```
- form标签，表单提交
```
<form method="post" action="save.php">
      <label for="username">用户名:</label>
      <input type="text"  name="username" id="username" value="" />
      <br/>
      <label for="pass">    密码:</label>
      <input type="password"  name="pass" id="pass" value="" />    
      <input type="submit" value="确定"  name="submit" />
      <input type="reset" value="重置" name="reset" />
</form> 
```
- textarea标签，多行文本输入框，
```
<textarea  rows="行数" cols="列数">文本</textarea>
```
- 单、复选框,如下（type为radio时时单选框，checkbox为复选框）
```
<input   type="radio/checkbox"   value="值"    name="名称"   checked="checked"/>
```
- option 下拉选项框,语法如下(selected 表示默认选中,加了mutiple属性则表示是多选)
```
<select multiple="multiple">
      <option value="看书">看书</option>
      <option value="旅游">旅游</option>
      <option value="运动">运动</option>
      <option value="购物" selected="selected">购物</option>
</select>
```
- input标签根据type选项不同而不同，标签对应如下
 - text 对应文本输入框
 - button 按钮
 - submit 表单提交按钮
 - radio/checkbox  单选/复选框
 - reset 表单提交中重置表单数据

- lable标签，label标签不会向用户呈现任何特殊效果，它的作用是为鼠标用户改进了可用性。如果你在 label 标签内点击文本，就会触发此控件。就是说，当用户单击选中该label标签时，浏览器就会自动将焦点转到和标签相关的表单控件上（就自动选中和该label标签相关连的表单控件上）。
 
**css**
css语法
css 样式由选择符和声明组成，而声明又由属性和值组成，如下图所示：
![](http://img.mukewang.com/52fde5c30001b0fe03030117.jpg)
选择符：又称选择器，指明网页中要应用样式规则的元素，如本例中是网页中所有的段（p）的文字将变成蓝色，而其他的元素（如ol）不会受到影响。
声明：在英文大括号“｛｝”中的的就是声明，属性和值之间用英文冒号“：”分隔。当有多条声明时，中间可以英文分号“;”分隔，如下所示：
p{font-size:12px;color:red;}

设置css有三种方式，三种方式优先级分别是内联式 > 嵌入式 > 外部式

css选择器有标签选择器，类选择器，id选择器，三者应用如下

标签选择器
```
<!DOCTYPE HTML>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>认识html标签</title>
<style type="text/css">
h1{
    font-weight:normal;
    color:red;}<!-- 标签选择器 ->
</style>
</head>
<body>
    <h1>勇气</h1>
    <p>三年级时，我还是一个胆小如鼠的小女孩，上课从来不敢回答老师提出的问题，生怕回答错了老师会批评我。就一直没有这个勇气来回答老师提出的问题。学校举办的活动我也没勇气参加。</p>
    <p>到了三年级下学期时，我们班上了一节公开课，老师提出了一个很简单的问题，班里很多同学都举手了，甚至成绩比我差很多的，也举手了，还说着："我来，我来。"我环顾了四周，就我没有举手。</p>
    <img src="http://img.mukewang.com/52b4113500018cf102000200.jpg" >
</body>
</html>
```
类选择器
```
<!DOCTYPE HTML>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>认识html标签</title>
<style type="text/css">
.stress{
    color:red;
}
.setGreen{
    color:green;
}
</style>
</head>
<body>
    <h1>勇气</h1>
    <p>三年级时，我还是一个<span class="stress">胆小如鼠</span>的小女孩，上课从来不敢回答老师提出的问题，生怕回答错了老师会批评我。就一直没有这个<span class="stress">勇气</span>来回答老师提出的问题。学校举办的活动我也没勇气参加。</p>
    <p>到了三年级下学期时，我们班上了一节<span class="setGreen">公开课</span>，老师提出了一个很简单的问题，班里很多同学都举手了，甚至成绩比我差很多的，也举手了，还说着："我来，我来。"我环顾了四周，就我没有举手。</p>
    <img src="http://img.mukewang.com/52b4113500018cf102000200.jpg" >
</body>
</html>
```
id选择器
```
<!DOCTYPE HTML>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>认识html标签</title>
<style type="text/css">
#setGreen{
    color:green;
}
#stress{
    color:red;
}

</style>
</head>
<body>
    <h1>勇气</h1>
    <p>三年级时，我还是一个<span id="stress">胆小如鼠</span>的小女孩，上课从来不敢回答老师提出的问题，生怕回答错了老师会批评我。就一直没有这个勇气来回答老师提出的问题。学校举办的活动我也没勇气参加。</p>
    <p>到了三年级下学期时，我们班上了一节<span id="setGreen">公开课</span>，老师提出了一个很简单的问题，班里很多同学都举手了，甚至成绩比我差很多的，也举手了，还说着："我来，我来。"我环顾了四周，就我没有举手。</p>

</body>
</html>
```
伪类选择符，它允许给html不存在的标签（标签的某种状态）设置样式，比如说我们给html中一个标签元素的鼠标滑过的状态来设置字体颜色：
a:hover{color:red;}
上面一行代码就是为 a 标签鼠标滑过的状态设置字体颜色变红。


<br/><br/>
<hr/>
JavaScript

document.write("输出内容");  输出字符串

alert("弹窗显示内容");  弹窗警告

confirm("弹窗对话框，有确定和取消选项");
```
function rec(){
    var mymessage= confirm("你喜欢电影吗");
    if(mymessage==true)
    {
    	document.write("你是女士!");
    }
    else
    {
        document.write("你是男士!");
    }
  }    
```

prompt  提问消息对话框

window.open(),方法可以查找一个已经存在或者新建的浏览器窗口
语法
```
window.open([URL], [窗口名称], [参数字符串])
```
URL：可选参数，在窗口中要显示网页的网址或路径。如果省略这个参数，或者它的值是空字符串，那么窗口就不显示任何文档。
窗口名称：可选参数，被打开窗口的名称。
    1.该名称由字母、数字和下划线字符组成。
    2. "_top"、"_blank"、"_self"具有特殊意义的名称。
     _blank：在新窗口显示目标网页
       _self：在当前窗口显示目标网页
       _top：框架网页中在上部窗口中显示目标网页
    3.相同 name 的窗口只能创建一个，要想创建多个窗口则 name 不能相同。
   4.name 不能包含有空格。
参数字符串：可选参数，设置窗口参数，各参数用逗号隔开。

可以通过document.getEelementById()方法获取html页面元素
```
var mychar=document.getElementById("con")           ;
  document.write("结果:"+mychar); //输出获取的P标签。
```

innerHTML获取元素内容，语法示例
```
<h2 id="con">javascript</H2>
<p> JavaScript是一种基于对象、事件驱动的简单脚本语言，嵌入在HTML文档中，由浏览器负责解释和执行，在网页上产生动态的显示效果并实现与用户交互功能。</p>
<script type="text/javascript">
  var mychar=document.getElementById("con");           ;
  document.write("原标题:"+mychar.innerHTML+"<br>"); //输出原h2标签内容
    mychar.innerHTML="HELLO WORLD";
  document.write("修改后的标题:"+mychar.innerHTML); //输出修改后h2标签内容
</script>
```

HTML DOM 允许 JavaScript 改变 HTML 元素的样式。语法示例
```
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>style样式</title>
</head>
<body>
  <h2 id="con">I love JavaScript</H2>
  <p> JavaScript使网页显示动态效果并实现与用户交互功能。</p>
  <script type="text/javascript">
    var mychar= document.getElementById("con");
    mychar.style.color = "red";
    mychar.style.backgroundColor = "gray";
    mychar.style.width = "300px";
  </script>
</body>
</html>
```

className 属性设置或返回元素的class 属性,语法示例
```
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<title>className属性</title>
<style>
    body{ font-size:16px;}
    .one{
		border:1px solid #eee;
		width:230px;
		height:50px;
		background:#ccc;
		color:red;
    }
	.two{
		border:1px solid #ccc;
		width:230px;
		height:50px;
		background:#9CF;
		color:blue;
	}
	</style>
</head>
<body>
    <p id="p1" > JavaScript使网页显示动态效果并实现与用户交互功能。</p>
    <input type="button" value="添加样式" onclick="add()"/>
	<p id="p2" class="one">JavaScript使网页显示动态效果并实现与用户交互功能。</p>
    <input type="button" value="更改外观" onclick="modify()"/>

	<script type="text/javascript">
	   function add(){
	      var p1 = document.getElementById("p1");
	      p1.className = "one";
	   }
	   function modify(){
	      var p2 = document.getElementById("p2");
	      p2.className = "two";
	   }
	</script>
</body>
</html>
```


JavaScript中的数组的定义方式
```
var myarr=new Array();
```
可以通过.length获取数组长度，如var a = myArray.length
定义二维数组：var myarr = new Array(2);

鼠标事件
- onclick  鼠标点击
- onmouseover /onmouseout 鼠标经过/移开
- onfocus 光标聚焦
- onblur 失焦
- onselect 内容选中
- onchange 文本框内容改变
- onload  加载事件
- onunload  卸载事件

JavaScript有Date、Math、Array三个内置对象

JavaScript计时器
在执行时,从载入页面后每隔指定的时间执行代码。语法演示如下
```
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>定时器</title>
<script type="text/javascript">
  var attime;
  function clock(){
    var time=new Date();          
    attime= time.getHours() + ":" + time.getMinutes() + ":" + time.getSeconds() ;
    
    document.getElementById("clock").value = attime;
  }
  setInterval(clock,100);
</script>
</head>
<body>
<form>
<input type="text" id="clock" size="50"  />
</form>
</body>
</html>
```
每隔100ms会执行一次clock函数

clearInterval(id_of_setInterval)，取消计时器的执行

setTimeout(代码,延迟时间);在载入后延迟指定时间后,去执行一次表达式,仅执行一次。

clearTimeout(id_of_setTimeout)，取消延时计时器

window.hsitory.back(),window.hsitory.forward(),进入前/后一个页面
window.history.go(number); 加载任意一个历史页面

document.getElementByName()，getElementByTagName(),getElementById(),getElementsByTagName(),都是用来获取html元素的，byname时是在元素里指定name属性，tagName即标签名，id也是指定的