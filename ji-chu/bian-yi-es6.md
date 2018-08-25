# 编译 ES6

npm install babel-loader@8.0.0-beta.0 @babel/core -D 语法编译

npm install --save-dev babel-cli babel-preset-env 或者 npm install @babel/preset-env -S 语法规范 \`\`\`javascript 1.8 rules:\[ { test:/.js$/, use:{ loader:'babel-loader', options:{ presets:\[ \['@babel/preset-env',{ targets:{ browsers:\['&gt; 1%','last 2 versions'\] } }\] \] } } } \]

```text
npm i babel-polyfill -s
全局垫片  
import "babel-polyfill"

npm install babel-runtime babel-polyfill -save
npm install babel-plugin-transform-runtime -save-dev
局部垫片
```json
//.babelrc
{
  "presets":[
      ["@babel/preset-env",{
          "targets":{
              "browsers":["last 2 versions"]
          }
      }]
  ],
  "plugins":[
      "transform-runtime"
  ]
}
```

