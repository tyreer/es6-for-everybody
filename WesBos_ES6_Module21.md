## Module 21 - ES7, ES8 + Beyond

#### 73 - Class Properties
```js
class Dog {
  constructor(name, breed) {
    this.name = name;
    this.breed = breed;
  }

  barks = 0;

  bark() {
    console.log(`Bark Bark! My name is ${this.name}`);
    this.barks = this.barks + 1;
  }
}
```
 + Declaring _barks_ outside constructor is an example of a __class property__
 + Commonly used in __React__

 ```js
 class Mouse extends React.Component {
  static propTypes = {
    render: PropTypes.func.isRequired
  }

  state = { x: 0, y: 0 }
  ...
  }
  ```
  https://cdb.reacttraining.com/use-a-render-prop-50de598f11ce


Needs this Babel transform
https://babeljs.io/docs/plugins/transform-class-properties/

#### 74 - padStart and padEnd
> The padStart() method pads the current string with another string (repeated, if needed) so that the resulting string reaches the given length.

```js
"1".padStart(3, 0) // 001
```
+ Useful if trying to right align text to the longest string in a series

```js
const strings = ['short', 'medium size', 'this is really really long', 'this is really really really really really really long'];
const longestString = strings.sort(str => str.length).map(str => str.length)[0];

strings.forEach(str => console.log(str.padStart(longestString)));
```

#### 75 -  ES7 Array.includes() + Exponential Operator
__Array.includes()__
```js
['a', 'b', 'c'].includes('c') //true
```

__Exponential Operator__
```js
3 ** 3 //27
```

#### 76 - Function Arguments Trailing Comma
```js
const names = [
  'wes',
  'kait',
  'lux',
  'poppy',
];

const people = {
  wes: 'Cool',
  kait: 'Even Cooler!',
  lux: 'Coolest',
  poppy: 'Smallest',
  snickers: 'Bow wow',
}

function family(
  mom,
  dad,
  children,
  dogs,
) {

}
```
+ Just as with arrays and objects, function arguments can accept __trailing commas__
+ Rationale is that someone contributing will not have a line they're not really associated with come up as their edit just because they added a comma
+ Typically best to just add rule to __ESLint__ and __Prettier__

#### 77 - Object.entries() + Object.values()
```js
const inventory = {
  backpacks: 10,
  jeans: 23,
  hoodies: 4,
  shoes: 11
};

// Make a nav for the inventory
const nav = Object.keys(inventory).map(item => `<li>${item}</li>`).join('');
console.log(nav);

// tell us how many values we have
const totalInventory = Object.values(inventory).reduce((a, b) => a + b);
console.log(totalInventory);

// Print an inventory list with numbers
Object.entries(inventory).forEach(([key, val]) => {
  console.log(key, val);
});

for (const [key, val] of Object.entries(inventory)) {
  console.log(key);
  if (key === 'jeans') break;
}
```
+ __Object.values()__ is like the other side of _Object.keys()_
+ __Object.entries()__ returns each point in the object as a two-value array
+ Benefit of __for...of__ is the ability to __break__ out of it, which cannot be done in _map_ or _forEach_
