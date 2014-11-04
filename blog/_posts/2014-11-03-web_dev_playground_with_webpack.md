---
layout: post
title: "Web Dev Playground with Webpack"

tags: [webpack]

meta-description: "The easiest way to setup web dev playground with webpack."
meta-robots: ""
---

[webpack][] is an amazing module bundler. I used to use [browserify](http://browserify.org/) for bundling modules but ever since I learned about webpack, I didn't have to think twice.

How to configure webpack for big projects deserves its own post. However, here I'd like to share how to set up a quick web dev playground using webpack.


### Install
```
$ npm install -g webpack webpack-dev-server
$ npm init
  ...
$ npm install css-loader style-loader
```


### Setup
```
$ mkdir playground
$ cd playground
playground $ vi webpack.config.js
```

```js
// webpack.config.js
module.exports = {
  entry: './main.js',
  output: {
    path: __dirname,
    filename: 'bundle.js',
  },
  module: {
    loaders: [
      { test: /\.css$/, loader: 'style!css' }
    ],
  },
};
```
The simple configuration above is mostly self-explanatory. The webpack's entry point is `main.js` and it's going to bundle all the files following `require()` calls and outputs it to `bundle.js`. For loading css files, we specify it to load it first using `css-loader` and then `style-loader` (i.e. `'style!css'`; read it right-to-left).

Let's write a simple hello world in `main.js`.
```js
// main.js
console.log('hello world');
```


### Run
```
$ webpack-dev-server --progress --colors --watch
http://localhost:8080/webpack-dev-server/
webpack result is served from /
content is served from /Users/brianpark/playground
Hash: 98878c79082b0e0d51b7
Version: webpack 1.4.12
Time: 743ms
    Asset   Size  Chunks             Chunk Names
bundle.js  10985       0  [emitted]  main
chunk    {0} bundle.js (main) 23617 [rendered]
    [0] ./main.js 148 {0} [built]
    [1] ./~/css-loader/cssToString.js 352 {0} [built]
    [2] ./~/css-loader!./main.css 173 {0} [built]
    [3] ./~/style-loader/addStyles.js 5513 {0} [built]
    [4] ./main.css 940 {0} [built]
    [5] ./~/transducers.js/transducers.js 16491 {0} [built]

```
Now, if you go to http://localhost:8080/webpack-dev-server, you'll see a list of links. Click on [webpack-dev-server](http://localhost:8080/webpack-dev-server/bundle) link to enjoy the live-reloading functionality (see Iterating section below).

Do you see 'hello world' in the console? Great!


### Iterating
webpack is a bundler but with our simple hello world, we couldn't realize the true power of webpack yet. Let's create another file `name.js`
```js
// name.js
module.exports = 'brian park';
```
And inside `main.js`, require `name.js`.
```js
// main.js
var name = require('./name');
console.log('hello', name);
```
Save this file and watch the page you opened before reload automatically and check if console says 'hello brian park'. All you had to do was just coding it normally. webpack automatically bundled up all the necessary files and served it. How awesome is that?


## Loading CSS
Let's spice it up a bit and load CSS using webpack, too. Let's create `main.css`.
```css
/* main.css */
html, body {
  background: yellow;
  height: 100%;
}
```
How do we load this CSS now? Simple, just `require()` it.
```js
// main.js
require('./main.css');
var name = require('./name');
console.log('hello', name);
```
Once you save the above files, you will see the magic happens. The page is automatically reloaded with yellow background!

To briefly explain how webpack loads css (remember module loaders in `webpack.config.js`?), it first reads css code from `main.css` and resolves imports and url in it (using `css-loader`) and then writes javascript that puts the css code into &lt;style&gt; tag (using `style-loader`).

Hope you have lots of fun with webpack!


[webpack]:   http://webpack.github.io/
