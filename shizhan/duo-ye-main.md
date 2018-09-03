# 多页面

![](https://i.imgur.com/TXp609i.png)

```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    mode: 'production',
    entry: {
        "main": __dirname + "/src/indexController.js",
        "about": __dirname + "/src/aboutController.js",
        "list": __dirname + "/src/listController.js",
    },
    output: {
        filename: '[name].boundle.js',
        path: __dirname + '/build'
    },
    plugins: [
        //index.html
        new HtmlWebpackPlugin({
            title:'MainPage',
            template:'src/index.html',
            filename:'index.html',
            templateParameters:{
                param1:'tony stark',
                param2:'bruce banner'
            },
            chunks:['main'],
        }),
        //about.html
        new HtmlWebpackPlugin({
            title:'AboutPage',
            template:'src/about.html',
            filename:'about.html',
            templateParameters:{
                param1:'tony stark',
                param2:'bruce banner'
            },
            chunks:['about'],
        }),
        //list.html
        new HtmlWebpackPlugin({
            title:'ListPage',
            template:'src/list.html',
            filename:'list.html',
            templateParameters:{
                param1:'tony stark',
                param2:'bruce banner'
            },
            chunks:['list'],
        }),
    ]
}
```