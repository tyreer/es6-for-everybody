## Module 7 - An Array of Array Improvements

#### 25 - Array.from() and Array.of()
+ Neither are on the prototype, but on _Array_ itself

__Arrary.from()__

```html
<div class="people">
  <p>Wes</p>
  <p>Kait</p>
  <p>Snickers</p>
</div>
```
```js
const people = document.querySelectorAll('.people p');
const peopleArray = Array.from(people, person => person.textContent);
console.log(peopleArray); //["Wes", "Kait", "Snickers"]
```
+ __Second parameter runs map method__

```js
function sumAll() {
  const nums = Array.from(arguments);
  return nums.reduce((prev, next) => prev + next, 0);
}

sumAll(2, 34, 23, 234, 234, 234234, 234234, 2342);
```
+ Common use case is to __convert _arguments_ to an array__ in order to use all the array methods, such as _reduce_ above
  + Note that _sumAll_ needs to be a non-arrow function since it relies on accessing _arguments_

__Arrary.of()__
```js
const ages = Array.of(12,4,23,62,34);
console.log(ages); // [12, 4, 23, 62, 34]
```
+ Pretty straightforward

#### 26 - Array. find() and .findIndex()

```js
const posts = [
  {
     "code":"VBgtGQcSf",
     "caption":"Trying the new Hamilton Brewery beer. Big fan.",
     "likes":27,
     "id":"1122810327591928991",
     "display_src":"https://scontent.cdninstagram.com/hphotos-xaf1/t51.2885-15/e35/12224456_175248682823294_1558707223_n.jpg"
  },
  {
     "code":"FpTyHQcau",
     "caption":"I'm in Austin for a conference and doing some training. Enjoying some local brew with my baby.",
     "likes":82,
     "id":"1118481761857291950",
     "display_src":"https://scontent.cdninstagram.com/hphotos-xpt1/t51.2885-15/e35/11326072_550275398458202_1726754023_n.jpg"
  }
];
const code = 'VBgtGQcSf';
const post = posts.find(post => post.code === code);
console.log(post); // { FULL OBJECT MATCHING CONDITION }
```
+ __find()__ returns once a condition is true
  + If you wanted multiple results, then use __filter()__

```js
  const postIndex = posts.findIndex(post => post.code === code);
  console.log(postIndex); // 22
```
+ Useful if you want to do something like delete a post or apply a highlight class

#### 27 - Array .some() and .every()
+ Not a part of ES6, but included as just for utility

```js
const ages = [32, 15, 19, 12];

// 👵👨 is there at least one adult in the group?
const adultPresent = ages.some(age => age >= 18);
console.log(adultPresent); / /true

// 🍻 is everyone old enough to drink?
const allOldEnough = ages.every(age => age >= 19);
console.log(allOldEnough); // false
```
