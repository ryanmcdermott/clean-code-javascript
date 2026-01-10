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
[_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882), adapted for JavaScript.
This is not a style guide. It’s a guide to producing [readable, reusable, and refactorable](https://github.com/ryanmcdermott/3rs-of-software-architecture) JavaScript software.

Robert C. Martin の著書 [_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) に記載されたソフトウェアエンジニアリングの原則を、JavaScript 向けに適用したものです。
これはスタイルガイドではありません。[可読性が高く、再利用でき、リファクタリングしやすい](https://github.com/ryanmcdermott/3rs-of-software-architecture) JavaScript を書くためのガイドです。

Not every principle herein has to be strictly followed, and even fewer will be universally agreed upon.
These are guidelines—codified over many years of collective experience.

ここに書かれた原則すべてに厳密に従う必要はありませんし、すべてが普遍的に合意されているわけでもありません。
これはガイドラインであり、長年の経験からまとめられた知見です。

Software engineering is only about 50 years old—we are still learning.
Perhaps in the future we’ll have harder rules, but for now let these guidelines serve as a touchstone for your team’s JavaScript quality.

ソフトウェアエンジニアリングはまだ 50 年ほどの歴史しかなく、私たちは常に学び続けています。
いつか厳格なルールが確立されるかもしれませんが、今のところは _あなたとあなたのチームが JavaScript の品質を判断するための指針_ として役立ててください。

One more thing: knowing these won’t instantly make you a better developer.
Every piece of code starts as a first draft—like wet clay that will be shaped later.
Don’t beat yourself up for imperfect drafts. Beat up the code instead!

もう 1 つ大切なことがあります。
これらの知識を覚えたからといって、すぐに優れた開発者になれるわけではありません。
すべてのコードは最初は「未完成のたたき台」です。
改善が必要な初稿に対して自分を責めないでください。責めるべきはコードそのものです！

## **Variables**

### Use meaningful and pronounceable variable names

### 意味があり、発音しやすい変数名を使うこと

**Bad:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Good:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ back to top](#table-of-contents)**

### Use the same vocabulary for the same type of variable

### 同じ種類の変数には同じ語彙を使うこと

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

### 検索可能な名前を使うこと

We will read more code than we will ever write. It's important that the code we do write is readable and searchable. By not naming variables that end up being meaningful for understanding our program, we hurt our readers. Make your names searchable. Tools like [buddy.js](https://github.com/danielstjules/buddy.js) and [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) can help identify unnamed constants.

私たちはコードを書くよりも、読むほうがずっと多いです。
そのため、書くコードが読みやすく、検索しやすいことは非常に重要です。
プログラムの理解にとって意味を持つ変数に名前を付けないと、読者を苦しめることになります。

名前は検索可能にしてください。
[buddy.js](https://github.com/danielstjules/buddy.js) や [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) といったツールは、名前が付いていない定数を検出するのに役立ちます。

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
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

**[⬆ back to top](#table-of-contents)**

### Use explanatory variables

### 説明的な変数を利用すること

**Bad:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**Good:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ back to top](#table-of-contents)**

### Avoid Mental Mapping

### メンタルマップ（暗黙的な読み替え）を避ける

Explicit is better than implicit.
明示は暗黙より優れている。

**Bad:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
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
const locations = ["Austin", "New York", "San Francisco"];
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

### 不必要なコンテキストを追加しない

If your class/object name tells you something, don't repeat that in your
variable name.

もしクラス名やオブジェクト名がすでに何かを表しているのであれば、変数名の中で同じ情報を繰り返してはいけません。

**Bad:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue",
};

function paintCar(car, color) {
  car.carColor = color;
}
```

**Good:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue",
};

function paintCar(car, color) {
  car.color = color;
}
```

**[⬆ back to top](#table-of-contents)**

### Use default parameters instead of short circuiting or conditionals

### 短絡評価や条件分岐の代わりにデフォルト引数を使う

Default parameters are often cleaner than short circuiting. Be aware that if you use them, your function will only provide default values for `undefined` arguments. Other "falsy" values such as `''`, `""`, `false`, `null`, `0`, and `NaN`, will not be replaced by a default value.

デフォルト引数のほうが、コードがより明確で読みやすくなります。

ただし注意点として、デフォルト引数が適用されるのは `undefined` の場合だけ です。
`''`、`""`、`false`、`null`、`0`、`NaN` といった “falsy” な値は、デフォルト値では置き換えられません。

**Bad:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**Good:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

## **Functions**

### Function arguments (2 or fewer ideally)

### 関数の引数（理想は 2 つ以下）

Limiting the amount of function parameters is incredibly important because it makes testing your function easier. Having more than three leads to a combinatorial explosion where you have to test tons of different cases with each separate argument.

関数の引数の数を制限することは非常に重要です。引数が少なければ、関数をテストしやすくなるからです。
引数が 3 つを超えると、組み合わせが爆発し、それぞれの引数について膨大なケースをテストしなければならなくなります。

One or two arguments is the ideal case, and three should be avoided if possible. Anything more than that should be consolidated. Usually, if you have more than two arguments then your function is trying to do too much. In cases where it's not, most of the time a higher-level object will suffice as an argument.

理想的なのは 1〜2 個の引数 であり、可能であれば 3 個も避けるべき です。
それ以上の引数が必要になる場合は、まとめられるか検討してください。
一般的には、引数が 2 個を超える関数は「やりすぎ」ていることが多いです。
そうでない場合でも、たいていはより高レベルのオブジェクトを引数として渡すことで十分です。

Since JavaScript allows you to make objects on the fly, without a lot of class boilerplate, you can use an object if you are finding yourself needing a lot of arguments.

JavaScript ではクラスのような面倒な準備なしにその場でオブジェクトを作れるため、引数が多くなりそうなときはオブジェクトを使うのが有効です。

To make it obvious what properties the function expects, you can use the ES2015/ES6 destructuring syntax. This has a few advantages:

1. When someone looks at the function signature, it's immediately clear what properties are being used.
1. It can be used to simulate named parameters.
1. Destructuring also clones the specified primitive values of the argument object passed into the function. This can help prevent side effects. Note: objects and arrays that are destructured from the argument object are NOT cloned.
1. Linters can warn you about unused properties, which would be impossible without destructuring.

関数がどのプロパティを期待しているか明確にするために、ES2015/ES6 の分割代入（destructuring）構文 を使うとよいでしょう。
これには次のメリットがあります：

1. 関数シグネチャを見るだけで、どのプロパティが使われているかすぐに分かる。
1. 名前付きパラメータのように使える。
1. 分割代入された プリミティブ値 は複製されるため、副作用を防ぐ助けになる。※ただし、分割代入された オブジェクトや配列は複製されない 点に注意。
1. Linter による「未使用プロパティ」の警告が効くようになる。分割代入しなければこれは不可能。

**Bad:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);
```

**Good:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true,
});
```

**[⬆ back to top](#table-of-contents)**

### Functions should do one thing

### 関数は 1 つのことだけを行うべき

This is by far the most important rule in software engineering. When functions do more than one thing, they are harder to compose, test, and reason about. When you can isolate a function to just one action, it can be refactored easily and your code will read much cleaner. If you take nothing else away from this guide other than this, you'll be ahead of many developers.

これはソフトウェアエンジニアリングにおけるもっとも重要なルールです。
関数が複数のことを行うと、構成しにくくなり、テストもしにくくなり、理解もしづらくなります。

関数を 1 つの動作に切り分けることができれば、リファクタリングが格段に容易になり、コードの読みやすさも大きく向上します。

このガイドから他のことを何も覚えなくても、このルールだけを守れれば、多くの開発者より一歩先に進めます。

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
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ back to top](#table-of-contents)**

### Function names should say what they do

### 関数名はその関数が何をするのかを明確に表すべき

**Bad:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
// 関数名からは何が追加されているのか分かりにくい。
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

### 関数は 1 つの抽象レベルだけを扱うべき

When you have more than one level of abstraction your function is usually doing too much. Splitting up functions leads to reusability and easier testing.

複数の抽象レベルを 1 つの関数に混在させてしまうと、その関数はたいてい「やりすぎている」状態になります。
関数を適切に分割すれば、再利用しやすくなり、テストもしやすくなります。

**Bad:**

```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
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
  const syntaxTree = parse(tokens);
  syntaxTree.forEach((node) => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach((REGEX) => {
    statements.forEach((statement) => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens) {
  const syntaxTree = [];
  tokens.forEach((token) => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

**[⬆ back to top](#table-of-contents)**

### Remove duplicate code

### 重複したコードを取り除くこと

Do your absolute best to avoid duplicate code. Duplicate code is bad because it means that there's more than one place to alter something if you need to change some logic.

重複コードは必ず避けるようにしましょう。
重複があるということは、あるロジックを変更したいときに 複数の場所を修正しなければならない ということであり、非常に悪い状態です。

Imagine if you run a restaurant and you keep track of your inventory: all your tomatoes, onions, garlic, spices, etc. If you have multiple lists that you keep this on, then all have to be updated when you serve a dish with tomatoes in them. If you only have one list, there's only one place to update!

たとえば、あなたがレストランを経営していて、トマト・玉ねぎ・にんにく・スパイスなどの在庫を管理しているとします。
もし在庫を記録したリストが複数あれば、トマトを使った料理を出すたびに すべてのリストを更新しなければならなくなります。
しかしリストが 1 つだけなら、更新すべき場所も 1 つだけで済みます。

Oftentimes you have duplicate code because you have two or more slightly different things, that share a lot in common, but their differences force you to have two or more separate functions that do much of the same things. Removing duplicate code means creating an abstraction that can handle this set of different things with just one function/module/class.

重複コードが生まれる典型的な理由は、「とてもよく似ているが、少しだけ違う」処理が複数存在するために、似たような関数を複数つくってしまう からです。
重複コードをなくすということは、こうした「少し違うだけの複数の処理」を 1 つの関数・モジュール・クラスに抽象化する ということです。

Getting the abstraction right is critical, that's why you should follow the SOLID principles laid out in the Classes section. Bad abstractions can be worse than duplicate code, so be careful! Having said this, if you can make a good abstraction, do it! Don't repeat yourself, otherwise you'll find yourself updating multiple places anytime you want to change one thing.

ただし、この「抽象化」を正しく行うことは非常に重要です。
これが難しい理由でもあり、Classes セクションで説明した SOLID 原則 に従うべき理由でもあります。
悪い抽象化は重複コードよりも悪影響を及ぼすので注意してください。

しかし、もし 良い抽象化 がつくれるなら、迷わずそうすべきです。
「同じことを繰り返さない（Don't repeat yourself）」を徹底しないと、あなたは 1 箇所の変更を行うために 複数箇所を更新し続ける未来 に直面することになります。

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
      githubLink,
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
      portfolio,
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
      experience,
    };

    switch (employee.type) {
      case "manager":
        data.portfolio = employee.getMBAProjects();
        break;
      case "developer":
        data.githubLink = employee.getGithubLink();
        break;
    }

    render(data);
  });
}
```

**[⬆ back to top](#table-of-contents)**

### Set default objects with Object.assign

### Object.assign を使ってデフォルトのオブジェクトを設定する

**Bad:**

```javascript
const menuConfig = {
  title: null,
  body: "Bar",
  buttonText: null,
  cancellable: true,
};

function createMenu(config) {
  config.title = config.title || "Foo";
  config.body = config.body || "Bar";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**Good:**

```javascript
const menuConfig = {
  title: "Order",
  // User did not include 'body' key
  // ユーザーは body キーを含めていない
  buttonText: "Send",
  cancellable: true,
};

function createMenu(config) {
  let finalConfig = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true,
    },
    config
  );
  return finalConfig;
  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // config は次のようになります：{ title: "Order", body: "Bar", buttonText: "Send", cancellable: true }
  // ...
}

createMenu(menuConfig);
```

**[⬆ back to top](#table-of-contents)**

### Don't use flags as function parameters

### フラグ（真偽値）を関数の引数として使わない

Flags tell your user that this function does more than one thing. Functions should do one thing. Split out your functions if they are following different code paths based on a boolean.

フラグが必要になるということは、その関数が複数のことをしているというサインです。
関数は 1 つのことだけを行うべきです。

もし真偽値によって処理が分岐するのであれば、関数を分割してそれぞれ別の処理として定義してください。

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

### 副作用を避ける（part 1）

A function produces a side effect if it does anything other than take a value in and return another value or values. A side effect could be writing to a file, modifying some global variable, or accidentally wiring all your money to a stranger.

関数は、値を受け取って別の値を返す以外のことを行うと、その時点で副作用を持ちます。
副作用には、ファイルへの書き込み、グローバル変数の変更、あるいは誤って全財産を知らない人に送金する、といったものが含まれます。

Now, you do need to have side effects in a program on occasion. Like the previous example, you might need to write to a file. What you want to do is to centralize where you are doing this. Don't have several functions and classes that write to a particular file. Have one service that does it. One and only one.

重要なのは、副作用の発生場所を 1 つに集中させること です。
特定のファイルに書き込む処理を、複数の関数やクラスに散在させてはいけません。
その役割を担う 単一のサービスを 1 つだけ用意してください。

The main point is to avoid common pitfalls like sharing state between objects without any structure, using mutable data types that can be written to by anything, and not centralizing where your side effects occur. If you can do this, you will be happier than the vast majority of other programmers.

ポイントは、構造もなくオブジェクト間で状態を共有したり、どこからでも書き換え可能な可変データ型を使ったり、副作用を発生させる場所を分散させたりする、といった典型的な落とし穴を避けることです。

これができるようになれば、あなたは大多数のプログラマーよりもずっと快適にコードを書けるようになるでしょう。

**Bad:**

```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
// グローバル変数がこのあとに続く関数から参照されている。
// もし同じ名前を別の関数でも使っていた場合、今やこの値は配列になってしまい、動作が壊れる可能性がある。
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Good:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ back to top](#table-of-contents)**

### Avoid Side Effects (part 2)

### 副作用を避ける（part 2）

In JavaScript, some values are unchangeable (immutable) and some are changeable (mutable). Objects and arrays are two kinds of mutable values so it's important to handle them carefully when they're passed as parameters to a function. A JavaScript function can change an object's properties or alter the contents of an array which could easily cause bugs elsewhere.

JavaScript では、変更できない値（immutable）と変更できる値（mutable）があります。
オブジェクトと配列は mutable な値 に分類されるため、関数に渡す際には特に注意が必要です。
JavaScript の関数は、オブジェクトのプロパティを変更したり、配列の内容を変更したりすることができ、その結果、思わぬ箇所でバグを引き起こす可能性があります。

Suppose there's a function that accepts an array parameter representing a shopping cart. If the function makes a change in that shopping cart array - by adding an item to purchase, for example - then any other function that uses that same `cart` array will be affected by this addition. That may be great, however it could also be bad. Let's imagine a bad situation:

たとえば、ショッピングカートを表す配列を受け取る関数があるとします。
もしその関数がカートの配列を変更（購入予定の商品を追加する、など）すると、同じ配列を使っている別の関数にもその変更が影響します。
これが望ましい場合もありますが、悪影響になるケースもあります。

悪いケースを想像してみましょう。

The user clicks the "Purchase" button which calls a `purchase` function that spawns a network request and sends the `cart` array to the server. Because of a bad network connection, the `purchase` function has to keep retrying the request. Now, what if in the meantime the user accidentally clicks an "Add to Cart" button on an item they don't actually want before the network request begins? If that happens and the network request begins, then that purchase function will send the accidentally added item because the `cart` array was modified.

ユーザーが「購入」ボタンを押すと、purchase 関数が呼ばれ、ネットワークリクエストを発行してカートの配列をサーバーに送信します。
しかしネットワークが不安定で、このリクエストが繰り返し再試行されるとします。

その間に、ユーザーが誤って「カートに追加」ボタンを押し、本当は購入したくない商品を追加してしまうかもしれません。
もしそのタイミングでリクエストが再試行されると、`purchase` 関数は誤って追加された商品まで送信してしまいます。
これは、カート配列が変更されてしまったために起こります。

A great solution would be for the `addItemToCart` function to always clone the `cart`, edit it, and return the clone. This would ensure that functions that are still using the old shopping cart wouldn't be affected by the changes.

この問題の優れた解決策は、`addItemToCart` 関数が 常にカートを clone（複製）し、複製を編集して返すことです。

Two caveats to mention to this approach:

1. There might be cases where you actually want to modify the input object, but when you adopt this programming practice you will find that those cases are pretty rare. Most things can be refactored to have no side effects!
1. Cloning big objects can be very expensive in terms of performance. Luckily, this isn't a big issue in practice because there are [great libraries](https://facebook.github.io/immutable-js/) that allow this kind of programming approach to be fast and not as memory intensive as it would be for you to manually clone objects and arrays.

このアプローチには 2 つの注意点があります：

1. 入力オブジェクトを実際に変更したいケースもまれにあるが、この手法を採用すると、そのようなケースは非常に少ないことに気づくでしょう。多くの処理は、副作用なしにリファクタリングできます。
1. 大きなオブジェクトを複製するのは、パフォーマンス面で高コストです。ただし現実的にはあまり問題になりません。なぜなら、手動で複製するより高速でメモリ効率も良い[優れたライブラリ](https://facebook.github.io/immutable-js/)が存在するからです。

**Bad:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Good:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ back to top](#table-of-contents)**

### Don't write to global functions

### グローバル関数に書き込まない

Polluting globals is a bad practice in JavaScript because you could clash with another library and the user of your API would be none-the-wiser until they get an exception in production. Let's think about an example: what if you wanted to extend JavaScript's native Array method to have a `diff` method that could show the difference between two arrays? You could write your new function to the `Array.prototype`, but it could clash with another library that tried to do the same thing. What if that other library was just using `diff` to find the difference between the first and last elements of an array? This is why it would be much better to just use ES2015/ES6 classes and simply extend the `Array` global.

グローバルを汚染することは、JavaScript における悪いプラクティスです。
なぜなら、他のライブラリと衝突する可能性があり、あなたの API の利用者は本番環境で例外が発生するまでその問題に気づけないからです。

例を考えてみましょう。
JavaScript のネイティブな Array に `diff` メソッドを追加して、2 つの配列の差分を取得できるようにしたいとします。
`Array.prototype` に新しいメソッドを書き込むことはできますが、同じ名前の `diff` を追加しようとしている他のライブラリと衝突する可能性 があります。

もしその別のライブラリが `diff` を「配列の最初と最後の要素の差分を求める関数」として実装していたらどうなるでしょうか？
互いの実装が上書きされ、どちらかが正常に動作しなくなる可能性があります。

このような理由から、Array.prototype を汚染するのではなく、ES2015/ES6 のクラス構文を使って `Array` を拡張するほうがずっと安全です。

**Bad:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter((elem) => !hash.has(elem));
};
```

**Good:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter((elem) => !hash.has(elem));
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Favor functional programming over imperative programming

### 手続き型プログラミングより、関数型プログラミングを優先する

JavaScript isn't a functional language in the way that Haskell is, but it has a functional flavor to it. Functional languages can be cleaner and easier to test. Favor this style of programming when you can.

JavaScript は Haskell のような純粋な関数型言語ではありませんが、関数型の特徴をある程度備えています。
関数型言語は、よりクリーンでテストしやすい傾向があります。

可能な場面では、このスタイルのプログラミングを優先しましょう。

**Bad:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500,
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500,
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150,
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000,
  },
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
    name: "Uncle Bobby",
    linesOfCode: 500,
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500,
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150,
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000,
  },
];

const totalOutput = programmerOutput.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
```

**[⬆ back to top](#table-of-contents)**

### Encapsulate conditionals

### 条件式をカプセル化する

**Bad:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**Good:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

### Avoid negative conditionals

### 否定形の条件式を避ける

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

This seems like an impossible task. Upon first hearing this, most people say, "how am I supposed to do anything without an `if` statement?" The answer is that you can use polymorphism to achieve the same task in many cases. The second question is usually, "well that's great but why would I want to do that?" The answer is a previous clean code concept we learned: a function should only do one thing. When you have classes and functions that have `if` statements, you are telling your user that your function does more than one thing. Remember, just do one thing.

これは一見、不可能なことのように思えるかもしれません。
この話を聞くと、多くの人は「`if` 文なしでどうやって処理を書くの？」と言います。

答えは、多くの場合、ポリモーフィズム（多態性）を使うことで同じことが実現できる ということです。
次によく出てくる質問は「それは良さそうだけど、なぜわざわざそうする必要があるの？」というものです。

その答えは、以前出てきたクリーンコードの基本原則、“関数は 1 つのことだけを行うべき” にあります。

クラスや関数の中に `if` 文があるということは、その関数が 複数のことを行っている ことを意味します。

常に思い出してください。
関数は 1 つのことだけを行うべきです。

**Bad:**

```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case "777":
        return this.getMaxAltitude() - this.getPassengerCount();
      case "Air Force One":
        return this.getMaxAltitude();
      case "Cessna":
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

### 型チェックを避ける（part 1）

JavaScript is untyped, which means your functions can take any type of argument. Sometimes you are bitten by this freedom and it becomes tempting to do type-checking in your functions. There are many ways to avoid having to do this. The first thing to consider is consistent APIs.

JavaScript には型がないため、関数はどんな型の引数でも受け取れてしまいます。
この自由さが裏目に出て、関数の中で型チェックをしたくなる誘惑が生まれることがあります。

しかし、これを避ける方法はたくさんあります。
まず最初に考えるべきことは、API の一貫性を保つこと です。

**Bad:**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**Good:**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ back to top](#table-of-contents)**

### Avoid type-checking (part 2)

### 型チェックを避ける（part 2）

If you are working with basic primitive values like strings and integers, and you can't use polymorphism but you still feel the need to type-check, you should consider using TypeScript. It is an excellent alternative to normal JavaScript, as it provides you with static typing on top of standard JavaScript syntax. The problem with manually type-checking normal JavaScript is that doing it well requires so much extra verbiage that the faux "type-safety" you get doesn't make up for the lost readability. Keep your JavaScript clean, write good tests, and have good code reviews. Otherwise, do all of that but with TypeScript (which, like I said, is a great alternative!).

もし、文字列や数値のような基本的なプリミティブ値を扱っていて、ポリモーフィズムを使えないにもかかわらず型チェックがどうしても必要だと感じる のであれば、TypeScript を使うことを検討すべきです。

TypeScript は標準の JavaScript 構文の上に 静的型付け を提供してくれるため、通常の JavaScript の優れた代替手段です。

通常の JavaScript で手動の型チェックをしようとすると、きちんとやるには大量の余分な記述が必要になり、せっかく得られる「なんちゃって型安全性」よりも、失われる可読性のデメリットの方が大きくなってしまいます。

JavaScript をクリーンに保ち、良いテストを書き、良いコードレビューを行ってください。

もしそれで不十分だと感じるなら、それらすべてを TypeScript で行えばよい のです。
（先ほど言ったように、TypeScript は本当に優れた選択肢です！）

**Bad:**

```javascript
function combine(val1, val2) {
  if (
    (typeof val1 === "number" && typeof val2 === "number") ||
    (typeof val1 === "string" && typeof val2 === "string")
  ) {
    return val1 + val2;
  }

  throw new Error("Must be of type String or Number");
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

Modern browsers do a lot of optimization under-the-hood at runtime. A lot of times, if you are optimizing then you are just wasting your time. [There are good resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) for seeing where optimization is lacking. Target those in the meantime, until they are fixed if they can be.

モダンなブラウザは、ランタイムで多くの最適化を内部的に行っています。
そのため、多くの場合あなたが「最適化しているつもり」でも、それは単なる時間の無駄です。

どこに最適化不足があるのかを確認するための[良い資料](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)も存在します。
それらを参考にし、必要であれば修正されるまで本当に問題となる箇所だけを対象にしてください。

**Bad:**

```javascript
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
// 古いブラウザでは、キャッシュしていない `list.length` をループごとに参照すると `list.length` の再計算が行われるためコストが高かった。
// しかしモダンブラウザでは、この部分は最適化されている。
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

Dead code is just as bad as duplicate code. There's no reason to keep it in your codebase. If it's not being called, get rid of it! It will still be safe in your version history if you still need it.

使われていないコード（デッドコード）は、重複コードと同じくらい悪い存在です。
コードベースに残しておく理由はまったくありません。

もし呼び出されていないのであれば、すぐに削除しましょう！
どうしても必要になった場合でも、バージョン管理の履歴に残っているので安全です。

**Bad:**

```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**Good:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ back to top](#table-of-contents)**

## **Objects and Data Structures**

## **オブジェクトとデータ構造**

### Use getters and setters

### getters と setters を使うこと

Using getters and setters to access data on objects could be better than simply looking for a property on an object. "Why?" you might ask. Well, here's an unorganized list of reasons why:

- When you want to do more beyond getting an object property, you don't have to look up and change every accessor in your codebase.
- Makes adding validation simple when doing a `set`.
- Encapsulates the internal representation.
- Easy to add logging and error handling when getting and setting.
- You can lazy load your object's properties, let's say getting it from a server.

オブジェクトのプロパティに直接アクセスするよりも、getter と setter を使ってアクセスする方が良い場合があります。
「なぜ？」と思うかもしれませんが、理由はいくつかあります：

- オブジェクトのプロパティ取得以上の処理をしたくなった場合でも、すべてのアクセス箇所を探して書き換える必要がなくなる。
- setter を使えば、値を設定する際に簡単にバリデーションを追加できる。
- オブジェクトの内部表現（内部状態）を カプセル化 できる。
- 値取得・設定時に ログ記録やエラーハンドリング を簡単に追加できる。
- オブジェクトのプロパティを遅延ロードできる（例：サーバーから取得するなど）。

**Bad:**

```javascript
function makeBankAccount() {
  // this one is private
  // これはプライベートな値
  let balance = 0;

  // a "getter", made public via the returned object below
  // getter。返り値のオブジェクトを通じて public にする
  function getBalance() {
    return balance;
  }

  // a "setter", made public via the returned object below
  // setter。返り値のオブジェクトを通じて public にする
  function setBalance(amount) {
    // ... validate before updating the balance
    // ... balance を更新する前にバリデーションを行う
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

これは（ES5 以前では）クロージャを使うことで実現できます。

**Bad:**

```javascript
const Employee = function (name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

**Good:**

````javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    }
  };
}

const employee = makeEmployee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
// ```

**[⬆ back to top](#table-of-contents)**

## **Classes**

### Prefer ES2015/ES6 classes over ES5 plain functions

### ES5 の単なる関数より、ES2015/ES6 のクラス構文を優先する

It's very difficult to get readable class inheritance, construction, and method definitions for classical ES5 classes. If you need inheritance (and be aware that you might not), then prefer ES2015/ES6 classes. However, prefer small functions over classes until you find yourself needing larger and more complex objects.

古典的な ES5 のクラス構文では、読みやすいクラス継承・コンストラクタ・メソッド定義を実現するのが非常に難しい です。

もし継承が必要であれば（実際には必要ではないことも多いですが）、ES2015/ES6 のクラスを使う方を優先してください。

ただし、大きく複雑なオブジェクトが必要だと判断できるまでは、クラスよりも小さな関数を優先することをおすすめします。

**Bad:**

```
javascript
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error("Instantiate Animal with `new`");
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function(age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error("Instantiate Mammal with `new`");
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error("Instantiate Human with `new`");
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

  move() {
    /* ... */
  }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() {
    /* ... */
  }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() {
    /* ... */
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Use method chaining

### メソッドチェーンを利用する

This pattern is very useful in JavaScript and you see it in many libraries such as jQuery and Lodash. It allows your code to be expressive, and less verbose. For that reason, I say, use method chaining and take a look at how clean your code will be. In your class functions, simply return `this` at the end of every function, and you can chain further class methods onto it.


このパターンは JavaScript でとても有用で、jQuery や Lodash など多くのライブラリでも利用されています。

メソッドチェーンを使うことで、コードをより表現力豊かにし、冗長さを減らすことができます。

そのため私はこう言います：
メソッドチェーンを使ってみてください。どれだけコードがクリーンになるか実感できるはずです。

クラスのメソッドでは、各メソッドの最後で `this` を返すだけで、後続のクラスメソッドをチェーンさせることができます。

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

const car = new Car("Ford", "F-150", "red");
car.setColor("pink");
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
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTE: Returning this for chaining
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTE: Returning this for chaining
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTE: Returning this for chaining
    return this;
  }
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

**[⬆ back to top](#table-of-contents)**

### Prefer composition over inheritance

### 継承よりコンポジション（組み合わせ）を好む

As stated famously in [_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns) by the Gang of Four, you should prefer composition over inheritance where you can. There are lots of good reasons to use inheritance and lots of good reasons to use composition. The main point for this maxim is that if your mind instinctively goes for inheritance, try to think if composition could model your problem better. In some cases it can.

Gang of Four による有名な書籍[_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns) でも述べられているように、可能な限り継承よりコンポジションを優先 すべきです。

もちろん、継承を使うべき良い理由もあれば、コンポジションを使うべき良い理由もあります。

この格言のポイントは、あなたの思考が「継承を使うべきだ」と本能的に動いたとき、一度立ち止まり「コンポジションの方が問題をより適切に表現できないか？」と考えてみるべきということです。

実際、多くの場合でコンポジションの方が適しています。

You might be wondering then, "when should I use inheritance?" It depends on your problem at hand, but this is a decent list of when inheritance makes more sense than composition:

では、「どんなときに継承を使うべきなのか？」
以下は、コンポジションより継承のほうが適している典型的なケースです：

1. Your inheritance represents an "is-a" relationship and not a "has-a" relationship (Human->Animal vs. User->UserDetails).
1. You can reuse code from the base classes (Humans can move like all animals).
1. You want to make global changes to derived classes by changing a base class. (Change the caloric expenditure of all animals when they move).

1. 継承関係が “has-a（〜を持っている）” ではなく “is-a（〜である）” を表している場合（Human → Animal は "人間は動物である"、
User → UserDetails は "ユーザーはユーザー情報である" とは言えない）
1. 基底クラスのコードを再利用したい場合（例：すべての動物は移動でき、そのロジックを人間も再利用できる）
1. 基底クラスの変更を派生クラス全体に反映させたい場合（例：動物の移動時の消費カロリー計算を基底クラスで変えれば、全ての動物クラスに変更が反映される）


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
// よくない例。なぜなら従業員（Employee）は税情報を「持っている」だけであり、
// EmployeeTaxData は Employee の一種（型）ではない。
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

As stated in Clean Code, "There should never be more than one reason for a class to change". It's tempting to jam-pack a class with a lot of functionality, like when you can only take one suitcase on your flight. The issue with this is that your class won't be conceptually cohesive and it will give it many reasons to change. Minimizing the amount of times you need to change a class is important. It's important because if too much functionality is in one class and you modify a piece of it, it can be difficult to understand how that will affect other dependent modules in your codebase.

Clean Code でも述べられているように、「クラスが変更される理由は 1 つだけであるべき」 です。

クラスに多くの機能を詰め込むのは、一見便利に思えるかもしれません。
たとえば、飛行機にスーツケースを 1 つしか持ち込めないときのように、できる限り詰め込みたくなるものです。

しかし、そのように機能を詰め込みすぎると、クラスの概念的なまとまりが失われ、変更理由が増えすぎてしまいます。

クラスを変更しなければならない回数をできるだけ少なくすることは非常に重要です。
なぜなら、1 つのクラスに大量の役割を押し込んでしまうと、その一部を変更しただけでコードベース内の他の依存モジュールへどのような影響が及ぶのかが分かりにくくなるからです。

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

As stated by Bertrand Meyer, "software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification." What does that mean though? This principle basically states that you should allow users to add new functionalities without changing existing code.

Bertrand Meyer が述べているように、「ソフトウェアの構成要素（クラス、モジュール、関数など）は、拡張に対しては開かれており、変更に対しては閉じているべきである」というものです。

これはどういう意味でしょうか？

この原則が本質的に言っているのは、既存のコードを変更することなく、新しい機能を追加できるように設計すべきであるということです。

**Bad:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === "ajaxAdapter") {
      return makeAjaxCall(url).then(response => {
        // transform response and return
      });
    } else if (this.adapter.name === "nodeAdapter") {
      return makeHttpCall(url).then(response => {
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
    this.name = "ajaxAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
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
    return this.adapter.request(url).then(response => {
      // transform response and return
    });
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Liskov Substitution Principle (LSP)

### リスコフの置換原則 (LSP)

This is a scary term for a very simple concept. It's formally defined as "If S is a subtype of T, then objects of type T may be replaced with objects of type S (i.e., objects of type S may substitute objects of type T) without altering any of the desirable properties of that program (correctness, task performed, etc.)." That's an even scarier definition.

名前は難しそうですが、概念自体はとてもシンプルです。

正式な定義は次のとおりです：

「もし S が T のサブタイプであるなら、プログラムの望ましい性質（正しさ・実行結果など）を損なうことなく、T 型のオブジェクトを S 型のオブジェクトで置き換えることができなければならない。」

……これだけ読むと、さらに難しく感じるかもしれません。

The best explanation for this is if you have a parent class and a child class, then the base class and child class can be used interchangeably without getting incorrect results. This might still be confusing, so let's take a look at the classic Square-Rectangle example. Mathematically, a square is a rectangle, but if you model it using the "is-a" relationship via inheritance, you quickly get into trouble.

もっと分かりやすく言うと、親クラスと子クラスがあり、子クラスを親クラスの代わりとして問題なく扱えること。
これが LSP の本質です。

まだ少し分かりづらいかもしれないので、有名な「正方形と長方形」の例で考えてみましょう。

数学的には、正方形は長方形の一種です。
しかし継承を使って “正方形 は 長方形 である（is-a）” とモデル化すると、すぐに問題が生じます。

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
  rectangles.forEach(rectangle => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    // BAD: Returns 25 for Square. Should be 20.
    // だめ！Square（正方形）の場合 25 が返ってしまう。本来は 20 であるべき。
    const area = rectangle.getArea();
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
  shapes.forEach(shape => {
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

JavaScript doesn't have interfaces so this principle doesn't apply as strictly as others. However, it's important and relevant even with JavaScript's lack of type system.

JavaScript にはインターフェイスという仕組みがないため、この原則は他の言語ほど厳密に適用されるものではありません。
しかし、JavaScript に型システムがないことを踏まえても、この原則は非常に重要で、関連性の高いものです。


ISP states that "Clients should not be forced to depend upon interfaces that they do not use." Interfaces are implicit contracts in JavaScript because of duck typing.

ISP が述べているのは、「クライアント（利用者）は、使わないインターフェイスに依存させられるべきではない」ということです。

JavaScript ではダックタイピングにより、「インターフェイス」は暗黙的な契約として扱われます。

A good example to look at that demonstrates this principle in JavaScript is for classes that require large settings objects. Not requiring clients to setup huge amounts of options is beneficial, because most of the time they won't need all of the settings. Making them optional helps prevent having a "fat interface".

この原則を JavaScript で示す良い例は、大量の設定オプションを必要とするクラス です。

クライアントに膨大な設定項目の入力を強いるのは良くありません。
なぜなら、多くの場合それらすべてを必要としないからです。

設定を 任意（オプション） にすることで、使われないプロパティだらけの「太ったインターフェイス」を避けることができます。

**Bad:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.settings.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  // Most of the time, we won't need to animate when traversing.
  // ほとんどの場合、ツリーを走査するときにアニメーションは必要ない。
  animationModule() {}
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
  rootNode: document.getElementsByTagName("body"),
  options: {
    animationModule() {}
  }
});
```

**[⬆ back to top](#table-of-contents)**

### Dependency Inversion Principle (DIP)

### 依存性逆転の原則 (DIP)

This principle states two essential things:

1. High-level modules should not depend on low-level modules. Both should depend on abstractions.
1. Abstractions should not depend upon details. Details should depend on abstractions.

この原則は、次の 2 つの重要な点を述べています：

1. 上位レベルのモジュールは下位レベルのモジュールに依存してはならない。両者とも「抽象」に依存すべきである。
1. 抽象は詳細（実装）に依存してはならず、詳細が抽象に依存すべきである。

This can be hard to understand at first, but if you've worked with AngularJS, you've seen an implementation of this principle in the form of Dependency Injection (DI). While they are not identical concepts, DIP keeps high-level modules from knowing the details of its low-level modules and setting them up. It can accomplish this through DI. A huge benefit of this is that it reduces the coupling between modules. Coupling is a very bad development pattern because it makes your code hard to refactor.

最初は分かりづらいかもしれませんが、もしあなたが AngularJS を使ったことがあるなら、依存性注入（DI）という形でこの原則が実装されているのを見たことがあるでしょう。

DIP と DI は完全に同じ概念ではありませんが、DIP に従うことで、上位のモジュールが下位モジュールの詳細を知らずに済むようにする ことができます。
そのための手段として DI が利用できます。

この原則の大きな利点は、モジュール同士の結合度を下げられる ことです。
結合度が高いと、コードは極めてリファクタリングしにくくなり、悪い設計の典型例と言えます。

As stated previously, JavaScript doesn't have interfaces so the abstractions that are depended upon are implicit contracts. That is to say, the methods and properties that an object/class exposes to another object/class. In the example below, the implicit contract is that any Request module for an `InventoryTracker` will have a `requestItems` method.

先ほど述べたように、JavaScript にはインターフェイスがないため、「抽象」は 暗黙の契約（インターフェイスに相当するメソッド構成） として扱われます。

つまり、あるオブジェクト／クラスが別のオブジェクト／クラスに提供する メソッドやプロパティが「抽象」に相当します。

以下の例では、`InventoryTracker` が依存するすべての Request モジュールは `requestItems` メソッドを持っている という暗黙の契約が前提になっています。

**Bad:**

```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
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
    // だめ！特定のリクエスト実装に依存してしまっている。
    // requestItems は、`request` というリクエストメソッドだけに依存すべきである。
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(["apples", "bananas"]);
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
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ["WS"];
  }

  requestItem(item) {
    // ...
  }
}

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.
// 依存関係を外部で構築し、それを注入することで、
// WebSocket を使う新しいリクエストモジュールなどに簡単に置き換えられる。
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ back to top](#table-of-contents)**

## **Testing**

## **テスト**

Testing is more important than shipping. If you have no tests or an inadequate amount, then every time you ship code you won't be sure that you didn't break anything. Deciding on what constitutes an adequate amount is up to your team, but having 100% coverage (all statements and branches) is how you achieve very high confidence and developer peace of mind. This means that in addition to having a great testing framework, you also need to use a [good coverage tool](http://gotwarlost.github.io/istanbul/).

テストはリリースよりも重要です。
テストがなかったり量が不十分であれば、コードをリリースするたびに「なにか壊していないだろうか？」という不安がつきまといます。

どれくらいのテスト量が「十分」なのかはチームによって異なりますが、100% のカバレッジ（すべての文と分岐） を達成することは、非常に高い安心感と開発者の心の平穏をもたらします。

これは、優れたテストフレームワークを用意するだけでなく、[良いカバレッジ測定ツール](http://gotwarlost.github.io/istanbul/)を使う必要があることも意味します。

There's no excuse to not write tests. There are [plenty of good JS test frameworks](http://jstherightway.org/#testing-tools), so find one that your team prefers. When you find one that works for your team, then aim to always write tests for every new feature/module you introduce. If your preferred method is Test Driven Development (TDD), that is great, but the main point is to just make sure you are reaching your coverage goals before launching any feature, or refactoring an existing one.

テストを書かない言い訳は存在しません。
優れた[JavaScript のテストフレームワーク](http://jstherightway.org/#testing-tools)はたくさんあるので、チームに最適なものを選んでください。

チームに合うツールが見つかったら、新しい機能やモジュールを追加するたびに必ずテストを書くことを目指すべきです。

もしあなたがテスト駆動開発（TDD）を好むなら、それは素晴らしいことです。
しかし最も重要なのは、新しい機能をリリースしたり、既存コードをリファクタリングする前にカバレッジの目標をきちんと満たしていることを確認することです。

### Single concept per test

### テストごとに単一の概念だけ扱う

**Bad:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles date boundaries", () => {
    let date;

    date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);

    date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);

    date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**Good:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles 30-day months", () => {
    const date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);
  });

  it("handles leap year", () => {
    const date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);
  });

  it("handles non-leap year", () => {
    const date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**[⬆ back to top](#table-of-contents)**

## **Concurrency**

## **非同期処理**

### Use Promises, not callbacks

### Promise を使い、コールバックは使わないこと

Callbacks aren't clean, and they cause excessive amounts of nesting. With ES2015/ES6, Promises are a built-in global type. Use them!

コールバックはコードが煩雑になり、深いネストを招くためクリーンではありません。

ES2015/ES6 では Promise がグローバルで利用できる標準機能 なので、積極的に Promise を使いましょう！

**Bad:**

```javascript
import { get } from "request";
import { writeFile } from "fs";

get(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  (requestErr, response, body) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      writeFile("article.html", body, writeErr => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log("File written");
        }
      });
    }
  }
);
```

**Good:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**[⬆ back to top](#table-of-contents)**

### Async/Await are even cleaner than Promises

### Async/Await は Promise よりさらにクリーン

Promises are a very clean alternative to callbacks, but ES2017/ES8 brings async and await which offer an even cleaner solution. All you need is a function that is prefixed in an `async` keyword, and then you can write your logic imperatively without a `then` chain of functions. Use this if you can take advantage of ES2017/ES8 features today!

Promise はコールバックに対する非常にクリーンな代替手段ですが、ES2017/ES8 で導入された async と await は、さらに読みやすい解決策を提供します。

関数の先頭に async キーワードを付けるだけで、then を連ねる必要のない より自然な命令的な書き方 ができるようになります。

もし ES2017/ES8 の機能を利用できる環境であれば、ぜひ async/await を使いましょう。

**Bad:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**Good:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

async function getCleanCodeArticle() {
  try {
    const body = await get(
      "https://en.wikipedia.org/wiki/Robert_Cecil_Martin"
    );
    await writeFile("article.html", body);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}

getCleanCodeArticle()
```

**[⬆ back to top](#table-of-contents)**

## **Error Handling**

## **エラーハンドリング**

Thrown errors are a good thing! They mean the runtime has successfully identified when something in your program has gone wrong and it's letting you know by stopping function execution on the current stack, killing the process (in Node), and notifying you in the console with a stack trace.

エラーが投げられることは良いことです！
これは、ランタイムが あなたのプログラムで何か問題が起きたことを正しく検出し、現在のスタックで関数の実行を停止し、（Node では）プロセスを終了し、スタックトレース付きでコンソールに知らせてくれているという意味です。

### Don't ignore caught errors

### 捕捉したエラーを無視しない

Doing nothing with a caught error doesn't give you the ability to ever fix or react to said error. Logging the error to the console (`console.log`) isn't much better as often times it can get lost in a sea of things printed to the console. If you wrap any bit of code in a `try/catch` it means you think an error may occur there and therefore you should have a plan, or create a code path, for when it occurs.

エラーを捕捉しても何もしないのは最悪です。
そのエラーに対して修正も対応もできなくなります。

`console.log` でエラーを出力するだけでは改善にはなりません。
ログが他のメッセージに埋もれ、気づけない可能性が高いからです。

`try/catch` でコードを囲むということは、その場所でエラーが起こり得るとあなた自身が認識している ということです。
ならば、エラーが起きたときにどうするかという対処方法や分岐を用意すべきです。

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

### 失敗した Promise（reject）を無視しないこと

For the same reason you shouldn't ignore caught errors from `try/catch`.

これは `try/catch` で捕捉したエラーを無視してはいけないのと同じ理由です。

**Bad:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**Good:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
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

Formatting is subjective. Like many rules herein, there is no hard and fast rule that you must follow. The main point is DO NOT ARGUE over formatting. There are [tons of tools](http://standardjs.com/rules.html) to automate this. Use one! It's a waste of time and money for engineers to argue over formatting.

フォーマットは主観的なものです。
ここにある他のルールと同様、絶対に従わなければならない厳格なルールはありません。

重要なのは、フォーマットについて議論しないこと です。
この作業は[自動化ツールが大量に存在します](http://standardjs.com/rules.html)。
それらを使いましょう！
エンジニアがフォーマットを巡って議論するのは、時間とお金の無駄です。

For things that don't fall under the purview of automatic formatting (indentation, tabs vs. spaces, double vs. single quotes, etc.) look here for some guidance.

自動フォーマットの対象にならない部分（インデント、タブかスペースか、ダブルクォートかシングルクォートか、など）については、ここにいくつかガイドラインがあります。

### Use consistent capitalization

### 一貫性のある大文字・小文字（capitalization）を使うこと

JavaScript is untyped, so capitalization tells you a lot about your variables, functions, etc. These rules are subjective, so your team can choose whatever they want. The point is, no matter what you all choose, just be consistent.

JavaScript は型がないため、変数名や関数名などの大文字・小文字には多くの意味が含まれます。
これらのルールは主観的なので、チームで好きなルールを選べば構いません。

重要なのはどのルールを選んでも、必ず一貫性を保つということです。

**Bad:**

```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**Good:**

```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const ARTISTS = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

**[⬆ back to top](#table-of-contents)**

### Function callers and callees should be close

### 関数の呼び出し元と呼び出し先は近くに配置する

If a function calls another, keep those functions vertically close in the source file. Ideally, keep the caller right above the callee. We tend to read code from top-to-bottom, like a newspaper. Because of this, make your code read that way.

ある関数が別の関数を呼び出す場合、それらの関数はソースコード上で縦方向に近い位置に置くべきです。

理想的には、呼び出し元（caller）を呼び出し先（callee）の すぐ上 に配置します。

私たちは新聞のように、コードを上から下へと読む傾向があります。
だからこそ、コードもその流れで自然に読めるように並べるべきなのです。

**Bad:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
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
    return db.lookup(this.employee, "peers");
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
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

### ビジネスロジックが複雑な部分にのみコメントを書く

Comments are an apology, not a requirement. Good code mostly documents itself.

コメントは「言い訳」であって「必須」ではありません。良いコードは、ほとんどがコード自体で説明できるものです。

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
    hash = (hash << 5) - hash + char;
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
    hash = (hash << 5) - hash + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Don't leave commented out code in your codebase

### コメントアウトしたコードをコードベースに残さない

Version control exists for a reason. Leave old code in your history.

バージョン管理が存在するのには理由があります。
古いコードは履歴に残しておけば十分です。

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

### 日記のようなコメントを書かない

Remember, use version control! There's no need for dead code, commented code, and especially journal comments. Use `git log` to get history!

バージョン管理を使うことを忘れないでください！
使われていないコードやコメントアウトされたコード、そして特に日記のようなコメントは不要です。

履歴が必要なら、`git log` を使えば十分です。

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

### 位置取りのためのマーカーを使わない

They usually just add noise. Let the functions and variable names along with the proper indentation and formatting give the visual structure to your code.

それらはたいてい ただのノイズ を増やすだけです。

コードの視覚的な構造は、関数名・変数名・適切なインデント・フォーマットによって十分に表現できます。

**Bad:**

```javascript
////////////////////////////////////////////////////////////////////////////////
// Scope Model Instantiation
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: "foo",
  nav: "bar",
};

////////////////////////////////////////////////////////////////////////////////
// Action setup
////////////////////////////////////////////////////////////////////////////////
const actions = function () {
  // ...
};
```

**Good:**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar",
};

const actions = function () {
  // ...
};
```

**[⬆ back to top](#table-of-contents)**

## Translation

## 翻訳

This is also available in other languages:

本ガイドは以下の言語にも翻訳されています：

- 🇦🇲 Armenian: https://github.com/hanumanum/clean-code-javascript
- 🇧🇩 Bangla: https://github.com/InsomniacSabbir/clean-code-javascript/
- 🇧🇷 Brazilian Portuguese: https://github.com/fesnt/clean-code-javascript
- 🇨🇳 Simplified Chinese:
  - https://github.com/alivebao/clean-code-js
  - https://github.com/beginor/clean-code-javascript
- 🇹🇼 Traditional Chinese: https://github.com/AllJointTW/clean-code-javascript
- 🇫🇷 French: https://github.com/eugene-augier/clean-code-javascript-fr
- 🇩🇪 German: https://github.com/marcbruederlin/clean-code-javascript
- 🇮🇩 Indonesian: https://github.com/andirkh/clean-code-javascript/
- 🇮🇹 Italian: https://github.com/frappacchio/clean-code-javascript/
- 🇯🇵 Japanese: https://github.com/mitsuruog/clean-code-javascript/
- 🇰🇷 Korean: https://github.com/qkraudghgh/clean-code-javascript-ko
- 🇵🇱 Polish: https://github.com/greg-dev/clean-code-javascript-pl
- 🇷🇺 Russian:
  - https://github.com/BoryaMogila/clean-code-javascript-ru/
  - https://github.com/maksugr/clean-code-javascript
- 🇪🇸 Spanish: https://github.com/tureey/clean-code-javascript
- 🇺🇾 Spanish (Uruguay): https://github.com/andersontr15/clean-code-javascript-es
- 🇷🇸 Serbian: https://github.com/doskovicmilos/clean-code-javascript
- 🇹🇷 Turkish: https://github.com/bsonmez/clean-code-javascript/tree/turkish-translation
- 🇺🇦 Ukrainian: https://github.com/mindfr1k/clean-code-javascript-ua
- 🇻🇳 Vietnamese: https://github.com/hienvd/clean-code-javascript/
- 🇮🇷 Persian: https://github.com/hamettio/clean-code-javascript

**[⬆ back to top](#table-of-contents)**
````
