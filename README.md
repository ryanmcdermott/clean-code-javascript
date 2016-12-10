# clean-code-javascript
Software engineering principles, from Robert C. Martin's book
[*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
adapted for JavaScript. This is not a style guide. It's a guide to producing
readable, reusable, and refactorable software in JavaScript. Enjoy!

## Table of Contents
  1. [Variables](#variables)
  2. [Functions](#functions)
  3. [Classes](#classes)
  4. [Concurrency](#concurrency)
  5. [Comments](#comments)

## **Variables**
### Use meaningful and pronounceable variable names

**Bad:**
```javascript
var yyyymmdstr = moment().format('YYYY/MM/DD');
```

**Good**:
```javascript
var yearMonthDay = moment().format('YYYY/MM/DD');
```
**[⬆ back to top](#table-of-contents)**

### Use the same vocabulary for the same type of variable

**Bad:**
```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Good**:
```javascript
getUser();
```
**[⬆ back to top](#table-of-contents)**

### Use searchable names
We will read more code than we will ever write. It's important that the code we do write is readable and searchable. By *not* naming variables that end up being meaningful for understanding our program, we hurt our readers. Make your names searchable.

**Bad:**
```javascript
// What the heck is 525600 for?
for (var i = 0; i < 525600; i++) {
  runCronJob();
}
```

**Good**:
```javascript
// Declare them as capitalized `var` globals.
var MINUTES_IN_A_YEAR = 525600;
for (var i = 0; i < MINUTES_IN_A_YEAR; i++) {
  runCronJob();
}
```
**[⬆ back to top](#table-of-contents)**

### Avoid Mental Mapping
Explicit is better than implicit.

**Bad:**
```javascript
var locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((l) => {
  doStuff();
  doSomeOtherStuff();
  ...
  ...
  ...
  // Wait, what is `l` for again?
  dispatch(l);
});
```

**Good**:
```javascript
var locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((location) => {
  doStuff();
  doSomeOtherStuff();
  ...
  ...
  ...
  dispatch(location);
});
```
**[⬆ back to top](#table-of-contents)**

### Use consistent capitalization
JavaScript is untyped, so capitalization tells you a lot about your variables,
functions, etc. These rules are subjective, so your team can choose whatever
they want. The point is, no matter what you all choose, just be consistent.

**Bad:**
```javascript
var DAYS_IN_WEEK = 7;
var daysInMonth = 30;

var songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
var Artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**Good**:
```javascript
var DAYS_IN_WEEK = 7;
var DAYS_IN_MONTH = 30;

var songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
var artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```
**[⬆ back to top](#table-of-contents)**

### Don't add unneeded context
If your class/object name tells you something, don't repeat that in your
variable name.

**Bad:**
```javascript
var Car = {
  carMake: 'Honda',
  carModel: 'Accord',
  carColor: 'Blue'
};

function paintCar(car) {
  car.carColor = 'Red';
}
```

**Good**:
```javascript
var Car = {
  make: 'Honda',
  model: 'Accord',
  color: 'Blue'
};

function paintCar(car) {
  car.color = 'Red';
}
```
**[⬆ back to top](#table-of-contents)**

## **Functions**
### Limit the amount of function parameters (2 or less)
Use an object if you are finding yourself needing a lot of parameters.

**Bad:**
```javascript
function createMenu(title, body, buttonText, cancellable) {
  ...
}
```

**Good**:
```javascript
var menuConfig = {
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz'
  cancellable: true
}

function createMenu(config) {
  ...
}

```
**[⬆ back to top](#table-of-contents)**

### Use default arguments instead of short circuiting
**Bad:**
```javascript
function writeForumComment(subject, body) {
  subject = subject || 'No Subject';
  body = body || 'No text';
}

```

**Good**:
```javascript
function writeForumComment(subject='No subject', body='No text') {
  ...
}

```
**[⬆ back to top](#table-of-contents)**

### Set default objects with Object.assign

**Bad:**
```javascript
var menuConfig = {
  title: null,
  body: 'Bar',
  buttonText: null
  cancellable: true
}

function createMenu(config) {
  config.title = config.title || 'Foo'
  config.body = config.body || 'Bar'
  config.buttonText = config.title || 'Baz'
  config.cancellable = config.cancellable === undefined ? config.cancellable : true;

}

createMenu(menuConfig);
```

**Good**:
```javascript
var menuConfig = {
  title: null,
  body: 'Bar',
  buttonText: null
  cancellable: true
}

function createMenu(config) {
  Object.assign(config, {
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  });
}

createMenu(menuConfig);
```
**[⬆ back to top](#table-of-contents)**



### Don't use flags as function parameters
Flags tell your user that this function does more than one thing. Functions should do one thing. Split out your functions if they are following different code paths based on a boolean.

**Bad:**
```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create('./temp/' + name);
  } else {
    fs.create(name);
  }
}
```

**Good**:
```javascript
function createTempFile(name) {
  fs.create('./temp/' + name);
}

function createFile(name) {
  fs.create(name);
}
```
**[⬆ back to top](#table-of-contents)**

### Avoid Side Effects
A function produces a side effect if it does anything other than take a value in
and return another value or values. A side effect could be writing to a file,
modifying some global variable, or accidentally wiring all your money to a
Nigerian prince.

Now, you do need to have side effects in a program on occasion. Like the previous
example, you might need to write to a file. What you want to do is to
centralize where you are doing this. Don't have several functions and classes
that write to a particular file. Have one service that does it. One and only one.

The main point is to avoid common pitfalls like sharing state between objects
without any structure, using mutable data types that can be written to by anything,
and not centralizing where your side effects occur. If you can do this, you will
be happier than the vast majority of other programmers.

**Bad:**
```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
var name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

console.log(name); // ['Ryan', 'McDermott'];

```

**Good**:
```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

var name = 'Ryan McDermott'
var newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```
**[⬆ back to top](#table-of-contents)**

### Don't write to global functions
Polluting globals is a bad practice in JavaScript because you could clash with another
library and the user of your API would be none-the-wiser until they get an
exception in production. Let's think about an example: what if you wanted to
extend JavaScript's native Array method to have a `diff` method that could
show the difference between two arrays? You could write your new function
to the `Array.prototype`, but it could clash with another library that tried
to do the same thing. What if that other library was just using `diff` to find
the difference between the first and last elements of an array? This is why it
would be much better to just use ES6 classes and simply extend the `Array` global.

**Bad:**
```javascript
Array.prototype.diff = function(comparisonArray) {
  var values = [];
  var hash = {};

  for (var i of comparisonArray) {
    hash[i] = true;
  }

  for (var i of this) {
    if (!hash[i]) {
      values.push(i);
    }
  }

  return values;
}
```

**Good:**
```javascript
class SuperArray extends Array {
  constructor(...args) {
    super(...args);
  }

  diff(comparisonArray) {
    var values = [];
    var hash = {};

    for (var i of comparisonArray) {
      hash[i] = true;
    }

    for (var i of this) {
      if (!hash[i]) {
        values.push(i);
      }
    }

    return values;
  }
}
```
**[⬆ back to top](#table-of-contents)**

### Favor functional programming over imperative programming
If Haskell were an IPA then JavaScript would be an O'Douls. That is to say,
JavaScript isn't a functional language in the way that Haskell is, but it has
a functional flavor to it. Functional languages are cleaner and easier to test.
Favor this style of programming when you can.

**Bad:**
```javascript
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

var totalOutput = 0;

for (var i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**Good**:
```javascript
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  }, {
    name: 'Suzie Q',
    linesOfCode: 1500
  }, {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  }, {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

var totalOutput = programmerOutput
  .map((programmer) => programmer.linesOfCode)
  .reduce((acc, linesOfCode) => acc + linesOfCode, 0);
```
**[⬆ back to top](#table-of-contents)**


## **Classes**
### Prefer ES6 classes over ES5 plain functions
It's very difficult to get readable class inheritance, construction, and method
definitions for classical ES5 classes. If you need inheritance (and be aware
that you might not), then prefer classes. However, prefer small functions over
classes until you find yourself needing larger and more complex objects.

**Bad:**
```javascript
var Animal = function(age) {
    if (!(this instanceof Animal)) {
        throw new Error("Instantiate Animal with `new`");
    }

    this.age = age;
};

Animal.prototype.move = function() {};

var Mammal = function(age, furColor) {
    if (!(this instanceof Mammal)) {
        throw new Error("Instantiate Mammal with `new`");
    }

    Animal.call(this, age);
    this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function() {};

var Human = function(age, furColor, languageSpoken) {
    if (!(this instanceof Human)) {
        throw new Error("Instantiate Human with `new`");
    }

    Mammal.call(this, age, furColor);
    this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function() {};
```

**Good:**
```javascript
class Animal {
    constructor(age) {
        this.age = age;
    }

    move() {}
}

class Mammal extends Animal {
    constructor(age, furColor) {
        super(age);
        this.furColor = furColor;
    }

    liveBirth() {}
}

class Human extends Mammal {
    constructor(age, furColor, languageSpoken) {
        super(age, furColor);
        this.languageSpoken = languageSpoken;
    }

    speak() {}
}
```
**[⬆ back to top](#table-of-contents)**

## **Concurrency**
### Use Promises, not callbacks
Callbacks aren't clean, and they cause excessive amounts of nesting. With ES6,
Promises are a built-in global type. Use them!

**Bad:**
```javascript
require('request').get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', function(err, response) {
  if (err) {
    console.error(err);
  }
  else {
    require('fs').writeFile('article.html', response.body, function(err) {
      if (err) {
        console.error(err);
      } else {
        console.log('File written');
      }
    })
  }
})

```

**Good**:
```javascript
require('request-promise').get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then(function(response) {
    return require('fs-promise').writeFile('article.html', response);
  })
  .then(function() {
    console.log('File written');
  })
  .catch(function(err) {
    console.log(err);
  })

```
**[⬆ back to top](#table-of-contents)**

### Async/Await are even cleaner than Promises
Promises are a very clean alternative to callbacks, but ES7 brings async and await
which offer an even cleaner solution. All you need is a function that is prefixed
in an `async` keyword, and then you can write your logic imperatively without
a `then` chain of functions. Use this if you can take advantage of ES7 features
today!

**Bad:**
```javascript
require('request-promise').get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then(function(response) {
    return require('fs-promise').writeFile('article.html', response);
  })
  .then(function() {
    console.log('File written');
  })
  .catch(function(err) {
    console.log(err);
  })

```

**Good**:
```javascript
async function getCleanCodeArticle() {
  try {
    var request = await require('request-promise')
    var response = await request.get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin');
    var fileHandle = await require('fs-promise');

    await fileHandle.writeFile('article.html', response);
    console.log('File written');
  } catch(err) {
      console.log(err);
    }
  }
```
**[⬆ back to top](#table-of-contents)**


## **Comments**
### Only comment things that have business logic complexity.
Comments are an apology, not a requirement. Good code *mostly* documents itself.

**Bad:**
```javascript
function hashIt(data) {
  // The hash
  var hash = 0;

  // Length of string
  var length = data.length;

  // Loop through every character in data
  for (var i = 0; i < length; i++) {
    // Get character code.
    var char = i.charCodeAt(i);
    // Make the hash
    hash = ((hash << 5) - hash) + char;
    // Convert to 32-bit integer
    hash = hash & hash;
  }
}
```

**Good**:
```javascript

function hashIt(data) {
  var hash = 0;
  var length = data.length;

  for (var i = 0; i < length; i++) {
    var char = i.charCodeAt(i);
    hash = ((hash << 5) - hash) + char;

    // Convert to 32-bit integer
    hash = hash & hash;
  }
}

```
**[⬆ back to top](#table-of-contents)**

### Don't leave commented code in your codebase
Version control exists for a reason. Leave old code in your history.

**Bad:**
```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Good**:
```javascript
doStuff();
```
**[⬆ back to top](#table-of-contents)**
