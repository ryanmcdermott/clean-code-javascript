আসল রিপোজিটরি: ryanmcdermott/clean-code-javascript

# clean-code-javascript

## সূচিপত্র
1. [ভূমিকা](#ভূমিকা)
2. [ভ্যারিয়েবলস](#ভ্যারিয়েবলস)
3. [ফাংশনস](#ফাংশনস)
4. [অবজেক্ট এবং ডাটা স্ট্রাকচার](#অবজেক্ট-এবং-ডাটা-স্ট্রাকচার)
5. [ক্লাস](#ক্লাস)
6. [সলিড(SOLID)](#সলিড(SOLID))
7. [টেস্টিং](#টেস্টিং)
8. [কনকারেন্সি](#কনকারেন্সি)
9. [এরর হ্যান্ডলিং](#এরর-হ্যান্ডলিং)
10. [ফরম্যাটিং](#ফরম্যাটিং)
11. [কমেন্টস](#কমেন্টস)
12. [অনুবাদ](#অনুবাদ)

## **ভূমিকা**

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

এখানে রবার্ট সি. মার্টিন এর [_Clean Code_ ](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) বইয়ে বর্নিত সফটওয়্যার ইঞ্জিনিয়ারিং এর নীতিগুলোকে জাভাস্ক্রিপ্ট এর জন্য কিছুটা পরিবর্তিত করা হয়েছে। এটা কোন স্টাইল গাইড না। এটা হল জাভাস্ক্রিপ্টের মাধ্যমে [সুপাঠ্য, পুনব্যবহারযোগ্য, রিফ্যাক্টরযোগ্য](https://github.com/ryanmcdermott/3rs-of-software-architecture) সফটওয়্যার তৈরি করার গাইড। 

ব্যাপারটা এমন না যে এখানে বর্নিত সব নিয়ম কঠোরভাবে মেনে চলতে হবে, এমনকি এখানে বর্নিত খুব কম নিয়মের সাথে সবাই একমত। এগুলো নির্দেশিকা ব্যাতিত কিছুই না। তবে এগুলো _Clean Code_ বইয়ের লেখকদের বহু বছরের সমষ্টিগত অভিজ্ঞতা থেকে বিধিবদ্ধ করা। 

আমাদের সফটওয়ার ইঞ্জিনিয়ারিং শিল্পের বয়স ৫০ বছরের কিছু বেশি এবং আমরা এখনও অনেক কিছু শিখছি। যখন সফটওয়ার আর্কিটেকচার স্থাপত্যকলার মত পুরনো হবে, তখন হয়ত আমরা মেনে চলার জন্য কিছু কঠোর নিয়ম পাব। সেদিনের আগ পর্যন্ত আমরা যে জাভাস্ক্রিপ্ট কোড তৈরি করছি তার মান যাচাই করার জন্য এই নির্দেশিকাটিকে ব্যবহার করতে পারি। 

ভূমিকা শেষ করার আগে একটা কথা, এই নিয়ম গুলো জানলেই তুমি আগের থেকে ভালো সফটওয়্যার ডেভেলপার  হয়ে যাবে না এবং এগুলো অনেক বছর ধরে মেনে চলা মানে এই না যে তুমি আর ভুল করবে না। মাটির দলা থেকে যেমন বিভিন্ন আকৃতি তৈরি হয়, তেমনি প্রতিটা কোড শুরু হয় প্রথম খসড়া থেকে। আমরা যখন সবশেষে আমাদের সহকর্মীদের সাথে কোড রিভিও করতে বসি তখন আমাদের কোডের অসম্পূর্ণতাগুলোকে চেঁছে ফেলে দিই। প্রথম খসড়াতে ভুল থাকবেই। এজন্য নিজেকে শাস্তি দিও না, বরং তোমার কোড ঝেড়েমুছে ঠিক কর। 

## **ভ্যারিয়েবলস**

### ভ্যারিয়েবল এর নাম অর্থবহ হতে হবে

**খারাপ কোড:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**ভালো কোড:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### একই ধরণের ভ্যারিয়েবলের নামকরনের জন্য একই ধরণের শব্দ ব্যবহার করতে হবে। 

**খারাপ কোড:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**ভালো কোড:**

```javascript
getUser();
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### খুঁজতে সুবিধা হয় এমন নাম ব্যবহার করতে হবে

আমরা আমাদের ডেভেলপার জীবনে যত কোড লিখব, পড়তে হবে তার থেকে অনেক বেশি। একারণে সুপাঠ্য এবং সহজে খুঁজে পাওয়া যায় এমন কোড লিখা খুবই গুরুত্তপুর্ন। আমরা যদি আমাদের লিখা প্রোগ্রাম বুঝার জন্য ভ্যারিয়েবলের নাম যথেষ্ট অর্থবহ না করি, আমাদের পাঠকদের কষ্ট বাড়বে বই কমবে না। যেসকল ধ্রুবক এবং ভ্যারিয়েবলের নামকরন করা হয় নি সেগুলো চিহ্নিত করতে [buddy.js](https://github.com/danielstjules/buddy.js) এবং [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) এর মত টুলগুলো আমাদের  সাহায্য করতে পারে। 

**খারাপ কোড:**

```javascript
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);
```

**ভালো কোড:**

```javascript
// Declare them as capitalized named constants.
const MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### নাম দেখেই যেন বুঝা যায় এইরকমভাবে ভ্যারিয়েবল এর নামকরন করতে হবে। 

**খারাপ কোড:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**ভালো কোড:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### মেন্টাল ম্যাপিং এড়িয়ে চলতে হবে

নাই মামার চেয়ে কানা মামা ভালো। ভ্যারিয়েবলের নামকরনের সময় এর খেয়াল রাখতে হবে নাম থেকে যেন এর কাজ বুঝা যায়। 

**খারাপ কোড:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Wait, what is `l` for again?
  dispatch(l);
});
```

**ভালো কোড:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### অপ্রয়োজনীয় কন্টেক্সট যোগ করার দরকার নেই

ক্লাস/অবজেক্ট এর নাম থেকে কোন তথ্য জানা গেলে সেই তথ্য আবার ভ্যারিয়েবলের নামের মধ্যে রাখার দরকার নেই। 

**খারাপ কোড:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car) {
  car.carColor = "Red";
}
```

**ভালো কোড:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car) {
  car.color = "Red";
}
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### শর্ট সার্কিটিং/কন্ডিশনাল স্টেটমেন্ট থেকে ডিফন্ট ভ্যালু ব্যবহার করা ভালো। 

শর্ট সার্কিটিং থেকে ডিফল্ট আর্গুমেন্ট  অনেক বেশি পরিচ্ছন্ন। তবে এটা মাথায় রাখতে হবে যে, যদি আমরা ফাংশনে ডিফল্ট আর্গুমেন্ট  ব্যবহার করি তবে অসঙ্গায়িত আর্গুমেন্ট এর ক্ষেত্রেই শুধু মাত্র ডিফল্ট ভ্যালু ব্যবহৃত হবে। অন্যান্য falsy ভ্যালু, যেমনঃ `''`, `""`, `false`, `null`, `0`, এবং `NaN`, এগুলোর পরিবর্তে ডিফল্ট ভ্যালু বসবে না। 

**খারাপ কোড:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**ভালো কোড:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

## **ফাংশনস**

### ফাংশন আর্গুমেন্টস (২টার বেশি নয়, ১ টা হলে ভাল হয়) 

ফাংশন টেস্টিং সহজ করার জন্য, ফাংশন প্যারামিটার কে সীমাবদ্ধ করা খুবই গুরুত্তপুর্ন । ফাংশন প্যারামিটার ৩ এর অধিক হলে, বিন্যাস সমাবেশের কারণে আমাদের টেস্ট কেসের সংখ্যা বেড়ে যায়। 

একদমই অপারগ হলে ৩ টি ফাংশন প্যারামিটার ব্যবহার করা উচিত। তবে আদর্শ হল ২ বা তার কম। এর থেকে বেশি প্যারামিটার হলে তাদেরকে একটা অবজেক্ট এর মধ্যে একত্রীকরণের মাধ্যমে প্যারামিটার এর সংখ্যা কমিয়ে আনতে হবে। 

যেহেতু জাভাস্ক্রিপ্ট এ  অবজেক্ট তৈরি করতে গেলে ক্লাস বয়লারপ্লেট লাগে না, তাই যদি তোমার অনেকগুলো প্যারামিটার প্রয়োজন হয় তবে তুমি অবজেক্ট ব্যবহার করতে পার। 

একটা ফাংশনে কি কি প্যারামিটার আসলে যাচ্ছে তা বুঝানোর জন্য ES2015/ES6 এর destructuring syntax ব্যবহার করতে পার। এটার কিছু সুবিধা আছে, 
1. যখন কেউ ফাংশন সিগনেচার দেখবে তখনি বুঝে ফেলবে কি কি প্যারামিটার দেয়া হচ্ছে ফাংশনের মধ্যে। 
2. Destructring করলে, ফাংশনের আর্গুমেন্ট হিসেবে যে প্রিমিটিভ ভ্যালু দেয়া হয়েছে সেগুলো ক্লোন হয়ে যায়। যা কিনা আমাদের কে সাইড ইফেক্ট প্রতিরোধ করতে সহায়তা করে। তবে মাথায় রাখতে হবে, যদি এরে বা অবজেক্ট destructure করা হয়, সেক্ষেত্রে এরা ক্লোন হয় না। 
3. অব্যবহৃত প্যারামিটার খুঁজে বের করতে লীনটার আমাদের হেল্প করতে পারে। কিন্তু Destructure না করলে হয়ত এটা সম্ভব হত না। 

**খারাপ কোড:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```

**ভালো কোড:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### প্রতিটা ফাংশনের শুধু মাত্র একটি কাজ করা উচিত

সফটওয়্যার ইঞ্জিনিয়ারিং এ এখন পর্যন্ত এটাই সব থেকে গুরুত্তপুর্ন নীতি। যেসব ফাংশন যদি একের অধিক কাজ করে, সেসব ফাংশন টেস্ট করা, অন্য যায়গায় পুনব্যবহার করা কঠিন হয়ে যায়। একটি ফাংশন যদি কেবল একটি কাজ করে তবে তোমার কোড সুপাঠ্য এবং সহজে পুনরায় লেখা যাবে। তুমি যদি এই গাইডের আর কিছু না গ্রহণ করে  শুধু এই নিয়মটি গ্রহণ কর, তাহলেই তুমি অন্য ডেভেলপের থেকে অনেক এগিয়ে যাবে। 

**খারাপ কোড:**

```javascript
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**ভালো কোড:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### ফাংশনের নামেই বলা থাকতে হবে সেটি কি করে

**খারাপ কোড:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**ভালো কোড:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### ফাংশনে শুধু একধাপ অ্যাবস্ট্রাকশন থাকতে পারবে 

তোমার ফাংশনে একের অধিক ধাপে অ্যাবস্ট্রাকশন থাকা মানেই তোমার ফাংশন একের অধিক কাজ করছে। একে বিভিন্ন ছোটছোট ভাগে বিভক্ত করলে পুনরায় ব্যবহার করা , টেস্টিং করা সহজ হয়ে যায়। 

**খারাপ কোড:**

```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach(token => {
    // lex...
  });

  ast.forEach(node => {
    // parse...
  });
}
```

**ভালো কোড:**

```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);
  syntaxTree.forEach(node => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens) {
  const syntaxTree = [];
  tokens.forEach(token => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### ডুপ্লিকেট কোড থাকা যাবে না

তোমার সর্বোচ্চ চেষ্টা করবে যেন ডুপ্লিকেট কোড না থাকে। ডুপ্লিকেট কোড থাকা মানেই, কখনো কোন একটার লজিক পরিবর্তন করা লাগলে তোমার সবগুলো ডুপ্লিকেট কোডে পরিবর্তন করা লাগবে। 

চিন্তা কর তুমি একটা রেস্টুরেন্ট এ আছো এবং তোমার কাজ হচ্ছে রেস্টুরেন্ট এর স্টোরে কি কি আছে সেগুলোর খবর রাখা। মানে হল রেস্টুরেন্টে কতটুকু টমাটো, পেঁয়াজ, হলুদ, মশলা আছে সেগুলোর হিসাব রাখা। তুমি যদি অনেকগুলো যায়গায় এদের হিসাব রাখ, কোন একটার হিশাব কমলে বা বাড়লে তোমার সব লিস্টে পরিবর্তন করা লাগবে। কিন্তু তুমি যদি একটা লিস্টে এদের হিসাব রাখতে তাহলে একটা লিস্টে পরিবর্তন করলেই হত। 

মাঝে মাঝে এমন হয় যে আমাদের ডুপ্লিকেট কোড লিখতে হয়, কারণ দেখা যায়, ২ টা ফাংশন প্রায় একই কাজ করছে শুধু সামান্য একটু পার্থক্য আছে। এই সামান্য পার্থক্যের জন্য আমাদের ২ টা ফাংশন লেখা লাগে। এক্ষেত্রে একটা সমাধান হল, একটা অ্যাবস্ট্রাকশন তৈরি করা। 

এই অ্যাবস্ট্রাকশনটা ঠিকঠাক ভাবে করতে পারা খুবই গুরুত্বপুর্ন। একারণে তোমার উচিত _ক্লাস_ অধ্যায়ে বর্নিত SOLID প্রিন্সিপাল মেনে চলা। ভুল অ্যাবস্ট্রাকশন ডুপ্লিকেট কোড থেকেও ক্ষতিকর, অতএব সাধু সাবধান! তুমি যদি সঠিক ভাবে অ্যাবস্ট্রাকশন তৈরি করতে পার তবে করে ফেল। তা নাহলে একটা পরিবর্তনের জন্য একাধিক জায়গায় পরিবর্তন করা লাগবে। 

**খারাপ কোড:**

```javascript
function showDeveloperList(developers) {
  developers.forEach(developer => {
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
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio### Function names should say what they do
    };

    render(data);
  });
}
```

**ভালো কোড:**

```javascript
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    const data = {
      expectedSalary,
      experience
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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### ডিফল্ট অবজেক্ট সেট করার সময় Object.assign ব্যবহার কর

**খারাপ কোড:**

```javascript
const menuConfig = {
  title: null,
  body: "Bar",
  buttonText: null,
  cancellable: true
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

**ভালো কোড:**

```javascript
const menuConfig = {
  title: "Order",
  // User did not include 'body' key
  buttonText: "Send",
  cancellable: true
};

function createMenu(config) {
  config = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    },
    config
  );

  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### ফাংশন প্যারামিটার হিসেবে ফ্ল্যাগ ব্যবহার করবে না 

ফ্ল্যাগ ব্যবহার করলে তোমার কোড রিভিওয়ার বুঝতে পারে যে এই ফাংশনটি একাধিক কাজ করছে। যদি ফ্ল্যাগ এর ভ্যালু এর উপর নির্ভর করে তোমার কোডফ্লো বিভিন্ন দিকে যায়, তবে তাদের কে আলাদা ফাংশনে রূপান্তরিত কর। তারপর তোমার ফ্ল্যাগ এর উপর নির্ভর করে বিভিন্ন ফাংশন কে কল কর। 

**খারাপ কোড:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**ভালো কোড:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**


### Avoid Side Effects (part 1)

A function produces a side effect if it does anything other than take a value in
and return another value or values. A side effect could be writing to a file,
modifying some global variable, or accidentally wiring all your money to a
stranger.

Now, you do need to have side effects in a program on occasion. Like the previous
example, you might need to write to a file. What you want to do is to
centralize where you are doing this. Don't have several functions and classes
that write to a particular file. Have one service that does it. One and only one.

The main point is to avoid common pitfalls like sharing state between objects
without any structure, using mutable data types that can be written to by anything,
and not centralizing where your side effects occur. If you can do this, you will
be happier than the vast majority of other programmers.

**খারাপ কোড:**

```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**ভালো কোড:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Avoid Side Effects (part 2)

In JavaScript, primitives are passed by value and objects/arrays are passed by
reference. In the case of objects and arrays, if your function makes a change
in a shopping cart array, for example, by adding an item to purchase,
then any other function that uses that `cart` array will be affected by this
addition. That may be great, however it can be bad too. Let's imagine a bad
situation:

The user clicks the "Purchase", button which calls a `purchase` function that
spawns a network request and sends the `cart` array to the server. Because
of a bad network connection, the `purchase` function has to keep retrying the
request. Now, what if in the meantime the user accidentally clicks "Add to Cart"
button on an item they don't actually want before the network request begins?
If that happens and the network request begins, then that purchase function
will send the accidentally added item because it has a reference to a shopping
cart array that the `addItemToCart` function modified by adding an unwanted
item.

A great solution would be for the `addItemToCart` to always clone the `cart`,
edit it, and return the clone. This ensures that no other functions that are
holding onto a reference of the shopping cart will be affected by any changes.

Two caveats to mention to this approach:

1. There might be cases where you actually want to modify the input object,
   but when you adopt this programming practice you will find that those cases
   are pretty rare. Most things can be refactored to have no side effects!

2. Cloning big objects can be very expensive in terms of performance. Luckily,
   this isn't a big issue in practice because there are
   [great libraries](https://facebook.github.io/immutable-js/) that allow
   this kind of programming approach to be fast and not as memory intensive as
   it would be for you to manually clone objects and arrays.

**খারাপ কোড:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**ভালো কোড:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Don't write to global functions

Polluting globals is a bad practice in JavaScript because you could clash with another
library and the user of your API would be none-the-wiser until they get an
exception in production. Let's think about an example: what if you wanted to
extend JavaScript's native Array method to have a `diff` method that could
show the difference between two arrays? You could write your new function
to the `Array.prototype`, but it could clash with another library that tried
to do the same thing. What if that other library was just using `diff` to find
the difference between the first and last elements of an array? This is why it
would be much better to just use ES2015/ES6 classes and simply extend the `Array` global.

**খারাপ কোড:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**ভালো কোড:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Favor functional programming over imperative programming

JavaScript isn't a functional language in the way that Haskell is, but it has
a functional flavor to it. Functional languages can be cleaner and easier to test.
Favor this style of programming when you can.

**খারাপ কোড:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**ভালো কোড:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

const totalOutput = programmerOutput.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Encapsulate conditionals

**খারাপ কোড:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**ভালো কোড:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Avoid negative conditionals

**খারাপ কোড:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**ভালো কোড:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Avoid conditionals

This seems like an impossible task. Upon first hearing this, most people say,
"how am I supposed to do anything without an `if` statement?" The answer is that
you can use polymorphism to achieve the same task in many cases. The second
question is usually, "well that's great but why would I want to do that?" The
answer is a previous clean code concept we learned: a function should only do
one thing. When you have classes and functions that have `if` statements, you
are telling your user that your function does more than one thing. Remember,
just do one thing.

**খারাপ কোড:**

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

**ভালো কোড:**

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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Avoid type-checking (part 1)

JavaScript is untyped, which means your functions can take any type of argument.
Sometimes you are bitten by this freedom and it becomes tempting to do
type-checking in your functions. There are many ways to avoid having to do this.
The first thing to consider is consistent APIs.

**খারাপ কোড:**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**ভালো কোড:**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Avoid type-checking (part 2)

If you are working with basic primitive values like strings and integers,
and you can't use polymorphism but you still feel the need to type-check,
you should consider using TypeScript. It is an excellent alternative to normal
JavaScript, as it provides you with static typing on top of standard JavaScript
syntax. The problem with manually type-checking normal JavaScript is that
doing it well requires so much extra verbiage that the faux "type-safety" you get
doesn't make up for the lost readability. Keep your JavaScript clean, write
good tests, and have good code reviews. Otherwise, do all of that but with
TypeScript (which, like I said, is a great alternative!).

**খারাপ কোড:**

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

**ভালো কোড:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Don't over-optimize

Modern browsers do a lot of optimization under-the-hood at runtime. A lot of
times, if you are optimizing then you are just wasting your time. [There are good
resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
for seeing where optimization is lacking. Target those in the meantime, until
they are fixed if they can be.

**খারাপ কোড:**

```javascript
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**ভালো কোড:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Remove dead code

Dead code is just as bad as duplicate code. There's no reason to keep it in
your codebase. If it's not being called, get rid of it! It will still be safe
in your version history if you still need it.

**খারাপ কোড:**

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

**ভালো কোড:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

## **অবজেক্ট এবং ডাটা স্ট্রাকচার**

### Use getters and setters

Using getters and setters to access data on objects could be better than simply
looking for a property on an object. "Why?" you might ask. Well, here's an
unorganized list of reasons why:

- When you want to do more beyond getting an object property, you don't have
  to look up and change every accessor in your codebase.
- Makes adding validation simple when doing a `set`.
- Encapsulates the internal representation.
- Easy to add logging and error handling when getting and setting.
- You can lazy load your object's properties, let's say getting it from a
  server.

**খারাপ কোড:**

```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**ভালো কোড:**

```javascript
function makeBankAccount() {
  // this one is private
  let balance = 0;

  // a "getter", made public via the returned object below
  function getBalance() {
    return balance;
  }

  // a "setter", made public via the returned object below
  function setBalance(amount) {
    // ... validate before updating the balance
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance
  };
}

const account = makeBankAccount();
account.setBalance(100);
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Make objects have private members

This can be accomplished through closures (for ES5 and below).

**খারাপ কোড:**

```javascript
const Employee = function(name) {
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

**ভালো কোড:**

```javascript
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
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

## **ক্লাস**

### Prefer ES2015/ES6 classes over ES5 plain functions

It's very difficult to get readable class inheritance, construction, and method
definitions for classical ES5 classes. If you need inheritance (and be aware
that you might not), then prefer ES2015/ES6 classes. However, prefer small functions over
classes until you find yourself needing larger and more complex objects.

**খারাপ কোড:**

```javascript
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

**ভালো কোড:**

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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Use method chaining

This pattern is very useful in JavaScript and you see it in many libraries such
as jQuery and Lodash. It allows your code to be expressive, and less verbose.
For that reason, I say, use method chaining and take a look at how clean your code
will be. In your class functions, simply return `this` at the end of every function,
and you can chain further class methods onto it.

**খারাপ কোড:**

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

**ভালো কোড:**

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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Prefer composition over inheritance

As stated famously in [_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns) by the Gang of Four,
you should prefer composition over inheritance where you can. There are lots of
good reasons to use inheritance and lots of good reasons to use composition.
The main point for this maxim is that if your mind instinctively goes for
inheritance, try to think if composition could model your problem better. In some
cases it can.

You might be wondering then, "when should I use inheritance?" It
depends on your problem at hand, but this is a decent list of when inheritance
makes more sense than composition:

1. Your inheritance represents an "is-a" relationship and not a "has-a"
   relationship (Human->Animal vs. User->UserDetails).
2. You can reuse code from the base classes (Humans can move like all animals).
3. You want to make global changes to derived classes by changing a base class.
   (Change the caloric expenditure of all animals when they move).

**খারাপ কোড:**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**ভালো কোড:**

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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

## **সলিড(SOLID)**

### Single Responsibility Principle (SRP)

As stated in Clean Code, "There should never be more than one reason for a class
to change". It's tempting to jam-pack a class with a lot of functionality, like
when you can only take one suitcase on your flight. The issue with this is
that your class won't be conceptually cohesive and it will give it many reasons
to change. Minimizing the amount of times you need to change a class is important.
It's important because if too much functionality is in one class and you modify
a piece of it, it can be difficult to understand how that will affect other
dependent modules in your codebase.

**খারাপ কোড:**

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

**ভালো কোড:**

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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Open/Closed Principle (OCP)

As stated by Bertrand Meyer, "software entities (classes, modules, functions,
etc.) should be open for extension, but closed for modification." What does that
mean though? This principle basically states that you should allow users to
add new functionalities without changing existing code.

**খারাপ কোড:**

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

**ভালো কোড:**

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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Liskov Substitution Principle (LSP)

This is a scary term for a very simple concept. It's formally defined as "If S
is a subtype of T, then objects of type T may be replaced with objects of type S
(i.e., objects of type S may substitute objects of type T) without altering any
of the desirable properties of that program (correctness, task performed,
etc.)." That's an even scarier definition.

The best explanation for this is if you have a parent class and a child class,
then the base class and child class can be used interchangeably without getting
incorrect results. This might still be confusing, so let's take a look at the
classic Square-Rectangle example. Mathematically, a square is a rectangle, but
if you model it using the "is-a" relationship via inheritance, you quickly
get into trouble.

**খারাপ কোড:**

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
    const area = rectangle.getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**ভালো কোড:**

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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Interface Segregation Principle (ISP)

JavaScript doesn't have interfaces so this principle doesn't apply as strictly
as others. However, it's important and relevant even with JavaScript's lack of
type system.

ISP states that "Clients should not be forced to depend upon interfaces that
they do not use." Interfaces are implicit contracts in JavaScript because of
duck typing.

A good example to look at that demonstrates this principle in JavaScript is for
classes that require large settings objects. Not requiring clients to setup
huge amounts of options is beneficial, because most of the time they won't need
all of the settings. Making them optional helps prevent having a
"fat interface".

**খারাপ কোড:**

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
  rootNode: document.getElementsByTagName("body"),
  animationModule() {} // Most of the time, we won't need to animate when traversing.
  // ...
});
```

**ভালো কোড:**

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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Dependency Inversion Principle (DIP)

This principle states two essential things:

1. High-level modules should not depend on low-level modules. Both should
   depend on abstractions.
2. Abstractions should not depend upon details. Details should depend on
   abstractions.

This can be hard to understand at first, but if you've worked with AngularJS,
you've seen an implementation of this principle in the form of Dependency
Injection (DI). While they are not identical concepts, DIP keeps high-level
modules from knowing the details of its low-level modules and setting them up.
It can accomplish this through DI. A huge benefit of this is that it reduces
the coupling between modules. Coupling is a very bad development pattern because
it makes your code hard to refactor.

As stated previously, JavaScript doesn't have interfaces so the abstractions
that are depended upon are implicit contracts. That is to say, the methods
and properties that an object/class exposes to another object/class. In the
example below, the implicit contract is that any Request module for an
`InventoryTracker` will have a `requestItems` method.

**খারাপ কোড:**

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

**ভালো কোড:**

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
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

## **টেস্টিং**

Testing is more important than shipping. If you have no tests or an
inadequate amount, then every time you ship code you won't be sure that you
didn't break anything. Deciding on what constitutes an adequate amount is up
to your team, but having 100% coverage (all statements and branches) is how
you achieve very high confidence and developer peace of mind. This means that
in addition to having a great testing framework, you also need to use a
[good coverage tool](https://gotwarlost.github.io/istanbul/).

There's no excuse to not write tests. There are [plenty of good JS test frameworks](https://jstherightway.org/#testing-tools), so find one that your team prefers.
When you find one that works for your team, then aim to always write tests
for every new feature/module you introduce. If your preferred method is
Test Driven Development (TDD), that is great, but the main point is to just
make sure you are reaching your coverage goals before launching any feature,
or refactoring an existing one.

### Single concept per test

**খারাপ কোড:**

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

**ভালো কোড:**

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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

## **কনকারেন্সি**

### Use Promises, not callbacks

Callbacks aren't clean, and they cause excessive amounts of nesting. With ES2015/ES6,
Promises are a built-in global type. Use them!

**খারাপ কোড:**

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

**ভালো কোড:**

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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Async/Await are even cleaner than Promises

Promises are a very clean alternative to callbacks, but ES2017/ES8 brings async and await
which offer an even cleaner solution. All you need is a function that is prefixed
in an `async` keyword, and then you can write your logic imperatively without
a `then` chain of functions. Use this if you can take advantage of ES2017/ES8 features
today!

**খারাপ কোড:**

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

**ভালো কোড:**

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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

## **এরর হ্যান্ডলিং**

Thrown errors are a good thing! They mean the runtime has successfully
identified when something in your program has gone wrong and it's letting
you know by stopping function execution on the current stack, killing the
process (in Node), and notifying you in the console with a stack trace.

### Don't ignore caught errors

Doing nothing with a caught error doesn't give you the ability to ever fix
or react to said error. Logging the error to the console (`console.log`)
isn't much better as often times it can get lost in a sea of things printed
to the console. If you wrap any bit of code in a `try/catch` it means you
think an error may occur there and therefore you should have a plan,
or create a code path, for when it occurs.

**খারাপ কোড:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**ভালো কোড:**

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

For the same reason you shouldn't ignore caught errors
from `try/catch`.

**খারাপ কোড:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**ভালো কোড:**

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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

## **ফরম্যাটিং**

Formatting is subjective. Like many rules herein, there is no hard and fast
rule that you must follow. The main point is DO NOT ARGUE over formatting.
There are [tons of tools](https://standardjs.com/rules.html) to automate this.
Use one! It's a waste of time and money for engineers to argue over formatting.

For things that don't fall under the purview of automatic formatting
(indentation, tabs vs. spaces, double vs. single quotes, etc.) look here
for some guidance.

### Use consistent capitalization

JavaScript is untyped, so capitalization tells you a lot about your variables,
functions, etc. These rules are subjective, so your team can choose whatever
they want. The point is, no matter what you all choose, just be consistent.

**খারাপ কোড:**

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

**ভালো কোড:**

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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Function callers and callees should be close

If a function calls another, keep those functions vertically close in the source
file. Ideally, keep the caller right above the callee. We tend to read code from
top-to-bottom, like a newspaper. Because of this, make your code read that way.

**খারাপ কোড:**

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

**ভালো কোড:**

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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

## **কমেন্টস**

### Only comment things that have business logic complexity.

Comments are an apology, not a requirement. Good code _mostly_ documents itself.

**খারাপ কোড:**

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

**ভালো কোড:**

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

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Don't leave commented out code in your codebase

Version control exists for a reason. Leave old code in your history.

**খারাপ কোড:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**ভালো কোড:**

```javascript
doStuff();
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Don't have journal comments

Remember, use version control! There's no need for dead code, commented code,
and especially journal comments. Use `git log` to get history!

**খারাপ কোড:**

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

**ভালো কোড:**

```javascript
function combine(a, b) {
  return a + b;
}
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

### Avoid positional markers

They usually just add noise. Let the functions and variable names along with the
proper indentation and formatting give the visual structure to your code.

**খারাপ কোড:**

```javascript
////////////////////////////////////////////////////////////////////////////////
// Scope Model Instantiation
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: "foo",
  nav: "bar"
};

////////////////////////////////////////////////////////////////////////////////
// Action setup
////////////////////////////////////////////////////////////////////////////////
const actions = function() {
  // ...
};
```

**ভালো কোড:**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```

**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**

## **অনুবাদ**

This is also available in other languages:

- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**:
  [GavBaros/clean-code-javascript-fr](https://github.com/GavBaros/clean-code-javascript-fr)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Spanish**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Simplified Chinese**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Traditional Chinese**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
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
- ![bd](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bangladesh.png) **Bangla(বাংলা)**:
  [InsomniacSabbir/clean-code-javascript/](https://github.com/InsomniacSabbir/clean-code-javascript/)
  
**[⬆ উপরে ফিরে যেতে এখানে ক্লিক করতে হবে](#সূচিপত্র)**
