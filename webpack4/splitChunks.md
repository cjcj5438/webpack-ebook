# splitChunks
> webpack4废弃了CommonsChunkPlugin插件，使用optimization.splitChunks和optimization.runtimeChunk来代替。关于runtimeChunk参数，有的文章说是提取出入口chunk中的runtime部分，形成一个单独的文件，由于这部分不常变化，可以利用缓存。


>  google社区：The runtimeChunk option is also specified to move webpack's runtime into the vendors chunk to avoid duplication of it in our app code. 指定了runtimeChunk选项，将webpack的运行时移动到vendors中，以避免在应用程序代码中重复运行时。 
## splitChunks默认的代码自动分割要求 ##
1. node_modules中的模块或其他被重复引用的模块
2. 分离前模块最小体积下限（默认30k，可修改）
3. 对于异步模块，生成的公共模块文件不能超出5个（可修改）
4. 对于入口模块，抽离出的公共模块文件不能超出3个（可修改）

## 参数配置 ##
[SplitChunksPlugin的demo](https://github.com/cjcj5438/SplitChunksPlugin) 常规配置
```javascript
module.exports = {
  //...
  optimization: {
    splitChunks: {
      chunks: 'async',//默认只作用于异步模块，为`all`时对所有模块生效,`initial`对同步模块有效
      minSize: 30000,//合并前模块文件的体积
      minChunks: 1,//最少被引用次数
      maxAsyncRequests: 5,
      maxInitialRequests: 3,
      automaticNameDelimiter: '~',//自动命名连接符
		// 缓存组 看配置可以简单理解的。
      cacheGroups: {
        vendors: {
          test: /[\\/]node_modules[\\/]/,
          minChunks:1,//敲黑板
          priority: -10//优先级更高
        },
        default: {
          test: /[\\/]src[\\/]js[\\/]/
          minChunks: 2,//一般为非第三方公共模块
          priority: -20,
          reuseExistingChunk: true
        }
      },
      runtimeChunk:{
          name:'manifest'
      }
    }
  }
```
这里讲下我对runtimeChunk的理解：
runtimeChunk是webpack固定生成的一段代码，用来维护模块之间的以来关系的，比如给每个模块一个ID之类的，这部分代码跟你写的代码完全没有关系，所以单独切割出来能够防止他的变化影响你自己的代码的hash变化

**优化持久化缓存**的, runtime 指的是 webpack 的运行环境(具体作用就是模块解析, 加载) 和 模块信息清单, 模块信息清单在每次有模块变更(hash 变更)时都会变更, 所以我们想把这部分代码单独打包出来, 配合后端缓存策略, 这样就不会因为某个模块的变更导致包含模块信息的模块(通常会被包含在最后一个 bundle 中)缓存失效. optimization.runtimeChunk 就是告诉 webpack 是否要把这部分单独打包出来.

## 代码分割实例 ##
### 单页面应用 ###
单页面应用只有一个入口文件，splitChunks的主要作用是将引用的第三方库拆分出来。从下面的分包结果就可以看出，node_modules中的*第三方引用被分离了出来*，放在了vendors-main.[hash].js中。
### 多页面应用 ###
demo ↑