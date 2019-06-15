# Banner设计器2.0

 使用jQuery +jQuery UI 当中的Resizeable()设计一个改进版本的Banner设计器。

## 初步效果

![1554917321711](U:\Banner设计器2.0.assets\1554917321711.png)



## 左右布局改进

![1554918203105](U:\Banner设计器2.0.assets\1554918203105.png)

html:

```html
<html>
<head>
<title>Banner设计器2.0</title>
<script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
<script src="https://cdn.bootcss.com/jqueryui/1.12.1/jquery-ui.min.js"></script>
<link href="https://cdn.bootcss.com/jqueryui/1.12.1/jquery-ui.min.css" rel="stylesheet">
<link rel="stylesheet" href="index.css">
<script src="index.js"></script>
</head>

<body>
	<div class="left">
		<div id="banner"></div>
	</div>
	<div class="right">
		<h3>属性设置</h3>
		<div>
			宽度：<input type="text" id="w">px<br>
			高度：<input type="text" id="h">px<br>
			背景色：<input type="color" name="" id="bgcolor">
		</div>
		
	</div>
</body>
</html>
```

css:

```css

*{margin: 0px; padding: 0px;}
.left{width: calc(100% - 250px);height:100%; float: left;overflow: auto}
.right{width:250px;float: right;background-color: azure; height: 100%;}
.right>h3{background-color: aquamarine;}
.right>div{padding: 10px;}



/*-- banner--*/
#banner{
background:#ccc;
width:400px;
height:200px;
}
```

js:

```js
$(function(){
    $("#banner").resizable({
		resize:function(e,ui){
			$("#w").val(ui.size.width);
			$("#h").val(ui.size.height);
		}
    });
    //宽度
	$("#w").on("change",function(){
		$("#banner").css("width",$("#w").val());
    })
    //高度
	$("#h").on("change",function(){
		$("#banner").css("height",$("#h").val());
	})
	//背景色
	$("#bgcolor").on("change",function(){
		$("#banner").css("background-color",$("#bgcolor").val());
	})
})
```

## 设置文字的可编辑状态

![1554919629457](U:\Banner设计器2.0.assets\1554919629457.png)

![1554920096614](U:\Banner设计器2.0.assets\1554920096614.png)

通过直接设置`contenteditable="true"`让文字可以直接进行编辑。

```html
<div id="banner">
    <h1 contenteditable="true">标题</h1>
    <p contenteditable="true">文本</p>
</div>
```

## 设计表单保存数据

这一步暂时缺省。

## 实现图片保存下载

使用网上的html2canvas代码实例，实现了DIV转图片下载保存。

![1554921621366](U:\Banner设计器2.0.assets\1554921621366.png)

保存的图片效果还不错：

![1554921560000](U:\Banner设计器2.0.assets\1554921560000.png)

这样比起Banner设计器1.0版本使用手动截图的方式要先进的多了。

### 目前为止的源码

html:

```html
<html>
<head>
<title>Banner设计器2.0</title>
<script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
<script src="https://cdn.bootcss.com/jqueryui/1.12.1/jquery-ui.min.js"></script>
<script src="https://cdn.bootcss.com/html2canvas/0.4.1/html2canvas.js"></script> 
<link href="https://cdn.bootcss.com/jqueryui/1.12.1/jquery-ui.min.css" rel="stylesheet">
<link rel="stylesheet" href="index.css">
<script src="index.js"></script>
</head>

<body>
	<div class="left">
		<div id="banner">
			<h1 contenteditable="true">标题</h1>
			<p contenteditable="true">文本</p>
		</div>
	</div>
	<div class="right">
		<form action="">
			<h3>属性设置</h3>
			<h4>画布</h4>
			<div>
				宽度：<input type="text" id="w">px<br>
				高度：<input type="text" id="h">px<br>
				背景色：<input type="color" name="" id="bgcolor"><br>
				背景图片: <input type="file" name="" id="bgImg">
			</div>
			<h4>大标题</h4>
			<div>
				top：<input type="text" id="h1_top">px<br>
				left：<input type="text" id="h1_left">px<br>
				字色：<input type="color" name="" id="h1_color">
			</div>
			<h4>小标题</h4>
			<div>
			
			</div>
			<input type="button" value="保存" id="saveBtn">
			<a href="#" id="down1">下载图片</a>
			<hr>
			<h4>背景素材</h4>
			<a href="https://image.baidu.com/search/index?tn=baiduimage&ipn=r&ct=201326592&cl=2&lm=-1&st=-1&fm=result&fr=&sf=1&fmq=1554921490476_R&pv=&ic=&nc=1&z=&hd=&latest=&copyright=&se=1&showtab=0&fb=0&width=&height=&face=0&istype=2&ie=utf-8&word=banner%E8%83%8C%E6%99%AF&f=3&oq=banner&rsp=0" target="_blank" rel="noopener noreferrer">百度图片-anner背景</a>
		</form>
	</div>
</body>
</html>
```

