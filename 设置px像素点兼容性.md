手淘

npm install lib-flexible --save-dev     
main.js中       
import 'lib-flexible'       

px转rem     

npm install px2rem-loader --save-dev      

build下utils.js     

```
exports.cssLoaders = function (options) {  
    <!--添加-->     
    const px2remLoader = {      
        loader:'px2rem-loader',     
        options:{       
          remUnit:75 //按照设计图除10       
        }       
    }       
}    
function generateLoaders (loader,loaderOptions) {        
    <!--改-->       
    const loaders = options.usePostCSS ? [cssLoader,postcssLoader,px2remLoader]:[cssLoader,px2remLoader]        
}       
```


css里直接写设计图上的尺寸       

.text{      
    <!--如果不想变化-->     
    border: 1px solid black;/*no*/      
    <!--如果现在检查里以px显示-->       
    width: 128px;/*px*/     
}       

