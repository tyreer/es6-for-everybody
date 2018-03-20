## Module 9 - Promises

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
+ At time of declaration _postPromise_ is like an __IOU__
+ Think of __then()__ like a callback that allows for synchronous execution (once the IOU returns)


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
