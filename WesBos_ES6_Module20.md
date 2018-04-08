## Module 20 - Async + Await Flow Control
Also a great resource:
https://developers.google.com/web/fundamentals/primers/async-functions

#### 67 - Async Await - Native Promises Review
Two models of native promises
+ Each use __then()__ + __catch()__

__fetch__
```js
fetch('https://api.github.com/users/wesbos').then(res => {
    return res.json();
  }).then(res => {
    console.log(res);
  }).catch(err => {
    console.error('OH NOO!!!!!!!');
    console.error(err);
})
```
+ _Fetch()_ is a native method that returns a promise
  + Using __console.error__ provides more of a stack trace

__getUserMedia__
```js
const video = document.querySelector('.handsome'); //<video controls class="handsome"></video>

navigator.mediaDevices.getUserMedia({ video: true }).then(mediaStream => {
  video.srcObject = mediaStream;
  video.load();
  video.play();
}).catch(err => {
  console.log(err);
})
```

#### 68 - Async Await - Custom Promises Review
```js
function breathe(amount) {
  return new Promise((resolve, reject) => {
    if (amount < 500) {
      reject('That is too small of a value');
    }
    setTimeout(() => resolve(`Done for ${amount} ms`), amount);
  });
}

breathe(1000).then(res => {
  console.log(res);
  return breathe(500);
}).then(res => {
  console.log(res);
  return breathe(600);
}).catch(err => {
  console.error(err);
})
```
+ We can write a function that __returns a custom promise__
+ __reject__ will throw an error, which needs to be caught with __catch()__

#### 69 - All About Async + Await
Almost all the time, JS is __asynchronous__, meaning the execution of code is __not blocking__ other code from executing

__Promises and Async + Await__ allow a degree of __flow control__

```js
function breathe(amount) {
  return new Promise((resolve, reject) => {
    if (amount < 500) {
      reject('That is too small of a value');
    }
    setTimeout(() => resolve(`Done for ${amount} ms`), amount);
  });
}

async function go() {
  console.log(`Starting`);
  const res = await breathe(1000);
  console.log(res);
  const res2 = await breathe(300);
  console.log(res2);
  const res3 = await breathe(750);
  console.log(res3);
  const res4 = await breathe(900);
  console.log(res4);
  console.log('end');
}

go();
```
+ Within an __async__ function, we can use __await__ which allows a __promise__ to be _resolved_ or _rejected_ prior to moving on

#### 70 - Async + Await Error Handling
__Basic try/catch__
```js
async function go(firstName, lastName) {
  try {
    console.log(`Starting for ${firstName} ${lastName}`);
    const res = await breathe(1000);
    console.log(res);
    const res2 = await breathe(100);
    console.log(res2);
    console.log('end');
  } catch(err) {
    console.error(err);
  }
}
```
+ __Use case:__ You need to handle an error unique to this function
  + For instance, in a form

__Higher-order function for error handling__
```js
function breathe(amount) {
  return new Promise((resolve, reject) => {
    if (amount < 500) {
      reject('That is too small of a value');
    }
    setTimeout(() => resolve(`Done for ${amount} ms`), amount);
  });
}

function catchErrors(fn) {
  return function(...args){
    return fn(...args).catch((err) => {
      console.error(err);
    });
  }
}

async function go(firstName, lastName) {
  console.log(`Starting for ${firstName} ${lastName}`);
  const res = await breathe(1000);
  console.log(res);
  const res2 = await breathe(100);
  console.log(res2);
  console.log('end');
}

const wrappedFunction = catchErrors(go);

wrappedFunction('Joe', 'Winner');
```
+ __Use case:__ You need a general error handler that can be reused on various functions
+ The wrapped function has error handling "sprinkled onto it" by the __higher-order function__

```js
function catchErrors(fn) {
  return function(...args){
    return fn(...args).catch((err) => {
      console.error(err);
    });
  }
}
```
+ The first _...args_ is a __rest operator__ and the second _...args_ is a __spread operator__
  + This allows us to pass in any number of parameters to the wrapped function

#### 71 - Waiting on Multiple Promises
We want multiple fetches to go out into the world rather than waiting for each one to return before the next is issued. So, this would most likely be __bad__:
```js
async function go() {
  const p1 = await fetch('https://api.github.com/users/wesbos');
  const p2 = await fetch('https://api.github.com/users/stolinski');
```

__Method one__
```js
async function go() {
  const p1 = fetch('https://api.github.com/users/wesbos');
  const p2 = fetch('https://api.github.com/users/stolinski');
  const res = await Promise.all([p1, p2]);
  const dataPromises = res.map(r => r.json());
  const [wes, scott] = await Promise.all(dataPromises);
  console.log(wes, scott);
  // const people = await Promise.all(dataPromises); // Alternative if we don't know how many people being returned  
}
go();
```
+ Need two __await Promise.all__ when handling fetched data

```js
const [wes, scott] = await Promise.all(dataPromises);
```
+ __Destructing__ two immediately declared variables from the array returned by _Promise.all()_

__Method two__

```js
async function getData(names) {
  const promises = names.map(name => fetch(`https://api.github.com/users/${name}`).then(r => r.json()));
  const people = await Promise.all(promises);
  console.log(people);
}

getData(['wesbos', 'stolinski', 'darcyclarke']);
```
+ Can chain the _fetch()_ and _then()_ calls together
  + I like 👆

Related example from Jake Archibald:

```js
async function logInOrder(urls) {
  // fetch all the URLs in parallel
  const textPromises = urls.map(async url => {
    const response = await fetch(url);
    return response.text();
  });

  // log them in sequence
  for (const textPromise of textPromises) {
    console.log(await textPromise);
  }
}
```
https://developers.google.com/web/fundamentals/primers/async-functions

#### 72 - Promisifying Callback Based Functions

```js
// navigator.geolocation.getCurrentPosition(function (pos) {
//   console.log('it worked!');
//   console.log(pos);
// }, function (err) {
//   console.log('it failed!');
//   console.log(err);
// });

function getCurrentPosition(...args) {
  return new Promise((resolve, reject) => {
    navigator.geolocation.getCurrentPosition(...args, resolve, reject);
  });
}

async function go() {
  console.log('starting');
  const pos = await getCurrentPosition();
  console.log(pos);
  console.log('finished');
}

go();
```
+ Allows us to make a callback-oriented API work on promises
+ 3rd party options also, including a built-in Node utility
