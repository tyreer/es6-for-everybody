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

#### 13 - Tagged Template Literals

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
  + Suppose this is how you could make something like "drunkify text"
