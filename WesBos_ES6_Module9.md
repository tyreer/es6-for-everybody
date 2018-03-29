## Module 9 - Object Literal Upgrades

#### 33 - Object Literal Upgrades

If __key has same name as variable__ can omit colon:
```js
const first = 'snickers';
const last = 'bos';
const age = 2;
const breed = 'King Charles Cav';
const dog = {
  firstName: first,
  last,
  age,
  breed,
  pals: ['Hugo', 'Sunny']
};
```

Simple modal syntax:
```js
const modal = {
  create(selector) {

  },
  open(content) {

  },
  close(goodbye) {

  }
}
```
+ Can pass in parameter:

Previously would need to use:

```js
const modal = {
  create: function(selector) {

  }
```

Can __dynamically set keys__ on an object:

```js
const key = 'pocketColor';
const value = '#ffc600';

const tShirt = {
  [key]: value,
  [`${key}Opposite`]: invertColor(value)
};
```

__Computed property keys__ are possible in ES6:

```js
const keys = ['size', 'color', 'weight'];
const values = ['medium', 'red', 100];

const shirt = {
  [keys.shift()]: values.shift(),
  [keys.shift()]: values.shift(),
  [keys.shift()]: values.shift(),
}
```
