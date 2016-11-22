# clean-code-javascript
Software engineering principles, from Robert C. Martin's wonderful book *Clean Code*, adapted for JavaScript.

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
