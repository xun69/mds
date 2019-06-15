# 星工具数据PHP存取化以及与星wiki整合

## 星工具数据PHP存取化

星工具是我对jQuery EasyUI的一种尝试，也是对HTML标签自定义属性和JQuery包裹等新的组件渲染思想的尝试。但是最后它过度的依赖了JS来存储数据，这是一个错误的行为。

现在，可以利用PHP文件存取和其他手段来优化数据存取行为。

![1555079533636](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555079533636.png)

### 数据设计

#### E-R图

![1555081955623](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555081955623.png)

#### 数据实现

上面的数据设计结构可以用不同的方式实现，避开数据库，可以使用json、XML或自定义数据文件实现。

其中json的优势是：

1. 设计简单,天然适合JS解析；
2. 灵活使用数组、对象嵌套的设计方式发散分解字段值；
3. PHP提供解析和封装函数，利于存取。

```json
{
    "gups":[
        {"id":"g0001","name":"看电影"},
        {"id":"g0001","name":"PHP开发"}
    ],
    "sites":[
        {"gupID":"","id":"s0001","icon":"","name":"百度","url":"","addTime":"","vCount":""},
        {"gupID":"","id":"s0002","icon":"","name":"百度","url":"","addTime":"","vCount":""}
    ]
}
```

### 逻辑设计

基本的功能逻辑设计如下：

![1555083953614](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555083953614.png)

简化的数据逻辑如下

![1555084506720](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555084506720.png)

### 静态模版代码实现

#### 数据加载实现

##### 初始数据加载

通过jQuery的`$.getJSON()`方法来加载json数据。并保存到全局变量中用于修改和保存。

```js
var json1={};//全局存储加载的json对象
//初始加载json数据并显示
$.getJSON("x.js",function(rs){
    json1=rs;
    alert(json1.gups[0].id);
})
```

##### 完整数据加载代码

```js
$(function(){
    var editMode=false;//编辑模式开关变量
    var json1={};//全局存储加载的json对象
    //初始加载json数据并显示
    $.getJSON("x.js",function(rs){
        json1=rs;
        for(i=0;i<json1.gups.length;i++){//添加分组标题
            var h3='<h3>' + json1.gups[i].name + '<span><input type="button" value="添加"><input type="button" value="重命名"></span></h3>';
            var ctn='<div class="xt-sites-content" id="' + json1.gups[i].id + '"><ul></ul></div>';
            var clear='<div class="clear"></div>';
            $(".xt-sitesLVW").append(h3  + ctn  + clear);
            //分组折叠
            $(".xt-sitesLVW h3").on("click",function(){
                $(this).next(".xt-sites-content").toggle("fast");
            });
        }
        for(j=0;j<json1.sites.length;j++){//添加网址列表
            var li='<li>' +
                        '<span class="tools"><img src="images/err.ico" title="删除" class="err"><br><img src="images/edit.ico" title="编辑" class="edt"></span>' +
                        '<a href="https://pc.qq.com/search.html#!keyword=VScode" target="_blank">' +
                            '<img src="https://pc3.gtimg.com/softmgr/logo/48/22856_48_1534813530.png" alt="">' +
                            '<h4>VScode</h4>' +
                        '</a>' +
                    '</li>';
            var gID=json1.sites[j].gupID;
            $("#" + gID + " ul").append(li);

            //鼠标经过显示删除图标
            $(".xt-sitesLVW li").on("mouseover",function(){
                if(editMode){//编辑模式
                    $(this).find("span.tools").show();
                }
            });
            $(".xt-sitesLVW li").on("mouseout",function(){
                if(editMode){//编辑模式
                    $(this).find("span.tools").hide();
                }
            });
            //删除图标单击
            $(".xt-sitesLVW li img.err").on("click",function(){
                alert();
            });
        }
    })
```

##### 原始结构代码

