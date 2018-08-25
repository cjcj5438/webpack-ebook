# Eslint检查代码格式

```text
npm i eslint eslint-loader eslint-plugin-htm eslint-friendly-formatter -D
```

eslint

* eslint-loader
  * options.failOnWarning 
  * options.failOnError
  * options.formatter
  * options.outputReport 输出代码报告

eslint-plugin-html 检查html 里面的js

eslint-friendly-formatter错误警告格式

.eslintrc.\* 或者是在 package.json 中的 eslintConfig里面写

配置eslint

* javascript Standard style \(http://standardjs.com\)遵循这里的js规范 
  * eslint-config-standard
  * eslint-plugin-promise
  * eslint-plugin-standard
  * eslint-plugin-import
  * eslint-plugin-node
  * eslint-config-xxx v

devServer.overlay   可以在浏览器中看到, 就可以用在控制台中看代码了



