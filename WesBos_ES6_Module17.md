## Module 17 - Proxies

#### 58 - What are Proxies?

```js
const person = { personName: 'Wes', age: 100 };
const personProxy = new Proxy(person, {
  get(target, name) {
    return target[name].toUpperCase();
  },
  set(target, name, value) {
    if(typeof value === 'string') {
      target[name] = `${value.trim()} ✂️'d`;
    }
  }
});
```
+ __Proxies__ allow us to jump in between and overwrite default operations
+ First parameter is the __target__
+ Second parameter is a custom __handler__ with __traps__
  + Good to note that the single-word method (_get()_, _set()_) is an example of the ES6 reduced syntax for methods on object literals demoed in video 33
  + Prior to ES6 this syntax was _get: function(target, name) {...}_

```js
personProxy.cool = "     hella spaces here man   "
console.log(personProxy.cool) // "HELLA SPACES HERE MAN ✂️'D";
```
+ The first dot notation above is the equivalent of calling __set__ on the Proxy, which we __trap__ in our handler and trim
+ The second dot notation is the equivalent of calling __get__, which our __trap__ transforms to uppercase

#### 59 - Another Proxy Example?

```js
const phoneHandler = {
  set(target, name, value) {
    target[name] = value.match(/[0-9]/g).join('');
  },
  get(target, name) {
    return target[name].replace(/(\d{3})(\d{3})(\d{4})/, '($1)-$2-$3');
  }
}

const phoneNumbers = new Proxy({}, phoneHandler);
```
+ Standardizes phone number values

```js
const phoneNumbers = new Proxy({}, phoneHandler);
```
+ Starting the proxy from an empty array base.
+ Passing in a __handler object__ with various __traps__ that override built-in methods

```js
```
