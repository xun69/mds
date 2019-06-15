# Vue编程测试

## 1.循环输出测试

### 1.表格输出

表格输出数据是最常用的循环输出。

```html
<div id="app1">
    <table>
        <tr v-for="rec in recs">
            <td>{{rec.name}}</td>
            <td>{{rec.sex}}</td>
            <td>{{rec.age}}</td>
        </tr>
    </table>
</div>

<script>
var vm = new Vue({
    el:"#app1",
    data:{
        recs:[
            {name:"张三",sex:"man",age:"12"},
            {name:"李四",sex:"man",age:"22"},
            {name:"王五",sex:"man",age:"32"}
        ]
    }
})
</script>
```

### 2.div列表输出

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
    <style>
    *{padding: 0px; margin: 0px;}
    .rec{margin: 20px; border: 1px solid #ccc; background: #eee;padding: 10px}
    .rec:hover{background: #ccc}
    </style>
</head>
<body>
<div id="app1">
    <div class="rec" v-for="rec in recs">
        <h3>{{rec.name}}</h3>
        {{rec.sex}}{{rec.age}}
    </div>
</div>

<script>
var vm = new Vue({
    el:"#app1",
    data:{
        recs:[
            {name:"张三",sex:"man",age:"12"},
            {name:"李四",sex:"man",age:"22"},
            {name:"王五",sex:"man",age:"32"}
        ]
    }
})
</script>

</body>
</html>
```

效果如下：

![1559399706695](U:\Vue编程测试.assets\1559399706695.png)

## 2.列表项表单修改

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
    <style>
    *{padding: 0px; margin: 0px;}
    .rec{ border: 1px solid #ccc; background: #eee;}
    .rec:hover{background: #ccc}


    .rec,.setform{margin: 20px;padding: 10px}
    </style>
</head>
<body>
<div id="app1">
    <div class="setform">
        姓名：<input type="text" v-model="recs[nIndex].name"><br>
        性别：<select v-model="recs[nIndex].sex">
            <option value="男">男</option>
            <option value="女">女</option>
        </select>
        年龄：<input type="text" v-model="recs[nIndex].age"><br>
    </div>
    <div class="rec" v-for="(rec,index) in recs" @click="changeNindex(index)">
        <h3>{{rec.name}}</h3>
        {{rec.sex}}-{{rec.age}}
    </div>
</div>

<script>
var vm = new Vue({
    el:"#app1",
    data:{
        recs:[
            {name:"张三",sex:"男",age:"12"},
            {name:"李四",sex:"男",age:"22"},
            {name:"王五",sex:"男",age:"32"}
        ],
        nIndex:0,//当前选中记录的数组下标值
    },
    methods:{
        changeNindex:function(index){//修改nIndex
            //alert(index);
           
            this.nIndex = index;
        }
        
    }
})

vm.$watch("recs[nIndex].sex",function(nval,oval){
    alert();
})
</script>

</body>
</html>
```

 效果如下：

![1559405846650](U:\Vue编程测试.assets\1559405846650.png)

通过列表项的单击事件修改data中的nIndex，而顶部的表单采用的是动态绑定的方式，所以实现了简单的表单修改当前项数据的功能。

- 尝试使用计算属性来将性别翻译成汉字，但是无法做到及时更新数据。Vue还是有很多坑要去踩的。

