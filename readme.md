# Making react app emvironment using webpack

## Initialize
```bash
# make package.json
$ npm init -y

# edit package.json if you need.
# vim package.json

# make directories for source and built js files.
$ mkdir src
$ mkdir dist
```
## Install webpack
```bash
$ npm install -D webpack webpack-cli
```

## Install babel
```bash
$ npm install -D @babel/core babel-loader @babel/preset-env @babel/preset-react
```

Make your babel configuration file.
```bash
$ touch .babelrc
```

```json :.babelrc
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}
```

## Configure webpack

Make your webpack configuration file.
```bash
$ touch webpack.config.js development.js
```

Make sure the main config loads development settings.
```javascript :webpack.config.js
module.exports = require('./webpack.development');
```

Write your development settings in webpack.development.js
```javascript :webpack.development.js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    'main': [path.resolve(__dirname, 'src', 'index.jsx')]
  },

  output: {
    path: path.join(__dirname, 'dist'),
    filename: 'bundle.js'
  },

  module: {
    rules: [
      {
        test: /\.jsx$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader'
        }
      },
      {
        test: /\.m?js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader'
        }
      }
    ]
  },

  resolve: {
    extensions: ['.js', '.jsx']
  },

  plugins: []
};
```

## Install React
```bash
$ npm install -S react react-dom
```

Make entry point js file designated in your webpack.development.js.
```bash
$ touch src/index.jsx
```

```javascript :index.jsx
import React from 'react';
import ReactDOM from 'react-dom';

class App extends React.Component {
  render () {
    return (<p>Hello world!</p>);
  }
}

ReactDOM.render(
  <App />,
  document.getElementById('app')
);
```

Make index.html to apply React components.
```html :index.html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Index</title>
  </head>
  <body>
    <div id="app" />
    <script src="./bundle.js"></script>
  </body>
</html>
```

# Build and run development server

Install webpack-dev-server
```bash
$ npm install -D webpack-dev-server
```

Add configuration of development server into webpack.development.js
```javascript :webpack.development.js
module.exports = {
  // ...

  devServer: {
    contentBase: path.join(__dirname, 'dist'),
    open: true,
    openPage: 'index.html',
    port: 8000
  }

  // ...
}
```

Register start script command in package.json.
```json :package.json
{
  //...
  "scripts": {
    "start": "webpack-dev-server --devtool inline-source-map"
  }
  //...
}
```

Run server.
```bash
$ npm start

> react-wp@1.0.0 start C:\Users\isd\source\react\react-wp
> webpack-dev-server --devtool inline-source-map

i ｢wds｣: Project is running at http://localhost:8080/
i ｢wds｣: webpack output is served from /
i ｢wdm｣: Hash: 861957d02c083389c6ae
Version: webpack 4.30.0
Time: 1597ms
Built at: 2019-04-25 11:34:34
    Asset      Size  Chunks             Chunk Names
bundle.js  3.01 MiB    main  [emitted]  main
Entrypoint main = bundle.js
[0] multi (webpack)-dev-server/client?http://localhost ./src/index.jsx 40 bytes {main} [built]
[./node_modules/ansi-html/index.js] 4.16 KiB {main} [built]
[./node_modules/events/events.js] 13.3 KiB {main} [built]
[./node_modules/loglevel/lib/loglevel.js] 7.68 KiB {main} [built]
[./node_modules/querystring-es3/index.js] 127 bytes {main} [built]
[./node_modules/react-dom/index.js] 1.33 KiB {main} [built]
[./node_modules/react/index.js] 190 bytes {main} [built]
[./node_modules/url/url.js] 22.8 KiB {main} [built]
[./node_modules/webpack-dev-server/client/index.js?http://localhost] (webpack)-dev-server/client?http://localhost 8.26 KiB {main} [built][./node_modules/webpack-dev-server/client/overlay.js] (webpack)-dev-server/client/overlay.js 3.59 KiB {main} [built]
[./node_modules/webpack-dev-server/client/socket.js] (webpack)-dev-server/client/socket.js 1.05 KiB {main} [built]
[./node_modules/webpack-dev-server/node_modules/strip-ansi/index.js] (webpack)-dev-server/node_modules/strip-ansi/index.js 161 bytes {main} [built][./node_modules/webpack/hot sync ^\.\/log$] (webpack)/hot sync nonrecursive ^\.\/log$ 170 bytes {main} [built]
[./node_modules/webpack/hot/emitter.js] (webpack)/hot/emitter.js 75 bytes {main} [built]
[./src/index.jsx] 2.66 KiB {main} [built]
    + 23 hidden modules
i ｢wdm｣: Compiled successfully.
```