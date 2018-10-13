# clean-code-javascript

## Lista dei contenuti
  1. [Introduzione](#introduzione)
  2. [Variabili](#variabili)
  3. [Funzioni](#funzioni)
  4. [Ogetti e strutture dati](#objects-and-data-structures)
  5. [Classi](#Classi)
  6. [SOLID](#solid)
  7. [Test](#Test)
  8. [Concurrency](#concurrency)
  9. [Error Handling](#error-handling)
  10. [Formatting](#formatting)
  11. [Comments](#comments)
  12. [Translation](#translation)

## Introduzione
![Immagine umoristica che rappresenta quanto sia possibile stimare la qualità di un software attraverso il numero di parolacce espresse durante la lettura del codice](http://www.osnews.com/images/comics/wtfm.jpg)

Principi di Ingegneria del Software, dal libro di Robert C. Martin
[*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
adattati a JavaScript.

Non si tratta di una guida stilistica, bensì una guida per cercare di produrre software
[leggibile, riutilizzabile e rifattorizzabile](https://github.com/ryanmcdermott/3rs-of-software-architecture) in JavaScript.

Non tutti i principi di questa guida devono essere seguiti alla lettera, e solo alcuni sono universalmente condivisi. Sono linee guida e niente più, ma sono state tutte apprese in anni di esperienza collettiva dall'autore di *Clean code*.

Il nostro lavoro come ingegneri del software ha solo 50 anni e stiamo ancora cercando di apprendere molto. Quando l'architettura del software sarà antica come l'architettura in sè, probabilmente avremo regole più rigide da seguire. Per ora facciamo si che queste linee guida servano come termine di paragone per valutare la qualità del software che tu ed il tuo team producete.

Un ultima cosa: conoscere queste regole non farà di te immediatamente uno sviluppatore di software migliore, e lavorare per tanti anni come tale non ti eviterà di commettere errori.
Ogni singola parte di codice parte come bozza, prima, per per poi prendere forma come una scultura di argilla.
Solo alla fine perfezioneremo il nostro software, quando revisioneremo il codice con i nostri colleghi. Ma non ti abbattre alla prima revisione che richiederà miglioramenti: *Beat up the code instead!*

## **Variabili**
### Utilizza nomi di variabili comprensibili e pronunciabili

**Da evitare**
```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
```

**Bene:**
```javascript
const currentDate = moment().format('YYYY/MM/DD');
```
**[⬆ torna su](#lista-dei-contenuti)**

### Usa lo stesso lessico per lo stesso tipo di variabili

**Da evitare**
```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Bene:**
```javascript
getUser();
```
**[⬆ torna su](#lista-dei-contenuti)**

### Utilizza nomi ricercabili
Leggeremo molto più codice di quanto non ne scriveremo mai. È importante che il codice che noi scriviamo sia leggibile e ricercabile. Nominando variabili che non assumono uno specifico contesto all'interno del nostro software, irritiamo il lettore.
Fai in modo che i nomi delle tue variabili siano ricercabili.
Strumenti come [buddy.js](https://github.com/danielstjules/buddy.js) e
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) possono aiutarti ad identificare costanti non rinominate.

**Da evitare**
```javascript
// Cosa caspita significa 86400000?
setTimeout(blastOff, 86400000);

```

**Bene:**
```javascript
// Dichiarala come costante in maiuscolo.
const MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);

```
**[⬆ torna su](#lista-dei-contenuti)**

### Utilizza nomi di variabili esplicartivi
**Da evitare**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(address.match(cityZipCodeRegex)[1], address.match(cityZipCodeRegex)[2]);
```

**Bene:**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```
**[⬆ torna su](#lista-dei-contenuti)**

### Evita mappe mentali
Essere espliciti è meglio che non esserlo.

**Da evitare**
```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((l) => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // A cosa fa riferimento esattamente `l`?
  dispatch(l);
});
```

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**

### Non contestualizzare inutilmente

Se il nome della tua classe/oggetto ti indica a cosa fa riferimento, non ripeterlo nei nomi delle sue proprietà o funzioni.

**Da evitare**
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

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**

### Utilizza i valori di default (predefiniti), anzichè usare condizioni o cortocircuiti

I valori di default, generalmente sono più chiari dei cortocircuiti. Tieni presente che se non utilizzerai questo approccio, la tua funzione restituirà solo `undefined` come valore di default.
Tutti gli altri valori "falsi" come `''`, `""`, `false`, `null`, `0`, e
`NaN`, non saranno sostituiti da un valore predefinito.

**Da evitare**
```javascript
function createMicrobrewery(name) {
  const breweryName = name || 'Hipster Brew Co.';
  // ...
}

```

**Bene:**
```javascript
function createMicrobrewery(name = 'Hipster Brew Co.') {
  // ...
}

```
**[⬆ torna su](#lista-dei-contenuti)**

## **Funzioni**
### Argomenti di una funzione (idealmente 2 o anche meno)

Limitare il numero di argomenti di una funzione è incredibilmente importante perchè ti permette di testarla più facilmente. Avere più di 3 argomenti può portare ad un'esplosione di combinazioni da testare, che produrranno una lunga serie di casi da verificare.

1 o 2 argomenti sono l'ideale e dovremmo evitarne un terzo se possibile. Generalmente se la tua funzione ha più di 2 argomenti, forse, sta facendo troppe operazioni. In alcuni casi, in cui questo non è del tutto vero, un oggetto può aiutare ad ovviare a questo problema.

Dal momento in cui JavaScript permette la creazione di oggetti al volo, senza dover passare attraverso classi specifiche, puoi usare un oggetto se pensi che il tuo metodo richieda molti argomenti.

Per rendere evidente cosa la funzione si aspetta di ricevere, puoi utilizzare la sintassi destrutturata (destructuring syntax) di ES2015/ES6 che ha diversi vantaggi:

1. Quando qualcuno osserva la firma della tua funzione, è immediatamente chiaro che proprietà sono state utilizzate

2. Destrutturare, oltretutto, clona i valori primitivi passati alla funzione. Questo può prevenire effetti collaterali. Nota: oggetti ed array destrutturati nell'oggetto usato come argomento NON saranno clonati.

3. Un Linter può avvisarti che non stai utilizzando alcune delle proprietà del tuo oggetto, non utilizzando la sintassi destrutturata non sarebbe possibile

**Da evitare**
```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**


### Un metodo dovrebbe fare una sola cosa
Questa è di sicuro la regola più importante nell'ingegneria del software. Quando un metodo si occupa di più di un solo aspetto sarà più difficile da testare, comporre e ragioraci sopra.
Se è possibile far eseguire al metodo una sola azione sarà più facile da rifattorizzare e la leggibilità del tuo codice sarà maggiore e più chiara. Anche se non dovesse rimanerti in mente altro di questa guida, sarai comunque più avanti di molti sviluppatori.

**Da evitare**
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

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**

### I nomi delle funzioni dovrebbero farti capire cosa fanno

**Da evitare**
```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// Difficile da dire esattamente cosa viene aggiunto tramite questa funzione
addToDate(date, 1);
```

**Bene:**
```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```
**[⬆ torna su](#lista-dei-contenuti)**

### Le funzioni dovrebbero avere un solo livello di astrazione

Quando hai più di un livello di astrazione, la tua funzione generalmente sta facendo troppe cose. Dividere in più funzioni aiuta a riutilizzarla e testarla più facilmente. 

**Da evitare**
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

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**

### Rimuovi il codice duplicato
Fai del tuo meglio per evitare codice duplicato. Duplicare il codice è un male, perchè vuol dire che c'è più di un punto da modificare nel caso in cui dovessi cambiare alcune logiche.

Immagina di avere un ristorante e di dover tener traccia del tuo magazzino: la riserva di pomodori, cipolle, aglio, spezie, etc. Se hai più di una lista in cui tieni traccia di queste quantità dovrai aggiornarle tutte, ogni volta che servirai un piatto con dei pomodori. Al contrario, se dovessi avere una sola lista, avrai un solo posto un cui dovrai tenere traccia delle modifiche sulle quantità in magazzino.

Generalmente si duplica il codice perchè ci sono due o tre piccole differenze tra una parte e l'altra del software. Questo permette di condividere le parti comuni del codice, ma allo stesso tempo avrai dei duplicati di parti che fanno la stessa cosa.
Rimuovere questi duplicati, significa creare un'astrazione che permette di gestire queste differenze attraverso un unico metodo/modulo/classe.

Ottenere la sufficiente astrazione può essere complicato. Per questo dovresti seguire i principi SOLID, approfonditi nella sezione *Classi*.
Un'astrazione non ottimale potrebbe anche essere peggio del codice duplicato, per cui fai attenzione! Non ripeterti, altrimenti dovrai aggiornare tutte le occorrenze della stessa logica ogni volta che vorrai cambiare qualcosa.

**Da evitare**
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

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**

### Estendi un oggetto con Object.assign

**Da evitare**
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

**Bene:**
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

  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```
**[⬆ torna su](#lista-dei-contenuti)**


### Non usare valori flag (true/false) come parametri di una funzione

Un valore di tipo flag indica che la tua funzione può eseguire più di una sola operazione. Una funzione dovrebbe eseguire una sola operazione. Separa la tua funzione se deve eseguire più di una operazione in base al parametro flag che riceve in input.

**Da evitare**
```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Bene:**
```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```
**[⬆ torna su](#lista-dei-contenuti)**

### Evitare effetti collaterali (parte 1)

Una funzione può generare un effetto collaterale se fa altro oltre a ricevere un valore e restituirne uno o più. L'effetto collaterale potrebbe essere scrivere su un file, modificare una variabile globale o accidentalmente girare tutti i tuoi soldi ad uno sconosciuto.

Probabilmente e occasionalmente avrai bisogno di generare un effetto collaterale: come nell'esempio precedente magari proprio scrivere su un file.
Quello che dovrai fare è centralizzare il punto in cui lo fai. Non avere più funzioni e classi che fanno la stessa cosa, ma averne un servizio ed uno soltanto che se ne occupa.

La questione più importante è evitare le insidie che stanno dietro ad errate manipolazioni di oggetti senza alcuna struttura, utilizzando strutture che possono essere modificate da qualunque parte.
Se riuscirai ad evitare che questo accada...sarai ben più felice della maggior parte degli altri programmatori.

**Da evitare**
```javascript

// Variable globale utilizzata dalla funzione seguente.
// Nel caso in cui dovessimo utilizzarla in un'altra funzione a questo punto si tratterebbe di un array e potrebbe generare un errore.

let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Bene:**
```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

const name = 'Ryan McDermott';
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```
**[⬆ torna su](#lista-dei-contenuti)**

### Evitare effetti collaterali (parte 2)
In Javascript i valori primitivi sono passati come valori, mentre oggetti ed array vengon passati come riferimento. In caso di oggetti ed array, se la tua funzione modifica l'array contenente un carrello della spesa, per esempio aggiungendo o rimuovendo un oggetto da acquistare, tutte le altre funzioni che utilizzano l'array `carrello` saranno condizionati da questa modifica. Queso potrebbe essere un bene ed un male allo stesso tempo. Immaginiamo una situazione in cui questo è un male:

l'utente clicca sul tasto "Acquista", che richiamerà una funzione `acquista` che effettua una richiesta ed invia l'array `carrello` al server. Per via di una pessima connessione, il metodo `acquista` riproverà ad effettuare la richiesta al server. Cosa succede se nello stesso momento accidentalmemte l'utente clicca su "Aggiungi al carrello" su di un oggetto che non ha intenzione di acquistare prima che venga eseguita nuovamente la funzione?
Verrà inviata la richiesta con il nuovo oggetto accidentalmente aggiunto al carrello utilizzando la funzione `aggiungiOggettoAlCarrello`.

Un'ottima soluzione è quella di di clonare sempre l'array `carrello`, modificarlo e restituire il clone.
Questi ci assicurerà che che nessun'altra funzione che gestisce il carrello subirà cambiamenti non voluti.

Due precisazioni vanno fatte su questo approccio:

1. Potrebbe essere che tu voglia realmente modificare l'oggetto in input, ma vedrai che utilizzando questo approccio ti accorgerai che le questi casi sono veramente rari. La maggior parte delle volte dovrai utilizzare questo approccio per non generare effetti collaterali

2. Clonare oggetti molto grandi potrebbe essere davvero dispendioso in termini di risorse. Fortunatamente questo non è un problema perchè esistono [ottime librerie](https://facebook.github.io/immutable-js/) che permettono di utilizzare questo approccio senza dispendio di memoria e più velocemente rispetto al dovelo fare manualmente.

**Da evitare**
```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Bene:**
```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ torna su](#lista-dei-contenuti)**

### Don't write to global funzioni
### Non aggiungere funzioni globali

Contaminare delle variabili globali è una pratica sconsigliata, in quanto potresti entrare in conflitto con altre librerie e l'utilizzatore delle tue API potrebbe non accorgersene fintanto che non si trova in produzione, generando un'eccezione.
Facciamo un esempio pratico: supponiamo che tu voglia estendere il costruttore Array nativo di JavaScript aggiungendo il metodo `diff` che mostra le differenze tra due array. Come puoi fare?
Potresti scrivere il metodo utilizzando `Array.prototype`, che però potrebbe entrare in conflitto con con un'altra libreria che fa la stessa cosa. Cosa succederebbe se anche l'altra libreria utilizzasse `diff` per trovare le differenze tra due array? Ecco perchè è molto meglio utilizzare le classi ES2015/ES6 e semplicemente estendere `Array`.

**Da evitare**
```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Bene:**
```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```
**[⬆ torna su](#lista-dei-contenuti)**

### Preferisci la programmazione funzionale a quella imperativa

*[Programmazione funzionale](https://it.wikipedia.org/wiki/Programmazione_funzionale)* - 
*[Programmazione imperativa](https://it.wikipedia.org/wiki/Programmazione_imperativa)*

Javascript non è un linguaggio funzionale alla stregua di Haskell, ma entrambi hanno qualcosa che li accomuna.
I linguaggi funzionali generalmente sono più puliti e facili da testare.
Preferisci questo stile se possibile.

**Da evitare**
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

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**

### Incapsula i condizionali

**Da evitare**
```javascript
if (fsm.state === 'fetching' && isEmpty(listNode)) {
  // ...
}
```

**Bene:**
```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === 'fetching' && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```
**[⬆ torna su](#lista-dei-contenuti)**

### Evita di verificare condizioni in negativo

**Da evitare**
```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Bene:**
```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```
**[⬆ torna su](#lista-dei-contenuti)**

### Evita i condizionali
Sembrerebbe un task impossibile. Ad un rapido sguardo molti sviluppatori potrebbero pensare "come posso pensare di far funzionare qualcosa senza utilizzare un `if`?"
La risposta è che puoi utilizzare il polimorfismo per ottenere lo stesso risultato in molti casi.
La seconda domanda generalmente è "Ottimo! Ma perchè dovrei farlo?".
La risposta è data in uno dei concetti precedentemente descritti: una funzione dovrebbe eseguire una sola operazione.
Quando hai una Classe con delle funzioni che utilizzano lo stato `if` stai dicendo all'utente che la tua funzione può fare più di una operazione. 


**Da evitare**
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

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**

### Evita la validazione dei tipi (parte 1)

JavaScript è un linguaggio non tipizzato, il che significa che le tue funzioni possono accettare qualunque tipo di argomento.
Qualche volta potresti essere tentato da tutta questa libertà e potresti essere altrettanto tentato di veririfcare il tipo di dato ricevuto nella tua funzione.
Ci sono molti modi per evitare di dover fare questo tipo di controllo.
Come prima cosa cerca scrivere API consistenti.

**Da evitare**
```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location('texas'));
  }
}
```

**Bene:**
```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location('texas'));
}
```
**[⬆ torna su](#lista-dei-contenuti)**

### Evita la validazione dei tipi (part 2)

Se stai lavorando con tipi di dati primitivi come stringhe o interi e non puoi utilizzare il paradigma del polimorfismo, ma senti ancora l'esigenza di validare il tipo di dato considera l'utilizzo di TypeScript.
È una vlidissima alternativa al normale JavaScript che fornicsce una validazione di tipi statica utilizzando la sintassi JavaScript.
Il problema con la validazione manuale dei tipi utilizzando JavaScript standard è che richiede tanto codice extra che non compensa la scarsa leggibilità del codice ottenuto.
Mantieni pulito il tuo codice, scrivi dei test validi e cerca di fare delle buone revisioni del coidice. Altrimenti utilizza TypeScript (che come detto in precedenza è una validissima alternativa!)

**Da evitare**
```javascript
function combine(val1, val2) {
  if (typeof val1 === 'number' && typeof val2 === 'number' ||
      typeof val1 === 'string' && typeof val2 === 'string') {
    return val1 + val2;
  }

  throw new Error('Must be of type String or Number');
}
```

**Bene:**
```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```
**[⬆ torna su](#lista-dei-contenuti)**

### Non ottimizzare eccessivamente
I browser moderni eseguono un sacco di ottimizzazione "sottobanco". Molte volte, quando cerchi di ottimizzare il tuo codice, stai perdendo tempo prezioso. [Ci sono tante guide](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) che aiutano a capire quali tipi di ottimizzazione sono superflui e quali no. Utilizza queste guide nel frattempo, fintanto che queste mancanze non verranno colmate.

**Da evitare**
```javascript

// Nei vecchi browser ogni volta che in un ciclo verifichi `list.length` potrebbe
// essere dispendioso per via del ricalcolo della lunghezza. Nei browser moderni
// questa operazione è stata ottimizzata
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Bene:**
```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```
**[⬆ torna su](#lista-dei-contenuti)**

### Rimuovi il codice inutilizzato
Il codice inutilizzato è dannoso quanto il codice duplicato. Non c'è motivo per cui tenerlo nella tua codebase. Se realmente non viene utilizzato rimuovilo!
Sarà comunque presente nella storia del tuo file se utilizzi un sistema di versioning nel caso in cui dovesse servirti.

**Da evitare**
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

**Bene:**
```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```
**[⬆ torna su](#lista-dei-contenuti)**

## **Ogetti e strutture dati**
### Utilizza getter e setter
Utilizzare getter e setter per acceedere ai dati di un oggetto può essere meglio che accedere direttamente alle sue proprietà. Ti starai chiedendo il motivo.
Eccoti qualche motivo per cui utilizzare getter e setter:
* Quando hai bisogno di eseguire un'operazione col dato recuperato, non devi andare a modificarlo ogni volta che accedi al dato nel tuo codice.
* Valida il dato nel momento in cui lo setti con `set`
* Incapsula la rappresentazione interna
* È più facile loggare e gestire gli errori quando imposti o recuperi un dato
* Puoi caricare i dati del tuo oggetto in modalità *lazy*, per esempio caricandoli dal server.


**Da evitare**
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

**Bene:**
```javascript
function makeBankAccount() {
  // questa proprietà è privata
  let balance = 0;

  // un "getter", la rende pubblica restituendola in questo modo
  function getBalance() {
    return balance;
  }

  // un "setter", la rende pubblica in questo modo
  function setBalance(amount) {
    // ... e la valida prima di impostarla
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
**[⬆ torna su](#lista-dei-contenuti)**


### Imposta proprietà private in un oggetto
Può essere fatto attraverso le *closure* (per ES5 e versioni precedenti).

**Da evitare**
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

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**


## **Classi**
### Utilizza le classi ES2015/ES6 piuttosto che le funzioni di ES5
È molto difficile ottenere leggibilità per ereditarietà, costrutti e metodi in definizioni di oggetti in ES5. Se hai bisogno di ereditarietà (e bada bene, non è detto che tu ne abbia bisogno), utilizza il costrutto Class di ES2015/ES6. Altrimenti utilizza piccole funzioni fintanto che non avrai bisogno di gestire oggetti più complessi.

**Da evitare**
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

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**


### Concatena i metodi
Questo pattern è molto utilizzato in JavaScript e puoi trovarne applicazione in molte liberie come jQuery e Loadash. Permette al tuo codice di essere maggiormente espressivo e meno verboso.
Proprio per questo motivo, insisto, utilizza la concatenazione dei metodi e guarda come può essere più pulito il tuo codice. Nei metodi della tua classe, semplicemente restituisci il riferimento `this` alla fine di ogni metodo, in modo da poter concatenare altri metodi.


**Da evitare**
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

**Bene:**
```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // NOTA: restituisci this per poter concatenare altri metodi.
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTA: restituisci this per poter concatenare altri metodi.
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTA: restituisci this per poter concatenare altri metodi.
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTA: restituisci this per poter concatenare altri metodi.
    return this;
  }
}

const car = new Car('Ford','F-150','red')
  .setColor('pink')
  .save();
```
**[⬆ torna su](#lista-dei-contenuti)**

### Preferisci la composizione all'ereditarietà
Come dichiarato dalla Gang of four in [*Design Patterns*](https://en.wikipedia.org/wiki/Design_Patterns) dovresti preferire la composizione all'ereditarietà quando puoi. Ci sono validi motivi per utilizzare l'ereditarietà e altrettanto validi motivi per utilizzare la composizione.
Il punto principale di questo assunto è che mentalmente sei portato a preferire l'ereditarietà; prova a pensare alla composizione per risolvere il tuo problema, tante volte è davvero la soluzione migliore.
Ti potresti chiedere: "Quando dovrei utilizzare l'ereditarietà?". Dipende dal problema che devi affrontare, ma c'è una discreta lista di suggerimenti che ti potrebbero aiutare a capire quando l'ereditarietà è meglio della composizione:

1. L'estensione che stai mettendo in atto rappresenta una relazione di tipo "è di questo tipo" e non "ha questa proprietà" (Umano->Animale vs. Utente->DettagliUtente).
2. Puoi riutilizzare il codice dalla classe padre (Umano)
3. Vuoi fare cambiamenti globali a tutte le classi estese tramite la classe di partenza.

**Da evitare**
```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Male because Employees "have" tax data. EmployeeTaxData is not a type of Employee
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**

## **SOLID**
### Single Responsibility Principle (SRP) (Principio della singola responsabilità)
Come indicato in *Clean code*, "Non dovrebbe mai esserci più di un solo motivo per modificare una classe". La tentazione è sempre quella di fare un'unica classe con molte funzionalità, come quando vuoi portarti un unico bagaglio sul volo.
Il problema con questo approccio è che le tue classi non saranno concettualmente coese e ti potrebbero dare più di una ragione per modificarle in seguito.
Minimizzare il numero di volte in cui modificare una classe è importante.
È importante perchè quando ci sono troppe funzionalità in una classe è difficile capire che effetto avrà sulle classe che la estendono, nel caso in cui farai un cambiamento.

**Da evitare**
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

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**

### Open/Closed Principle (OCP) (Principio di apertura/chiusura)
Come dichiarato da Bertrand Meyer, "Le entità di un software (Classi, moduli, funzioni, etc.) dovrebbero essere aperte all'estensione, ma chiuse alla modifica. Cosa significa esattamente?
Quello che intende è che dovresti dare ai tuoi utilizzatori la possibilità di aggiungere nuove funzionalità, non modificando quelle esistenti.

**Da evitare**
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

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**

### Liskov Substitution Principle (LSP) (Principio dell'intercambiabilità di Liskov)
Questo nome sembra molto più spaventoso di quello che in realtà significa.
Formalmente la sua defnizione è "Se S è un sottotipo di T, allora gli oggetti di tipo T possono essere sostituiti con oggetti di tipo S () senza modificare alcuna della proprietà del software (correttezza, compito da svolgere, etc.)". Questa definizione suona ovviamente come più complessa.
Forse una spiegazione più esaustiva potrebbe essere: "Se hai una classe *figlio* che estende una classe *genitore* allora le due classi possono essere intercambiate all'interno del codice senza generare errori o risultati inattesi".
Potrebbe esssere ancora poco chiaro, ma vediamo con un esempio (Quadrato/Rettangolo): matematicamente il Quadrato è un Rettangolo, ma se il tuo modello usa una relazione di tipo "è-un" per eredità, potresti avere presto qualche problema.

**Da evitare**
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
    const area = rectangle.getArea(); // Male: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**

### Interface Segregation Principle (ISP) (Principio della segregazione delle interfacce)

JavaScript non utilizza Interfacce, quindi non è possibile applicare questo principio alla lettera. Tuttavia è importante anche per via della sua mancanza di tipizzazione.

ISP indica che "Gli utenti non dovrebbero mai esssere forzati a dipendere da interfacce che non utilizza.". Le interfacce sono contratti impliciti in JavaScript per via del [*duck-typing*](https://it.wikipedia.org/wiki/Duck_typing) 
Un buon esempio in JavaScript potrebbe essere fatto per le classi che richiedono la deinizione di un set di proprietà molto grande.
Non utilizzare classi che richiedono la definizione di molte proprietà per essere istanziate è sicuramente un beneficio perchè spesso non tutte queste proprietà richiedono di essere impostate per utilizzare la classe.
Rendere questi parametri opzionali evita di avere un'interfaccia pesante.


**Da evitare**
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
  animationModule() {} //Il più delle volte potremmo non dover animare questo oggetto
  // ...
});

```

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**

### Dependency Inversion Principle (DIP) (Principio di inversione delle dipendenze)
Questo principio sostanzialmente indica due cose:
1. Moduli ad alto livello non dovrebbero necessariamente dipendere da moduli di basso livello
2. L'astrazione non dovrebbe dipendere dai dettagli. I dettagli non dovrebbero dipendere dall'astrazione.

A primo impatto questo concetto potrebbe essere difficile da capire, ma nel caso in cui tu abbia lavorato con AngularJS avrai sicuramente visto l'applicazione di questo principio nel concetto di *Dependency injection (DI)*. Nonostante non sia esattamente identico come concetto, DIP evita che i moduli di alto livello conoscano i dettagli dei moduli di basso livello pur utilizzandoli.
Uno dei benefici di questo utilizzo è che riduce la dipendenza tra due moduli.
La dipendenza tra due moduli è un concetto negativo, perchè ne rende difficile il refactor.
Come detto in precedenza, non essendoci il concetto di interfaccia in JavaScript, tutte le dipendenze sono contratte implicitamente.
Nell'esempio successivo, la dipendenza implicita è che tutte le istanze di `InventoryTracker` avranno un metodo `requestItems`.

**Da evitare**
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

    //Da evitare: abbiamo creato una dipendenza specifica per ogni istanza.
    //Dovremmo fare in modo che requestItems dipenda dal metodo `request`

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

**Bene:**
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

//Avendo dichiarato la nostra dipendenza esternamente ed aggiunta dall'esterno, la possiamo
//sostituire facilemente con un'altra che utilizza WebSockets

const inventoryTracker = new InventoryTracker(['apples', 'bananas'], new InventoryRequesterV2());
inventoryTracker.requestItems();
```
**[⬆ torna su](#lista-dei-contenuti)**

## **Test**
Test is more important than shipping. If you have no tests or an
inadequate amount, then every time you ship code you won't be sure that you
didn't break anything. Deciding on what constitutes an adequate amount is up
to your team, but having 100% coverage (all statements and branches) is how
you achieve very high confidence and developer peace of mind. This means that
in addition to having a great Test framework, you also need to use a
[good coverage tool](http://gotwarlost.github.io/istanbul/).

There's no excuse to not write tests. There are [plenty of good JS test frameworks](http://jstherightway.org/#Test-tools), so find one that your team prefers.
When you find one that works for your team, then aim to always write tests
for every new feature/module you introduce. If your preferred method is
Test Driven Development (TDD), that is great, but the main point is to just
make sure you are reaching your coverage goals before launching any feature,
or refactoring an existing one.

### Single concept per test

**Da evitare**
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

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**

## **Concurrency**
### Use Promises, not callbacks
Callbacks aren't clean, and they cause excessive amounts of nesting. With ES2015/ES6,
Promises are a built-in global type. Use them!

**Da evitare**
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

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**

### Async/Await are even cleaner than Promises
Promises are a very clean alternative to callbacks, but ES2017/ES8 brings async and await
which offer an even cleaner solution. All you need is a function that is prefixed
in an `async` keyword, and then you can write your logic imperatively without
a `then` chain of funzioni. Use this if you can take advantage of ES2017/ES8 features
today!

**Da evitare**
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

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**


## **Error Handling**
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

**Da evitare**
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Bene:**
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

**Da evitare**
```javascript
getdata()
  .then((data) => {
    functionThatMightThrow(data);
  })
  .catch((error) => {
    console.log(error);
  });
```

**Bene:**
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

**[⬆ torna su](#lista-dei-contenuti)**


## **Formatting**
Formatting is subjective. Like many rules herein, there is no hard and fast
rule that you must follow. The main point is DO NOT ARGUE over formatting.
There are [tons of tools](http://standardjs.com/rules.html) to automate this.
Use one! It's a waste of time and money for engineers to argue over formatting.

For things that don't fall under the purview of automatic formatting
(indentation, tabs vs. spaces, double vs. single quotes, etc.) look here
for some guidance.

### Use consistent capitalization
JavaScript is untyped, so capitalization tells you a lot about your Variabili,
funzioni, etc. These rules are subjective, so your team can choose whatever
they want. The point is, no matter what you all choose, just be consistent.

**Da evitare**
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

**Bene:**
```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const ARTISTS = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```
**[⬆ torna su](#lista-dei-contenuti)**


### Function callers and callees should be close
If a function calls another, keep those funzioni vertically close in the source
file. Ideally, keep the caller right above the callee. We tend to read code from
top-to-bottom, like a newspaper. Because of this, make your code read that way.

**Da evitare**
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

**Bene:**
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

**[⬆ torna su](#lista-dei-contenuti)**

## **Comments**
### Only comment things that have business logic complexity.
Comments are an apology, not a requirement. Good code *mostly* documents itself.

**Da evitare**
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

**Bene:**
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
**[⬆ torna su](#lista-dei-contenuti)**

### Don't leave commented out code in your codebase
Version control exists for a reason. Leave old code in your history.

**Da evitare**
```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Bene:**
```javascript
doStuff();
```
**[⬆ torna su](#lista-dei-contenuti)**

### Don't have journal comments
Remember, use version control! There's no need for dead code, commented code,
and especially journal comments. Use `git log` to get history!

**Da evitare**
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

**Bene:**
```javascript
function combine(a, b) {
  return a + b;
}
```
**[⬆ torna su](#lista-dei-contenuti)**

### Avoid positional markers
They usually just add noise. Let the funzioni and variable names along with the
proper indentation and formatting give the visual structure to your code.

**Da evitare**
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

**Bene:**
```javascript
$scope.model = {
  menu: 'foo',
  nav: 'bar'
};

const actions = function() {
  // ...
};
```
**[⬆ torna su](#lista-dei-contenuti)**

## Translation

This is also available in other languages:

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

**[⬆ torna su](#lista-dei-contenuti)**
