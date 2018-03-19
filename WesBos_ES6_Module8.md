## Module 8 - Say Hello to ...Spread and ...Rest

#### 28 - Spread Operator Introduction

```js
console.log([...'wes']); //["w", "e", "s"]
```

```js
const featured = ['Deep Dish', 'Pepperoni', 'Hawaiian'];
const specialty = ['Meatzza', 'Spicy Mama', 'Margherita'];

console.log([...featured, ...specialty]);

const pizzas = [...featured, 'veg', ...specialty];
const fridayPizzas = [...pizzas];
```

+ Nice alternative to _.concat()_
+ Spread operator also allows an array to be __copied instead of referenced__
```js
const fridayPizzas = pizzas;
fridayPizzas[0] = 'Yum-1';
console.log(pizzas[0]) // 'Yum-1'  - OH NO;
```
+ _Referencing_ an array is just bad.

#### 29 - Spread Exercise

```html
  <h2 class="jump">SPREADS!</h2>
```

__Wes:__
```js
  const heading = document.querySelector('.jump');
  heading.innerHTML = sparanWrap(heading.textContent);

  function sparanWrap(word) {
    return [...word].map(letter => `<span>${letter}</span>`).join('');
  }
```

__Me:__
```js
const target = document.querySelector('.jump');
target.innerHTML = [...target.textContent].map(letter => `<span>${letter}</span>`).join('')
```

+ Separating the logic out into its own function makes Wes's solution __more reusable__ and __easier to reason about__

#### 30 - More Spread Examples

```js
  const people = Array.from(document.querySelectorAll('.people p'));
  const people = [...document.querySelectorAll('.people p')];
```
+ Fair point that __Array.from() reads more clearly__ to others

```js
const comments = [
  { id: 209384, text: 'I love your dog!' },
  { id: 523423, text: 'Cuuute! 🐐' },
  { id: 632429, text: 'You are so dumb' },
  { id: 192834, text: 'Nice work on this wes!' },
];
const id = 632429;
const commentIndex = comments.findIndex(comment => comment.id === id);
const newComments = [...comments.slice(0,commentIndex), ...comments.slice(commentIndex + 1)];
```

#### 31 - Spreading into a Function

```js
const inventors = ['Einstein', 'Newton', 'Galileo'];
const newInventors = ['Musk', 'Jobs'];
inventors.push(...newInventors);
```

```js
const name = ['Wes', 'Bos'];

function sayHi(first, last) {
  alert(`Hey there ${first} ${last}`);
}

sayHi(...name);
```

#### 32 - The ...rest param in Functions and destructuring
__Looks the exact same as spread__ operator but behaves differently as a function __parameter__

```js
function convertCurrency(rate, ...amounts) {
  return amounts.map(amount => amount * rate);
}

const amounts = convertCurrency(1.54, 10, 23, 52, 1, 56);
```

```js
const runner = ['Wes Bos', 123, 5.5, 5, 3, 6, 35];
const [name, id, ...runs] = runner;
console.log(name, id, runs); // Wes Bos 123 [5.5, 5, 3, 6, 35]

const team = ['Wes', 'Kait', 'Lux', 'Sheena', 'Kelly'];
const [captain, assistant, ...players] = team;
console.log(captain, assistant, players) // Wes Kait ["Lux", "Sheena", "Kelly"]
```
