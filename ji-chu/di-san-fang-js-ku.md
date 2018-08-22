# 第三方js库

webpack.providePlugin  
import-loader  
window

{% tabs %}
{% tab title="First Tab" %}
直接head标签里面嵌入 cdn就可以
{% endtab %}

{% tab title="Second Tab" %}
> 把jq 注入到每个模块中

```text
npm i jquery -S
```

```text
 plugins:[
       new webpack.ProvidePlugin({
             $:"jquery"
       })
   ]
```
{% endtab %}

{% tab title="" %}
本地jq

```text
  resolve:{
        alias:{
              //为什么加$呢? 确定知识把jquery这个关键字解析到某一个文件下,还是确切匹配
              //同时必须和下面的模块名字一致
              jquery$:path.resolve(__dirname,'src/libs/jquery.min.js')
        }
  }
  plugins:[
       new webpack.ProvidePlugin({
             $:"jquery"
       })
   ]
```
{% endtab %}

{% tab title="" %}
```text
modules:{ 
       rules:[
              {
                     test:path.resolv(__dirname,'src/app.js'),
                     use:[
                            {
                                   loader:'import-loader',
                                   options:{
                                          $:'jquery'
                                   }
                            }
                     ]
              }
       ]
},
plugins:[
       /*就可以注释掉这个
       new webpack.ProvidePlugin({
             $:"jquery"
       })
       */
   ]
```
{% endtab %}

{% tab title="" %}

{% endtab %}
{% endtabs %}

## 

