## Module 10 - Promises

#### 34 - Promises

```js
const postsPromise = fetch('http://wesbos.com/wp-json/wp/v2/posts');

postsPromise
  .then(data => data.json())
  .then(data => { console.log(data) })
  .catch((err) => {
    console.error(err);
  })
```
+ __fetch()__ returns a promise
+ At time of declaration _postPromise_ is like an __IOU__
+ Think of __then()__ like a callback that allows for synchronous execution (once the IOU returns)
+ __.json()__ method is key to translating returned data into useful JSON


#### 35 - Building your own Promises

Can think of a promise as: "_I don't want to stop JS from running. I just want to start this thing, and once it comes back pick it up_"

```js
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject(Error('Err wes isn\'t cool'));
  }, 1000);
});

myPromise
  .then(data => {
    console.log(data);
  })
  .catch(err => {
    console.error(err);
  });
  ```
+ __resolve()__ + __reject()__ methods come with the promise
+ Using an __Error__ object in _reject()_ allows the stack trace to locate the line where the error was thrown

#### 36 - Chaining Promises + Flow Control

```js
const posts = [
  { title: 'I love JavaScript', author: 'Wes Bos', id: 1 },
  { title: 'CSS!', author: 'Chris Coyier', id: 2 },
  { title: 'Dev tools tricks', author: 'Addy Osmani', id: 3 },
];

const authors = [
  { name: 'Wes Bos', twitter: '@wesbos', bio: 'Canadian Developer' },
  { name: 'Chris Coyier', twitter: '@chriscoyier', bio: 'CSS Tricks and CodePen' },
  { name: 'Addy Osmani', twitter: '@addyosmani', bio: 'Googler' },
];

function getPostById(id) {
  return new Promise((resolve, reject) => {
  // using a settimeout to mimick a databse
    setTimeout(() => {
      const post = posts.find(post => post.id === id);
      (post)
      ? resolve(post)
      : reject(Error('No Post Was Found!'));
    }, 1000)
  });
}

function hydrateAuthor(post) {
  return new Promise((resolve, reject) => {
    const authorObject = authors.find(author => author.name === post.author);
    if(authorObject) {
    // "hydrate" the post object with the author object
      post.author = authorObject;
      resolve(post)
    } else {
      reject(Error('Can not find the author'));
    }
  });
}

getPostById(1)
  .then(post => {
    return hydrateAuthor(post);
  })
  .then(post => {
    console.log(post);
  })
  .catch(err => {
    console.error(err);
  });
  ```

+ __debugger__ -> sources -> scope
  + Kind of like _console.log_ but with everything in the given scope
+ I suppose __flow control__ means using promises to synchronously execute functions?
+ I'm using a ternary above, but Bos uses a plain _if-else_, which might make more sense since the second promise will use _if-else_

#### 37 - Working with Multiple Promises

```js
const weather = new Promise((resolve) => {
  setTimeout(() => {
    resolve({ temp: 29, conditions: 'Sunny with Clouds'});
  }, 2000);
});

const tweets = new Promise((resolve) => {
  setTimeout(() => {
    resolve(['I like cake', 'BBQ is good too!']);
  }, 500);
});

Promise
  .all([weather, tweets])
  .then(responses => {
    const [weatherInfo, tweetInfo] = responses;
    console.log(weatherInfo, tweetInfo)
  });
  ```

  + __Promise.all()__ accepts an array of promises and waits for them all to resolve

```js
const [weatherInfo, tweetInfo] = responses;
```
+ Great use case for destructuring as a means to declare _const_ variables

```js
const postsPromise = fetch('http://wesbos.com/wp-json/wp/v2/posts');
const streetCarsPromise = fetch('http://data.ratp.fr/api/datasets/1.0/search/?q=paris');

Promise
  .all([postsPromise, streetCarsPromise])
  .then(responses => {
    return Promise.all(responses.map(res => res.json()))
  })
  .then(responses => {
    console.log(responses);
  });
  ```
+ With __Promise.all()__, we still need to __convert the response to JSON__ via a __second Promise.all()__ invocation
