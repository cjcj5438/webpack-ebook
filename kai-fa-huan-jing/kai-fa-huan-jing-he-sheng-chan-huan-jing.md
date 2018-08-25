---
description: 两个环节的区分
---

# 开发环境和生产环境

* 开发模式 会需要哪些呢
  * 模块热更新
  * sourceMap
  * 接口代理
  * 代码规范检查
* 生产环境需要哪些呢
  * 提取公共代码
  * 压缩混淆
  * 文件压缩或是 base64 编码
  * 去除无用的代码
* 两个环境的共同点
  * 同样的入口
  * 同样的代码处理 \(loader处理\)
  * 同样的解析配置
* 如何做呢? 
  * 可以使用webpack-merge 合并配置
  * webpack.dev.conf.js
  * webpack.prod.conf.js
  * webpakc

