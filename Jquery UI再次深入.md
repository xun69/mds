# Jquery UI再次深入

## 动态改变JQUI组件

Jquery UI组件采用的是结构代码加js方法实例化渲染的模式。所以通过动态改变结构代码是可以完全实现组件的动态变化效果。

### 1.手风琴面板组件accordion实现添加面板

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
    <link href="https://cdn.bootcss.com/jqueryui/1.12.1/jquery-ui.min.css" rel="stylesheet">
    <script src="https://cdn.bootcss.com/jqueryui/1.12.1/jquery-ui.min.js"></script>
    <script>
        $(function(){
            $("#add").accordion();
            $("#addBtn").on("click",function(){
                $("#add").append("<h3>12</h3>\n<div>121212</div>").accordion("refresh");
            })
        })
        
        </script>
</head>
<body>
    <div id="add">
        <h3>12</h3>
        <div>121212</div>
        <h3>12</h3>
        <div>121212</div>
        <h3>12</h3>
        <div>121212</div>
    </div>
    <input type="button" value="添加" id="addBtn">
    
</body>
</html>
```

上面这段代码只是实现了单击一个按钮添加一个固定的面板,核心代码如下：

```html
 $("#add").append("<h3>12</h3>\n<div>121212</div>").accordion("refresh");
```

- 首先通过append符合要求的结构代码；
- 然后通过组件的refresh命令重载部件;

### 2.结合模态对话框添加面板

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
    <link href="https://cdn.bootcss.com/jqueryui/1.12.1/jquery-ui.min.css" rel="stylesheet">
    <script src="https://cdn.bootcss.com/jqueryui/1.12.1/jquery-ui.min.js"></script>
    <script>
        $(function(){
            $("#add").accordion();//初次渲染结构代码块
            //添加面板
            $("#addBtn").on("click",function(){
                $("#dlg").dialog("open");
            });
            //删除面板
            $("#delBtn").on("click",function(){
                $("#add h3").last().remove();
                $("#add div").last().remove();
            });
            //添加对话框
            $("#dlg").dialog({
                autoOpen: false,
                modal:true,
                buttons:{
                    "确定":function(){
                        //添加面板并更新内容
                        var title="<h3>" + $("#title").val() + "</h3>\n";
                        var ctn="<div>" + $("#ctn").val() + "</div>\n";
                        $("#add").append(title+ctn).accordion("refresh");
                        $( this ).dialog( "close" );
                    },
                    "取消":function(){
                        $( this ).dialog( "close" );
                    }
                }
            });
        })
        
        </script>
</head>
<body>
    <div id="add">
        <h3>12</h3>
        <div>121212</div>
        <h3>12</h3>
        <div>121212</div>
        <h3>12</h3>
        <div>121212</div>
    </div>
    <div id="dlg" title="添加" >
        <label for="#title">标题</label><input type="text" id="title"><br>
        <label for="#ctn">内容</label><textarea name="ctn" id="ctn" cols="30" rows="5"></textarea>
    </div>
    <input type="button" value="添加" id="addBtn">
    <input type="button" value="删除" id="delBtn">
</body>
</html>
```

运行效果如下：

![1554726710384](U:\Jquery UI再次深入.assets\1554726710384.png)

单击确定后将添加新的面板。

## 改进JQUI书写体验

### 1.类和标签预实例化+具体处理结合

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
    <link href="https://cdn.bootcss.com/jqueryui/1.12.1/jquery-ui.min.css" rel="stylesheet">
    <script src="https://cdn.bootcss.com/jqueryui/1.12.1/jquery-ui.min.js"></script>
    <script>
        $(function(){
            //类和标签预实例化
            $("input[type=button]").button();
            $(".jq-acc").accordion();
            $(".jq-tabs").tabs();
            $(".jq-dlg").dialog({autoOpen: false});
            $("input[type=text].jq-date").datepicker();
            //id元素具体处理
            $("#dlg").dialog({
                buttons:{
                    "确定":function(){
                        //添加面板并更新内容
                        var title="<h3>" + $("#title").val() + "</h3>\n";
                        var ctn="<div>" + $("#ctn").val() + "</div>\n";
                        $("#add").append(title+ctn).accordion("refresh");
                        $( this ).dialog( "close" );
                    },
                    "取消":function(){
                        $( this ).dialog( "close" );
                    }
                }
            })
            $("#addBtn").on("click",function(){
                $("#dlg").dialog("open");
            });
        })
        
        </script>
</head>
<body>
    <div id="sl">
        <div id="add" class="jq-acc">
            <h3>12</h3>
            <div>121212</div>
            <h3>12</h3>
            <div>121212</div>
            <h3>12</h3>
            <div>121212</div>
        </div>
        <div id="tabs" class="jq-tabs">
                <ul>
                  <li><a href="#tabs-1">Nunc tincidunt</a></li>
                  <li><a href="#tabs-2">Proin dolor</a></li>
                  <li><a href="#tabs-3">Aenean lacinia</a></li>
                </ul>
                <div id="tabs-1">
                  <p>1</p>
                </div>
                <div id="tabs-2">
                  <p>2</p>
                </div>
                <div id="tabs-3">
                  <p>3
                </div>
               </div>              
        <div id="dlg" title="添加" class="jq-dlg">
            <label for="#title">标题</label><input type="text" id="title"><br>
            <label for="#ctn">内容</label><textarea name="ctn" id="ctn" cols="30" rows="5"></textarea>
        </div>
        <input type="button" value="添加" id="addBtn">
        <input type="button" value="删除" id="delBtn">
        <input type="text" class="jq-date">
    </div>
    
</body>
</html>
```

通过类名或标签的预实例化，可以减少实例化代码，并且实现某种半自动化，提高效率。