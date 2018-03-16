## Module 4 - Additional String Improvements

#### 17 - New String Methods
__Four new methods__
+ Largely conveniences to avoid reliance on regex
+ Case sensitivity is a bit of a drawback compared to regex

```js
const course = 'RFB2';
const flightNumber = '20-AC2018-jz';
const accountNumber = '825242631RT0001';
```

__.startsWith()__
```js
course.startsWith('RF'); //true
course.startsWith('rf'); //false
```
+ Cannot make case insensitive

__.endsWith()__
```js
flightNumber.endsWith('jz'); //true
accountNumber.endsWith('RT'); //false
accountNumber.endsWith('RT', 11); //true
```
+ Second parameter determines how much of the original string to consider
  + Here the first 11 characters

__.includes()__
```js
flightNumber.includes('AC'); //true
flightNumber.includes('ac'); //false
```

__.repeat()__
```js
function leftPad(str, length = 20) {
  return `→ ${' '.repeat(length - str.length)}${str}`;
}
```
+ Repeats a given string as many times as specified in the parameter
+ In this case, _repeat_ is used to right align text by inserting spaces programatically
