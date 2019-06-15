# Vue.js实现数据计算程序实例

## data的结构深化

```html
<div class="main" id="app1">
    <h3>{{message.sub}}</h3>
</div>
<script>
    new Vue({
        el:"#app1",
        data:{
            message:{
                sub:"as"
            },
        }
    })
</script>
```

可以看到Vue.js的data部分可以采取复杂的对象或类JSON结构。

## 实现简单的总和计算

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Vue计算器</title>
    <link rel="stylesheet" href="index.css">
    <script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
</head>
<body>
    <div class="main" id="app1">
        <h3>支付宝</h3>
        花呗：<input type="text" v-model="zfb.hb"><br>
        借呗：<input type="text" v-model="zfb.jb"><br>
        <h3>京东</h3>
        白条：<input type="text" v-model="jd.bt"><br>
        金条：<input type="text" v-model="jd.jt"><br>
        <h3>微信</h3>
        微粒贷：<input type="text" v-model="wx.wld"><br>
        <h3>苏宁金融</h3>
        任性贷：<input type="text" v-model="snjr.rxd"><br>
        总计：{{wdsum}}
    </div>
    <script>
    new Vue({
        el:"#app1",
        data:{
            zfb:{//支付宝
                hb:0,//花呗
                jb:0//借呗
            },
            jd:{//京东
                bt:0,//白条
                jt:0//金条
            },
            wx:{//微信
                wld:0,//微粒贷
            },
            snjr:{//苏宁金融
                rxd:0//任性贷
            }
        },
        computed:{
            wdsum:function(){//网上借款合计
                return parseFloat(this.zfb.hb) 
                     + parseFloat(this.zfb.jb)
                     + parseFloat(this.jd.bt)
                     + parseFloat(this.jd.jt)
                     + parseFloat(this.wx.wld)
                     + parseFloat(this.snjr.rxd)
                     ;
            }
        }
    })
    </script>
</body>
</html>
```

可以看到Vue的代码真的很省，同样的功能用jQuery可能要写半天。

![1558714605386](U:\Vue.js实现数据计算.assets\1558714605386.png)

