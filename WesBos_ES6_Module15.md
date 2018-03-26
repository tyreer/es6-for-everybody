## Module 15 - Classes

#### 51 - Prototypal Inheritance Review
+ ES6 has brought classes into JavaScript

__Prototypal inheritance__ has long been present in JS
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
  constructor() {
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
```
+ __constructor__ = what happens when class is invoked and an instance is created
  + This syntax for a method without the word _function_ was earlier modelled in video 33 on object literals
+ A __static method__ is not accessible on instances
  + __Array.of()__ is another instance of a static method that is __not inherited__ by other arrays off the prototype.

```
duck.bark()   // "Bark, from Duck"
duck.info()   // Error
Dog.info()    // "This text should not be accessible on any instance"
```

+ A __getter__ is not a method but a property that is computed
+ A __setter__  allows you to assign a property
