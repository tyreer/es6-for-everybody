## Module 14 - ES6 Tooling

#### 48 - Tool-Free Modules with SystemJS (+bonus BrowserSync setup)
https://github.com/systemjs/systemjs

Nice model of a quick way to set up module bundling w/o Webpack overhead
+ Not for production because too slow, but useful for a quick exercise

```html
<script src="https://cdn.polyfill.io/v2/polyfill.js"></script>
<script src="https://jspm.io/system@0.19.js"></script>
<script>
  System.config({ transpiler: 'babel' });
  System.import('./main.js');
</script>
```

```js
import { sum, kebabCase } from 'npm:lodash';
console.log(kebabCase('Wes is soooo cool ⛓⛓⛓⛓'));
```
+ Note: _npm:lodash_

```json
"scripts": {
  "server": "browser-sync start --directory --server  --files '*.js, *.html, *.css'"
},
```
+ __BrowserSync start script__
  + Just need to _save-dev_ BrowserSync

#### 49 - All About Babel + npm scripts
+ If you're not using modules, can transpile via Babel w/o Webpack

```bash
npm install babel-cli@next
```
Allows use of Babel 7 features

```json
"dependencies": {
  "babel-cli": "^7.0.0-beta.1",
  "babel-preset-env": "^2.0.0-beta.1"
},
```

```json
"scripts": {
  "babel": "babel app.js --watch --out-file app-compiled.js"
},
```

```json
"babel": {
  "presets": [
    [
      "env",
      {
        "targets": {
          "browsers": [
            "last 2 versions",
            "safari >= 8"
          ]
        }
      }
    ]
  ]
},
```
+ The _"babel"_ property in a _package.json_ can accept all the same settings as a _.babelrc_
+ __"presets" > "env"__ allows babel to manage the scope of transpiling required to to support designated targets.
  + As browser's change, this will automatically adapt
  + Very similar to __auto-prefixer__
  + __"env" may be a wiser choice than "es2015"__ because it avoids unnecessarily transpiling JS that is already supported by most browsers
https://babeljs.io/docs/plugins/preset-env/

```json
"plugins": [
  "transform-object-rest-spread"
]
```

#### 50 - Polyfilling ES6 for Older Browsers

+ Babel accounts for the __syntax__ changes in ES6, but new methods like __Arrary.from()__ are not transpiled
+ They need a polyfill
__Two options__
+ babel-polyfill
+ https://polyfill.io/v2/docs/
  + Pretty cool as it dynamically detects a browser's user agent and polyfills only as needed
