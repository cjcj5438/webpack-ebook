# 编译 typescript

安装
npm i typescript ts-loader --save-dev
或
npm i typescript awesome-typecript-loader -D

配置
tsconfig.json
webpack.config.js
配置选项
官网/docs/handbook/compiler-options.html
常用的选项
    compilerOptions
    include
    exclude

```js
//webpack.config.js
module.exports={
    entry:{
        'app':'./app.ts'
    },
    output:{
        filename:'[name].bundle.js'
    },
    module:{
        rules:{
            test:/\.tex?$/,
            ues:{
                loader:'ts-loader'
            }
        }
    }
}
```   
```json
{
  "compilerOptions":{
    "module":"commonjs",
    "target":"es5",
    "allowJS":true,
    "typeRoots":["./node_modules/@type","./typings/modules"]
  },
  "include":[ "./src/*"],
   "exclude":[
      "./node_module"
   ]
}
```

安装声明文件 主要是在ts环境下: 我们已 lodash为例
npm i lodash --save  
npm i @types/lodash 
为什么要装@types的声明文件呢? 可以帮助我们使用的过程中提示 严谨的语法错误
但是每次安装 都要按装两遍. 解决方案
npm install typings
typings install lodash****