css:

```css
*{margin: 0px; padding: 0px;}
.left{width: calc(100% - 250px);height:100%; float: left;overflow: auto;background-color: #444;}
.right{width:250px;float: right;background-color: azure; height: 100%;}
.right>form>h3{background-color: aquamarine;}
.right>form>div{padding: 10px;}



/*-- banner--*/
#banner{
background:#ccc;
width:400px;
height:200px;
overflow: hidden;
}
```

js:

```js
$(function(){
    $("#banner").resizable({
        autoHide:true,
		resize:function(e,ui){
			$("#w").val(ui.size.width);
			$("#h").val(ui.size.height);
		}
    });

    $("#banner h1").draggable({
        drag:function(e,ui){
            $("#h1_left").val(ui.position.left);
			$("#h1_top").val(ui.position.top);
        }
    });
    $("#banner p").draggable();
    //****************画板****************
    //宽度
	$("#w").on("change",function(){
		$("#banner").css("width",$("#w").val());
    })
    //高度
	$("#h").on("change",function(){
		$("#banner").css("height",$("#h").val());
	})
	//背景色
	$("#bgcolor").on("change",function(){
		$("#banner").css("background-color",$("#bgcolor").val());
    })

    //载入背景图片
	$("#bgImg").on("change",function(){
        var file = this.files[0];
          if (window.FileReader) {    
           var reader = new FileReader();    
           reader.readAsDataURL(file);    
           //监听文件读取结束后事件    
         reader.onloadend = function (e) {
             $("#banner").css("background-image","url(" + e.target.result +")");
           };    
      } 
       
   });
    //****************画板****************
    //宽度
	$("#h1_left").on("change",function(){
		$("#banner h1").css("left",$("#h1_left").val());
    })
    //高度
	$("#h1_top").on("change",function(){
		$("#banner h1").css("top",$("#h1_top").val());
	})
	//背景色
	$("#h1_color").on("change",function(){
		$("#banner h1").css("color",$("#h1_color").val());
    })



    //保存
    $("#saveBtn").click(function(){
        html2canvas($("#banner"), {  
            height: $("#banner").outerHeight() + 20,  
                width: $("#banner").outerWidth() + 20  ,
                onrendered: function(canvas) {
                //将canvas画布放大若干倍，然后盛放在较小的容器内，就显得不模糊了
                var timestamp = Date.parse(new Date());
                //把截取到的图片替换到a标签的路径下载  
                $("#down1").attr('href',canvas.toDataURL());  
                //下载下来的图片名字  
                $("#down1").attr('download',timestamp + '.png') ;   
                //document.body.appendChild(canvas);  
            }  
            //可以带上宽高截取你所需要的部分内容 
        });  
    });  
})
```

### 下一步设计思路

#### 1.左侧控件栏

用类似可视化设计器的方式，快速拖放无数的文本、图标、图片、h1~h6以及其他可用于设计Banner的元素，让Banner设计器更加强大。

#### 2.数据保存与再现

将设计数据保存为可解析的XML文件或Json文件，可以二次加载显示，进行重新设计。

#### 3.实现尺寸预设

可以将常用Banner尺寸作为预设保存为XML等数据进行加载。

#### 4.颜色选择器的自定义化

颜色选择一定要方便快速，可以借鉴1.0的色板和类似Coreldraw的取色操作。



### 扩展编程思路

可以设计一些网页组件库的可视化设计程序。比如easy ui等。