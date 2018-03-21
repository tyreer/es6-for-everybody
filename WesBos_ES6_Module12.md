## Module 12 - Code Quality with ESLint

#### 39 - Getting Started with ESLint
```
npm install -g eslint
```
+ Globally installing ESLint

__.eslintrc__

__"env"__: what environments will you use the JS in
```json
"env": {
  "es6": true,
  "browser": true
},
```
+ Hella options:  https://eslint.org/docs/user-guide/configuring#specifying-environments

__"rules"__: overrides to the extended set of rules
```json
"rules": {
  "no-console": 0,
  "no-unused-vars": 1
},
```
+ 0 = off, 1 = warn, 2 = error

#### 40 - Airbnb ESLint Settings

https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb

```
eslint bad-code.js --fix
```
+ CLI allows easy fixes to be auto corrected with ___--fix___ flag

+ _.eslintrc_ files in a project directory work well to allow each project/team its own conventions, but it's also useful to make a global _.eslintrc_ that can serve as a default

```
atom ~/.eslintrc
```
+ System file in __home directory (~)__
+ This will be overridden by a project specific _.eslintrc_

#### 41 - Line and File Specific Settings

```js
/* globals twttr ga */
...
ga.track();
twttr.trackConversion();
```

+ __globals__ keyword in comment block at head of file allows global variables

```js
/* eslint-disable no-extend-native */
if (!Array.prototype.includes) {
  ...
}
/* eslint-enable no-extend-native */
  ```

```js
/* eslint-disable */
if (!Array.prototype.includes) {
  ...
}
/* eslint-enable */
  ```
+ Can __disable__ and then re-__enable__ a specific rule, or all linting
