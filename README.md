# metamask-extension

## Install

Install [Node.js](https://nodejs.org/en/) version 10  (not version 12)

[node-gyp#on-windows](https://github.com/nodejs/node-gyp#on-windows)
`npm install --global --production windows-build-tools`  
`npm config set msvs_version 2017`  


app\_locales\index.json 減少語系

## gulpfile.js

only build chrome

```js
const browserPlatforms = [
  //'firefox',
  'chrome',
  // 'brave',
  // 'opera',
]

gulp.task('dev:copy',
  gulp.series(
    gulp.parallel(...copyDevTaskNames),
    //'manifest:dev',
    'manifest:chrome',
    //'manifest:opera'
  )
)
```

## 無法載入擴充功能

無法載入背景指令碼「bg-libs.js」

```js
createTasksForBuildJsDeps({ filename: 'bg-libs', key: 'background' })

gulp.task('dev:extension',
  gulp.series(
    'clean',
    'dev:scss',
    gulp.parallel(
      'build:extension:js:deps:background', //build bg-libs.js
      //'build:extension:js:deps:ui',  // build ui-libs.js
      'dev:extension:js',
      'dev:copy',
      'dev:reload'
    )
  )
)
```

popup.html

```html
    <script src="./ui-libs.js" type="text/javascript" charset="utf-8"></script>
    <script src="./ui.js" type="text/javascript" charset="utf-8"></script>
```

## sourcemaps

關閉 sourcemaps  可將 `ui.js` 檔案 50M降到10M

```js
const sourcemaps = require('gulp-sourcemaps')
function createTasksForBuildJsExtension (
    bundleTaskOpts = Object.assign({
    buildSourceMaps: false, // true
    sourceMapDir: '../sourcemaps',
```

```js
  const bundleTaskOpts = Object.assign({
    buildSourceMaps: false, // true
    sourceMapDir: '../sourcemaps',
    minifyBuild: true,
    devMode: false,
  })
```

## Debug

update: yarn.lock 要複製至

Cannot find module 'multicodec/src/name-table'

`yarn add multicodec`

Error: Cannot find module '@babel/runtime/core-js/object/define-property'

`yarn add ethjs` 會幫裝　core-js@2.6.11　提供　web3　使用

## manifest.json

remove vendor/trezor

```js
  "content_scripts": [
    {
      "matches": [
        "file://*/*",
        "http://*/*",
        "https://*/*"
      ],
      "js": [
        "contentscript.js"
      ],
      "run_at": "document_start",
      "all_frames": true
    }
  ],
```