```html
<div class="xt-sitesLVW">
        <h3>分组1<span><input type="button" value="添加"><input type="button" value="重命名"></span></h3>
        <div class="xt-sites-content">
            <ul>
                <li>
                    <span class="tools"><img src="images/err.ico" title="删除" class="err"><br><img src="images/edit.ico" title="编辑" class="edt"></span>
                    <a href="https://pc.qq.com/search.html#!keyword=VScode" target="_blank">
                        <img src="https://pc3.gtimg.com/softmgr/logo/48/22856_48_1534813530.png" alt="">
                        <h4>VScode</h4>
                    </a>
                </li>
            </ul>
        </div>
        <div class="clear"></div>
    </div>
```

#### 实现item的删除和编辑图标

![1555088023493](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555088023493.png)

##### item删除的正确代码

item删除需要实现：

1. DOM显示上的删除;
2. 数据JS对象上相应记录的删除;
3. 通过AJAX传递当前数据对象到save.php,由save.php将该数据更新保存到xxx.js文件。

下面的代码实现了前两步：

```js
//删除图标单击
$(".xt-sitesLVW li img.err").on("click", function () {
    var siteID = $(this).parents("li").attr("id");
    if (confirm("确定要删除吗？")) {
        for (l = 0; l < json1.sites.length; l++) {
            if(json1.sites[l]){
                if (json1.sites[l].id == siteID) {
                    delete json1.sites[l];//删除js对象中site
                }
            } 
        }
        $(this).parentsUntil("ul").remove();
    }
});
```

delete语句可以用于实现数据JS对象上相应记录的删除，但是delate删除会保留被删除元素的位置，标记为empty，整个数组的长度length是不会改变的，这样在第二次遍历时，`json1.sites[l].id == siteID` 判断就会出现json1.sites[l].id未定义错误。正确的做法是在判断id这层之外首先判断json1.sites[l]是否存在。（除非使用其他的方法删除数组元素）

![1555164866238](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555164866238.png)

改用splice()删除数组元素可以避免上面的情况

```js
json1.sites.splice(l,1);//数据JS对象上相应记录的删除
```

格式：splice(起始位置,删除个数)，返回结果为新的数组。

##### 服务器端数据保存实现

首先在前端编写saveJson函数：

```js
//向服务器传递数据js对象的字符串并由save.php保存数据到相应的文件
function saveJson(jsonObj,fName,callBack){
    var jsonData = JSON.stringify(jsonObj);//将JS对象转化为字符串；
    $.post("save.php",{json:jsonData,fName:fName},callBack)
}
```

save.php:

```php
<?php
$jsonData = $_POST["json"];//传回的json数据
$fName=$_POST["fName"];//数据文件名

//数据保存
$myfile = fopen($fName, "w");
fwrite($myfile,$jsonData);
fclose($myfile);
```

数据更新保存已经已经实现，但是保存后的json代码排版有点不好：

```json
{\"gups\":[{\"id\":\"g1\",\"name\":\"看电影\"},{\"id\":\"g2\",\"name\":\"PHP开发\"},{\"id\":\"g3\",\"name\":\"PHP开发\"}],\"sites\":[{\"gupID\":\"g1\",\"id\":\"s2\",\"icon\":\"\",\"name\":\"百度\",\"url\":\"\",\"addTime\":\"\",\"vCount\":\"\"},{\"gupID\":\"g3\",\"id\":\"s3\",\"icon\":\"\",\"name\":\"百度\",\"url\":\"\",\"addTime\":\"\",\"vCount\":\"\"}]}
```

这样的json代码不适合人工阅读和手工修改。

寻找的解决方案是JSON.stringify方法添加后续两个参数，如下：

```js
var jsonData = JSON.stringify(jsonObj,null,"\t");//将JS对象转化为字符串；
```

"\t"代表缩进一个Tab；

重新获得保存的json文件如下：

