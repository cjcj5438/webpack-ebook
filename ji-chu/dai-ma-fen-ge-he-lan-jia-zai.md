# 代码分割和懒加载



import\(\)放回的是Promise  可以点.then

```text
import(
    /*webpackChunkName:async-chunk-name*/ 魔法注释
    /*webpackMode:lazy*/
    modulename
)
```

## 代码分割

1. 分离业务代码和第三方依赖
2. 分离业务代码和业务公共代码和第三方依赖
3. 分离首次加载和访问后加载的代码

webpack内置的方法 动态加载模块

### require.ensure 

* **\[\]:dependencies**
* **callback**
* **errorCallback**
* **chunkName**



实战

```text
//分离第三方依赖
import './subPageA'
import './subPageB'
/*
异步加载 抽离第三方依赖
如果只是require.ensure 只是把代码加载到页面中. 并不会执行
下面在require('lodash') 才会真正的执行它
*/
require.ensure([/*'lodash' 可以省略 但是省略之后下面的代码也变成了异步了*/],function(){
    var _=require('lodash')
},'vendor')//--->指定chunk name 

export default 'pageA'
```

```text
分离业务代码
if(page==='subpageA'){
    require,ensure(['./subPageA'],function(){
        var subpageA=require('./subPageA')
    })//比写chunkname 就会默认用数字
}

if(page==='subpageB'){
    require,ensure(['./subPageA'],function(){
        var subpageB=require('./subPageB')
    })
}
```

### require.include 

可以把公共的模块抽离出来

此时subPageA和subPageB 都有公共的业务模块怎么办呢?我们是使用CommonseChunkPlugins还是使用require.include 抽离公共代码呢?

CommonseChunkPlugins:适用于多require.include用于单chunk 的.所以此时我在加强下代码质量

```text
require.include('./moduleA')//加载但并不运行
分离业务代码
if(page==='subpageA'){
    require,ensure(['./subPageA'],function(){
        var subpageA=require('./subPageA')
    })//比写chunkname 就会默认用数字
}
if(page==='subpageB'){
    require,ensure(['./subPageA'],function(){
        var subpageB=require('./subPageB')
    })
}
```

### 用import 动态加载

import\(\)放回的是Promise  可以点.then

```text
import(
    /*webpackChunkName:async-chunk-name*/ 魔法注释
    /*webpackMode:lazy*/
    modulename
)
```

```text
分离业务代码
if(page==='subpageA'){
   // 要是用import来写那么js 代码会加载进来同时还会执行
    import(/*webpackChunkName:subpageA*/'./subPageA').then(function(subpageA){/*conding...*/})
}

if(page==='subpageB'){
    require,ensure(['./subPageA'],function(){
        var subpageB=require('./subPageB')
    })
}
```

这个时候有一需求就是用CommonseChunkPlugins抽出来的公共代码也异步也异步加载

```text
plugins:[
    new webpack.optiomize.CommonsChunkPlugign({
        async:'async-common',
        children:true,
        minChunk:2
    })
]
```

