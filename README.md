# clean-code-javascript

## Cuprins

1. [Introducere](#introducere)
2. [Variabile](#variabile)
3. [Functii](#functii)
4. [Obiecte si structuri de date](#obiecte-si-structuri-de-date)
5. [Clase](#clase)
6. [SOLID](#solid)
7. [Testare](#testare)
8. [Concurenta](#concurenta)
9. [Manipularea erorilor](#manipularea-erorilor)
10. [Formatare](#formatare)
11. [Comentarii](#comentarii)
12. [Postarea originala in limba engleza](#postarea-originala-in-limba-engleza)

## Introducere

![Imagine umoristica a estimarii calitatii software-ului drept numarul de momente de confuzie cand recitesti codul](https://www.osnews.com/images/comics/wtfm.jpg)

Principii de inginerie software din cartea lui Robert C. Martin
[_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
adaptate pentru JavaScript. Acesta este un ghid pentru a dezvolta software
[citibil, reutilizabil si refactorabil](https://github.com/ryanmcdermott/3rs-of-software-architecture) in JavaScript.

Nu toate principiile de aici trebuie respectate cu strictete, iar mai putine vor fi unanim agreate.
Acestea sunt simple sfaturi in urma multor ani de experienta colectiva de catre autorii _Clean Code_.

Iti reamintim faptul ca stiind aceste sfaturi nu te vei transforma imediat intr-un
programator mai bun si, lucrand cu acestea pentru multi ani, nu inseamna ca nu vei mai face greseli.
Fiecare bucata de cod incepe ca o schita, asemanator lutului care, prin modelare continua, ajunge in forma sa finala.
In final, retusam imperfectiunile atunci cand il examinam alaturi de colegii nostri.
Nu va lasati descurajati de primele proiecte care au nevoie de imbunatatire. In schimb, ambitionati-va sa deveniti mai buni!

## **Variabile**

### Numele variabilelor trebuie sa fie sugestive si pronuntabile

**Gresit:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Corect:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ inapoi la cuprins](#cuprins)**

### Foloseste acelasi vocabular pentru acelasi tip de variabila

**Gresit:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Corect:**

```javascript
getUser();
```

**[⬆ inapoi la cuprins](#cuprins)**

### Foloseste nume cautabile

Vom citi mai mult cod decat vom scrie vreodata. Este important ca acel cod
pe care il scriem sa fie citibil si cautabil. _Nedenumind_ variabilele sugestiv
pentru intelegerea programului nostru ii incurcam pe cititorii nostri.
Alege sa denumesti variabilele cautabil. Unelte precum [buddy.js](https://github.com/danielstjules/buddy.js) si
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
pot ajuta la identificarea constantelor nedenumite.

**Gresit:**

```javascript
// Ce insemna 86400000 ?
setTimeout(blastOff, 86400000);
```

**Corect:**

```javascript
// Numele unei variabile scrise integral cu majuscule sugereaza ca aceea este o constanta.
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; // 86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

**[⬆ inapoi la cuprins](#cuprins)**

### Foloseste variabile explicative

**Gresit:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**Corect:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ inapoi la cuprins](#cuprins)**

### Evita codificarea mentala

Codul explicit este mai bun decat cel implicit.

**Gresit:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Am uitat ce era `l`...
  dispatch(l);
});
```

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

### Nu adauga context inutil

Daca numele clasei/obiectului este suficient de sugestiv, nu il repeta in numele variabilei.

**Gresit:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car, color) {
  car.carColor = color;
}
```

**Corect:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car, color) {
  car.color = color;
}
```

**[⬆ inapoi la cuprins](#cuprins)**

### Foloseste argumente implicite in loc de scurtcircuitari sau conditionari

Argumentele implicite sunt mai curate decat un scurtcircuit. Fii constient de faptul ca
daca le folosesti functia ta va oferi doar valori implicite pentru argumente "undefined".
Alte valori "false" precum `''`, `""`, `false`, `null`, `0` si `NaN` nu vor fi inlocuite cu o valoare implicita.

**Gresit:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**Corect:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ inapoi la cuprins](#cuprins)**

## **Functii**

### Argumentele functiei (ideal maxim 2)

Limitarea numarului de parametri ai functiei este incredibil de importanta, deoarece
face testarea functiei mai usoara. Mai mult de 3 parametri conduce la o explozie
combinatorica unde vei avea prea multe cazuri diferite de testat pentru fiecare
parametru in parte.

Unul sau doua argumente reprezinta cazul ideal, chiar si trei, dar ar trebui evitat pe cat de mult posibil.
De obicei, daca ai mai mult de 2 argumente inseamna ca functia ta face mai mult decat trebuie. In cazurile
unde functia face exact cat trebuie si nu poate fi simplificata/sparta in mai multe subfunctii, un obiect servit
drept argument va fi favorabil unei liste de argumente.

Deoarece JavaScript permite sa creezi obiecte ad-hoc, fara a defini neaparat o clasa separata si fara prea mult
cod inutil ("boilerplate"), poti utiliza un obiect in locul unei multimi de argumente.

Pentru a face evident la ce proprietati se asteapta functia, poti folosi sintaxa destructuratoare din ES2015/ES6.
Acest lucru are cateva avantaje:

1. Cand cineva se uita la semnatura functiei, va vedea imediat ce proprietati sunt utilizate.
2. Poate fi folosit pentru a simula parametrii denumiti.
3. Destructurarea cloneaza valorile primitive specificate ale argumentului obiect trecut in functie. Acest lucru poate ajuta
la prevenirea efectelor secundare. Nota: Obiectele si matricele (vectori, arrays) distruse din obiectul argument nu sunt clonate.
4. Uneltele "Linters" va pot avertiza cu privire la proprietatile neutilizate, ceea ce ar fi imposibil fara destructurare.

**Gresit:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

### Functiile ar trebui sa faca un singur lucru

Aceasta este de departe cea mai importanta regula in ingineria software.
Cand functiile fac mai mult de un lucru, sunt mai greu de compus, gandit si testat.
Cand puteti izola o functie sa serveasca unui singur rol, aceasta poate fi refactorizata
cu usurinta si codul va deveni mult mai curat.

**Gresit:**

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

**Corect:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ inapoi la cuprins](#cuprins)**

### Numele functiilor ar trebui sa fie sugestive, sa descrie rolul pe care il indeplinesc

**Gresit:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// Este greu de spus ce aduna aceasta functie judecand dupa denumirea sa
addToDate(date, 1);
```

**Corect:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ inapoi la cuprins](#cuprins)**

### Functiile ar trebui sa reprezinte un singur nivel de abstractizare

Cand aveti mai mult de un nivel de abstractizare, functia indeplineste, de obicei,
mai multe roluri. Divizarea functiilor duce la reutilizare si la testare facila.

**Gresit:**

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

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

### Elimina codul duplicat

Fa tot ce iti sta in putinta pentru a evita codul duplicat. Codul duplicat este
foarte rau, deoarece trebuie sa modifici in mai multe parti atunci cand trebuie
sa schimbi ceva in logica de functionare a programului.

Imagineaza-ti ca ai un restaurant si tii evidenta inventarului materiei prime:
rosii, ceapa, usturoi, condimente etc. Daca ai mai multe liste pe care tii evidenta,
atunci trebuie actualizate toate atunci cand servesti un fel de mancare cu, spre exemplu,
rosii. Daca ai o singura lista, este un singur loc unde trebuie sa faci actualizarea!

Adesea, codul duplicat apare, deoarece ai doua sau mai multe lucruri de indeplinit care
sunt usor diferite, dar au multe in comun, insa diferentele te obliga sa ai mai multe
functii separate cu roluri similare. Eliminarea codului duplicat inseamna abstractizarea care
sa poata gestiona acest set de factori diferiti, folosindu-te de o singura functie / modul / clasa.

Obtinerea corecta a abstractizarii este esentiala, de aceea ar trebui sa urmezi principiile SOLID
din sectiunea _Clase_. Abstractizarea facuta gresit poate fi mai rea decat codul duplicat, deci fii atent!

**Gresit:**

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
      portfolio
    };

    render(data);
  });
}
```

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

### Seteaza obiecte implicite cu Object.assign

**Gresit:**

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

**Corect:**

```javascript
const menuConfig = {
  title: "Order",
  // Utilizatorul nu a inclus cheia 'body'
  buttonText: "Send",
  cancellable: true
};

function createMenu(config) {
  let finalConfig = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    },
    config
  );
  return finalConfig
  // Obiectul "config" este acum: { title: "Order", body: "Bar", buttonText: "Send", cancellable: true }
  // ...
}

createMenu(menuConfig);
```

**[⬆ inapoi la cuprins](#cuprins)**

### Nu utiliza "flags" drept parametri ai functiei

"Flags" transmit programatorului ca functia face mai mult decat un singur lucru, iar functiile ar trebui sa faca un singur lucru!
Divizeaza functiile tale daca urmaresc cai diferite bazate pe o variabila tip "boolean" (bool);

**Gresit:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Corect:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ inapoi la cuprins](#cuprins)**

### Evita efectele secundare (Partea I)

O functie produce efecte secundare daca face mai multe decat sa primeasca o valoare
si sa returneze o alta valoare/valori. Un efect secundar ar putea fi scrierea intr-un
fisier, modificarea unei variabile globale sau, accidental, sa transfere toti banii
intr-un cont nedorit.

In unele cazuri ai nevoie de efecte secundare. Ca in exemplul precedent, ai putea
avea nevoie sa scrii intr-un fisier. Pentru a face acest lucru in siguranta, ai nevoie
sa centralizezi codul. Nu trebuie sa ai multiple functii si clase care scriu intr-un anumit
fisier, ci un singur serviciu care se ocupa de acest lucru.

Ideea principala este sa eviti capcanele comune precum partajarea starii intre obiecte
fara structura, folosirea de tipuri de date care pot fi scrise de oricine si necentralizarea unde
pot aparea efecte secundare nedorite. Daca respecti acest sfat, vei fi ferit de multe batai de cap!

**Gresit:**

```javascript
// Variabila globala la care face referire urmatoarea functie.
// Daca aveam o alta functie care folosea acest nume, acum ar fi fost un array care ar fi putut defecta programul.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Corect:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ inapoi la cuprins](#cuprins)**

### Evita efectele secundare (Partea II)

In JavaScript, unele valori sunt neschimbabile ("imutabile") si unele sunt
schimbabile ("mutabile"). Obiectele si array-urile sunt 2 tipuri de valori
mutabile, deci este important sa le manipulezi cu grija cand le trimiti drept
parametrii spre o functie. O functie poate schimba o proprietate a unui obiect
sau altera continutul unui array si poate genera un bug la care nici nu te-ai fi
asteptat, totul intr-un mod foarte usor.

Presupunem ca este o functie care accepta un parametru array reprezentand un cos
de cumparaturi. Daca functia face o modificare in array-ul cos de cumparaturi, adaugand
un obiect spre cumparare, de exemplu, atunci orice alta functie care foloseste acelasi
array drept cos va fi afectata de aceasta adaugare. Asta ar putea fi grozav, dar
ar putea fi si groaznic! Sa ne imaginam o situatie nefavorabila:

Un utilizator apasa pe butonul de "Cumpara" care apeleaza functia `purchase`, trimitand
un request si array-ul `cart` catre server. Din cauza unei probleme de Internet, functia
`purchase` trebuie sa reincerce sa trimita request-ul. Acum, ce s-ar intampla daca utilizatorul
apasa in acest timp, accidental, pe "Adauga in cos" la un obiect pe care nu il doreste, totul
inainte ca request-ul sa fie finalizat cu succes? Daca presupunerea se intampla, atunci functia
`purchase` va trimite accidental si obiectul adaugat din greseala in array-ul `cart`, deoarece
array-ul va fi fost deja modificat.

O solutie grozava ar fi pentru functia `addItemToCart` sa cloneze intotdeauna array-ul `cart`,
sa-l editeze si sa returneze clona. Acest lucru ar asigura faptul ca functiile care folosesc in continuare
array-ul vechi `cart` nu vor fi afectate de modificari.

Doua avertismente de mentionat pentru aceasta abordare:

1. Ar putea exista cazuri in care doriti sa modificati obiectul de input, dar cand adoptati
aceasta practica de programare veti constata ca aceste cazuri sunt destul de rare. Majoritatea
lucrurilor pot fi refactorizate pentru a nu avea efecte secundare.

2. Clonarea obiectelor mari poate fi foarte costisitoare din punct de vedere al performantei. Din fericire,
aceasta nu este o problema importanta in practica, deoarece exista [librarii grozave](https://facebook.github.io/immutable-js/)
care permit acest tip de abordare de programare sa fie rapida si nu la fel de intensa din punct de vedere
al memoriei utilizate precum clonarea manuala de obiecte si array-uri.

**Gresit:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Corect:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ inapoi la cuprins](#cuprins)**

### Nu suprascrie functii globale

Poluarea spatiului global este o practica foarte rea in JavaScript, deoarece te-ai putea
ciocni de alta librarie care a pus in practica aceasta metoda nedorita. Un exemplu ar fi:
ce ar fi daca ai vrea sa extinzi metoda nativa Array a JavaScript pentru a adauga metoda
`diff` care sa arate diferenta dintre 2 array-uri? Ai putea scrie noua ta functie in `Array.prototype`,
dar te-ai putea ciocni de alta librarie care a incercat sa faca acelasi lucru. Ce ar fi daca acea
librarie folosea `diff` pentru a gasi diferenta dintre primul si ultimul element al unui array? De aceea este
de dorit sa folosesti, datorita ES2015/ES6, clase si sa extinzi `Array` global.

**Gresit:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Corect:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ inapoi la cuprins](#cuprins)**

### Favorieaza paradigma programarii functionale impotriva celei imperative

JavaScript nu este un limbaj functional asa cum este Haskell, dar cu siguranta
poate fi folosit astfel. Limbajele functionale sunt adesea mai curate si mai usor
de testat. Alege intotdeauna acest stil de programare cand poti!

**Gresit:**

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

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

### Incapsuleaza conditiile

**Gresit:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**Corect:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ inapoi la cuprins](#cuprins)**

### Evita conditiile negative

**Gresit:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Corect:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ inapoi la cuprins](#cuprins)**

### Evita conditiile

Aceasta pare o sarcina imposibila. La prima vedere, majoritatea oamenilor ar spune: "cum ar trebui sa fac ceva fara
`if` ?" Raspunsul este ca puteti utiliza polimorfismul pentru a realiza aceeasi sarcina in multe cazuri. A doua intrebare
este, de obicei, "Ok, dar de ce sa fac asta?", iar raspunsul este un concept mentionat anterior: o functie ar trebui sa faca
un singur lucru. Cand aveti clase si functii cu instructiuni `if`, programatorul primeste informatia ca functia face mai mult de
un lucru.

**Gresit:**

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

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

### Evita verificarea tipului variabilei (Partea I)

JavaScript nu impune tipuri stricte de date, ceea ce inseamna ca functiile pot primi
orice tip de argument. Uneori aceasta libertate se intoarce impotriva ta si devine tentant
sa faci verificarea tipului de date in interiorul functiei. Exista multe modalitati de a evita
sa faci acest lucru. Primul lucru de luat in considerare sunt API-urile consistente.

**Gresit:**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**Corect:**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ inapoi la cuprins](#cuprins)**

### Evita verificarea tipului variabilei (Partea II)

Daca lucerzi cu valori primitive de baza, cum ar fi siruri si numere intregi,
si nu poti folosi polimorfismul, dar simti nevoia sa verifici tipul variabilei,
ia in considerare utilizarea TypeScript. Aceasta este o alternativa excelenta
JavaScript-ului normal, deoarece ofera tipizare statica pe langa sintaxa standard
JavaScript. Problema cu verificarea manuala a tipului variabilei este ca, pentru
a o face corect, trebuie scris mult cod in plus, iar sentimentul fals de siguranta nu
compenseaza cu lizibilitatea pierduta. Pastreaza codul curat, scrie teste multe si dese
si cauta sa primesti recenzii pozitive pentru codul scris.

**Gresit:**

```javascript
function combine(val1, val2) {
  if (
    (typeof val1 === "number" && typeof val2 === "number") ||
    (typeof val1 === "string" && typeof val2 === "string")
  ) {
    return val1 + val2;
  }

  throw new Error("Trebuie sa fie de tipul String sau Number");
}
```

**Corect:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ inapoi la cuprins](#cuprins)**

### Nu supraoptimiza

Browserele moderne fac multe optimizari in spate, la runtime. De multe ori, daca supraoptimizezi
nu faci altceva decat sa pierzi mult timp. [Exista multe resurse](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
pentru a vedea unde lipsesc optimizari. Ocupa-te de acelea intre timp, pana cand sunt reparate, daca pot fi reparate.

**Gresit:**

```javascript
// Pe browserele vechi, fiecare iteratie fara sa salvezi valoarea `list.length` este costisitoare
// fiindca `list.length` este recalculat. In browserele moderne, acest lucru este optimizat.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Corect:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ inapoi la cuprins](#cuprins)**

### Elimina codul neutilizat

Codul neutilizat este la fel de rau precum codul duplicat. Nu exista absolut niciun motiv
pentru care sa nu il inlaturi. Daca nu este apelat, scapa de el! Va fi stocat in siguranta in
istoricul sistemului de versionare daca vei avea vreodata nevoie de el.

**Gresit:**

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

**Corect:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ inapoi la cuprins](#cuprins)**

## **Obiecte si structuri de date**

### Foloseste getters si setters

Folosind getters si setters pentru a accesa proprietatile unui obiect este mult mai bine
decat sa accesezi direct proprietatile obiectului. Daca te intrebi "de ce?", aici este o lista
cu cateva motive:

- Cand vrei sa faci mai mult decat doar sa primesti valoarea proprietatii obiectului, nu trebuie
sa schimbi asta peste tot in cod.
- Este mai usor sa adaugi validare unei singure functii `set` decat sa adaugi validarea peste tot in cod.
- Incapsuleaza reprezentarea interna.
- Este mai usor sa adaugi loguri si sa manipulezi erorile folosind functii tip `getters` si `setters`.
- Te poti folosi de avantajul "lazy loading" pentru proprietatile obiectului (spre exemplu, le primesti de la un server).

**Gresit:**

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

**Corect:**

```javascript
function makeBankAccount() {
  // variabila privata
  let balance = 0;

  // functie "getter", facuta publica prin returnare
  function getBalance() {
    return balance;
  }

  // functie "setter", facuta publica prin returnare
  function setBalance(amount) {
    // ... valideaza inainte sa actualizezi soldul
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

**[⬆ inapoi la cuprins](#cuprins)**

### Faceti ca obiectele sa aiba membri privati

Acest lucru poate fi realizat prin "closures" (pentru ES5 si mai jos).

**Gresit:**

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

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

## **Clase**

### Alege clasele din ES2015/ES6 peste functiile simple din ES5

Este foarte dificil sa implementezi lizibil mostenirea, constructia si definitiile metodelor pentru clasele ES5 clasice.
Daca ai nevoie de mostenire, atunci opteaza pentru clasele ES2015/ES6. Cu toate acestea, alege functiile mici peste
clasele unde vei genera obiecte mari si complexe.

**Gresit:**

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

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

### Foloseste metoda inlantuirii

Acest pattern este foarte util in JavaScript si poate fi observat adesea in
multe librarii precum jQuery sau Lodash. Aceasta metoda permite codului sa fie expresiv
si mai compact. Pentru a putea folosi aceasta metoda, pur si simplu returnati `this` la sfarsitul
fiecarei functii din clasa, astfel puteti inlantui alte metode ale clasei asupra obiectului respectiv.

**Gresit:**

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

**Corect:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // NOTA: Returneaza `this` pentru inlantuire
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTA: Returneaza `this` pentru inlantuire
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTA: Returneaza `this` pentru inlantuire
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTA: Returneaza `this` pentru inlantuire
    return this;
  }
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

**[⬆ inapoi la cuprins](#cuprins)**

### Prefera compunerea in locul mostenirii

Conform [_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns) de Gang of Four,
ar trebui sa optezi pentru compunere in defavoarea mostenirii oriunde poti. Sunt foarte multe
motive pentru a folosi mostenirea si extraordinar de multe motive pentru a folosi compunerea.
Probabil te intrebi "cand sa folosesc mostenirea?". O lista unde mostenirea face mai mult
sens decat compunerea este:

1. Mostenirea reprezinta o relatie "este un/o ...", nu "are un/o..." (Om->Animal vs Utilizator->DetaliiUtilizator).
2. Poti reutiliza codul clasei de baza (Oamenii se pot misca la fle ca toate animalele).
3. Doresti sa faci modificari globale la clasele derivate prin schimbarea unei clase de baza (Schimba cheltuielile calorice
ale tuturor animalelor atunci cand se misca).

**Gresit:**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Gresit pentru ca Employee "au" EmployeeTaxData. EmployeeTaxData nu este un tip de Employee.
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

## **SOLID**

### Single Responsibility Principle (SRP)

Conform Clean Code, "Niciodata nu ar rtebui sa ife mai mult de un motiv pentru a schimba o clasa".
Este tentant sa supradotezi o clasa cu o multime de functionalitati, ca si cum ai lua o valiza pentru zbor.
Problema cu aceasta mentalitate intervine atunci cand apar o multitudine de motive de a schimba componentele,
fiecare schimbare implicand riscuri de a aparea efecte secundare in functionarea programului.

**Gresit:**

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

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

### Open/Closed Principle (OCP)

Conform lui Bertrand Meyer, "entitatile software (clase, module, functii etc.) ar trebui sa fie deschise
pentru extindere, dar inchise pentru modificare. Practic, acest principiu enunta faptul ca trebuie sa permiti
utilizatorilor sa adauge noi functionalitati fara a schimba codul existent.

**Gresit:**

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
        // transforma raspunsul si returneaza
      });
    } else if (this.adapter.name === "nodeAdapter") {
      return makeHttpCall(url).then(response => {
        // transforma raspunsul si returneaza
      });
    }
  }
}

function makeAjaxCall(url) {
  // trimite request si returneaza un promise
}

function makeHttpCall(url) {
  // trimite request si returneaza un promise
}
```

**Corect:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }

  request(url) {
    // trimite request si returneaza un promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }

  request(url) {
    // trimite request si returneaza un promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then(response => {
      // transforma raspunsul si returneaza
    });
  }
}
```

**[⬆ inapoi la cuprins](#cuprins)**

### Liskov Substitution Principle (LSP)

Formal, principiul este enuntat astfel: "Daca S este un subtip al lui T, atunci obiectele tip t
pot fi inlocuite cu obiecte de tip S (obiectele tip S pot inlocui obiectele tip T) fara a modifica
nicio proprietate a acelui program (corectitudine, sarcina efectuata etc.).". Cea mai buna explicatie
pentru acest principiu este: daca aveti o clasa de parinti si o clasa de copii, atunci clasa de baza si clasa
de copii pot fi utilizate in mod interschimbabil fara a obtine rezultate incorecte. Un exemplu cu siguranta
va elucida misterul: patratul este un caz particular de dreptunghi.

**Gresit:**

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
    const area = rectangle.getArea(); // Gresit: Returneaza 25 pentru patrat, trebuie sa fie 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

### Interface Segregation Principle (ISP)

JavaScript nu are interfete, deci acest principiu nu poate fi aplicat la fel de strict.

ISP afirma ca "Clientii nu trebuie obligati sa depinda de interfete pe care nu le folosesc.".
"Interfetele sunt contracte implicite in JavaScript din cauza 'duck typing'-ului.".

Un exemplu pentru acest principiu in JavaScript este cel al claselor care necesita obiecte mari pentru setari.
Este benefic sa nu fie necesar pentru clienti sa configureze cantitati uriase de optiuni, deoarece de cele mai multe ori
nu vor avea nevoie de toate setarile. Facand setari optionale ajuta la prevenirea unei interfete alambicate.

**Gresit:**

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
  animationModule() {} // De cele mai multe ori, nu va trebui sa animam in timp ce traversam.
  // ...
});
```

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

### Dependency Inversion Principle (DIP)

Acest principiu prevede doua lucruri esentiale:

1. Modulele de nivel superior nu ar trebui sa depinda de modulele de nivel inferior. Ambele ar trebui sa depinda de abstractii.
2. Abstractiile nu ar trebui sa depinda de detalii. Detaliile ar trebui sa depinda de abstractii.

Daca ati lucrat cu AngularJS, ai obaservat o implementare a acestui principiu sub forma de "Dependency Injection" (DI). Desi nu sunt
concepte identice, DIP impiedica modulele de nivel inalt sa cunoasca detaliile modulelor de nivel inferior si sa le configureze. Un avantaj
imens al acestui fapt este ca reduce cuplarea intre module. Cuplarea este un model de dezvoltare gresit, deoarece face codul greu de refactorizat.

Dupa cum am mentionat anterior, JavaScript nu are interfete, deci abstractiile care depind de ele sunt contracte implicite, adica metodele si proprietatile
pe care un obiect/clasa le expune unui alt obiect/clasa. In exemplul de mai jos, contractul implicit este ca orice modul Request pentru un `InventoryTracker`
va avea o metoda `requestItems`.

**Gresit:**

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

    // Gresit: Tocmai ce am creat o dependenta asupra unei implementari specifice de request.
    // Ar trebui ca requestItems sa depinda de o metoda de request: `request`
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

**Corect:**

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

// Construind dependentele noastre extern si injectandu-le, putem sa inlocuim
// cu usurinta modulul de request cu unul modern care foloseste WebSockets.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ inapoi la cuprins](#cuprins)**

## **Testare**

Testarea este mai importanta decat livrarea. Daca nu ai teste sau ai teste insuficiente,
atunci cand livrezi codul nu poti fi sigur ca nu ai defectat ceva. Daca ai acoperire totala,
vei avea incredere maxima in codul scris si vei fi mult mai linistit. Pe langa un framework grozav
de testare, vei avea nevoie si de o [unealta buna de acoperire](https://gotwarlost.github.io/istanbul/).

Nu exista nicio scuza pentru a nu scrie teste. Exista [o multime de framework-uri pentru teste in JavaScript](https://jstherightway.org/#testare-tools),
asa ca gaseste-o pe cea care o prefera echipa si testeaza tot! Daca metoda ta preferata este Test Driven Development (TDD), este perfect! Ideea principala
este sa te asiguri ca ai acoperit codul ca sa nu apara erori neasteptate dupa lansarea actualizarilor in urma modificarilor sau refactorizarilor.

### Testeaza individual

**Gresit:**

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

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

## **Concurenta**

### Foloseste Promises, nu callback-uri

Callback-urile nu mai reprezinta o metoda buna, generand "spaghetti code" (mult nesting).
Cu ES2015/ES6, Promises sunt un tip global incorporat.

**Gresit:**

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

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

### Async/Await sunt si mai bune decat Promises

Promises sunt o alternativa foarte buna pentru callback-uri, dar ES2017/ES8 a adus async si await, care ofera o solutie
si mai curata. Tot ce ai nevoie este o functie cu prefixul `async` si poti scrie logica imperativa a programului fara un lant
de functii `then`.

**Gresit:**

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

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

## **Manipularea erorilor**

Erorile sunt un lucru bun, deoarece inseamna ca au fost identificate cazuri in care programul
tau nu functioneaza cum ar trebui si te instiinteaza de acest lucru, oferindu-ti, de asemenea,
detalii cu privire la remedierea situatiei.

### Nu ignora erorile

Ignorand erorile nu inseamna ca le-ai reparat. Simpla notificare in consola (`console.log`) nu este varianta
ideala, adesea fiind pierduta in marea de mesaje printate in consola. Daca infasori fiecare parte a codului in
blocuri de `try/catch` vei avea control mai mare asupra erorilor.

**Gresit:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Corect:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // O optiune (mai vizibila decat console.log):
  console.error(error);
  // Alta optiune:
  notifyUserOfError(error);
  // Alta optiune:
  reportErrorToService(error);
  // Sau foloseste-le pe toate!
}
```

### Nu ignora "rejected promises"

Pentru acelasi motiv nu trebuie sa ignori erorile aruncate de `try/catch`.

**Gresit:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**Corect:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    // O optiune (mai vizibila decat console.log):
    console.error(error);
    // Alta optiune:
    notifyUserOfError(error);
    // Alta optiune:
    reportErrorToService(error);
    // Sau foloseste-le pe toate!
  });
```

**[⬆ inapoi la cuprins](#cuprins)**

## **Formatare**

Formatarea este subiectiva. Foloseste-te de [tonele de programe](https://standardjs.com/rules.html) care automatizeaza formatarea.
Este o risipa de timp si bani pentru programatori sa se certe asupra formatarii.

### Foloseste majusculele cu incredere

JavaScript este "untyped" (nu cere impunerea unui tip strict variabilelor), deci scrierea cu
majuscule iti va spune multe despre variabilele, functiile, clasele tale. Aceste reguli sunt subiective,
singurul lucru pe care trebuie sa il tii minte este sa fii consistent in notatie.

**Gresit:**

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

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

### Functiile apelante si functiile apelate trebuie sa fie aproape

Daca o functie apeleaza o alta, tine-le aproape in codul sursa. Ideal este sa ai functia
apelanta fix deasupra celei apelate. Tindem sa citim codul de sus in jos, ca un ziar.
Din acest motiv, redacteaza codul pentru a putea fi citit natural.

**Gresit:**

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

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

## **Comentarii**

### Comenteaza doar liniile de cod care au complexitate ridicata

Comentariile sunt o scuza, nu o cerinta. Codul bun se autodocumenteaza in mare parte.

**Gresit:**

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

**Corect:**

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

**[⬆ inapoi la cuprins](#cuprins)**

### Nu lasa cod comentat printre liniile de cod curente

Versionarea exista dintr-un motiv. Lasa codul vechi in urma!

**Gresit:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Corect:**

```javascript
doStuff();
```

**[⬆ inapoi la cuprins](#cuprins)**

### Nu tine un jurnal in comentarii

Aminteste-ti, foloseste sisteme de versionare! Nu este nevoie de cod neutilizat, cod comentat si nici de comentarii inutile.
Foloseste `git log` pentru a obtine istoricul modificarilor!

**Gresit:**

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

**Corect:**

```javascript
function combine(a, b) {
  return a + b;
}
```

**[⬆ inapoi la cuprins](#cuprins)**

### Evita markerele pozitionale

Lasa functiile si numele variabilelor impreuna cu indentarea si formatarea corespunzatoare sa confere structura vizuala a codului.

**Gresit:**

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

**Corect:**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```

**[⬆ inapoi la cuprins](#cuprins)**

## Postarea originala in limba engleza

- [ryanmcdermott/clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript/)

**[⬆ inapoi la cuprins](#cuprins)**