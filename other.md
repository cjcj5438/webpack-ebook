# other

## webpack-bundle-analyzer ##
> webpack的可视化资源分析工具webpack-bundle-analyzer的使用

```
npm/cnpm install --save-dev webpack-bundle-analyzer
```
```
config/index.js文件中
module.exports = {
  build: {
  ...
    // Run the build command with an extra argument to
    // View the bundle analyzer report after build finishes:
    // `npm run build --report`
    // Set to `true` or `false` to always turn it on or off
    bundleAnalyzerReport: process.env.npm_config_report
  }
  ...
}
```

```
webpack生产环境中 build/webpack.prod.conf.js文件中
if (config.build.bundleAnalyzerReport) {
  var BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin
  webpackConfig.plugins.push(new BundleAnalyzerPlugin())
}

```
**控制台输入cnpm run build --report**
等待构建完成后，在浏览器中输入localhost:8888打开分析结果，就可以开始分析啦。