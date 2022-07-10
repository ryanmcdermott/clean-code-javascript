# clean-code-javascript

clean code JavaScript(日本語訳)

## Table of Contents
  1. [Introduction](#introduction)
  2. [Variables](#variables)
  3. [Functions](#functions)
  4. [Objects and Data Structures](#objects-and-data-structures)
  5. [Classes](#classes)
  6. [SOLID](#solid)
  7. [Testing](#testing)
  8. [Concurrency](#concurrency)
  9. [Error Handling](#error-handling)
  10. [Formatting](#formatting)
  11. [Comments](#comments)
  12. [Translation](#translation)

## Introduction
## はじめに
![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](http://www.osnews.com/images/comics/wtfm.jpg)

Software engineering principles, from Robert C. Martin's book
[*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
adapted for JavaScript. This is not a style guide. It's a guide to producing
[readable, reusable, and refactorable](https://github.com/ryanmcdermott/3rs-of-software-architecture) software in JavaScript.

Robert C. Martinの本[*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)、ソフトウェアエンジニアリングの原則をJavaScriptに適用させたもの。
これはスタイルガイドではありません。JavaScriptで[可読性が良く、再利用でき、リファクタリング可能](https://github.com/ryanmcdermott/3rs-of-software-architecture)なソフトウェアを提供するためのガイドです。

Not every principle herein has to be strictly followed, and even fewer will be
universally agreed upon. These are guidelines and nothing more, but they are
ones codified over many years of collective experience by the authors of
*Clean Code*.

すべての原則に厳密に厳格に従う必要はありません、さらに普遍的に合意されているものはさらに少くなります。
これらはガイドライン以上の何ものでもありませんが、*Clean Code* の著者達による長年の経験を集めて文書化したものの一つです。

Our craft of software engineering is just a bit over 50 years old, and we are
still learning a lot. When software architecture is as old as architecture
itself, maybe then we will have harder rules to follow. For now, let these
guidelines serve as a touchstone by which to assess the quality of the
JavaScript code that you and your team produce.

ソフトウェアエンジニアリング技術は50歳を少し超えただけであり、いまだ多くのことを私たちは学び続けています。
もし、ソフトウェアアーキテクチャがそのアーキテクチャと同じくらい古いものであれば、おそらくそれに従うためのより厳格なルールがあることでしょう。
今のところ、これらのガイドラインはあなたとチームが提供するJavaScriptコードの品質を確認するための試金石として役立つでしょう。

One more thing: knowing these won't immediately make you a better software
developer, and working with them for many years doesn't mean you won't make
mistakes. Every piece of code starts as a first draft, like wet clay getting
shaped into its final form. Finally, we chisel away the imperfections when
we review it with our peers. Don't beat yourself up for first drafts that need
improvement. Beat up the code instead!

もう一つ大事なこと:  
これらのことを知っているからといっても、すぐにより良い開発者にしてくれる訳ではありません。
また、長年これにしたがって作業していたとしても、間違いを犯さない訳でもありません。
全てのコードのかけらは、湿った粘土をこねて最終形を目指すように、最初ドラフトとして始まります。
最終的に同僚とこれをレビューする時に、この不完全な部分を取り除いていきます。
どうか改善が必要な最初のドラフトで自分自身を叩かないでください。代わりにコードを叩きましょう！

## **Variables**
### Use meaningful and pronounceable variable names
### 意味があり発音可能な変数名を利用すること

**Bad:**
```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
```

**Good:**
```javascript
const currentDate = moment().format('YYYY/MM/DD');
```
**[⬆ back to top](#table-of-contents)**

### Use the same vocabulary for the same type of variable
### 同じタイプの変数には同じ単語を利用すること

**Bad:**
```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Good:**
```javascript
getUser();
```
**[⬆ back to top](#table-of-contents)**

### Use searchable names
### 検索できる名前を利用すること

We will read more code than we will ever write. It's important that the code we
do write is readable and searchable. By *not* naming variables that end up
being meaningful for understanding our program, we hurt our readers.
Make your names searchable. Tools like
[buddy.js](https://github.com/danielstjules/buddy.js) and
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
can help identify unnamed constants.

私たちはコードを書くよりも読む方が多いでしょう。そのため、コードを読みやすく検索できるように書くことは重要なことです。
プログラムを理解するために有意義な名前を付けない変数によって、私たちは読み手を傷つけています。
変数を検索可能にしておいてください。[buddy.js](https://github.com/danielstjules/buddy.js)や
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)のようなツールは、名前が付いていない変数を識別する手助けをしてくれます。

**Bad:**
```javascript
// What the heck is 86400000 for?
// 一体、なんのための86400000なんだい？
setTimeout(blastOff, 86400000);

```

**Good:**
```javascript
// Declare them as capitalized named constants.
// それらを大文字の名前付き定数として宣言してください。
const MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);

```
**[⬆ back to top](#table-of-contents)**

### Use explanatory variables
### 説明的な変数を利用すること

**Bad:**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(address.match(cityZipCodeRegex)[1], address.match(cityZipCodeRegex)[2]);
```

**Good:**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```
**[⬆ back to top](#table-of-contents)**

### Avoid Mental Mapping
### メンタルマップを避ける
Explicit is better than implicit.
明らかなことは暗黙的なことよりも優れています。

> 訳注：メンタルマップとは、認知心理学において記憶の中に構成される「あるべき姿」のイメージをさす言葉です。

**Bad:**
```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((l) => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Wait, what is `l` for again?
  // ちょっと待って、もう一度`l`ってなんだっけ？
  dispatch(l);
});
```

**Good:**
```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((location) => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```
**[⬆ back to top](#table-of-contents)**

### Don't add unneeded context
### 不必要なコンテキストを加えない
If your class/object name tells you something, don't repeat that in your
variable name.

もしクラスやオブジェクト名が何かを伝えているのであれば、変数名でそのことを繰り返してはいけません。

**Bad:**
```javascript
const Car = {
  carMake: 'Honda',
  carModel: 'Accord',
  carColor: 'Blue'
};

function paintCar(car) {
  car.carColor = 'Red';
}
```

**Good:**
```javascript
const Car = {
  make: 'Honda',
  model: 'Accord',
  color: 'Blue'
};

function paintCar(car) {
  car.color = 'Red';
}
```
**[⬆ back to top](#table-of-contents)**

### Use default arguments instead of short circuiting or conditionals
### 短絡評価や条件の代わりにデフォルト引数を利用すること
Default arguments are often cleaner than short circuiting. Be aware that if you
use them, your function will only provide default values for `undefined`
arguments. Other "falsy" values such as `''`, `""`, `false`, `null`, `0`, and
`NaN`, will not be replaced by a default value.

デフォルト引数は多くの場合、短絡評価よりも明確です。
ご存知の通り、これらを使った場合、関数は`undefined`の引数のみにデフォルト値を提供します。
他の`''`、`""`、`false`、`null`、`0`や`NaN`のような"falsy"値は、デフォルト値で置き換わることはありません。

**Bad:**
```javascript
function createMicrobrewery(name) {
  const breweryName = name || 'Hipster Brew Co.';
  // ...
}

```

**Good:**
```javascript
function createMicrobrewery(name = 'Hipster Brew Co.') {
  // ...
}

```
**[⬆ back to top](#table-of-contents)**

## **Functions**
### Function arguments (2 or fewer ideally)
### 関数の引数(2つ以下が理想的)
Limiting the amount of function parameters is incredibly important because it
makes testing your function easier. Having more than three leads to a
combinatorial explosion where you have to test tons of different cases with
each separate argument.

関数の引数の数を制限することは、テストを簡単に行えるという点において非常に重要なことです。
3つ以上あるということは、それぞれ別の引数を伴った数多くの異なるケースをテストしなければならないという、組み合わせ爆発につながります。

One or two arguments is the ideal case, and three should be avoided if possible.
Anything more than that should be consolidated. Usually, if you have
more than two arguments then your function is trying to do too much. In cases
where it's not, most of the time a higher-level object will suffice as an
argument.

1つか2つの場合は理想的で、3つは可能であれば避けるべきです。
それ以上のものは統合する必要があります。通常2つ以上の引数を持つ場合、その関数は余りにも多くのことをやろうとしています。
そうでない場合、ほとんどの場合、上位のオブジェクトを引数とすれば十分でしょう。

Since JavaScript allows you to make objects on the fly, without a lot of class
boilerplate, you can use an object if you are finding yourself needing a
lot of arguments.

JavaScriptは、多くのクラスの雛形がなくとも素早くオブジェクトを作成することができるため、もし多くの引数を必要としているとわかった場合は、オブジェクトを使うことができます。

To make it obvious what properties the function expects, you can use the ES2015/ES6
destructuring syntax. This has a few advantages:

1. When someone looks at the function signature, it's immediately clear what
properties are being used.
2. Destructuring also clones the specified primitive values of the argument
object passed into the function. This can help prevent side effects. Note:
objects and arrays that are destructured from the argument object are NOT
cloned.
3. Linters can warn you about unused properties, which would be impossible
without destructuring.

関数がどのプロパティを期待しているかを明らかにするために、ES2015/ES6の分割代入(destructuring)構文を利用することができます。
これにはいくつかの利点があります。

1. 誰かが関数の定義を見たときに、どのプロパティが利用されているかが明らかです。
2. 分割代入は、関数の中に渡された引数オブジェクトの指定されたプリミティブ型の値を複製します。これは副作用を防ぐ役割をします。注意:引数オブジェクトから再構成されたオブジェクトや配列は複製されません。
3. Lintツールが、未使用のプロパティについて警告を出すことができます。このようなことは、分割代入しない限り不可能でしょう。

**Bad:**
```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```

**Good:**
```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```
**[⬆ back to top](#table-of-contents)**


### Functions should do one thing
### 関数は1つのことを行うこと
This is by far the most important rule in software engineering. When functions
do more than one thing, they are harder to compose, test, and reason about.
When you can isolate a function to just one action, they can be refactored
easily and your code will read much cleaner. If you take nothing else away from
this guide other than this, you'll be ahead of many developers.

これはいままでのところ、ソフトウェアエンジニアリングにとってもっとも重要なルールです。
関数が2つ以上のことをやるときは、それを作ったり、テストしたり、理由付けすることが難しくなります。
関数をただ1つのことをやるように分離できた場合、それらを簡単にリファクタリングしたり、コードをかなりしっかりと読むことができます。
このガイドのこれ以外のことをなにもしなかったとしても、これをすることだけで、あなたは他の開発者よりも少し先に進んでいると言えます。

**Bad:**
```javascript
function emailClients(clients) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Good:**
```javascript
function emailActiveClients(clients) {
  clients
    .filter(isActiveClient)
    .forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```
**[⬆ back to top](#table-of-contents)**

### Function names should say what they do
### 関数名は何をするかを表すこと

**Bad:**
```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
// 関数名からは何が追加されたのかがわかりにくい
addToDate(date, 1);
```

**Good:**
```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```
**[⬆ back to top](#table-of-contents)**

### Functions should only be one level of abstraction
### 関数はただ1つの抽象化をすること
When you have more than one level of abstraction your function is usually
doing too much. Splitting up functions leads to reusability and easier
testing.

関数が1つ以上の抽象化を行なっている場合、通常その関数は多くのことをやり過ぎています。関数を分割することで、再利用やテストが簡単になります。

**Bad:**
```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // lex...
  });

  ast.forEach((node) => {
    // parse...
  });
}
```

**Good:**
```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const ast = lexer(tokens);
  ast.forEach((node) => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      tokens.push( /* ... */ );
    });
  });

  return tokens;
}

function lexer(tokens) {
  const ast = [];
  tokens.forEach((token) => {
    ast.push( /* ... */ );
  });

  return ast;
}
```
**[⬆ back to top](#table-of-contents)**

### Remove duplicate code
### 重複したコードを削除すること
Do your absolute best to avoid duplicate code. Duplicate code is bad because it
means that there's more than one place to alter something if you need to change
some logic.

重複したコードを避けるために絶対にベストを尽くしてください。
重複したコードは、もし何かのロジックを変更しようとした場合、何か変更する場所が1つ以上あるという意味で悪です。

Imagine if you run a restaurant and you keep track of your inventory: all your
tomatoes, onions, garlic, spices, etc. If you have multiple lists that
you keep this on, then all have to be updated when you serve a dish with
tomatoes in them. If you only have one list, there's only one place to update!

あなたがレストランを経営していて、すべてのトマト、たまねぎ、ニンニク、スパイスなどの在庫を追跡しているとします。
もし複数のリストを持っている場合、トマトが入った料理を提供したら、全てを更新しなければなりません。
もしそれが1つだった場合、更新する場所はたった1つです！

Oftentimes you have duplicate code because you have two or more slightly
different things, that share a lot in common, but their differences force you
to have two or more separate functions that do much of the same things. Removing
duplicate code means creating an abstraction that can handle this set of
different things with just one function/module/class.

しばしば、共有点が多いにも関わらず、2つ以上のわずかに異なる部分があるために、重複したコードを持つことがあります。
しかしその違いによって、ほとんど同じことを行う2つ以上の別々の関数が必要になります。
重複したコードを削除するということは、関数/モジュール/クラスを1つだけ利用して、これらのわずかにを異なる一連のものを処理することができる抽象化を作るということを意味します。

Getting the abstraction right is critical, that's why you should follow the
SOLID principles laid out in the *Classes* section. Bad abstractions can be
worse than duplicate code, so be careful! Having said this, if you can make
a good abstraction, do it! Don't repeat yourself, otherwise you'll find yourself
updating multiple places anytime you want to change one thing.

抽象化を正しく行うことが重要です。そのため、*クラス* セクションで説明されているSOLIDの原則に従う必要があります。
悪い抽象化は、重複コードより悪い可能性があります、注意深くしましょう！
少し難しいことではありますが、もし良い抽象化ができるのであればそれをやってください！
自分自身を繰り返さないこと。そうしなければ、1つの場所を変更したいときはいつでも、複数の場所を変更することになります。

**Bad:**
```javascript
function showDeveloperList(developers) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Good:**
```javascript
function showEmployeeList(employees) {
  employees.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    const data = {
      expectedSalary,
      experience
    };

    switch (employee.type) {
      case 'manager':
        data.portfolio = employee.getMBAProjects();
        break;
      case 'developer':
        data.githubLink = employee.getGithubLink();
        break;
    }

    render(data);
  });
}
```
**[⬆ back to top](#table-of-contents)**

### Set default objects with Object.assign
### Object.assignでデフォルトオブジェクトを設定すること

**Bad:**
```javascript
const menuConfig = {
  title: null,
  body: 'Bar',
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable = config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**Good:**
```javascript
const menuConfig = {
  title: 'Order',
  // User did not include 'body' key
  // ユーザーは `body` キーを含めなくていい
  buttonText: 'Send',
  cancellable: true
};

function createMenu(config) {
  config = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);

  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // configはこれと同じになります
  // ...
}

createMenu(menuConfig);
```
**[⬆ back to top](#table-of-contents)**


### Don't use flags as function parameters
### フラグを関数の引数のように利用しない
Flags tell your user that this function does more than one thing. Functions should do one thing. Split out your functions if they are following different code paths based on a boolean.

フラグは、この関数が複数のことを行うことを利用者に伝えます。
関数は1つのことを行うべきです。関数が真偽値によって異なるコードの経路を経由する場合、その関数を分割してください。

**Bad:**
```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Good:**
```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```
**[⬆ back to top](#table-of-contents)**

### Avoid Side Effects (part 1)
### 副作用を避ける (part 1)
A function produces a side effect if it does anything other than take a value in
and return another value or values. A side effect could be writing to a file,
modifying some global variable, or accidentally wiring all your money to a
stranger.

関数が、値を受け取り何か他の値を返す以外のことを行う場合、副作用を引き起こします。
副作用とは、ファイルを書き込みしたり、なにかのグローバル変数を書き換えたり、誤ってあなたの全てのお金を見知らぬ人に振込みするようなものです。

Now, you do need to have side effects in a program on occasion. Like the previous
example, you might need to write to a file. What you want to do is to
centralize where you are doing this. Don't have several functions and classes
that write to a particular file. Have one service that does it. One and only one.

時には、副作用を持つプログラムを必要とします。少し前に例で挙げた、ファイルに書き込みしなければならない場合のように。
あなたがしたいことは、どこでこれを行うかを集中させることです。
ファイルを部分的に書き換えするような、関数やクラスをいくつも持たないでください。
それを行う1つのサービスを持ってください。1つ、1だけです。

The main point is to avoid common pitfalls like sharing state between objects
without any structure, using mutable data types that can be written to by anything,
and not centralizing where your side effects occur. If you can do this, you will
be happier than the vast majority of other programmers.

重要なポイントは、何でも書き込むことができる可変長のデータ型を用いて、何の構造も無しにオブジェクト間で状態を共有し、
副作用が発生する場所を集中させないといった、共通の落とし穴を避けることです。
これを行うことができれば、大多数の他のプログラマーよりも幸せになれます。

**Bad:**
```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
// グローバル変数があとの関数から参照されている
// もし、この名前を別の関数で（直接）使っている場合、これは配列となって、そして壊れる。
let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Good:**
```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

const name = 'Ryan McDermott';
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```
**[⬆ back to top](#table-of-contents)**

### Avoid Side Effects (part 2)
### 副作用を避ける (part 2)
In JavaScript, primitives are passed by value and objects/arrays are passed by
reference. In the case of objects and arrays, if your function makes a change
in a shopping cart array, for example, by adding an item to purchase,
then any other function that uses that `cart` array will be affected by this
addition. That may be great, however it can be bad too. Let's imagine a bad
situation:

JavaScriptでは、プリミティブ型は値渡しであり、オブジェクトと配列は参照渡しです。
オブジェクトと配列の場合、もし関数がショッピングカート内の配列に変更を加える場合、（例えば、購入するために商品を加えるなど）
この`cart`と同じ配列を使っている他の関数は、この追加の影響を受けます。

The user clicks the "Purchase", button which calls a `purchase` function that
spawns a network request and sends the `cart` array to the server. Because
of a bad network connection, the `purchase` function has to keep retrying the
request. Now, what if in the meantime the user accidentally clicks "Add to Cart"
button on an item they don't actually want before the network request begins?
If that happens and the network request begins, then that purchase function
will send the accidentally added item because it has a reference to a shopping
cart array that the `addItemToCart` function modified by adding an unwanted
item.

ユーザーが購入ボタンをクリックすると、`purchase`関数を呼び出し、その関数はネットワークリクエストを生成して、その`cart`配列をサーバーへ送信します。
しかし、ネットワーク接続が悪いために、`purchase`関数はリクエストを繰り返し送信し続けなければならない。

ユーザーが、ネットワークリクエストが始まる前に欲しいとは思っていない商品の"カートに追加する"ボタンをうっかりクリックしてしまった場合。
もしそれが起こって、ネットワークリクエストが始まった場合、その時、そのpurchase関数は間違って追加された商品を送信してしまいます。
なぜなら、その関数は`addItemToCart`によって望んでいない商品を追加され、変更されてしまったショッピングカート配列を参照しているからです。

A great solution would be for the `addItemToCart` to always clone the `cart`,
edit it, and return the clone. This ensures that no other functions that are
holding onto a reference of the shopping cart will be affected by any changes.

良い解決策は、`addItemToCart`が常に`cart`を複製して変更し、その変更したものを返すことでしょう。
このことは、ショッピングカートへの参照を保持している他の関数は、いかなる変更の影響も受けないことを保証します。

Two caveats to mention to this approach:
  1. There might be cases where you actually want to modify the input object,
but when you adopt this programming practice you will find that those cases
are pretty rare. Most things can be refactored to have no side effects!

  2. Cloning big objects can be very expensive in terms of performance. Luckily,
this isn't a big issue in practice because there are
[great libraries](https://facebook.github.io/immutable-js/) that allow
this kind of programming approach to be fast and not as memory intensive as
it would be for you to manually clone objects and arrays.

このアプローチに関する2つの注意点:

  1. 時に、渡されたオブジェクトを変更したい場所があるケースがありますが、このプログラミング手法を採用している場合、このケースは稀だということに気づくでしょう。
  そしてほとんどの場合、副作用がないようにリファクタリングすることができます。

  2. 巨大なオブジェクトを複製することは、パフォーマンスの面で非常にコストが高いことになります。
  幸運なことに、これはこの手法においては大きな問題ではありません。なせなら、このようなプログラミングアプローチをより高速かつ、手作業でオブジェクトや配列を複製するよりもメモリ使用量を抑えることができる[素晴らしいライブラリ](https://facebook.github.io/immutable-js/)が存在するからです。

**Bad:**
```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Good:**
```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date : Date.now() }];
};
```

**[⬆ back to top](#table-of-contents)**

### Don't write to global functions
### グローバル関数に書き込まない
Polluting globals is a bad practice in JavaScript because you could clash with another
library and the user of your API would be none-the-wiser until they get an
exception in production. Let's think about an example: what if you wanted to
extend JavaScript's native Array method to have a `diff` method that could
show the difference between two arrays? You could write your new function
to the `Array.prototype`, but it could clash with another library that tried
to do the same thing. What if that other library was just using `diff` to find
the difference between the first and last elements of an array? This is why it
would be much better to just use ES2015/ES6 classes and simply extend the `Array` global.

グローバルを汚染することはJavaScriptにおけるバッドプラクティスです。
なぜなら、他のライブラリをクラッシュさせるかもしれないし、あなたのAPIを使っているユーザーは、プロダクション環境で例外を受け取るまで、そのことについて何もわからないからです。
例を考えてみましょう。もし、JavaScriptの既存のArray関数を拡張して、`diff`という2つの配列間の差分をみることができる関数を追加したいとしたらどうでしょうか？
`Array.prototype`に新しい関数を作成することができるかもしれないですが、他のライブラリで同じことをやろうとしているものをクラッシュさせるかもしれません。
もし、他のライブラリが、`diff`を単に配列の最初と最後の差分を見つけるために利用していたとしたらどうでしょう？
このことは、なぜグローバルの`Array`を単純に拡張するよりも、ES2015/ES6のクラスを使った方がより良いかという理由です。

**Bad:**
```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Good:**
```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```
**[⬆ back to top](#table-of-contents)**

### Favor functional programming over imperative programming
### 手続き型プログラミングより関数型プログラミングを優先する
JavaScript isn't a functional language in the way that Haskell is, but it has
a functional flavor to it. Functional languages can be cleaner and easier to test.
Favor this style of programming when you can.

JavaScriptは、Haskellがやっているような関数型言語ではありませんが、部分的にその機能があります。
関数型言語はよりクリーンでテストしやすいものになります。あなたができる時は、このプログラミングスタイルを優先してください。

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

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**Good:**
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

const totalOutput = programmerOutput
  .map(output => output.linesOfCode)
  .reduce((totalLines, lines) => totalLines + lines);
```
**[⬆ back to top](#table-of-contents)**

### Encapsulate conditionals
### 条件をカプセル化する

**Bad:**
```javascript
if (fsm.state === 'fetching' && isEmpty(listNode)) {
  // ...
}
```

**Good:**
```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === 'fetching' && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```
**[⬆ back to top](#table-of-contents)**

### Avoid negative conditionals
### 否定的な条件を避ける

**Bad:**
```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Good:**
```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```
**[⬆ back to top](#table-of-contents)**

### Avoid conditionals
### 条件文を避ける
This seems like an impossible task. Upon first hearing this, most people say,
"how am I supposed to do anything without an `if` statement?" The answer is that
you can use polymorphism to achieve the same task in many cases. The second
question is usually, "well that's great but why would I want to do that?" The
answer is a previous clean code concept we learned: a function should only do
one thing. When you have classes and functions that have `if` statements, you
are telling your user that your function does more than one thing. Remember,
just do one thing.

これは一見不可能なタスクのように見えます。
最初にこれを聞いて、ほとんどの人はこう言います。「`if`文なしで、何をするの？」
この答えは、多くの場合、同じタスクを実行するためにポリモーフィズム(多様性)を使ってできるよ。ということです。
2つめの質問は大抵これです。「うーん、いいと思うんだけど、なぜそれをやりたいんだろう。。。」
この答えは、私たちが先に学んだクリーンなコードコンセプト、「関数はただ1つのことを行うべき」です。
あなたのクラスや関数が`if`文を持っているとき、この関数は1つ以上のことを行なっていることを示唆しています。
たった1つのことをやるということを覚えておいてください。

**Bad:**
```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case '777':
        return this.getMaxAltitude() - this.getPassengerCount();
      case 'Air Force One':
        return this.getMaxAltitude();
      case 'Cessna':
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**Good:**
```javascript
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```
**[⬆ back to top](#table-of-contents)**

### Avoid type-checking (part 1)
### 型チェックを避ける (part 1)
JavaScript is untyped, which means your functions can take any type of argument.
Sometimes you are bitten by this freedom and it becomes tempting to do
type-checking in your functions. There are many ways to avoid having to do this.
The first thing to consider is consistent APIs.

JavaScriptには型がありません、このことは、関数がどんな型の引数でも受け取ることができることを意味します。
ときにはこの自由に夢中になって、関数の中でタイプチェックをするような誘惑に駆られるようになります。
これをやらなければならないときに、これを避ける方法はたくさんあります。まず、最初に考えるべきことは、一貫性のあるAPIです。

**Bad:**
```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location('texas'));
  }
}
```

**Good:**
```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location('texas'));
}
```
**[⬆ back to top](#table-of-contents)**

### Avoid type-checking (part 2)
### 型チェックを避ける (part 2)
If you are working with basic primitive values like strings and integers,
and you can't use polymorphism but you still feel the need to type-check,
you should consider using TypeScript. It is an excellent alternative to normal
JavaScript, as it provides you with static typing on top of standard JavaScript
syntax. The problem with manually type-checking normal JavaScript is that
doing it well requires so much extra verbiage that the faux "type-safety" you get
doesn't make up for the lost readability. Keep your JavaScript clean, write
good tests, and have good code reviews. Otherwise, do all of that but with
TypeScript (which, like I said, is a great alternative!).

もし、あなたが文字列、数値のような基本的なプリミティブな値を扱っていて、ポリモーフィズムを使えない、しかしまだタイプチェックが必要だと感じている場合。
TypeScriptの利用を検討してみてください。
これは標準のJavaScript構文の上に静的な型を提供するので、通常のJavaScriptに変わる優れた代替品です。
通常のJavaScript手作業のタイプチェックの問題は、偽物の型安全を得るために、あまりにも多くの余計な構文が必要なことです。これらは失った可読性を補うようなものではありません。
JavaScriptをクリーンに保ち、良いテストを書き。良いコードレビューを行なってください。
それ以外の場合は、それらの全てをTypeScriptで行います。(私が言ったように、それは素晴らしい代替え品です！)

**Bad:**
```javascript
function combine(val1, val2) {
  if (typeof val1 === 'number' && typeof val2 === 'number' ||
      typeof val1 === 'string' && typeof val2 === 'string') {
    return val1 + val2;
  }

  throw new Error('Must be of type String or Number');
}
```

**Good:**
```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```
**[⬆ back to top](#table-of-contents)**

### Don't over-optimize
### 行き過ぎた最適化をしない
Modern browsers do a lot of optimization under-the-hood at runtime. A lot of
times, if you are optimizing then you are just wasting your time. [There are good
resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
for seeing where optimization is lacking. Target those in the meantime, until
they are fixed if they can be.

モダンブラウザは、ランタイムの中で多くの最適化を行います。
何度も最適化を行なっているのであれば、それは時間の無駄です。[ここにどこで最適化が不足するかをみるための良い資料](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)があります。
可能であれば、それらが修正されるまでは、それらだけを最適化の対象としてください。

**Bad:**
```javascript

// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
// 古いブラウザにおいては、キャッシュされていない`list.length`はコストが掛かる
// なぜなら、`list.length`が再計算されるから。しかし、モダンプラウザでは最適化されている
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Good:**
```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```
**[⬆ back to top](#table-of-contents)**

### Remove dead code
### 使っていないコードを削除する
Dead code is just as bad as duplicate code. There's no reason to keep it in
your codebase. If it's not being called, get rid of it! It will still be safe
in your version history if you still need it.

使っていないコードは重複したコードと同じくらい悪いだけのものです。コードベースに残しておく理由はなにもありません。
もし、呼び出されていないのであれば、それらを取り除きましょう！
もし、まだ必要であってもバージョン管理の中にあるだけで安全でしょう。

**Bad:**
```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');

```

**Good:**
```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```
**[⬆ back to top](#table-of-contents)**

## **Objects and Data Structures**
## **オブジェクトとデータ構造**

### Use getters and setters
### gettersとsettersを使うこと
Using getters and setters to access data on objects could be better than simply
looking for a property on an object. "Why?" you might ask. Well, here's an
unorganized list of reasons why:

* When you want to do more beyond getting an object property, you don't have
to look up and change every accessor in your codebase.
* Makes adding validation simple when doing a `set`.
* Encapsulates the internal representation.
* Easy to add logging and error handling when getting and setting.
* You can lazy load your object's properties, let's say getting it from a
server.

gettersとsettersを使ってオブジェクト上のデータにアクセスする方が、単純にオブジェクトのプロパティをみるよりも良くなります。
「なぜ？」と尋ねるかもしれない。これはあまりまとまっていないものですが、その理由のリストです。

* もし、オブジェクトのプロパティを取得する以上のことをやろうとした場合、コードベースの全てのアクセッサを検索して変更して回る必要がない。
* 単純に`set`を行う時に、バリデーションを追加することができる。
* 内部をカプセル化する。
* 取得や設定の時に、簡単にロギングやエラーハンドリングを追加できる。
* オブジェクトのプロパティを遅延評価することができる、これをその値をサーバーから取得すると言おう。

**Bad:**
```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0,
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**Good:**
```javascript
function makeBankAccount() {
  // this one is private
  // これはprivate
  let balance = 0;

  // a "getter", made public via the returned object below
  // getter、以下でオブジェクトを返すことでpublicにします
  function getBalance() {
    return balance;
  }

  // a "setter", made public via the returned object below
  // setter、パラメータオブジェクト受け取ることでpublicにします(なんか原文が違う。。。)
  function setBalance(amount) {
    // ... validate before updating the balance
    // ... balanceを更新するまえにバリデーションを行う
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance,
  };
}

const account = makeBankAccount();
account.setBalance(100);
```
**[⬆ back to top](#table-of-contents)**


### Make objects have private members
### オブジェクトはプライベートなメンバーを持つようにする
This can be accomplished through closures (for ES5 and below).

これはグロージャによって達成することができます。（ES5以前の場合）

**Bad:**
```javascript

const Employee = function(name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee('John Doe');
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

**Good:**
```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    },
  };
}

const employee = makeEmployee('John Doe');
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```
**[⬆ back to top](#table-of-contents)**


## **Classes**
### Prefer ES2015/ES6 classes over ES5 plain functions
### ES5の関数よりもES2015/ES6のクラスの方を好むこと
It's very difficult to get readable class inheritance, construction, and method
definitions for classical ES5 classes. If you need inheritance (and be aware
that you might not), then prefer ES2015/ES6 classes. However, prefer small functions over
classes until you find yourself needing larger and more complex objects.

古典的なES5クラスでは、可読性の良いクラス継承、構築、メソッド定義を行うことが難しいです。
もし、継承が必要な場合（そう認識していないかもしれない）、ES2015/ES6クラスを優先してください。
しかしならがら、巨大で複雑なオブジェクトが必要だとわかるまでは、クラスよりも小さな関数を優先してください。

**Bad:**
```javascript
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error('Instantiate Animal with `new`');
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function(age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error('Instantiate Mammal with `new`');
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error('Instantiate Human with `new`');
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**Good:**
```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() { /* ... */ }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() { /* ... */ }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() { /* ... */ }
}
```
**[⬆ back to top](#table-of-contents)**


### Use method chaining
### メソッドチェーンを利用すること
This pattern is very useful in JavaScript and you see it in many libraries such
as jQuery and Lodash. It allows your code to be expressive, and less verbose.
For that reason, I say, use method chaining and take a look at how clean your code
will be. In your class functions, simply return `this` at the end of every function,
and you can chain further class methods onto it.

これは、JavaScriptの中で非常に有用なパターンで、jQueryやLodashのような多くのライブラリの中でみることができます。
それは、コードを表現力を豊かにし、冗長であることを少なくします。
その理由から、私は、「メソッドチェーンを使って、あなたのコードがどれくらい綺麗になるか見てください。」と言います。
クラスの関数の中の全ての関数の終わりで、単純に`this`を返すことで、クラスの中にあるメソッドをチェーンすることができます。

**Bad:**
```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car('Ford','F-150','red');
car.setColor('pink');
car.save();
```

**Good:**
```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // NOTE: Returning this for chaining
    // NOTE: チェーンするためにthisを返します
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTE: Returning this for chaining
    // NOTE: チェーンするためにthisを返します
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTE: Returning this for chaining
    // NOTE: チェーンするためにthisを返します
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTE: Returning this for chaining
    // NOTE: チェーンするためにthisを返します
    return this;
  }
}

const car = new Car('Ford','F-150','red');
  .setColor('pink')
  .save();
```
**[⬆ back to top](#table-of-contents)**

### Prefer composition over inheritance
### 継承よりコンポジション（組み合わせ）を好む

As stated famously in [*Design Patterns*](https://en.wikipedia.org/wiki/Design_Patterns) by the Gang of Four,
you should prefer composition over inheritance where you can. There are lots of
good reasons to use inheritance and lots of good reasons to use composition.
The main point for this maxim is that if your mind instinctively goes for
inheritance, try to think if composition could model your problem better. In some
cases it can.

有名なGang of Four(四人組)による[*デザインパターン*](https://en.wikipedia.org/wiki/Design_Patterns)のように、
可能な場所では継承よりもコンポジションを優先するべきです。
継承を利用する良い理由もコンポジションを利用する良い理由もたくさんあります。
この章の要点は、もしあなたが本能的に継承を使うように心が動くのであれば、コンポジションがその問題をよりよくモデル化できるかどうか考えてみてください。
いくつかの場合、それができます。

You might be wondering then, "when should I use inheritance?" It
depends on your problem at hand, but this is a decent list of when inheritance
makes more sense than composition:

1. Your inheritance represents an "is-a" relationship and not a "has-a"
relationship (Human->Animal vs. User->UserDetails).
2. You can reuse code from the base classes (Humans can move like all animals).
3. You want to make global changes to derived classes by changing a base class.
(Change the caloric expenditure of all animals when they move).

あなたは、「いつ継承を使うべきか？」について疑問に思うかもしれません。
それは、手元にある問題に依存します。しかし、これらは継承がコンポジションよりも意味をなす場合のまともなリストです。

1. 継承が「had-a(訳注：「A had a B」で「AはBを含んでいる」の意味)」ではなくて「is-a(訳注：「A is a B」で「AはBである」の意味)」を表している場合(Human->Animal(人間は動物である) と User->UserDetails(人は属性情報を含んでいる))
2. 基底クラスからコードを再利用できる(人は動物のように動くことができる)
3. 基底クラスを変更することで、派生クラスを全体的に変更したい(全ての動物の移動中の消費カロリーを変更する)

**Bad:**
```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee
// よくない。なぜなら、従業員(Employee)は税情報を持っている。しかし、従業員税情報(EmployeeTaxData)は従業員ではない
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**Good:**
```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```
**[⬆ back to top](#table-of-contents)**

## **SOLID**
### Single Responsibility Principle (SRP)
### 単一責任の原則(SRP)
As stated in Clean Code, "There should never be more than one reason for a class
to change". It's tempting to jam-pack a class with a lot of functionality, like
when you can only take one suitcase on your flight. The issue with this is
that your class won't be conceptually cohesive and it will give it many reasons
to change. Minimizing the amount of times you need to change a class is important.
It's important because if too much functionality is in one class and you modify
a piece of it, it can be difficult to understand how that will affect other
dependent modules in your codebase.

クリーンコードに記載されているように、「クラスが変更される理由は、1つ以上あってはならない」。
多くの機能を詰め込んだ超満員のクラスは、フライトで1つのスーツケースしか持てない場合などは魅力的に見えます。
これに関する問題は、そのクラスが概念的に一貫性が乏しく、さまざなな理由により変更ことが多くあるからです。
クラスを変更することに掛かる時間が最小限にすることが重要です。
それが重要なのは、あまりにも多くの機能が1つのクラスにあって、その中のひとまとまりを変更する場合、コードベースの中の他の依存しているモジュールに対してどのように影響を与えるか理解することが難しいことになるからです。

**Bad:**
```javascript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**Good:**
```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}


class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```
**[⬆ back to top](#table-of-contents)**

### Open/Closed Principle (OCP)
### オープン・クローズドの原則(OCP)
As stated by Bertrand Meyer, "software entities (classes, modules, functions,
etc.) should be open for extension, but closed for modification." What does that
mean though? This principle basically states that you should allow users to
add new functionalities without changing existing code.

Bertrand Meyer(バートランド・メイヤー)が記したように、「ソフトウエアの構成要素(クラス、モジュール、関数など)は、拡張に対してオープンで、変更に対してはクローズであるべき」
それはどう言う意味でしょうか？
この原則は基本的に、ユーザーに対して既存のコードを変更することなく、新しい機能を加えることができるようにするべきと記されている。

**Bad:**
```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'ajaxAdapter';
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'nodeAdapter';
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === 'ajaxAdapter') {
      return makeAjaxCall(url).then((response) => {
        // transform response and return
      });
    } else if (this.adapter.name === 'httpNodeAdapter') {
      return makeHttpCall(url).then((response) => {
        // transform response and return
      });
    }
  }
}

function makeAjaxCall(url) {
  // request and return promise
}

function makeHttpCall(url) {
  // request and return promise
}
```

**Good:**
```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'ajaxAdapter';
  }

  request(url) {
    // request and return promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'nodeAdapter';
  }

  request(url) {
    // request and return promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then((response) => {
      // transform response and return
    });
  }
}
```
**[⬆ back to top](#table-of-contents)**

### Liskov Substitution Principle (LSP)
### リスコフの置換原則 (LSP)
This is a scary term for a very simple concept. It's formally defined as "If S
is a subtype of T, then objects of type T may be replaced with objects of type S
(i.e., objects of type S may substitute objects of type T) without altering any
of the desirable properties of that program (correctness, task performed,
etc.)." That's an even scarier definition.

これは非常に単縦なコンセプトと呼ぶには恐れ多いです。
正式には、次のように定義されています。
「もしSがTの派生型の場合、T型のオブジェクトは、そのプログラムの特性(正確さ、作業能力、など)を損なうことなく、S型のオブジェクトで置き換えることができなければならない(いういなれば、S型のオブジェクトはT型のオブジェクトと置換することができる)」
これでもさらにやっかいな定義です。

The best explanation for this is if you have a parent class and a child class,
then the base class and child class can be used interchangeably without getting
incorrect results. This might still be confusing, so let's take a look at the
classic Square-Rectangle example. Mathematically, a square is a rectangle, but
if you model it using the "is-a" relationship via inheritance, you quickly
get into trouble.

これのもっとも良い説明は、もし親クラスと子クラスがある場合、親クラスと子クラスは異なる振る舞いを起こすことなく、交互に利用することができるということです。
まだ混乱しているかもしれないので、古典的な正方形と長方形の例を見てましょう。
数学的には、正方形は長方形ですが、もし継承による「is-a(AはBである)」の関係を利用している場合、容易に問題に陥ります。

**Bad:**
```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach((rectangle) => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // BAD: Returns 25 for Square. Should be 20.　だめ！正方形に25が返ってしまう、20になるべきです。
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Good:**
```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach((shape) => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```
**[⬆ back to top](#table-of-contents)**

### Interface Segregation Principle (ISP)
### インターフェイス分離の原則 (ISP)
JavaScript doesn't have interfaces so this principle doesn't apply as strictly
as others. However, it's important and relevant even with JavaScript's lack of
type system.

JavaScriptはインターフェイスを持っていないため、この原則は他のように厳密には適用されません。
しかしながら、これは重要かつ、JavaScriptの欠落した型システムにも関連しています。

ISP states that "Clients should not be forced to depend upon interfaces that
they do not use." Interfaces are implicit contracts in JavaScript because of
duck typing.

ISPは「クライアントに、彼らが利用していないインターフェイスへの依存を強要してはならない」と記されています。
JavaScriptにおいては、ダックタイピングのため、インターフェイスは暗黙的な契約です。

A good example to look at that demonstrates this principle in JavaScript is for
classes that require large settings objects. Not requiring clients to setup
huge amounts of options is beneficial, because most of the time they won't need
all of the settings. Making them optional helps prevent having a
"fat interface".

このJavaScriptでこの原則を説明を見せるための良い例は、巨大な設定オブジェクトが必要なクラスです。
クライアントに巨大な数のオプション設定を要求しないことが効果的です。なぜなら、多くの場合すべての設定は必要ないからです。
それらを任意にすることが「肥ったインターフェイス」を持つことを防ぐために有効です。

**Bad:**
```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName('body'),
  animationModule() {} // Most of the time, we won't need to animate when traversing. ほとんどの場合、アニメーションを必要としていない
  // ...
});

```

**Good:**
```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName('body'),
  options: {
    animationModule() {}
  }
});
```
**[⬆ back to top](#table-of-contents)**

### Dependency Inversion Principle (DIP)
### 依存性逆転の原則 (DIP)
This principle states two essential things:
1. High-level modules should not depend on low-level modules. Both should
depend on abstractions.
2. Abstractions should not depend upon details. Details should depend on
abstractions.

この原則では2つの重要なことを述べています。
1. 上位のモジュールは下位のモジュールに依存してはならない。それぞれは、抽象に依存するべき。
2. 抽象は実装に依存してはいけない、実装は抽象に依存するべき。

This can be hard to understand at first, but if you've worked with AngularJS,
you've seen an implementation of this principle in the form of Dependency
Injection (DI). While they are not identical concepts, DIP keeps high-level
modules from knowing the details of its low-level modules and setting them up.
It can accomplish this through DI. A huge benefit of this is that it reduces
the coupling between modules. Coupling is a very bad development pattern because
it makes your code hard to refactor.

これははじめの内は理解することが難しですが、もしAngularJSを触ったことがある人であれば、依存性注入(DI)と言う形でその実装をみたことがあるでしょう。
それらは同一のコンセプトではありませんが、DIPは上位のモジュールが下位のモジュールの実装を知り、それらを設定することを保ちます（え？保つ？逆じゃないの？？）。
このことの巨大なメリットは、モジュール間の密結合を削減することです。
密結合することは、非常に悪い開発のパターンです。なぜなら、それはコードをリファクタしにくくするからです。

As stated previously, JavaScript doesn't have interfaces so the abstractions
that are depended upon are implicit contracts. That is to say, the methods
and properties that an object/class exposes to another object/class. In the
example below, the implicit contract is that any Request module for an
`InventoryTracker` will have a `requestItems` method.

以前に述べたように、JavaScriptはインターフェイスを持たないため、その抽象とは暗黙の契約の上に依存しています。
すなわち、あるオブジェクトやクラスのメソッドやプロパティは、他のオブジェクトやクラスにさらけ出されています。
下の例の中の暗黙の契約とは、全ての `InventoryTracker` リクエストモジュールは、 `requestItems` メソッドを持っていることです。

**Bad:**
```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ['HTTP'];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // BAD: We have created a dependency on a specific request implementation.
    // We should just have requestItems depend on a request method: `request`
    // だめ！ある特別なリクエスト実装に依存したものを持っている
    // `request`のような、リクエストメソッドに依存したrequestItemsだけを持つべき
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach((item) => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(['apples', 'bananas']);
inventoryTracker.requestItems();
```

**Good:**
```javascript
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach((item) => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ['HTTP'];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ['WS'];
  }

  requestItem(item) {
    // ...
  }
}

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.
// 外部から依存関係を構築しそれを注入することで、リクエストモジュールを
// 新しいWebSocketを使ったイケているものに置き換えることができる。
const inventoryTracker = new InventoryTracker(['apples', 'bananas'], new InventoryRequesterV2());
inventoryTracker.requestItems();
```
**[⬆ back to top](#table-of-contents)**

## **Testing**
## **テスト**
Testing is more important than shipping. If you have no tests or an
inadequate amount, then every time you ship code you won't be sure that you
didn't break anything. Deciding on what constitutes an adequate amount is up
to your team, but having 100% coverage (all statements and branches) is how
you achieve very high confidence and developer peace of mind. This means that
in addition to having a great testing framework, you also need to use a
[good coverage tool](http://gotwarlost.github.io/istanbul/).

テストはリリースするよりも大事なことです。
もしテストがなかったり不十分だった場合、コードをリリースするたびに、何も壊れていないことを確かめることはできません。
何をもって十分な量であるかを決定することはチームに任されていますが、(すべての文や分岐に対して)100%のカバレッジを持つことは、非常に高い信頼性と開発者の安心を達成する方法です。
この意味することは、あなたは良いテストフレームワークを持つことに加えて、[良いカバレッジツール](http://gotwarlost.github.io/istanbul/)も使うことが必要だということです。

There's no excuse to not write tests. There are [plenty of good JS test frameworks](http://jstherightway.org/#testing-tools), so find one that your team prefers.
When you find one that works for your team, then aim to always write tests
for every new feature/module you introduce. If your preferred method is
Test Driven Development (TDD), that is great, but the main point is to just
make sure you are reaching your coverage goals before launching any feature,
or refactoring an existing one.

テストを書かない理由はありません。ここに[多くの優れたJSテストフレームワーク](http://jstherightway.org/#testing-tools)があるので、チームが好むものを一つ見つけてください。
チームに対して一つのフレームワークを見つけた場合は、導入する全ての機能・モジュールごとに、常にテストを書くことを目指します。
もし、あなたが気に入っている方法がテスト駆動開発(TDD)であれば、それは素晴らしいことです。
しかし重要なポイントは、なにかの機能をリリースする前にカバレッジのゴールに達成するか、すでに存在するものをリファクタリングすることを確実にすることだけです。

### Single concept per test
### テストごとに単純なコンセプトを持つこと

**Bad:**
```javascript
import assert from 'assert';

describe('MakeMomentJSGreatAgain', () => {
  it('handles date boundaries', () => {
    let date;

    date = new MakeMomentJSGreatAgain('1/1/2015');
    date.addDays(30);
    assert.equal('1/31/2015', date);

    date = new MakeMomentJSGreatAgain('2/1/2016');
    date.addDays(28);
    assert.equal('02/29/2016', date);

    date = new MakeMomentJSGreatAgain('2/1/2015');
    date.addDays(28);
    assert.equal('03/01/2015', date);
  });
});
```

**Good:**
```javascript
import assert from 'assert';

describe('MakeMomentJSGreatAgain', () => {
  it('handles 30-day months', () => {
    const date = new MakeMomentJSGreatAgain('1/1/2015');
    date.addDays(30);
    assert.equal('1/31/2015', date);
  });

  it('handles leap year', () => {
    const date = new MakeMomentJSGreatAgain('2/1/2016');
    date.addDays(28);
    assert.equal('02/29/2016', date);
  });

  it('handles non-leap year', () => {
    const date = new MakeMomentJSGreatAgain('2/1/2015');
    date.addDays(28);
    assert.equal('03/01/2015', date);
  });
});
```
**[⬆ back to top](#table-of-contents)**

## **Concurrency**
## **同期処理**
### Use Promises, not callbacks
### コールバックではなく、Promiseを使う
Callbacks aren't clean, and they cause excessive amounts of nesting. With ES2015/ES6,
Promises are a built-in global type. Use them!

コールバックは簡潔ではありません、そしてそれらは過剰な量のネストを引き起こします。
ES2015/ES6ではPromiseがグローバルに組み込まれています。それらを使いましょう！

**Bad:**
```javascript
import { get } from 'request';
import { writeFile } from 'fs';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', (requestErr, response) => {
  if (requestErr) {
    console.error(requestErr);
  } else {
    writeFile('article.html', response.body, (writeErr) => {
      if (writeErr) {
        console.error(writeErr);
      } else {
        console.log('File written');
      }
    });
  }
});

```

**Good:**
```javascript
import { get } from 'request';
import { writeFile } from 'fs';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then((response) => {
    return writeFile('article.html', response);
  })
  .then(() => {
    console.log('File written');
  })
  .catch((err) => {
    console.error(err);
  });

```
**[⬆ back to top](#table-of-contents)**

### Async/Await are even cleaner than Promises
### Async/AwaitはPromiseよりさらに簡潔です
Promises are a very clean alternative to callbacks, but ES2017/ES8 brings async and await
which offer an even cleaner solution. All you need is a function that is prefixed
in an `async` keyword, and then you can write your logic imperatively without
a `then` chain of functions. Use this if you can take advantage of ES2017/ES8 features
today!

Promiseはコールバックに対しては非常に簡潔ですが、S2017/ES8はより簡潔な解決案としてasyncとawaitを引き連れてきました。
あなたが必要なことは`async`キーワードを関数の先頭につけることです。そして`then`で関数を連結することなく、ロジックを命令的に書くことができます。
あなたが、今日のES2017/ES8機能の恩恵を受けたい場合は、これを使ってください。

**Bad:**
```javascript
import { get } from 'request-promise';
import { writeFile } from 'fs-promise';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then((response) => {
    return writeFile('article.html', response);
  })
  .then(() => {
    console.log('File written');
  })
  .catch((err) => {
    console.error(err);
  });

```

**Good:**
```javascript
import { get } from 'request-promise';
import { writeFile } from 'fs-promise';

async function getCleanCodeArticle() {
  try {
    const response = await get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin');
    await writeFile('article.html', response);
    console.log('File written');
  } catch(err) {
    console.error(err);
  }
}
```
**[⬆ back to top](#table-of-contents)**


## **Error Handling**
## **エラーハンドリング**
Thrown errors are a good thing! They mean the runtime has successfully
identified when something in your program has gone wrong and it's letting
you know by stopping function execution on the current stack, killing the
process (in Node), and notifying you in the console with a stack trace.

例外が発生することは良いことです！この意味は、ランタイムがあなたのプログラムが何かおかしいことを正常に突き止めたということです。
それは、関数の実行を直近のスタックで停止し、そのプロセスを停止し(ノード中)、コンソールのスタックトレースを通じてあなたに知らせてくれます。

### Don't ignore caught errors
### 例外が捕らえられたことを無視しない
Doing nothing with a caught error doesn't give you the ability to ever fix
or react to said error. Logging the error to the console (`console.log`)
isn't much better as often times it can get lost in a sea of things printed
to the console. If you wrap any bit of code in a `try/catch` it means you
think an error may occur there and therefore you should have a plan,
or create a code path, for when it occurs.

例外を捕捉して何もしないということは、あなたがそのエラーを修正したり、エラーが言ったことに対応したりすることができません。
コンソール(`console.log`)にエラーを出力することは、頻繁にコンソール出力された海に埋もれてしまうため、それほど良いことではありません。
コードの一部を `try/catch` で囲うということは、そこでエラー発生するかもしれないということを意味します。
したがって、エラーが発生した時のために、なにかの対策をしたり、コードの行き先を作らなければなりません。

**Bad:**
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Good:**
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // One option (more noisy than console.log):
  console.error(error);
  // Another option:
  notifyUserOfError(error);
  // Another option:
  reportErrorToService(error);
  // OR do all three!
}
```

### Don't ignore rejected promises
### 失敗したpromiseを無視しない
For the same reason you shouldn't ignore caught errors
from `try/catch`.

同じ理由により、`try/catch`にて発生した例外を無視するべきではありません。

**Bad:**
```javascript
getdata()
  .then((data) => {
    functionThatMightThrow(data);
  })
  .catch((error) => {
    console.log(error);
  });
```

**Good:**
```javascript
getdata()
  .then((data) => {
    functionThatMightThrow(data);
  })
  .catch((error) => {
    // One option (more noisy than console.log):
    console.error(error);
    // Another option:
    notifyUserOfError(error);
    // Another option:
    reportErrorToService(error);
    // OR do all three!
  });
```

**[⬆ back to top](#table-of-contents)**


## **Formatting**
## **フォーマット**
Formatting is subjective. Like many rules herein, there is no hard and fast
rule that you must follow. The main point is DO NOT ARGUE over formatting.
There are [tons of tools](http://standardjs.com/rules.html) to automate this.
Use one! It's a waste of time and money for engineers to argue over formatting.

フォーマットは主観的です。ここにある多くのルールと同様に、あなたが従わなければならないような、厳格でがんじがらめのルールはありません。
ここに自動化するための[多くのツール](http://standardjs.com/rules.html)があります。
1つを使いましょう！エンジニアがフォーマットについて議論することは、時間とお金の無駄です。

For things that don't fall under the purview of automatic formatting
(indentation, tabs vs. spaces, double vs. single quotes, etc.) look here
for some guidance.

自動フォーマット設定の対象にならないもの(インデント、タブ vs スペース、ダブル vs シングルクォートなど)については、ここでいくつかのガイダンスを見てください。

### Use consistent capitalization
### 一貫性を持った大文字を利用すること
JavaScript is untyped, so capitalization tells you a lot about your variables,
functions, etc. These rules are subjective, so your team can choose whatever
they want. The point is, no matter what you all choose, just be consistent.

JavaScriptには型がありません。そのため大文字は変数や関数などについて多くのことを教えてくれます。
これらのルールは主観的なので、あなたのチームが望んているものを選ぶことができます。
ここでのポイントは、あなたが選んだもの全てについて、一貫性を持たせてくださいということだけです。

**Bad:**
```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const SONGS = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const ARTISTS = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**Good:**
```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```
**[⬆ back to top](#table-of-contents)**


### Function callers and callees should be close
### 関数の呼び出し元と呼び出し先は近くにあること
If a function calls another, keep those functions vertically close in the source
file. Ideally, keep the caller right above the callee. We tend to read code from
top-to-bottom, like a newspaper. Because of this, make your code read that way.

関数は他を呼び出す場合、それらをソースコードのなかの垂直方向で近くにおくようにしてください。
理想的には、呼び出し元を呼び出し先の上においてください。私たちは新聞のように、コードを上から下に読む傾向があります。
このため、あなたのコードをこのように読ませるようにしてください。

**Bad:**
```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, 'peers');
  }

  lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**Good:**
```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, 'peers');
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**[⬆ back to top](#table-of-contents)**

## **Comments**
## **コメント**

### Only comment things that have business logic complexity.
### ビジネスロジックが複雑なものにのみコメントすること
Comments are an apology, not a requirement. Good code *mostly* documents itself.

コメントは弁明です、必須ではありません。良いコードは *ほとんどが* ドキュメントそのものです。

**Bad:**
```javascript
function hashIt(data) {
  // The hash
  let hash = 0;

  // Length of string
  const length = data.length;

  // Loop through every character in data
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = data.charCodeAt(i);
    // Make the hash
    hash = ((hash << 5) - hash) + char;
    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**Good:**
```javascript

function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = ((hash << 5) - hash) + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}

```
**[⬆ back to top](#table-of-contents)**

### Don't leave commented out code in your codebase
### コメントアウトしたコードをコードベースに残さない
Version control exists for a reason. Leave old code in your history.

バージョン管理があることがその理由です。古いコードは履歴に残しましょう。

**Bad:**
```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Good:**
```javascript
doStuff();
```
**[⬆ back to top](#table-of-contents)**

### Don't have journal comments
### 日記のようなコメントは持たない
Remember, use version control! There's no need for dead code, commented code,
and especially journal comments. Use `git log` to get history!

バージョン管理を使うことを覚えてほしい！使われていないコード、コメント付きのコード、特に日記の様なコメント。
履歴を取得するためには `git log` を使ってください！

**Bad:**
```javascript
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**Good:**
```javascript
function combine(a, b) {
  return a + b;
}
```
**[⬆ back to top](#table-of-contents)**

### Avoid positional markers
### 位置どりのためのマーカーを避ける
They usually just add noise. Let the functions and variable names along with the
proper indentation and formatting give the visual structure to your code.

それらは通常はただのノイズです。
関数や変数に適切なインデントとフォーマットを施すことで、コードに視覚的な構造を与えることができます。

**Bad:**
```javascript
////////////////////////////////////////////////////////////////////////////////
// Scope Model Instantiation
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: 'foo',
  nav: 'bar'
};

////////////////////////////////////////////////////////////////////////////////
// Action setup
////////////////////////////////////////////////////////////////////////////////
const actions = function() {
  // ...
};
```

**Good:**
```javascript
$scope.model = {
  menu: 'foo',
  nav: 'bar'
};

const actions = function() {
  // ...
};
```
**[⬆ back to top](#table-of-contents)**

## Translation
## 翻訳

This is also available in other languages:

これは他の言語でも読むことができます。

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Spanish**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese**:
    - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
    - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**:
    - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
    - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
  - ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)
  - ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)
  - ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Indonesia**:
  [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**:
  [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)

**[⬆ back to top](#table-of-contents)**
