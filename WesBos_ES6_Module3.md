## Module 3 - Template Strings

#### 13 - Creating HTML fragments with Template Literals

```js
const dogs = [
  { name: 'Snickers', age: 2 },
  { name: 'Hugo', age: 8 },
  { name: 'Sunny', age: 1 }
];

const markup = `
  <ul class="dogs">
    ${dogs.map(dog => `
      <li>
        ${dog.name}
        is
        ${dog.age * 7}
      </li>`).join('')}
  </ul>
`;
```

+ Possible to iterate and generate template literals _inside_ another template literal.

```js
const song = {
  name: 'Dying to live',
  artist: 'Tupac',
  featuring: 'Biggie Smalls'
};

const markup = `
  <div class="song">
    <p>
      ${song.name} — ${song.artist}
      ${song.featuring ? `(Featuring ${song.featuring})` : ''}
    </p>
  </div>
`;
```

+ __Ternary within template literal__ to handle string that may or may not be present

```js
const beer = {
  name: 'Belgian Wit',
  brewery: 'Steam Whistle Brewery',
  keywords: ['pale', 'cloudy', 'spiced', 'crisp']
};

function renderKeywords(keywords) {
  return `
    <ul>
      ${keywords.map(keyword => `<li>${keyword}</li>`).join('')}
    </ul>
  `;
}

const markup = `
  <div class="beer">
    <h2>${beer.name}</h2>
    <p class="brewery">${beer.brewery}</p>
    ${renderKeywords(beer.keywords)}
  </div>
`;
```

```js
${renderKeywords(beer.keywords)}
```

+ Possible to __delegate to a render function__ the work of generating a template literal 😍
+ Parallels with React patterns where a component  is designated to handle a specific bit of markup generation

#### 14 - Tagged Template Literals

```js
function highlight(strings, ...values) {
  debugger;
}
```
+ Dev tools > sources > scope > local
  + Useful way to assess a function's local scope

```js
function highlight(strings, ...values) {
  let str = '';
  strings.forEach((string, i) => {
    str += `${string} <span contenteditable class="hl">${values[i] || ''}</span>`;
  });
  return str;
}

const name = 'Snickers';
const age = 100;

const sentence = highlight`My dog's name is ${name} and he is ${age} years old`;
```
+ Pass in the string and the tagged template literal is broken into its pieces, which can then be manipulated
  + __rest spread__ useful in accepting as many arguments as are passed in
  + Suppose this is how you could make something like "drunk-ify text"

#### 15 - Tagged Templates Exercise

```js
const dict = {
  HTML: 'Hyper Text Markup Language',
  CSS: 'Cascading Style Sheets',
  JS: 'JavaScript'
};

function addAbbreviations(strings, ...values) {
  const abbreviated = values.map(value => {
    if(dict[value]) {
      return `<abbr title="${dict[value]}">${value}</abbr>`;
    }
    return value;
  });

  return strings.reduce((sentence, string, i) => {
    return `${sentence}${string}${abbreviated[i] || ''}`;
  }, '');
}

const first = 'Wes';
const last = 'Bos';
const sentence = addAbbreviations`Hello my name is ${first} ${last} and I love to code ${'HTML'}, ${'CSS'} and ${'JS'}`;
```

```js
const abbreviated = values.map(value => {
  if(dict[value]) {
    return `<abbr title="${dict[value]}">${value}</abbr>`;
  }
  return value;
});
```
+ Mapping over all values in template literal and testing if a given _value_ can serve as key in the _dict_ object
  + If not, just returning the _value_

```js
return strings.reduce((sentence, string, i) => {
  return `${sentence}${string}${abbreviated[i] || ''}`;
}, '');
```
+ __Assembling a string programmatically__ from several sources using __reduce__

#### 16 - Sanitizing User Data with Tagged Templates

+ __XXS__ = Cross-site Scripting
  + Security risk in using __innerHTML__ w/o __sanitizing__ as evil JS could get run—as in __onload__ below

```js
  function sanitize(strings, ...values) {
    const dirty = strings.reduce((prev, next, i) => `${prev}${next}${values[i] || ''}`, '');
    return DOMPurify.sanitize(dirty);
  }

  const first = 'Wes';
  const aboutMe = `I love to do evil <img src="http://unsplash.it/100/100?random" onload="alert('you got hacked');" />`;

  const html = sanitize`
    <h3>${first}</h3>
    <p>${aboutMe}</p>
  `;

  const bio = document.querySelector('.bio');
  bio.innerHTML = html;
  ```

  ```js
  const dirty = strings.reduce((prev, next, i) => `${prev}${next}${values[i] || ''}`, '');
  ```
  + Seems the __minimal way to reconstruct__ from a tagged template using _reduce_

```js
return DOMPurify.sanitize(dirty);
```
+ __DOMPurify__ is imported
