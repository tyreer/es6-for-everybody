## Module 15 - Classes

#### 51 - Prototypal Inheritance Review
ES6 brought __classes__ into JavaScript, but __prototypal inheritance__ has long been present in JS
```js
function Dog(name, breed) {
  this.name = name;
  this.breed = breed;
}
```
+ Capital 'D' because this is a __constructor function__

```js
Dog.prototype.bark = function() {
  console.log(`Bark, from ${this.name}`);
}
```

```js
const duck = new Dog('Duck', 'Poodle')
```

#### 52 - Say Hello to Classes
__Class declaration__

```js
class Dog {

}
```
+ Preferred syntax

__Class expression__

```js
const Dog = class {

}
```
+ Some use cases, but generally not as common

```js
class Dog {
  constructor(name, breed) {
    this.name = name;
    this.breed = breed;
  }

  static info() {
    console.log(`This text should not be accessible on any instance`);
  }

  bark() {
    console.log(`Bark, from ${this.name}`);
  }

  get description() {
    return `${this.name} is a good dog`;
  }

  set nicknames(value) {
    this.nick = value.trim();
  }

  get nicknames() {
    return this.nick.toUpperCase();
  }
}
const duck = new Dog('Duck', 'Poodle')
```
+ __constructor__ = what happens when class is invoked and an instance is created
  + This ES6 syntax for a method without the word _function_ was earlier modelled in video 33 on object literals
+ A __static method__ is not accessible on instances
  + Kind of like it _doesn't move_ from the class to instance, but stays static or _in place_
  + __Array.of()__ is another instance of a static method that is __not inherited__ by other arrays off the prototype
```
duck.bark()   // "Bark, from Duck"
duck.info()   // Error
Dog.info()    // "This text should not be accessible on any instance"
```

+ A __getter__ is not a method but a property that is computed
+ A __setter__  allows you to assign a property

#### 53 - Extending Classes and using super()

```js
class Animal {
  constructor(name) {
    this.name = name;
    this.thirst = 100;
    this.belly = [];
  }
  drink() {
    this.thirst -= 10;
    return this.thirst;
  }
  eat(food) {
    this.belly.push(food);
    return this.belly;
  }
}

class Dolphasloth extends Animal {
  constructor (name, chillFactor) {
    super(name);
    this.chillFactor = chillFactor;
  }

  slobber() {
    this.thirst += 10;
    this.chillFactor -= 10;
    return this.thirst;
  }
}

const ChillDude = new Dolphasloth('Chill Dude', 100)
```

+ First need to create an instance of Animal before we can access _'this'_ in an extended classes
  + We can do this with __super()__
  + __super() will call whatever class you're extending__
    + Here _super()_ needs name passed in because _Animal()_ needs a name parameter for its constructor
  + It runs the constructor function in the extended class, which in turn establishes _this_

  ```js
  super(name)
  // equals
  Animal(name)
  ```

+ Good to note that this is happening all the time in React

```js
import React, { Component } from 'react';

export default class Battle extends Component {
  static propTypes = {
    match: PropTypes.object.isRequired,
  }
  ...
}
```

#### 54 - Extending Arrays with Classes for Custom Collections

Can extend any built-in primitives

```js
class MovieCollection extends Array {
  constructor(name, ...items) {
    super(...items);
    this.name = name;
  }
  add(movie) {
    this.push(movie);
  }
  topRated(limit = 10) {
    return this.sort((a, b) => (a.stars > b.stars ? -1 : 1)).slice(0, limit);
  }
}

const movies = new MovieCollection('Wes\'s Fav Movies',
  { name: 'Bee Movie', stars: 10 },
  { name: 'Star Wars Trek', stars: 1 },
  { name: 'Virgin Suicides', stars: 7 },
  { name: 'King of the Road', stars: 8 }
);

movies.add({ name: 'Titanic', stars: 5 });
```

```js
class MovieCollection extends Array {
  constructor(name, ...items) {
    super(...items);
```
+ These three lines are pretty trippy
  + The first _...items_ is a __rest operator__ that accepts as many parameters as are passed in (here movie objects)
  + The second _...items_ is a __spread operator__ that essentially creates a new array with the items passed in (because _super_ is invoking the _Array_ primitive)
    + new _Array('1', '2', '3')_;

```js
for (const movie of movies) {
  console.log(movie);
}
```
+ The new ES6 iteration method __for...of__ is particularly useful here as the previous methods like __for...in__ are more difficult to use resultant data from
