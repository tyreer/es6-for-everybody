## Module 13 - JavaScript Modules and Using npm

#### 45 - JavaScript Modules and WebPack 2 Tooling Setup
Decent demo but Webpack and Babel versions need to be tied to earlier releases.
+ Need to look into "zero-config" Webpack 4 for current standards
+ _es6-module-instructions.md_ have packages, etc.
+ Big picture is that __modules__ are files with one or more functions that can be made available to other modules. We want to write code in modules, and Webpack provides the __bundler__ to assemble all the modules together

(1) Make __entry point__ in _app.js_

(2) Create _package.json_ + install modules + add run scripts
  + ```npm init```
  + ```npm i -s lodash```
  + ```npm i -D lodash```
    + ___--save-dev (-D)___ = tool for development and not a part of the application we ship client side

+ In _package.json_
  ```json
  "scripts": {
      "build": "webpack --progress --watch"
  },
  ```

(3) Make _webpack.config_ file

In _webpack.config.js_
```js
const webpack = require('webpack');
```
+ We use CommonJS _require_ here because __Node doesn't deal with ES6 _import_ syntax__
  + Interesting to note that Webpack is running in Node rather than in the browser. Makes sense as Webpack is bundling all the code before it is delivered to the browser

```js
const nodeEnv = process.env.NODE_ENV || 'production';
```
+ When we make a __production build__, it will be much smaller than our development build.

```js
module: {
  loaders: [
  ]
}
```
+ Knowing that it can be used to accept almost anything, __loaders__ answers: _How should Webpack handle different file types?_

#### 45 - Creating your own Modules
+ Good to note that modules like these could also be published to npm, which is what we're doing at work.
+ Also, variables are scoped to their module

_Two types of exports in ES6_

__Default exports__
```js
export default apiKey;
```
+ The main thing a module needs to export
+ Can be named anything on import

```js
import apiKey from './src/config';
```
```js
import alternateNameforApiKey from './src/config';
```

__Named exports__
```js
export const apiKey = '123';
```
+ For methods, variables, etc.

```js
import { apiKey } from './src/config';
```
+ __Names must match__ between import and export
  + The curly braces in the __syntax for importing a named export__ look like destructuring, but are not.

```js
import { apiKey as key } from './src/config';
```
+ __Renaming with _as___ is useful if you've already got a variable using a particular namespace

```js
const age = 100;
const dog = 'snickers';

export { age as old, dog };
```
+ Can export __multiple things and rename__

#### 47 - More ES6 Module Practice

```js
export default function User(name, email, website) {
  return { name, email, website };
}
```
+ Nice model of __object literal same-name key-value syntax__

```js
import User, { createURL, gravatar } from './src/user';
```
+ Importing the default export as well as some named exports

```js
import slug from 'slug';
import { url } from './config';
import md5 from 'md5';

export default function User(name, email, website) {
  return { name, email, website };
}

export function createURL(name) {
  return `${url}/users/${slug(name)}`;
}

export function gravatar(email) {
  const hash = md5(email.toLowerCase());
  const photoURL = `https://www.gravatar.com/avatar/${hash}`;
  return photoURL;
}
```
+ Nice little example of a tidy module
