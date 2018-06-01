# vue-axios
[TOC]

## 开始使用
### 下载axios
**终端输入**        
`npm install axios --save-dev`

### 引入axios
**main.js**

```javascript
//引入axios包
import axios from 'axios' 

//将axios设置成全局
Vue.prototype.$http = axios
```

## 接口代理
**config > index.js**       

```javascript
dev: {
    //接口代理
    proxyTable: {
      '/api':{
        target:'http://pianke.me',
        changeOrigin:true,
        pathRewrite:{
          '^/api':''
        }
      }
    },
}

```
## 模块
**(片刻网为例，接口破解)**            
**解析事件**            
`npm install dateformat --save-dev`         
**md5加密**         
`npm install md5 --save-dev`        
**Base64加密**      
`npm install Base64 --save-dev`         

### 引入、使用模块
**src > common > requestParams.js**

```javascript
import dateformat from 'dateformat/'
import MD5 from 'md5/md5'
import Base64 from 'Base64'

var getParams = function () {
  let date = new Date();
  let time = dateformat(date.getTime(),'yyyymmddHHMMss');
  let sig = MD5('0' + '' + time).toUpperCase();
  let formatString = ''+':'+ time;
  let Authorization = Base64.btoa(formatString);
  let timeday = dateformat(date.getTime(),'yyyy-mm-dd');
  let h = date.getHours();
  return{
    h,
    timeday,
    sig,
    Authorization,
    time
  }

}
//导出
export default {
  getParams
}
```

### 引入到main.js
**模块的引入**

```javascript
import params from  './common/requestParama'
import axios from 'axios' //引入axios包

//将axios设置成全局
Vue.prototype.$http = axios
Vue.prototype.$util = {}
Vue.prototype.$util.params = params

```

## 网络请求

**App.vue**

```javascript
export default {
  name: 'App',
  components: {
    HelloWorld
  },
  methods:{
    //获取header数据
    getHeaderData:function () {
      //this.$http.get() === axios.get()
      let params = this.$util.params.getParams();
      this.$http.get('/api/version5.0/headline/recent.php?location=special&sig='+ params.sig,{
        headers:{
          Authorization:params.Authorization
        }
      }).then(function (res) {
        console.log(res);
      })


    },

  },
  mounted:function () {
    //网络请求的过程不要写在mounted里 定义在methods里 在mounted里调用
    this.getHeaderData();

  }
}

```

### Get请求

```javascript
const url = 'http://vue.studyit.io/api/getnewslist'

// url中带有query参数
axios.get('/user?id=89')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

// url和参数分离，使用对象
axios.get('/user', {
  params: {
    id: 12345
  }
})
```

### Post请求

```javascript
// 使用 qs 包，处理将对象序列化为字符串
// npm i -S qs
// var qs = require('qs')
import qs from 'qs'
qs.stringify({ 'bar': 123 }) ===> "bar=123"
axios.post('/foo', qs.stringify({ 'bar': 123 }))

// 或者：
axios.post('/foo', 'bar=123&age=19')
```

```javascript
const url = 'http://vue.studyit.io/api/postcomment/17'
axios.post(url, 'content=点个赞不过份')

axios.post('/user', qs.stringify({
    firstName: 'Fred',
    lastName: 'Flintstone'
})).then(function (response) {
    console.log(response);
}).catch(function (error) {
    console.log(error);
});
```