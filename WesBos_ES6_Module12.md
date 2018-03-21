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