```json
{
	\"gups\": [
		{
			\"id\": \"g1\",
			\"name\": \"看电影\"
		},
		{
			\"id\": \"g2\",
			\"name\": \"PHP开发\"
		},
		{
			\"id\": \"g3\",
			\"name\": \"PHP开发\"
		}
	],
	\"sites\": [
		{
			\"gupID\": \"g1\",
			\"id\": \"s2\",
			\"icon\": \"\",
			\"name\": \"百度\",
			\"url\": \"\",
			\"addTime\": \"\",
			\"vCount\": \"\"
		},
		{
			\"gupID\": \"g3\",
			\"id\": \"s3\",
			\"icon\": \"\",
			\"name\": \"百度\",
			\"url\": \"\",
			\"addTime\": \"\",
			\"vCount\": \"\"
		}
	]
}
```

反斜杠挺碍眼的，其他好多了，所以replace掉就行了。

各种replace 不行之后终于找到了正解：

![1555171633908](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555171633908.png)

PHP中专门有一个删除反斜杠的函数。

![1555171686119](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555171686119.png)

修改之后的save.php：

```php
<?php
$jsonData = $_POST["json"];//传回的json数据
$fName=$_POST["fName"];//数据文件名


$jsonData = stripslashes($jsonData);//删除反斜杠
//数据保存
$myfile = fopen($fName, "w");
fwrite($myfile,$jsonData);
fclose($myfile);
```

##### 解决$.getJSON的缓存问题

对于请求，JS貌似会建立文件缓存，导致服务器端数据文件已经改变，前端刷新却还是之前的数据文件。

```js
 $.getJSON(jsonFname,{time:(new Date()).valueOf()}, function (rs)
```

解决方法是为$.getJSON传入一个参数，而这个参数是当前时间戳，是一个随刷新永远在变的数值，从而让AJAX请求的URL永远在变，保证了数据文件不会产生缓存问题。

![1555173567367](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555173567367.png)

#### 实现item添加

为了方便操作，引入jQuery UI,使用其模态对话框来设计添加操作。

![1555178447753](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555178447753.png)

![a](U:\星工具数据PHP存取化以及与星wiki整合.assets\a.gif)



![b](U:\星工具数据PHP存取化以及与星wiki整合.assets\b-1555179118342.gif)

## 星工具与星wiki整合

星工具的一些设计思路还是非常不错的。通过iframe可以简单的整合进星wiki而不用改变彼此的整体代码环境。

![1555181068033](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555181068033.png)

### 扩展思路

dokuwiki系统有一些固有限制，一些HTML或PHP、JS代码、功能等完全可以通过iframe引入子页面实现。

星工具可以补足dokuwiki的网址收藏功能。

## 2019年4月14日 修改

### 改进鼠标经过样式细节

利用CSS3的属性变换，实现了更柔和的hover效果。

### 添加网址按钮的改进

改进后的效果就是最初预想的样子。

![1555255160736](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555255160736.png)

### 添加网址对话框空值提醒

```js
var icon= $("#itemAddDlg input[name='icon']").val()
var name= $("#itemAddDlg input[name='name']").val()
var url= $("#itemAddDlg input[name='url']").val()
if( icon =="" || name == "" || url == ""){//表单中有空项
	alert("输入字段不能有空值，请完整填写。")
}
```

这样可以初步进行数据有效验证，避免无效数据的录入。

![1555255814245](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555255814245.png)

### 添加网址对话框标题显示对应分组标题

![1555257358977](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555257358977.png)

#### 添加文本框提示文字placeholder

![1555257620733](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555257620733.png)

### 修改网址对话框实现



![1555259915410](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555259915410.png)

目前除了分组外已经彻底实现了修改网址功能。

## 2019年4月19日 改进

### 样式优化1

![1555663789882](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555663789882.png)

### 实现不同页面的初始化加载

datas/x.js

在index.php添加如下代码

```php
<?php
if($_GET[fName]){//利用PHP传参要加载的数据文件
echo <<<EOT
    var jFname = $_GET[fName]//加载文件名
    loadJson("datas/" + jFname);//初始加载json数据并显示
EOT;
}
?>
```

这段代码输出js代码，完成PHP到js的文件名传递。

