# webpack Watch Mode

## webpack watch mode

```text
cmd:
webpack -watch --progress --display-reasons --color
```

{% hint style="success" %}
每次打包前删除相应的文件

1. npm i clean-webpack-plugin -D
2. new CleanWebpackPlugin\(\['dist'\]\)
{% endhint %}

## webpack-dev-server \(官方\)

live reloading 会刷新浏览器

路径重定向

https

浏览器中显示编译错误

接口代理

模块热更新

* devServer
  * inline
  * contentBase 提供内容的路径, 如果内容是静态的需要配置,默认是公共目录
  * port 监听端口
  * historyApiFallback  在使用html5的history api 的时候,如果访问某一个路径不会导致404,去指定一个重定向规则
  * https trus 就会本地生产证书
  * proxy 远程接口的代理
  * hot 打开  模块热更新 在某一个时间代码替换
  * openpage 最先代开的页面
  * lazy 开打,   不编译所有的页面, 只会在打开某个页面的时候才会去编译 ,设置为懒模式 在多页面开发用得多
  * overlay,  当页面出现错误的时候, 出现一个 页面遮罩错误提示



express + webpack-dev-middleware

