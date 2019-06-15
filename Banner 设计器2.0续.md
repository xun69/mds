# Banner 设计器2.0续

## 组件工具的实例化加载实现

通过建立左侧的组件工具栏来加载多文本。

![1554983189553](U:\Banner 设计器2.0续.assets\1554983189553.png)

## 引入第三方拾色器插件

![1554984790269](U:\Banner 设计器2.0续.assets\1554984790269.png)

## 自制色盘

第三方拾色器插件有太多问题，不如直接自己生成一个色盘来的方便

![1554988912896](U:\Banner 设计器2.0续.assets\1554988912896.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
    <style>
    #colors span{
        width:20px;height:20px; display: inline-block; margin: 2px; border: 1px solid #000;
    }
    #banner{
        width: 200px; height: 200px;border: 1px solid #000;
    }
    </style>
    <script>
    $(function(){
        var stepVal = 10;//色盘步幅，步幅越小生成的颜色越多
        for(i=0;i<255;i=i+stepVal){
            var cRGB="rgb(" + i + "," + i + "," + i + ")";
            var span="<span style='background-color:" + cRGB + ";'></span>";
            $("#colors").append(span);
        }
        for(i=0;i<255;i=i+stepVal){
            var cRGB="rgb(" + 0 + "," + i + "," + i + ")";
            var span="<span style='background-color:" + cRGB + ";'></span>";
            $("#colors").append(span);
        }
        for(i=0;i<255;i=i+stepVal){
            var cRGB="rgb(" + i + "," + 0 + "," + i + ")";
            var span="<span style='background-color:" + cRGB + ";'></span>";
            $("#colors").append(span);
        }
        for(i=0;i<255;i=i+stepVal){
            var cRGB="rgb(" + i + "," + i + "," + 0 + ")";
            var span="<span style='background-color:" + cRGB + ";'></span>";
            $("#colors").append(span);
        }
        $("#colors span").on("mousedown",function(){
            $("#banner").css("background-color",$(this).css("background-color"));
        })
    })
    
    </script>
</head>
<body>
    <div id="banner">

    </div>
    <div id="colors">

    </div>
</body>
</html>
```

### 实现多元素的颜色设置

#### 记录当前对象currntObj

通过Banner中文字元素的单击事件和“选中Banner”按钮单击事件，可以实现选中对象的颜色设置。从而可以为不同的文字元素设置不同的颜色。

![1554996670200](U:\Banner 设计器2.0续.assets\1554996670200.png)

修正输出尺寸Bug后，图片输出效果完全正确。

![1554996625000](U:\Banner 设计器2.0续.assets\1554996625000.png)

![1554999305851](U:\Banner 设计器2.0续.assets\1554999305851.png)



![1554998332000](U:\Banner 设计器2.0续.assets\1554998332000.png)

### 实现图标添加

![1554998818916](U:\Banner 设计器2.0续.assets\1554998818916.png)

采用一个御用图标库加载的方式是最好最轻松的图标插入解决方案。

![1554999738129](U:\Banner 设计器2.0续.assets\1554999738129.png)

简单Banner清空功能实现。

### 加入表情

![1555002104628](U:\Banner 设计器2.0续.assets\1555002104628.png)