## 如何在js项目引入ts

1. 先引入项目依赖
`npm i` 或者 `npm install`

2. 查看TS官网搜索 Tutorials
查看 Migrating from JavaScript 中 [tutorial on React and Webpack](https://webpack.js.org/guides/typescript/)部分  (如何从JS迁移到TS)


3. 给项目引入ts依赖
`npm i --save-dev typescript ts-loader` 或者 `npm install --save-dev typescript ts-loader`

4. 项目根目录下新建tsconfig.json
``` json
{
  "compilerOptions": {
    "outDir": "./dist/",
    "noImplicitAny": true,
    "module": "es6",
    "target": "es5",
    "jsx": "react",
    "allowJs": true,
    "moduleResolution": "node"
  }
}
```

5. 修改入口文件
将原有的入口js文件改为ts文件
1. 修改 webpack.config.js 入口文件
```diff
- entry: './src/index.js',
+ entry: './src/index.ts',
```

- 修改src下入口文件后缀
``` diff
-  index.js
+  index.ts
```

6. 在 webpack.config.js 新增 ts 相关的 rules

1. module.exports 新增属性 resolve
```diff
+  resolve: {
+    extensions: ['.tsx', '.ts', '.js'],
+  },
```
使用 ts-loader 可以替换掉 babel-loader
2. rules中替换loader
```diff
-   {
-       test: /\.m?js$/,
-       exclude: /node_modules/,
-       use: {
-           loader: "babel-loader",
-       }
-   }
+ 
+  {
+       test: /\.tsx?$/,
+       use: 'ts-loader',
+       exclude: /node_modules/,
+  },
```

7. 开启sourceMap
方便后续开发debug

1. tsconfig.json 开启sourceMap
```diff
{
    "compilerOptions": {
      "outDir": "./dist/",
      "sourceMap": true,
      "noImplicitAny": true,
      "module": "es6",
      "target": "es5",
      "jsx": "react",
      "allowJs": true,
      "moduleResolution": "node",
    }
}
```

2. webpack.config.js 替换sourceMap
```diff
-   devtool: 'source-map'
+   devtool: 'inline-source-map'
```

8. babel-loder
如果一定要 babel-loder (会失去类型检查)
1. 安装 @babel/preset-typescript
`npm install --save-dev @babel/plugin-transform-typescript`

2. 更改设置

```json
{
  "presets": ["@babel/preset-env"],
  "plugins": ["@babel/plugin-transform-typescript"]
}
```