在星辰维基的页面中只需要修改iframe的URL为传参模式即可。

```html
<html>
<iframe src="http://xun69.cn/xtools/index.php?fName=x.js" frameborder="0" style="width:100%;height:800px;"></iframe>
</html>
```

#### LVW的类化

因为不能使用多个$(function())实现多个代码段之间的调用，直接暴露全局变量和函数又是纯粹业余的做法，所以直接写个全局类是最好的做法。

```
//LVW类
var LVW = {
    cNew : function(jFname){
        //*************** 私有成员***************
        var json1={};//存储加载的json数据对象
        var fName=jFname;//json数据文件名
        //返回的实例对象
        var obj={};
        //*************** 私有成员***************
        obj.editMode = false;//编辑模式开关变量
        obj.loadJson = function(jsonFname){//初始加载json数据并显示
                $.getJSON(jsonFname,{time:(new Date()).valueOf()}, function (rs) {
                    json1 = rs;
                    //1.加载分组标题
                    for (i = 0; i < json1.gups.length; i++) {//添加分组标题
                        var h3 = '<h3>' + json1.gups[i].name + '</h3>';
                        //var addItem ='<input type="button" value="添加" class="itmAddBtn">';
                        var addItem ='<div class="itmAddBtn"><img src="images/add.ico"><h4>添加网址</h4></div>';
                        var ctn = '<div class="xt-sites-content" id="' + json1.gups[i].id  + '"><ul></ul>' + addItem + '</div>';
                        var clear = '<div class="clear"></div>';
                        $(".xt-sitesLVW").append(h3 + ctn + clear);
            
                    }
                    //2.添加分组折叠效果
                    $(".xt-sitesLVW h3").on("click", function () {
                        if ($(this).next(".xt-sites-content").find("ul")[0]) {//已经存在ul标签
                            $(this).next(".xt-sites-content").toggle("fast");
                        }
                    });
                    //3.加载网址列表到对应的分组下
                    for (j = 0; j < json1.sites.length; j++) {
                        var item =json1.sites[j];
                        var li = '<li id="' + item.id + '">' +
                            '<span class="tools"><img src="images/err.ico" title="删除" class="err"><br><img src="images/edit.ico" title="编辑" class="edt"></span>' +
                            '<a href="' + item.url + '" target="_blank">' +
                            '<img src="' + item.icon + '" alt="">' +
                            '<h4>' + item.name + '</h4>' +
                            '</a>' +
                            '</li>';
                        var gID =item.gupID;
                        $("#" + gID + " ul").append(li);
                    };
                    //删除图标显示
                    $(".xt-sitesLVW li").on("mouseover", function () {
                        if (obj.editMode) {//编辑模式下
                            $(this).find("span.tools").show();
                        }
                    });
                    $(".xt-sitesLVW li").on("mouseout", function () {
                        if (obj.editMode) {//编辑模式
                            $(this).find("span.tools").hide();
                        }
                    });
                    //**************item的增删改**************
                    //删除图标单击 - 删除item
                    $(".xt-sitesLVW li img.err").on("click", function () {
                        var siteID = $(this).parents("li").attr("id");
                        if (confirm("确定要删除吗？")) {
                            for (l = 0; l < json1.sites.length; l++) {
                                if (json1.sites[l].id == siteID) {
                                    json1.sites.splice(l,1);//数据JS对象上相应记录的删除
                                    //post到服务器保存数据修改
                                    obj.saveJson();
                                    
                                }
                            }
                            $(this).parentsUntil("ul").remove();
                        }
                    });
                    //修改图标单击 - 修改item
                    $(".xt-sitesLVW li img.edt").on("click", function () {
                        var siteID = $(this).parents("li").attr("id");
                        var siteRec = {};//当前item对应的js记录对象
                        for (l = 0; l < json1.sites.length; l++) {
                            if (json1.sites[l].id == siteID) {
                                siteRec = json1.sites[l];
                            }
                        }
                        //显示修改对话框
                        $("#itemUpdateDlg").dialog({
                            resizable: false,
                            height:280,width:400,
                            modal: true,
                            title:"修改网址 - " + siteRec.name,
                            close:function(){//对话框关闭后清空表单数据
                                $("#itemUpdateDlg select[name='gupID']").empty();
                                $("#itemUpdateDlg input[name='icon']").val("");
                                $("#itemUpdateDlg input[name='name']").val("");
                                $("#itemUpdateDlg input[name='url']").val("");
                            },
                            open:function(){
                                //1.加载分组标题
                                for (i = 0; i < json1.gups.length; i++) {//添加分组标题
                                    var option ="";
                                    if(siteRec.gupID == json1.gups[i].id){//为当前item的分组
                                        option = '<option value="' + json1.gups[i].id + '" selected>' + json1.gups[i].name + '</option>';
                                    }else{
                                        option = '<option value="' + json1.gups[i].id + '">' + json1.gups[i].name + '</option>';
                                    }
                                    
                                    $("#itemUpdateDlg select[name='gupID']").append(option);
                                }
                                //加载当前item的数据到对话框表单
                                $("#itemUpdateDlg input[name='icon']").val(siteRec.icon);
                                $("#itemUpdateDlg input[name='name']").val(siteRec.name);
                                $("#itemUpdateDlg input[name='url']").val(siteRec.url);
                            },
                            buttons: {
                                "取消": function() {
                                    $( this ).dialog( "close" );
                                },
                                "确定": function() {
                                    //修改数据对象
                                    var gupID= $("#itemUpdateDlg select[name='gupID']").val()
                                    var icon= $("#itemUpdateDlg input[name='icon']").val()
                                    var name= $("#itemUpdateDlg input[name='name']").val()
                                    var url= $("#itemUpdateDlg input[name='url']").val()
                                    if( icon =="" || name == "" || url == ""){//表单中有空项
                                        alert("输入字段不能有空值，请完整填写。")
                                    }else{
                                        //1.修改数据对象记录
                                        siteRec.gupID = gupID;
                                        siteRec.icon = icon;
                                        siteRec.name = name;
                                        siteRec.url = url;
                                        //2.修改DOM显示
                                        $("#" + siteRec.id + " a>img").attr("src",siteRec.icon);
                                        $("#" + siteRec.id + " h4").text(siteRec.name);
                                        $("#" + siteRec.id + " a").attr("url",siteRec.url);
                                        //3.保存数据更改
                                        obj.saveJson();

                                    }
                                    $( this ).dialog( "close" );
                                }
                            }
                            
                        });
                        $("#itemUpdateDlg").dialog("open");
                    });
                     //添加item
                     $(".itmAddBtn").on("click",function(){
                        var gupID = $(this).parent().attr("id");
                        var gupTitle = $(this).parents("div.xt-sites-content").prev().text();
                        var lastItemId;//最后的记录ID
                        if(json1.sites.length>0){//存在至少1条记录
                            lastItemId = json1.sites[json1.sites.length-1].id;//最后的记录ID
                        }else{//不存在记录
                            lastItemId = "s0";
                        }
                        var newItmId = parseInt(lastItemId.slice(1)) + 1;
                            newItmId = "s" + newItmId;//新的ID
        
                        $("#itemAddDlg").dialog({
                            resizable: false,
                            height:230,width:400,
                            modal: true,
                            title:"添加网址 - " + gupTitle,
                            close:function(){//对话框关闭后清空表单数据
                                $("#itemAddDlg input[name='icon']").val("");
                                $("#itemAddDlg input[name='name']").val("");
                                $("#itemAddDlg input[name='url']").val("");
                            },
                            open:function(){
                                
                            },
                            buttons: {
                                "取消": function() {
                                    $( this ).dialog( "close" );
                                },
                                "确定": function() {
                                    var icon= $("#itemAddDlg input[name='icon']").val()
                                    var name= $("#itemAddDlg input[name='name']").val()
                                    var url= $("#itemAddDlg input[name='url']").val()
                                    if( icon =="" || name == "" || url == ""){//表单中有空项
                                        alert("输入字段不能有空值，请完整填写。")
                                    }else{
                                    var item = {
                                        gupID: gupID,
                                        id:  newItmId,
                                        icon: icon,
                                        name: name,
                                        url: url,
                                        addTime: (new Date()).valueOf().toString(),
                                        vCount: "0"
                                    }
                                    json1.sites.push(item);//数据对象添加记录
                                    //DOM添加记录
                                    var li = '<li id="' + item.id + '">' +
                                            '<span class="tools"><img src="images/err.ico" title="删除" class="err"><br><img src="images/edit.ico" title="编辑" class="edt"></span>' +
                                            '<a href="' + item.url + '" target="_blank">' +
                                            '<img src="' + item.icon + '" alt="">' +
                                            '<h4>' + item.name + '</h4>' +
                                            '</a>' +
                                            '</li>';
                                        var gID =item.gupID;
                                        $("#" + gID + " ul").append(li);
                                        //删除图标显示
                                        $(".xt-sitesLVW li").on("mouseover", function () {
                                            if (obj.editMode) {//编辑模式下
                                                $(this).find("span.tools").show();
                                            }
                                        });
                                        //修改图标显示
                                        $(".xt-sitesLVW li").on("mouseout", function () {
                                            if (obj.editMode) {//编辑模式
                                                $(this).find("span.tools").hide();
                                            }
                                        });
                                        //删除图标单击 - 删除item
                                        $(".xt-sitesLVW li img.err").on("click", function () {
                                            var siteID = $(this).parents("li").attr("id");
                                            if (confirm("确定要删除吗？")) {
                                                for (l = 0; l < json1.sites.length; l++) {
                                                    if (json1.sites[l].id == siteID) {
                                                        json1.sites.splice(l,1);//数据JS对象上相应记录的删除
                                                        //post到服务器保存数据修改
                                                        obj.saveJson();
                                                    
                                                    }
                                                }
                                                $(this).parentsUntil("ul").remove();
                                            }
                                        });
                                    //保存服务器数据
                                    obj.saveJson();
                                    $( this ).dialog( "close" );
                                }
                            }
                            }
                        });
                        $("#itemAddDlg").dialog("open");
                    })
            
                });
        }
        obj.saveJson=function(){//向服务器传递数据js对象的字符串并由save.php保存数据到相应的文件
            var jsonData = JSON.stringify(json1,null,"\t");//将JS对象转化为字符串；
            console.log(jsonData);
            $.post("save.php",{json:jsonData,fName:"datas/" + fName + ".js"});
        }
        obj.loadJson("datas/" + jFname + ".js")//初始加载json数据并显示
        return obj;
    }
};
```

index.php中的代码就可以写成下面这样了：

```php+HTML
    <script>
$(function(){
    $("#itemAddDlg").dialog({  autoOpen:false });
    $("#itemUpdateDlg").dialog({  autoOpen:false });
<?php
if($_GET["fName"]){//利用PHP传参要加载的数据文件
   $fName = $_GET["fName"];
echo <<<EOT
    var jFname = "{$fName}"//加载文件名
    var lvw =LVW.cNew(jFname);
    $("#edtModeBtn").on("click", function () {
        lvw.editMode = !lvw.editMode;
        if(lvw.editMode){
            $(".itmAddBtn").show();
        }else{
            $(".itmAddBtn").hide();
        }
    })
EOT;
}
    
?>
});

    </script>
```

浏览器测试如下地址：

```js
http://localhost/xtools/index.php?fName=x
```

可以完美的加载相应的文件。

![1555673459342](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555673459342.png)

![1555673480517](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555673480517.png)

![1555674850753](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555674850753.png)

### 管理员登录的简单实现

初步实现了管理员登录后对网址数据的修改（暂时存在直接暴露性的漏洞）

![1555677663376](U:\星工具数据PHP存取化以及与星wiki整合.assets\1555677663376.png)

