# clean-code-javascript

## Inhaltsverzeichnis
  1. [Einführung](#introduction)
  2. [Variablen](#variables)
  3. [Funktionen](#functions)
  4. [Objekte und Daten-Strukturen](#objects-and-data-structures)
  5. [Klassen](#classes)
  6. [Testing](#testing)
  7. [Parallelität](#concurrency)
  8. [Fehlerbehandlung](#error-handling)
  9. [Formatierung](#formatting)
  10. [Kommentare](#comments)

## Einführung
![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](http://www.osnews.com/images/comics/wtfm.jpg)

Softwareentwicklungs-Prinzipien aus Robert C. Martin's Buch
[*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
adaptiert für JavaScript. Dies ist kein Styleguide. Es ist ein Leitfaden um lesbare, wiederverwendbare 
und wartbare Software mit JavaScript zu entwickeln.

Nicht jede Regel hier muss streng befolgt werden und noch weniger Regeln werden universell 
anwendbar sein. Dies sind Richtlinien und nicht mehr. Sie sind jedoch das Ergebnis 
jahrelang gesammelter Erfahrung des Autors von *Clean Code*.

Unser Handwerk der Softwareentwicklung ist nur etwas mehr als 50 Jahre alt und wir lernen 
immer noch eine Menge. Wenn Softwarearchitektur ebenso alt ist wie die Architektur, haben wir 
womöglich strengere Regeln denen wir zu folgen haben. Für den Moment, lass diese Richtlinien 
ein Maßstab sein um die Qualität des JavaScript-Codes den du und dein Team produzierst, 
beurteilen zu können.

Eine Sache noch: Diese Regeln zu kennen macht dich nicht unverzüglich zu einem besseren 
Softwareentwickler. Und mit ihnen für viele Jahre zu arbeiten heißt nicht, dass du keine 
Fehler machst. Jedes Stück Code startet mit einem ersten Entwurf, wie nasser Ton der in 
seine finale Form ausgestaltet wird. Wir meißeln endlich die Unvollkommenheiten weg, wenn 
wir unseren Code mit Kollegen überprüfen. Mach dich wegen ersten Entwürfen die Verbesserungen 
benötigen nicht fertig. Verprügel stattdessen den Code!

## **Variablen**
### Verwende aussagekräftige und aussprechbare Variablennamen

**Schlecht:**
```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
```

**Gut:**
```javascript
const currentDate = moment().format('YYYY/MM/DD');
```
**[⬆ nach oben](#table-of-contents)**

### Verwende die selbe Bezeichnung für die selbe Art von Variablen

**Schlecht:**
```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Gut:**
```javascript
getUser();
```
**[⬆ nach oben](#table-of-contents)**

### Verwende suchbare Namen
Wir lesen mehr Code als wir jemals schreiben werden. Es ist daher wichtig, dass 
der Code den wir schreiben les- und durchsuchbar ist. Wir erschweren den Lesern
unseres Codes die Arbeit, wenn wir unsere Variablen *nicht* aussagekräftig benennen
und somit das Verständnis unseres Programms erschweren.
Mache deine Namen suchbar. Tools wie [buddy.js](https://github.com/danielstjules/buddy.js) und
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
können dabei helfen unbenannte Konstanten zu ermitteln.

**Schlecht:**
```javascript
// Wofür steht 86400000?
setTimeout(blastOff, 86400000);

```

**Gut:**
```javascript
// Deklariere diese als großgeschriebene `const` Globale.
const MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);

```
**[⬆ nach oben](#table-of-contents)**

### Verwende selbsterklärende Variablen
**Schlecht:**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(address.match(cityZipCodeRegex)[1], address.match(cityZipCodeRegex)[2]);
```

**Gut:**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```
**[⬆ nach oben](#table-of-contents)**

### Vermeide Mental Mapping
Explizites ist besser als Im­pli­zites.

**Schlecht:**
```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((l) => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Moment, wofür steht `l` nochmal?
  dispatch(l);
});
```

**Gut:**
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
**[⬆ nach oben](#table-of-contents)**

### Verzichte auf überflüssigen Kontext
Wenn dir deine Klasse/Objekt etwas sagt, wiederhole das nicht in deinem 
Variablennamen.

**Schlecht:**
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

**Gut:**
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
**[⬆ nach oben](#table-of-contents)**

### Verwende Default-Werte anstatt Short-Circuiting oder Bedingungen

**Schlecht:**
```javascript
function createMicrobrewery(name) {
  const breweryName = name || 'Hipster Brew Co.';
  // ...
}

```

**Gut:**
```javascript
function createMicrobrewery(breweryName = 'Hipster Brew Co.') {
  // ...
}

```
**[⬆ nach oben](#table-of-contents)**

## **Funktionen**
### Funktionsargumente (2 oder idealerweise weniger)
Die Anzahl der Funktionsargumente zu limitieren ist unglaublich wichtig, weil
dadurch die Testbarkeit deiner Funktion einfacher wird. Mehr als drei Argumente
führen zu einer kombinatorischen Explosion und du musst unzählige verschiedene 
Fälle mit jedem einzelnen Argument testen.

Keine Argumente ist der ideale Fall. Ein oder zwei Argumente sind okay, drei 
sollten aber vermieden werden. Alles darüber sollte zusammengefasst werden. Meistens
erledigt deine Funktion zu viel, wenn du mehr als zwei Argumente benötigst. 
Falls nicht, reicht häufig ein übergeordnetes Objekt als Argument.

Weil uns JavaScript erlaubt Objekte on-the-fly und ohne viel Klassen-Boilerplate zu 
erstellen, kannst du Objekte verwenden wenn du merkst, dass du eine Menge Argumente
benötigst.

**Schlecht:**
```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```

**Gut:**
```javascript
const menuConfig = {
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
};

function createMenu(config) {
  // ...
}

```
**[⬆ nach oben](#table-of-contents)**


### Funktionen sollten eine Sache erledigen
Dies ist bei weitem die wichtigste Regel in der Softwareentwicklung. Wenn Funktionen
mehr als eine Sache erledigen sind sie schwerer zu verfassen, zu testen und zu begründen.
Wenn du eine Funktion abgrenzen kannst nur eine Aktion auszuführen, dann kann diese
problemlos überarbeitet werden und dein Code liest sich sehr viel einfacher. Wenn du
aus diesem Leitfaden nichts außer diese Regel ziehen kannst, wirst du vielen Entwicklern
einen Schritt voraus sein. 

**Schlecht:**
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

**Gut:**
```javascript
function emailClients(clients) {
  clients
    .filter(isClientActive)
    .forEach(email);
}

function isClientActive(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```
**[⬆ nach oben](#table-of-contents)**

### Funktionsnamen sollten selbstdokumentierend sein

**Schlecht:**
```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// Es ist schwierig zu sagen was hinzugefügt wird
addToDate(date, 1);
```

**Gut:**
```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```
**[⬆ nach oben](#table-of-contents)**

### Funktionen sollten nur eine Ebene der Abstraktion sein
Wenn du mehr als eine Ebene der Abstraktion hast, erledigt deine Funktion 
möglicherweise zu viel. Funktionen in mehrere verschiedene Funktionen 
aufzuteilen sorgt für eine bessere Wiederverwendbarkeit und einfacheres 
Testing.

**Schlecht:**
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

**Gut:**
```javascript
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

function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const ast = lexer(tokens);
  ast.forEach((node) => {
    // parse...
  });
}
```
**[⬆ nach oben](#table-of-contents)**

### Entferne doppelten Code
Gebe dein absolut bestes um doppelten Code zu vermeiden. Doppelter Code ist schlecht
weil es bedeutet, dass es mehr als eine Stelle gibt um etwas anzupassen wenn an dieser
Logik etwas geändert wird.

Stelle dir vor du betreibst ein Restaurant und willst den Überblick über deinen Vorrat behalten:
All deine Tomaten, Zwiebeln, Knoblauch, Gewürze, etc. Wenn du deinen Lagerbestand auf mehrerem 
Listen festhälst, dann musst du all diese Listen aktualisieren wenn du einen Teller – auf dem 
sich Tomaten befinden – servierst. Wenn du nur eine List hast, dann musst du auch nur diese eine Liste 
aktualisieren!

Häufig hast du doppelten Code weil du zwei oder mehr nur geringfügig unterschiedliche Dinge hast, die
zwar eine Menge gemeinsam haben aber ihre Unterschiede zwingen dich dazu zwei oder mehr seperate Funktionen 
– die vieles exakt gleich erledigen - zu haben. Doppelten Code zu entfernen heißt Abstraktionen zu erstellen,
die diese Reihe von unterschiedlichen Dingen mit nur einer Funktion/Modul/Klasse handhaben können.

Diese Abstraktion gut hinzubekommen ist entscheidend und darum solltest du den SOLID-Regeln im *Klassen*-
Kapitel folgen. Schlechte Abstraktionen können schlimmer als doppelter Code sein, also sei vorsichtig! 
In diesem Sinne, wenn du eine gute Abstraktion hinbekommst - mach es! Wiederhole dich nicht selbst, ansonsten
wirst du jedes Mal verschiedene Stellen anpassen müssen wenn du eine Sache ändern möchtest.

**Schlecht:**
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

**Gut:**
```javascript
function showList(employees) {
  employees.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    let portfolio = employee.getGithubLink();

    if (employee.type === 'manager') {
      portfolio = employee.getMBAProjects();
    }

    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```
**[⬆ nach oben](#table-of-contents)**

### Setze Default-Objekte mit Object.assign

**Schlecht:**
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
  config.cancellable = config.cancellable === undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**Gut:**
```javascript
const menuConfig = {
  title: 'Order',
  // User did not include 'body' key
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

  // config entspricht nun: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```
**[⬆ nach oben](#table-of-contents)**


### Verwende keine Flags als Funktionsparameter
Flags zeigen deinem User dass diese Funktion mehr als eine Sache macht. Funktionen sollten 
nur eine Sache erledigen. Teile deine Funktionen auf wenn diese auf Basis eines Boolean 
andere Pfad verwenden.

**Schlecht:**
```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Gut:**
```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```
**[⬆ nach oben](#table-of-contents)**

### Vermeide Nebeneffekte (Teil 1)
Eine Funktion erzeugt einen Nebeneffekt wenn sie mehr macht als einen Wert entgegenzunehmen
und einen oder mehrere Werte zurück zu geben. Ein Nebeneffekt könnte das Schreiben einer Datei,
das modifizieren einiger gobaler Variablen oder das versehentliche Übertragen deines ganzen Geldes
an einen Fremden sein.

Jetzt, brauchst du bei einer Gelegenheit einen Nebeneffekt. Ähnlich wie im vorangegangen Beispiel möchtest
du möglicherweise eine Datei schreiben. Fasse dies zusammen um nicht etliche Funktionen und Klassen, die auf
bestimmte Datei bezogen sind zu haben. Habe eine Service der dies erledigt. Einen und nur einen.

Der Punkt ist, üblich Tücken wie beispielsweise das Teilen von State zwischen Objekten ohne jegliche
Struktur mit veränderbaren Datentypen – die überall überschrieben werden können – zu verhindern und die Stellen
an denen Nebeneffekte auftreten können zusammen zufassen. Wenn du das schaffst, wirst du glücklicher als die
Mehrheit der anderen Programmierer sein. 

**Schlecht:**
```javascript
// Globale Variable, die von der nachfolgenden Funktion referenziert wird.
// Wenn wir eine andere Funktion hätten, die diese Variable verwenden könnte diese nun nicht mehr funktionieren weil diese Variable nun ein Array ist.
let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Gut:**
```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

const name = 'Ryan McDermott';
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```
**[⬆ nach oben](#table-of-contents)**

### Vermeide Nebeneffekte (Teil 2)
In JavaScript werden primitive Datentypen als Werte und Objekte/Arrays als
Referenzen übergeben. Wenn deine Funktion beispielsweise im Falle von Objekten 
und Arrays eine Änderung an einem Warenkorb-Array – durch das Hinzufügen eines 
Items – durchführt. Dann werden alle anderen Funktionen die dieses `cart`-Array
verwenden von dieser Ergänzung betroffen sein. Das mag in manchen Fällen gewünscht 
sein. Es kann aber auch schlecht sein. Stellen wir uns folgende Situation vor:

Der User klickt auf den "Kaufen"-Button, der eine `purchase`-Funktion aufruft.
Diese wiederum erstellt einen Netzwerk-Request und sendet das `cart`-Array zu unserem
Server. Aufgrund einer schlechten Internetverbindung muss die `purchase`-
Funktion diesen Request wiederholen. Was passiert nun, wenn der User in der 
Zwischenzeit versehentlich auf einen „zum Warenkorb hinzufügen“-Button eines
Produkts klickt, dass er eigentlich gar nicht wollte? Wenn das passiert und der
Request beginnt, dann wird diese `purchase`-Funktion das versehentlich
hinzugefügte Produkt senden, weil diese Funktion eine Referenz zu einem 
Warenkorb-Array hat, dass die `addItemToCart`-Funktion – durch das Hinzufügen eines
ungewollten Produkts – modifiziert.

Eine tolle Lösung wäre, für die `addItemToCart`-Funktion jedesmal `cart` zu kopieren,
zu bearbeiten und die Kopie zurückzugeben. Dies stellt sicher, dass andere Funktionen 
die eine Referenz zum Warenkorb besitzen, keine Möglichkeit haben Änderungen durchzuführen.

Zwei Einsprüche die zu diesem Ansatz erwähnt werden sollten:
  1. Möglicherweise gibt es Fälle in denen du wirklich das Input-Objekt modifizieren willst.
Allerdings werden diese Fälle sehr selten sein, wenn du diese Übung übernimmst. Die meistens
Dinge können so überarbeitet werden, dass keine Nebeneffekte auftreten!

  2. Große Objekte zu kopieren kann sich hinsichtlich der Performance sehr negativ auswirken. Glücklicherweise 
ist das in der Praxis kein großes Problem. Es gibt [https://facebook.github.io/immutable-js/](tolle Bibliotheken) 
die es erlauben diese Art des Programmieransatzes schneller und nicht so speicherintensiv auszuführen 
wie es wäre wenn du manuell Objekte und Arrays kopierst.

**Schlecht:**
```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Gut:**
```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date : Date.now() }];
};
```

**[⬆ nach oben](#table-of-contents)**

### Schreibe keine globalen Funktionen
Den globalen Namespace zu verschmutzen ist eine schlechte Angewohnheit in JavaScript, 
weil du dadurch mit anderen Bibliotheken aneinandergeraten könntest und der Nutzer deiner 
Schnittstelle würde nichts davon mitbekommen, bis er einen Fehler bei der Veröffentlichung 
erhält. Stelle dir folgendes Beispiel vor: Was wenn du JavaScripts ursprüngliche Array-Methoden 
um eine weitere `diff`-Methode ergänzen möchtest, die dir die Unterschiede zwischen zwei Arrays 
darstellt? Du könntest deine neue Funktion mit `Array.prototype` schreiben. Allerdings könnte dies 
mit einer anderen Bibliothek aneinandergeraten, die das selbe versucht hat. Was ist, wenn diese 
andere Bibliothek `diff` nur verwendet hat um das erste und letzte Element eines Arrays zu finden?
Das ist der Grund warum es viel besser wäre die Klassen in ES2015/ES6 zu verwenden und einfach die 
`Array`-Globale zu ergänzen.

**Schlecht:**
```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Gut:**
```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```
**[⬆ nach oben](#table-of-contents)**

### Ziehe funktionale Programmierung imperativer Programmierung vor
JavaScript ist keine funktionale Sprache wie es beispielsweise Haskell ist.
JavaScript hat aber eine funktionale Variante. Funktionale Sprachen sind sauberer 
und einfacher zu testen. Ziehe diesen Stil der Programmierung vor wenn du kannst.

**Schlecht:**
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

**Gut:**
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
  .map((programmer) => programmer.linesOfCode)
  .reduce((acc, linesOfCode) => acc + linesOfCode, 0);
```
**[⬆ nach oben](#table-of-contents)**

### Fasse Bedingungen zusammen

**Schlecht:**
```javascript
if (fsm.state === 'fetching' && isEmpty(listNode)) {
  // ...
}
```

**Gut:**
```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === 'fetching' && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```
**[⬆ nach oben](#table-of-contents)**

### Vermeide negative Bedingungen

**Schlecht:**
```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Gut:**
```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```
**[⬆ nach oben](#table-of-contents)**

### Vermeide Bedingungen
Das hört sich nach einer unmöglichen Aufgabe an. Menschen die dies zum ersten 
Mal hören, fragen sich: „Wie soll ich etwas ohne eine `if`-Anweisung tun?“ Die 
Antwort ist, dass du Polymorphie verwenden kannst um die selbe Aufgabe in vielen 
Fällen zu erledigen. Die zweite Frage ist dann meistens: „Gut, das ist toll aber 
warum sollte ich das wollen?“ Die Antwort ist ein Clean-Code-Konzept das wir bereits 
gelernt haben: Eine Funktion sollte lediglich eine Aufgabe erledigen. Wenn du Klassen 
und Funktionen mit `if`-Anweisungen hast, teilst du dem Nutzer deines Codes mit, dass 
deine Funktion mehr als eine Sache erledigt. Vergiss nicht, mache nur eine Sache.

**Schlecht:**
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

**Gut:**
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
**[⬆ nach oben](#table-of-contents)**

### Vermeide es Datentypen zu prüfen (Teil 1)
JavaScript besitzt keine Datentypen. Das bedeutet, dass deine Funktionen jegliche Datentypen 
als Argument entgegennehmen. Gelegentlich wirst du dich mit dieser Freiheit unwohl fühlen 
und es wird dich dazu verleiten in deinen Funktionen Datentypen zu prüfen. Es gibt viele 
Möglichkeiten, dies zu vermeiden. Die erste Sache ist, auf einheitlich Schnittstellen zu achten.

**Schlecht:**
```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.peddle(this.currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location('texas'));
  }
}
```

**Gut:**
```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location('texas'));
}
```
**[⬆ nach oben](#table-of-contents)**

### Vermeide es Datentypen zu prüfen (Teil 2)
Wenn du mit grundlegenden einfachen Werten wie Strings, Integer und Arrays arbeitest, 
du Polymorphie nicht anwenden kannst und weiterhin das Bedüfniss verspürst, Datentypen 
zu prüfen. Dann solltest du dir überlegen TypeScript zu verwenden. TypeScript ist eine 
ausgezeichnet Alternative zu normalem JavaScript weil es statische Typen auf Grundlage 
der normalen JavaScript-Syntax anbietet. Das Problem mit dem manuellem Prüfen von Typen 
in normalem JavaScript ist, dass dieser Mehraufwand um diese "falsche" Typensicherheit 
zu erreichen die verlorene Lesbarkeit nicht wieder gut macht. Halte dein JavaScript 
sauber, schreibe gute Tests und habe gute Code-Reviews. Anderenfalls erledige dies mit 
TypeScript (was, wie ich gesagt habe, eine ausgezeichnete Alternative ist!).

**Schlecht:**
```javascript
function combine(val1, val2) {
  if (typeof val1 === 'number' && typeof val2 === 'number' ||
      typeof val1 === 'string' && typeof val2 === 'string') {
    return val1 + val2;
  }

  throw new Error('Must be of type String or Number');
}
```

**Gut:**
```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```
**[⬆ nach oben](#table-of-contents)**

### Überoptimiere nicht
Moderne Browser optimieren eine Menge während der Laufzeit. Du verschwendest häufig 
deine Zeit wenn du Dinge optimierst. [Es gibt gute Ressourcen](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) 
um herauszufinden, wo die Optimierung des Browsers mangelhaft ist. Fokusiere dich 
derweil auf diese Dinge bis sie behoben sind, wenn sie behoben werden können.

**Schlecht:**
```javascript

// In alten Browsern würde jeder Durchlauf mit einem ungespeichertem `list.length` speicherintensiv sein,
// weil `list.length` jedes Mal neu berechnet wird. In modernen Browsern ist das optimiert.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Gut:**
```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```
**[⬆ nach oben](#table-of-contents)**

### Entferne veralteten Code
Veralteter Code ist genau so schlecht wie doppelter Code. Es gibt keinen Grund diesen 
in deiner Code-Basis zu lassen. Wenn etwas nicht aufgerufen wird, werde es los! Es 
wird weiterhin in deiner Versionsverwaltung sein, wenn du es nochmal benötigst.

**Schlecht:**
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

**Gut:**
```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```
**[⬆ nach oben](#table-of-contents)**

## **Objects and Data Structures**
### Use getters and setters
JavaScript doesn't have interfaces or types so it is very hard to enforce this
pattern, because we don't have keywords like `public` and `private`. As it is,
using getters and setters to access data on objects is far better than simply
looking for a property on an object. "Why?" you might ask. Well, here's an
unorganized list of reasons why:

* When you want to do more beyond getting an object property, you don't have
to look up and change every accessor in your codebase.
* Makes adding validation simple when doing a `set`.
* Encapsulates the internal representation.
* Easy to add logging and error handling when getting and setting.
* Inheriting this class, you can override default functionality.
* You can lazy load your object's properties, let's say getting it from a
server.


**Schlecht:**
```javascript
class BankAccount {
  constructor() {
    this.balance = 1000;
  }
}

const bankAccount = new BankAccount();

// Buy shoes...
bankAccount.balance -= 100;
```

**Gut:**
```javascript
class BankAccount {
  constructor(balance = 1000) {
    this._balance = balance;
  }

  // It doesn't have to be prefixed with `get` or `set` to be a getter/setter
  set balance(amount) {
    if (verifyIfAmountCanBeSetted(amount)) {
      this._balance = amount;
    }
  }

  get balance() {
    return this._balance;
  }

  verifyIfAmountCanBeSetted(val) {
    // ...
  }
}

const bankAccount = new BankAccount();

// Buy shoes...
bankAccount.balance -= shoesPrice;

// Get balance
let balance = bankAccount.balance;

```
**[⬆ nach oben](#table-of-contents)**


### Make objects have private members
This can be accomplished through closures (for ES5 and below).

**Schlecht:**
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

**Gut:**
```javascript
const Employee = function (name) {
  this.getName = function getName() {
    return name;
  };
};

const employee = new Employee('John Doe');
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```
**[⬆ nach oben](#table-of-contents)**


## **Classes**
### Single Responsibility Principle (SRP)
As stated in Clean Code, "There should never be more than one reason for a class
to change". It's tempting to jam-pack a class with a lot of functionality, like
when you can only take one suitcase on your flight. The issue with this is
that your class won't be conceptually cohesive and it will give it many reasons
to change. Minimizing the amount of times you need to change a class is important.
It's important because if too much functionality is in one class and you modify a piece of it,
it can be difficult to understand how that will affect other dependent modules in
your codebase.

**Schlecht:**
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

**Gut:**
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
**[⬆ nach oben](#table-of-contents)**

### Open/Closed Principle (OCP)
As stated by Bertrand Meyer, "software entities (classes, modules, functions,
etc.) should be open for extension, but closed for modification." What does that
mean though? This principle basically states that you should allow users to
add new functionalities without changing existing code.

**Schlecht:**
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

**Gut:**
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
**[⬆ nach oben](#table-of-contents)**


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

**Schlecht:**
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
    const area = rectangle.getArea(); // BAD: Will return 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Gut:**
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
  constructor() {
    super();
    this.width = 0;
    this.height = 0;
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

class Square extends Shape {
  constructor() {
    super();
    this.length = 0;
  }

  setLength(length) {
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach((shape) => {
    switch (shape.constructor.name) {
      case 'Square':
        shape.setLength(5);
        break;
      case 'Rectangle':
        shape.setWidth(4);
        shape.setHeight(5);
    }

    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(), new Rectangle(), new Square()];
renderLargeShapes(shapes);
```
**[⬆ nach oben](#table-of-contents)**

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
all of the settings. Making them optional helps prevent having a "fat interface".

**Schlecht:**
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
  animationModule() {} // Most of the time, we won't need to animate when traversing.
  // ...
});

```

**Gut:**
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
**[⬆ nach oben](#table-of-contents)**

### Dependency Inversion Principle (DIP)
This principle states two essential things:
1. High-level modules should not depend on low-level modules. Both should
depend on abstractions.
2. Abstractions should not depend upon details. Details should depend on
abstractions.

This can be hard to understand at first, but if you've worked with Angular.js,
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

**Schlecht:**
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

**Gut:**
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
const inventoryTracker = new InventoryTracker(['apples', 'bananas'], new InventoryRequesterV2());
inventoryTracker.requestItems();
```
**[⬆ nach oben](#table-of-contents)**

### Prefer ES2015/ES6 classes over ES5 plain functions
It's very difficult to get readable class inheritance, construction, and method
definitions for classical ES5 classes. If you need inheritance (and be aware
that you might not), then prefer classes. However, prefer small functions over
classes until you find yourself needing larger and more complex objects.

**Schlecht:**
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

**Gut:**
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
**[⬆ nach oben](#table-of-contents)**


### Use method chaining
This pattern is very useful in JavaScript and you see it in many libraries such
as jQuery and Lodash. It allows your code to be expressive, and less verbose.
For that reason, I say, use method chaining and take a look at how clean your code
will be. In your class functions, simply return `this` at the end of every function,
and you can chain further class methods onto it.

**Schlecht:**
```javascript
class Car {
  constructor() {
    this.make = 'Honda';
    this.model = 'Accord';
    this.color = 'white';
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

const car = new Car();
car.setColor('pink');
car.setMake('Ford');
car.setModel('F-150');
car.save();
```

**Gut:**
```javascript
class Car {
  constructor() {
    this.make = 'Honda';
    this.model = 'Accord';
    this.color = 'white';
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

const car = new Car()
  .setColor('pink')
  .setMake('Ford')
  .setModel('F-150')
  .save();
```
**[⬆ nach oben](#table-of-contents)**

### Prefer composition over inheritance
As stated famously in [*Design Patterns*](https://en.wikipedia.org/wiki/Design_Patterns) by the Gang of Four,
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

**Schlecht:**
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

**Gut:**
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
**[⬆ nach oben](#table-of-contents)**

## **Testing**
Testing ist wichtiger als das Veröffentlichen. Wenn du keine oder eine unzureichende 
Anzahl an Tests hast, dann wirst du jedes Mal unsicher sein, ob du nicht irgendetwas
kaputt gemacht hast. Die Entscheidung welche Anzahl angemessen ist, entscheidet dein
Team aber eine 100%ige Abdeckung (Alle Anweisungen und Branches) wird dir ein sehr
hohes Vertrauen und Seelenfrieden geben. Das bedeutet, dass du als Ergänzung zu einem 
guten Testing-Framework auch ein [gutes Coverage-Tool](http://gotwarlost.github.io/istanbul/) 
verwenden musst.

Es gibt keine Ausrede um keine Tests zu schreiben. Es gibt [zahlreiche gute JS-Test-Frameworks]
(http://jstherightway.org/#testing-tools), also finde eines das dein Team bevorzugt.
Wenn du eines gefunden hast, dass für dein Team funktioniert, dann ziele darauf ab immer 
einen Test für jedes neue Feature/Modul zu schreiben. Wenn deine bevorzugte Arbeitsweise die 
testgetriebene Entwicklung ist, umso besser. Aber die Hauptsache ist, sicher zu stellen, dass 
du die Abdeckungsziele erreichst bevor ein Feature veröffentlicht oder vorhandener Code 
überarbeitet wird. 

### Eine Sache pro Test

**Schlecht:**
```javascript
const assert = require('assert');

describe('MakeMomentJSGreatAgain', () => {
  it('handles date boundaries', () => {
    let date;

    date = new MakeMomentJSGreatAgain('1/1/2015');
    date.addDays(30);
    date.shouldEqual('1/31/2015');

    date = new MakeMomentJSGreatAgain('2/1/2016');
    date.addDays(28);
    assert.equal('02/29/2016', date);

    date = new MakeMomentJSGreatAgain('2/1/2015');
    date.addDays(28);
    assert.equal('03/01/2015', date);
  });
});
```

**Gut:**
```javascript
const assert = require('assert');

describe('MakeMomentJSGreatAgain', () => {
  it('handles 30-day months', () => {
    const date = new MakeMomentJSGreatAgain('1/1/2015');
    date.addDays(30);
    date.shouldEqual('1/31/2015');
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
**[⬆ nach oben](#table-of-contents)**

## **Parallelität**
### Verwende Promises, keine Callbacks
Callbacks sind keine saubere Lösung und sie verursachen eine übermäßige Verschachtelung.
Mit ES2015/ES6 sind Promises ein integrierter globaler Typ. Verwende sie!

**Schlecht:**
```javascript
require('request').get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', (requestErr, response) => {
  if (requestErr) {
    console.error(requestErr);
  } else {
    require('fs').writeFile('article.html', response.body, (writeErr) => {
      if (writeErr) {
        console.error(writeErr);
      } else {
        console.log('File written');
      }
    });
  }
});

```

**Gut:**
```javascript
require('request-promise').get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then((response) => {
    return require('fs-promise').writeFile('article.html', response);
  })
  .then(() => {
    console.log('File written');
  })
  .catch((err) => {
    console.error(err);
  });

```
**[⬆ nach oben](#table-of-contents)**

### Async/Await sind noch sauberer als Promises
Promises sind eine sehr saubere Alternative zu Callbacks. ES2017/ES8 allerdings beinhaltet 
async und await, die eine viel sauberere Lösung bieten. Alles was du benötigst ist eine Funktion,
der das `async`-Keyword vorangestellt ist. Anschließend kannst du deine Logik imperativ ohne eine
Kette von `then`-Funktionen schreiben. Verwende dies, wenn du die Vorteile von ES2017/ES8 jetzt 
schon nutzen kannst!

**Schlecht:**
```javascript
require('request-promise').get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then((response) => {
    return require('fs-promise').writeFile('article.html', response);
  })
  .then(() => {
    console.log('File written');
  })
  .catch((err) => {
    console.error(err);
  });

```

**Gut:**
```javascript
async function getCleanCodeArticle() {
  try {
    const response = await require('request-promise').get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin');
    await require('fs-promise').writeFile('article.html', response);
    console.log('File written');
  } catch(err) {
    console.error(err);
  }
}
```
**[⬆ nach oben](#table-of-contents)**


## **Fehlerbehandlung**
Ausgegebene Fehler sind eine gute Sache! Das heißt, dass die Laufzeitumgebung
erfolgreich erkannt hat, dass etwas in deinem Programm schief gegangen ist und lässt
dich das wissen indem deine Funktion im aktuellen Stack beendet wurde, den Prozess
zerstört hat (in Node) und dich in der Konsole mit einem Stacktrace benachrichtigt.

### Ignoriere abgefangene Fehler nicht
Mit einem abgefangenem Fehler nichts anzufangen, wird dir niemals die Möglichkeit
geben auf diesen zu reagieren oder ihn zu fixen. Den Fehler in der Konsole auszugeben 
(`console.log`) ist nicht viel besser. Er wird oft in den unzähligen Dingen die 
in der Konsole ausgegeben werden untergehen. Wenn du ein Stückchen Code in einer
`try/catch`-Anweisung verschachtelst, heißt dass, das du davon ausgehst, dass an 
dieser Stelle ein Fehler auftreten kann und daher solltest du einen Plan haben oder
eine Anweisung schreiben, wenn dieser erscheint.

**Schlecht:**
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Gut:**
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // Eine Option (Erweckt mehr Aufmerksamkeit als console.log):
  console.error(error);
  // Eine andere Möglichkeit:
  notifyUserOfError(error);
  // Eine andere Möglichkeit:
  reportErrorToService(error);
  // ODER wende alle drei an!
}
```

### Ignoriere zurückgewiesen Promises nicht
Aus dem selben Grund, weshalb du keine Fehler von `try/catch`-Anweisungen 
ignorieren solltest.

**Schlecht:**
```javascript
getdata()
.then((data) => {
  functionThatMightThrow(data);
})
.catch((error) => {
  console.log(error);
});
```

**Gut:**
```javascript
getdata()
.then((data) => {
  functionThatMightThrow(data);
})
.catch((error) => {
  // Eine Option (Erweckt mehr Aufmerksamkeit als console.log):
  console.error(error);
  // Eine andere Möglichkeit:
  notifyUserOfError(error);
  // Eine andere Möglichkeit:
  reportErrorToService(error);
  // ODER wende alle drei an!
});
```

**[⬆ nach oben](#table-of-contents)**


## **Formatierung**
Formatierungen sind subjektiv. Wie bei vielen anderen Regeln hier gibt es kein
Richtig oder Falsch dem du folgen musst. Der Punkt ist, STREITE NICHT wegen 
Formatierungen. Es gibt [unzählige Tools](http://standardjs.com/rules.html) die
dies automatisieren. Verwende eines! Es ist für Entwickler eine Verschwendung 
von Zeit und Geld darüber zu streiten.

Für Dinge die nicht in den Bereich der automatischen Formatierung (Einrückungen, 
Tabs vs. Leerzeichen, doppelte vs. einfache Anführungszeichen, etc.) fallen schaue 
hier für ein paar Ratschläge.

### Verwende eine einheitliche Schreibweise
JavaScript hat keine Typen. Die Schreibweise deiner Variablen und Funktionen 
sagt viel über diese aus. Dieses Regeln sind subjektiv, dein Team kann wählen 
was sie bevorzugen. Die Hauptsache ist, egal was ihr wählt, wendet es konsequent an.

**Schlecht:**
```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const Artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**Gut:**
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
**[⬆ nach oben](#table-of-contents)**


### Funktionsaufrufe sollten nahe an der aufzurufenden Funktion stehen
Wenn eine Funktion eine andere aufruft, halte diese Funktionen im Quelltext
vertikal nahe beieinander. Behalte idealerweise die Funktion die von einer anderen
Funktion aufgerufen wird genau über dieser. Wir neigen dazu Code von oben nach unten 
wie eine Zeitung zu lesen. Aus diesem Grund sorge dafür, dass dein Code auf diese Weise 
lesbar ist.

**Schlecht:**
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

const review = new PerformanceReview(user);
review.perfReview();
```

**Gut:**
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

**[⬆ nach oben](#table-of-contents)**

## **Kommentare**
### Kommentiere nur Dinge, die komplexe Anwendungslogik beinhalten
Kommentare sind eine Entschuldigung, keine Anforderung. Guter Code dokumentiert
sich *meistens* selbst.

**Schlecht:**
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

**Gut:**
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
**[⬆ nach oben](#table-of-contents)**

### Lasse keinen auskommentierten Code in deiner Code-Basis
Versionsverwaltungen existieren aus einem Grund. Lasse alten Code in der
History deines Repositories.

**Schlecht:**
```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Gut:**
```javascript
doStuff();
```
**[⬆ nach oben](#table-of-contents)**

### Verzichte auf Journal-Kommentare
Denke daran, verwende eine Versionsverwaltung! Veralteter Code, auskommentierter
Code und insbesondere Journal-Kommentare werden nicht benötigt. Verwende `git log`
um vergangen Änderungen einzusehen.

**Schlecht:**
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

**Gut:**
```javascript
function combine(a, b) {
  return a + b;
}
```
**[⬆ nach oben](#table-of-contents)**

### Verzichte auf Positionsmarker
Positionsmarker sorgen meistens für einen gestörten Lesefluss. Lasse Funktionen 
und Variablen in der korrekten Einrückung und Formatierung stehen. Diese werden 
deinem Code ausreichend visuelle Struktur geben.

**Schlecht:**
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

**Gut:**
```javascript
$scope.model = {
  menu: 'foo',
  nav: 'bar'
};

const actions = function() {
  // ...
};
```
**[⬆ nach oben](#table-of-contents)**
