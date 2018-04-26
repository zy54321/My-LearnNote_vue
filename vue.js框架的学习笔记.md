[toc]
# vue.js 框架的学习
[vue.js文档链接](https://cn.vuejs.org/v2/guide/syntax.html)

## 安装vue
安装vue 通过终端npm install vue下载安装		
或者通过https://cdn.jsdelivr.net/npm/vue/dist/vue.js 外部引入

```js
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```
MVVM : MVC改进版---就是将其中的View 的状态和行为抽象化，让我们将视图 UI 和业务逻辑分开		
//mvvm m:data; v:id 对应的HTML; vm:app;

## 创建第一个vue
首先现在body里绑定div

```js
<div id="app">
<!--v-model 绑定并联动数据-->
    <input type="text" v-model="message">
    message:{{message}}
    <!--<button onclick="changeData()">改变数据</button>-->
</div>
```
当input输入内容时,外面的message:后的内容会跟着发生变化

然后在script里声明并创建vue

```js
//创建一个vue
var app = new Vue({
    //绑定视图 作用域是ID对应的子视图
	//指任何操作都必须在绑定的div内
    el:'#app',
    data:{//数据
        message:'hello vue!'
    }
});
app.message = "hello word";
app.message = "hello"; //后赋值的覆盖以前赋值的
function changeData() {
    app.message = "data"; //app.data.message  点击改变数据
}
```

## v-方法名 属于vue的操作方法
## 第一天学习 v-bind, v-if, v-show, v-for, v-on
body里的div绑定

```js
<div id="app">
    <!--v-bind 绑定属性 简写:属性名。可以直接不写v-bind-->
    <a :href="url">百度</a> 

    <!--v-if 值 boolean类型 true:显示  false:不显示 (是否渲染)-->
    <div v-if="isShow">是否显示</div>

    <!--v-show 是否显示对应DIV里的内容 true:显示  false:不显示-->
    <div v-show="isShow">阿斯顿</div>

    <ul>
        <!--v-for 循环标签 data是循环对象的元素 listFor循环对象 data或listFor名字都可以自定义-->
        <li v-for="data in listFor">
            <div>名字:{{data.name}}</div>
            <div>年龄:{{data.age}}</div>
        </li>
    </ul>

    <!--v-on 绑定事件 事件定义在实例的method属性里 简写@click-->
    <button v-on:click="changeShow">按钮</button>
    <button @click="changeText">改名字</button>
</div>
```

script里的代码
```js
var data = {
        name:'zhangsan',
        age:18,

        //v-bind 绑定属性 简写:属性名。可以直接不写v-bind
        //在data里给url设置的路径传给v-bind所绑定的属性
        url:"http://www.baidu.com", //标签里的属性不能用{()}

        //v-if 值 boolean类型 true:显示  false:不显示 (是否渲染)
        //v-show 是否显示对应DIV里的内容 true:显示  false:不显示
        isShow:false,

        //v-for 循环标签
        listFor:[
            {
                name:'张三',
                age:18
            },{
                name:'李四',
                age:16
            },{
                name:'阿斯顿',
                age:19
            }
        ]


    };
    // Object.freeze(data);//阻止数据更新
    var app = new Vue({
        el:'#app',
        data:data, //初始化赋值
        //v-on 绑定事件 事件定义在实例的method属性里 简写@click
        methods:{
            changeShow:function () {
                this.isShow = !this.isShow;
            },
            changeText:function () {
                this.name = "规划局";
            }
        }
    });
```
## 第二天学习 v-html, 计算属性 取值器 赋值器改数值, 大小写转换, watch监听器, :class, :style, v-if, v-else, v-else-if, v-for, v-model, 过滤器
### v-html, 计算属性 取值器 赋值器改数值, 大小写转换, watch监听器      
body里的div绑定
```js
<div id="app">
    <!--<div>{{msg}}</div>-->
    <div>{{html}}</div>
    <!--v-html  向调用 html 的对象里嵌套html标签-->
    <div v-html="html"></div>

    <!--算数计算不推荐的方法-->
    <div>¥{{price + 6}}</div>
    <!--算数计算函数调用方法-->
    <div>{{goods}}</div>

    <!--变成大写,不推荐这个方法-->
    <div>{{firstName.toUpperCase()}}</div>
    <!--变成大写,函数调用方法-->
    <!--<div>{{showFirstName}}</div>-->
    <!--methods方法函数调用-->
    <div>{{showFirstName2("san")}}</div>
    <!--全名-->
    <div>{{fullName}}</div>

    <!--watch 监听器-->
    <input type="text" v-model="message">
    <div>{{msg}}</div>
    <div>{{message}}</div>
</div>
```
script里的代码
```js
/*
    1,v-html  向调用 html 的对象里嵌套html标签
    2,计算属性 data里没有声明 是计算后得到的
    在div里直接调用计算函数即可
        价格100   $100   ¥100
        asdfgh  ASDFGH Asdfgh
    */

    //vm:app
    var app = new Vue({
        el:'#app',
        data:{
            html:'<span>这是一个span标签</span>',
            price:100, //¥100  包含快递费
            firstName:'zhang',
            lastName:'san',
            msg:'666',
            message:''
        },
        computed:{ //数据加载完成之后
            //计算属性 data里没有声明 是计算后得到的  拦截器可以实现类似功能
            goods:function () {
                return '¥' + (this.price + 6);
            },
            /*fullName:function () {
                return this.firstName + " " + this.lastName;
            },*/
            //计算属性默认值有取值器
            fullName:{
                //取值器
                get:function () {
                    return this.firstName + " " + this.lastName;
                },
                //赋值器
                set:function (newName) {
                    // this.fullName = newName; //报错 进入死循环,栈溢出
                    var names = newName.split(' ');
                    this.firstName = names[0];
                    this.lastName = names[names.length - 1];
                }
            }
            /*showFirstName:function () {
                return this.firstName.toUpperCase();
            }*/
        },
        methods:{
            showFirstName2:function (name) {
                //@click="showFirstName2('san')"
                return this.firstName.toUpperCase() + " " + name;
            }
        },
        watch:{ //监听器 检测到数据更改
            message:function (newvalue,oldvalue) {
                // this.message = value + 1;
                //可以实现数据绑定和实时更改
                this.msg = newvalue;
                //打印显示新的和旧的数据
                console.log(newvalue + " " + oldvalue);
            }
        }
    })
    // app.msg = "这是一条消息"; 后期无法添加属性
    // app.fullName("li si"); //会报错 fullName 不是方法

    //赋值改名
    app.fullName = "狗 蛋";
```

----------------------------分割线----------------------------     
### :class, :style      
body里的div绑定
```js
<div id="app">
    <!--添加class类名-->
    <!--方法1-->
    <div :class="{active:isActive,'img-text':has}">
        <!--方法2-->
        <div :class="classObj"></div>
        <!--方法3-->
        <div :class="[class1,class2]"></div>
        <!--方法4 嵌套写法-->
        <div :class="[class1,{div3:has}]"></div>
        <!--方法5-->
        <div :class="[isActive ? class1 : class2,class3]"></div>

        <!--:style 内联样式  方法1-->
        <div :style="{color:redColor,fontSize:font}">方式方法</div>
        <!--内联样式对象写法 值得推荐  方法2-->
        <div :style="styleObj">内联样式对象写法</div>
        <!--内联样式  方法3-->
        <div :style="[baseStyle,headerStyle]">方法3</div>
    </div>
</div>
```

在script里的代码
```js
/*
        :class  在html标签内添加class类名
    */
    var app = new Vue({
        el:'#app',
        data:{
            //方法1
            isActive:false, //false 不显示这个class 类名
            has:true, //true 显示这个class 类名
            //方法2
            classObj:{
                active:true, //false 不显示这个class 类名
                'img-has':true, //true 显示这个class 类名
            },
            //方法3
            class1:'div1',
            class2:'div2',
            class3:'div4',

            //:style 内联样式
            //方法1
            redColor:'red',
            font:'30px',
            //方法2
            styleObj:{
                color:'blue',
                fontSize:'50px',
                background:'pink',
            },
            //方法3
            baseStyle:{
                color:'red'
            },
            headerStyle:{ //.vue
                background:'green'
            }
        }
    })
```
----------------------------分割线----------------------------     
### v-if, v-else, v-else-if, v-for     
在body里的div绑定
```js
<div id="app">
    <div v-if="type === 'loading'">
        现在是正在加载
    </div>
    <!--<div v-else>
        加载完成
    </div>-->
    <div v-else-if="type === 'loaded'">
        加载完成
    </div>
    <div v-else-if="type === 'destory'">
        销毁
    </div>
    <div v-else>
        404
    </div>

    <!--
    vue有一个特性,值可以重复利用. 在input输入值后,切换input时,值依然在,不销毁.
    但有好也有坏
    取消数值重复利用是直接加 key ,让 key 不一样就可以
    -->
    <div v-if="type==='loading'">
        <input type="text" placeholder="请输入姓名..." key="input1">
    </div>
    <div v-else>
        <input type="text" placeholder="请输入年龄..." key="input2">
    </div>
    <button @click="changeType">切换</button>

    <ul>
        <!--{{data.element}}-->
        <!--在v-for作用块内 可以访问随意随意访问父级数据,但是父级不能访问 v-for 生成的数据,比如在父级使用 {{data.element}} 会报错-->
        <li v-for="(data,index) in list" :index="index">
            {{type}}
            {{data.element}}
            <!--index 自动添加编号-->
            <!--:index="index(自定义名字)" 自动给标签内联上添加 index 编号-->
            {{index}}
        </li>
        <!--{{data.element}}-->
    </ul>

    <ul>
        <li v-for="(value,key,index) in obj">
            <!--key 遍历对象,显示key值和value值,并且自动添加编号-->
            {{key}}
            {{value}}
            {{index}}
        </li>
    </ul>

    <ul>
        <li v-for="num in nums">
            {{num}}
        </li>
    </ul>
    <button @click="changeNum">改变数值</button>
    <button @click="changeName('狗蛋')">改变名字</button>
</div>
```
在script里的代码
```js
var app = new Vue({
        el:'#app',
        data:{
            type:'loading',
            list:[
                {
                    element:'el1'
                },
                {
                    element:'el2'
                },
                {
                    element:'el3'
                },
                {
                    element:'el4'
                }
            ],
            obj:{
                name:'shangsan',
                age:18,
                sex:'男'
            },
            nums:[1,2,3,4,5,6,7]
        },
        methods:{
            changeType:function () {
                this.type = 'loaded';
            },
            changeNum:function () {
                // this.nums[1] = 9; //这种方式虽然更改了但不能显示
                // console.log(this.nums[1]);

                //更新数组 Vue.set 和app.$set 一样的
                // 括号内(更新对象,更新位置,更新数值)
                // Vue.set(app.nums,1,9); //第一种写法
                app.$set(app.nums,1,9); //第二种写法
                //添加数值
                // this.nums.push(9)
            },
            changeName:function (name) {
                app.$set(app.obj,"name",name) //改name的value值
            }

        }
    });
```
----------------------------分割线----------------------------      
### v-model, 过滤器     
在body里的div绑定
```js
<div id="app">
    <!--v-model 绑定并联动数据-->
    <input type="checkbox" id="checkbox" v-model="checked">
    {{checked}}

    <div>
        <!--
        这波操作,你选中哪个,就自动把哪个 value 值写入 names 数组里
        v-model="names" 绑定空数组
        -->
        <input type="checkbox" id="zhangsan" value="zhangsan" v-model="names">
        <label for="zhangsan">zhangsan</label>
        <br>
        <input type="checkbox" id="lisi" value="lisi" v-model="names">
        <label for="lisi">lisi</label>
        <br>
        <input type="checkbox" id="wangwu" value="wangwu" v-model="names">
        <label for="wangwu">wangwu</label>
        <br>
        names:{{names}}
    </div>

    <div>
        <!--与上面类似-->
        <input type="radio" value="goudan1" name="goudan1" v-model="name">goudan1 <br>
        <input type="radio" value="goudan2" name="goudan2" v-model="name">goudan2 <br>
        <input type="radio" value="goudan3" name="goudan3" v-model="name">goudan3 <br>
        name:{{name}}
    </div>

    <div>
        <!--同上类似-->
        <!--multiple 多选-->
        <select name="" id="" v-model="selected" multiple>
            <option value="">请选择...</option>
            <option>A</option>
            <option>B</option>
            <option>C</option>
            <option>D</option>
        </select>
        select:{{selected}}
    </div>
    <!--调用upperCase方法,变成大写-->
    {{message | upperCase}}

</div>
```
script里的代码
```js
var app = new Vue({
        el:'#app',
        data:{
            checked:false,
            names:[],
            name:'',
            selected:[],
            message:'sdfsdfdf'
        },
        // 过滤器
        // 过滤器是一个通过输入数据，能够及时对数据进行处理并返回一个数据结果的简单函数。
        filters:{
            upperCase:function (value) {
                if (!value) return '';
                value = value.toString();
                return value.toUpperCase();
            }
        }
    })
```