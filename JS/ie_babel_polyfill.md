## IE対応 やってみたことメモ

試したこと

- babelを利用できるようにmoduleを導入
  - @babel/core
  - @babel/preset-env
  - @babel/preset-react
  - @babel/preset-typescript
  - @babel/plugin-proposal-class-properties
  - @babel/plugin-proposal-decorators
  - babel-loader
  - core-js
  - regenerator-runtime
- webpack.config.js 内のloaderの定義をts-loaderからbabel-loaderへ
```
diff --git a/js/webpack.config.js b/js/webpack.config.js
index 1fb86302..b9223a16 100644
--- a/js/webpack.config.js
+++ b/js/webpack.config.js
@@ -42,7 +42,7 @@ module.exports = () => ({
     rules: [
       {
         test: /\.tsx?$/,
-        use: ["ts-loader"],
+        use: ["babel-loader"],
         exclude: /node_modules/,
       },
```
- .babelrcを作成
```
  "presets": [
    [
      "@babel/preset-env",
      {
        "useBuiltIns": "usage",
        "corejs": {
          "version": 3,
          "proposals": false
        }
      }
    ],
    ["@babel/preset-react"],
    ["@babel/preset-typescript"]
  ],
  "plugins": [
    ["@babel/plugin-proposal-decorators", { "legacy": true }],
    ["@babel/plugin-proposal-class-properties", { "loose": true }]
  ],
  "targets": { "ie": "11" }
}
```
- `js/src/index.tsxのファイル先頭に以下の二行を追加
```
import 'core-js/stable';
import 'regenerator-runtime/runtime';
```
