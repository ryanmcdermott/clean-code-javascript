# Clean Code para JavaScript

Traducción al español latinoamericano del documento original [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript) escrito por [Ryan McDermott](https://github.com/ryanmcdermott).

## Tabla de Contenidos

1. [Introducción](#introducción)
2. [Variables](#variables)
3. [Funciones](#funciones)
4. [Objetos y Estructuras de Datos](#objectos-y-estructuras-de-datos)
5. [Clases](#clases)
6. [SOLID](#solid)
7. [Pruebas](#pruebas)
8. [Concurrencia](#concurrencia)
9. [Manejo de Errores](#manejo-de-errores)
10. [Dando Formato](#dando-formato)
11. [Comentarios](#comentarios)
12. [Traducción](#traducción)

---

## Introducción

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

Los principios de la ingeniería del software del libro de Robert C. Martin
[_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
adaptados a JavaScript. Esto no es una guía de estilo. Es una guía para producir sotfware [legible, reutilizable y refactorizable](https://github.com/ryanmcdermott/3rs-of-software-architecture) en JavaScript.

No hay que seguir estrictamente todos los principios aquí expuestos, y aún menos serán universalmente aceptados. Se trata de una serie de guías y nada más, pero han sido codificadas a lo largo de muchos años de experiencia colectiva por los autores de _Clean Code_.

Nuestro oficio de ingeniero de software tiene poco más de 50 años, y todavía estamos aprendiendo mucho. Cuando la arquitectura de software sea tan antigua como la propia arquitectura, quizá entonces tengamos reglas más estrictas que seguir. Por ahora, dejemos que estas pautas sirvan como punto de referencia para evaluar la calidad del código JavaScript que tú y tu equipo produzcan.

Una cosa más: conocerlas no te convertirá inmediatamente en un mejor desarrollador de software, y trabajar con ellas durante muchos años no significa que no vayas a cometer errores. Cada pieza de código comienza como un primer borrador, como la arcilla húmeda que se moldea hasta alcanzar su forma final. Al final, cincelamos las imperfecciones cuando lo revisamos con nuestros compañeros. No te castigues por los primeros borradores que necesitan ser mejorados. En lugar de eso, castiga el código.

## **Variables**

### Usa nombres de variables significativos y pronunciables

**Incorrecto:**

```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
```

**Correcto:**

```javascript
const currentDate = moment().format('YYYY/MM/DD');
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Usa el mismo vocabulario para el mismo tipo de variable

**Incorrecto:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Correcto:**

```javascript
getUser();
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Usa nombres que se puedan buscar

Leeremos más código del que escribiremos. Es importante que el código que escribimos sea legible y se pueda buscar. Al _no_ nombrar las variables que acaban siendo significativas para entender nuestro programa, perjudicamos a nuestros lectores. Haz que tus nombres sean buscables. Herramientas como [buddy.js](https://github.com/danielstjules/buddy.js) y [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) pueden ayudarte a identificar las constantes sin nombre.

**Incorrecto:**

```javascript
// ¿Para qué diablos sirve 86400000?
setTimeout(blastOff, 86400000);
```

**Correcto:**

```javascript
// Decláralas como constantes con nombre en mayúsculas.
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Usa variables explicativas

**Incorrecto:**

```javascript
const address = 'Un bucle infinito, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(address.match(cityZipCodeRegex)[1], address.match(cityZipCodeRegex)[2]);
```

**Correcto:**

```javascript
const address = 'Un bucle infinito, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Evita los mapas mentales

Lo explícito es mejor que lo implícito.

**Incorrecto:**

```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach(l => {
	doStuff();
	doSomeOtherStuff();
	// ...
	// ...
	// ...
	// Espera, ¿para qué es la "l"?
	dispatch(l);
});
```

**Correcto:**

```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach(location => {
	doStuff();
	doSomeOtherStuff();
	// ...
	// ...
	// ...
	dispatch(location);
});
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### No añadas contexto innecesario

Si el nombre de tu clase/objeto te dice algo, no lo repitas en el nombre de tu variable.

**Incorrecto:**

```javascript
const Car = {
	carMake: 'Honda',
	carModel: 'Accord',
	carColor: 'Azul',
};

function paintCar(car, color) {
	car.carColor = color;
}
```

**Correcto:**

```javascript
const Car = {
	make: 'Honda',
	model: 'Accord',
	color: 'Azul',
};

function paintCar(car, color) {
	car.color = color;
}
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Usa parámetros por defecto en lugar de cortocircuitos o condicionales

Los parámetros por defecto suelen ser más limpios que los cortocircuitos. Ten en cuenta que si los utilizas, tu función sólo proporcionará valores por defecto para los argumentos `undefined`. Otros valores "falsos" como `''`, `""`, `false`, `null`, `0`, y `NaN`, no serán reemplazados por un valor por defecto.

**Incorrecto:**

```javascript
function createMicrobrewery(name) {
	const breweryName = name || 'Hipster Brew Co.';
	// ...
}
```

**Correcto:**

```javascript
function createMicrobrewery(name = 'Hipster Brew Co.') {
	// ...
}
```

**[⬆ volver arriba](#tabla-de-contenidos)**

## **Funciones**

### Argumentos de la función (idealmente 2 o menos)

Limitar la cantidad de parámetros de la función es increíblemente importante porque facilita la comprobación de la función. Tener más de tres conduce a una explosión combinatoria en la que tienes que probar toneladas de casos diferentes con cada argumento por separado.

Uno o dos argumentos es el caso ideal, y tres deben ser evitados si es posible. Todo lo que sea más que eso debe ser consolidado. Normalmente, si tienes más de dos argumentos, tu función está intentando hacer demasiado. En los casos en que no es así, la mayoría de las veces un objeto de nivel superior será suficiente como argumento.

Dado que JavaScript le permite crear objetos sobre la marcha, sin un montón de código de clase, puede utilizar un objeto si te encuentras con que necesitas un montón de argumentos.

Para hacer obvio qué propiedades espera la función, puedes utilizar la sintaxis de desestructuración de ES2015/ES6. Esto tiene algunas ventajas:

1. Cuando alguien mira la firma de la función, queda inmediatamente claro qué propiedades se están utilizando.
2. Se puede utilizar para simular parámetros con nombre.
3. La desestructuración también clona los valores primitivos especificados del objeto argumento pasado a la función. Esto puede ayudar a prevenir efectos secundarios. Nota: los objetos y arrays que se desestructuran a partir del objeto argumento NO son clonados.
4. Los linters pueden avisar de las propiedades no utilizadas, lo que sería imposible sin la desestructuración.

**Incorrecto:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
	// ...
}

createMenu('Foo', 'Bar', 'Baz', true);
```

**Correcto:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
	// ...
}

createMenu({
	title: 'Foo',
	body: 'Bar',
	buttonText: 'Baz',
	cancellable: true,
});
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Las funciones deben hacer una sola cosa

Esta es, con mucho, la regla más importante en la ingeniería de software. Cuando las funciones hacen más de una cosa, son más difíciles de componer, probar y razonar. Cuando puedes aislar una función a una sola acción, puede ser refactorizada fácilmente y tu código se leerá mucho más limpio. Si no te llevas nada más de esta guía que esto, estarás por delante de muchos desarrolladores.

**Incorrecto:**

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

**Correcto:**

```javascript
function emailActiveClients(clients) {
	clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
	º;
	const clientRecord = database.lookup(client);
	return clientRecord.isActive();
}
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Los nombres de las funciones deben decir lo que hacen

**Incorrecto:**

```javascript
function addToDate(date, month) {
	// ...
}

const date = new Date();

// Por el nombre de la función, es difícil saber lo que es agregado
addToDate(date, 1);
```

**Correcto:**

```javascript
function addMonthToDate(month, date) {
	// ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Las funciones deben tener sólo un nivel de abstracción

Cuando tienes más de un nivel de abstracción tu función suele hacer demasiado. Dividir las funciones conduce a la reutilización y a una prueba más fácil.

**Incorrecto:**

```javascript
function parseBetterJSAlternative(code) {
	const REGEXES = [
		// ...
	];

	const statements = code.split(' ');
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

**Correcto:**

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

	const statements = code.split(' ');
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

**[⬆ volver arriba](#tabla-de-contenidos)**

### Elimina el código duplicado

Haz todo lo posible por evitar el código duplicado. El código duplicado es malo porque significa que hay más de un lugar para alterar algo si necesitas cambiar alguna lógica.

Imagina que tienes un restaurante y llevas la cuenta de tu inventario: todos tus tomates, cebollas, ajos, especias, etc. Si tienes varias listas en las que guardas esto, entonces todas tienen que ser actualizadas cuando sirves un plato con tomates. Si sólo tienes una lista, ¡sólo hay que actualizarla en un sitio!

A menudo tienes código duplicado porque tienes dos o más cosas ligeramente diferentes, que comparten mucho en común, pero sus diferencias te obligan a tener dos o más funciones separadas que hacen mucho de las mismas cosas. Eliminar el código duplicado significa crear una abstracción que pueda manejar este conjunto de cosas diferentes con una sola función/módulo/clase.

Conseguir la abstracción correcta es fundamental, por lo que debes seguir los principios SOLID expuestos en la sección _Clases_. Las abstracciones incorrectas pueden ser peor que el código duplicado, ¡así que ten cuidado! Dicho esto, si puedes hacer una buena abstracción, ¡hazlo! No hagas repeticiones, de lo contrario te encontrarás actualizando en varios sitios cada vez que quieras cambiar una cosa.

**Incorrecto:**

```javascript
function showDeveloperList(developers) {
	developers.forEach(developer => {
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
	managers.forEach(manager => {
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

**Correcto:**

```javascript
function showEmployeeList(employees) {
	employees.forEach(employee => {
		const expectedSalary = employee.calculateExpectedSalary();
		const experience = employee.getExperience();

		const data = {
			expectedSalary,
			experience,
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

**[⬆ volver arriba](#tabla-de-contenidos)**

### Define los objetos por defecto con Object.assign

**Incorrecto:**

```javascript
const menuConfig = {
	title: null,
	body: 'Bar',
	buttonText: null,
	cancellable: true,
};

function createMenu(config) {
	config.title = config.title || 'Foo';
	config.body = config.body || 'Bar';
	config.buttonText = config.buttonText || 'Baz';
	config.cancellable = config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**Correcto:**

```javascript
const menuConfig = {
	title: 'Order',
	// El usuario no incluyó la key 'body'
	buttonText: 'Send',
	cancellable: true,
};

function createMenu(config) {
	let finalConfig = Object.assign(
		{
			title: 'Foo',
			body: 'Bar',
			buttonText: 'Baz',
			cancellable: true,
		},
		config
	);
	return finalConfig;
	// config ahora es igual a: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
	// ...
}

createMenu(menuConfig);
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### No utilices banderas como parámetros de la función

Las banderas indican al usuario que esta función hace más de una cosa. Las funciones deben hacer una sola cosa. Separa tus funciones si siguen diferentes rutas de código basadas en un booleano.

**Incorrecto:**

```javascript
function createFile(name, temp) {
	if (temp) {
		fs.create(`./temp/${name}`);
	} else {
		fs.create(name);
	}
}
```

**Correcto:**

```javascript
function createFile(name) {
	fs.create(name);
}

function createTempFile(name) {
	createFile(`./temp/${name}`);
}
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Evita los efectos secundarios (parte 1)

Una función produce un efecto secundario si hace algo más que tomar un valor y devolver otro valor o valores. Un efecto secundario puede ser escribir en un archivo, modificar alguna variable global, o transferir accidentalmente todo su dinero a un extraño.

Ahora bien, en ocasiones es necesario tener efectos secundarios en un programa. Como en el ejemplo anterior, podrías necesitar escribir en un archivo. Lo que quieres hacer es centralizar donde estás haciendo esto. No tengas varias funciones y clases que escriban en un archivo en particular. Ten un servicio que lo haga. Uno y sólo uno.

El punto principal es evitar errores comunes como compartir el estado entre objetos sin ninguna estructura, usar tipos de datos mutables que pueden ser escritos por cualquier cosa, y no centralizar donde ocurren tus efectos secundarios. Si puedes hacer esto, serás más feliz que la gran mayoría de los programadores.

**Incorrecto:**

```javascript
// Variable global referenciada por la siguiente función.
// Si tuviéramos otra función que usara este nombre, este ahora sería un array y podría romperla.
let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
	name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Correcto:**

```javascript
function splitIntoFirstAndLastName(name) {
	return name.split(' ');
}

const name = 'Ryan McDermott';
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Evita los efectos secundarios (parte 2)

En JavaScript, algunos valores son inalterables (inmutables) y otros son cambiables (mutables). Los objetos y los arrays son dos tipos de valores mutables, por lo que es importante manejarlos con cuidado cuando se pasan como parámetros a una función. Una función de JavaScript puede cambiar las propiedades de un objeto o alterar el contenido de un array, lo que podría causar fácilmente errores en otro lugar.

Supongamos que hay una función que acepta un parámetro de un array que representa un carrito de compras. Si la función hace un cambio en ese array del carrito de compras -añadiendo un artículo para comprar, por ejemplo- entonces cualquier otra función que utilice ese mismo array `cart` se verá afectada por esta adición. Eso puede ser genial; sin embargo, también podría ser malo. Imaginemos una mala situación:

El usuario hace clic en el botón "Comprar" que llama a una función de "compra" que genera una solicitud de red y envía un array de "carrito" al servidor. Debido a una mala conexión de red, la función `compra` tiene que seguir reintentando la petición. Ahora bien, ¿qué pasa si mientras tanto el usuario hace clic accidentalmente en el botón "Añadir a la cesta" en un artículo que no quiere realmente antes de que comience la solicitud de red? Si eso ocurre y la solicitud de red comienza, entonces esa función de compra enviará el artículo añadido accidentalmente porque el array `cart` fue modificado.

Una buena solución sería que la función `addItemToCart` clonara siempre el `cart`, lo editara y devolviera el clon. Esto aseguraría que las funciones que todavía están usando el antiguo carro de compras no se verían afectadas por los cambios.

Dos advertencias a mencionar para este enfoque:

1. Puede haber casos en los que realmente quieras modificar el objeto de entrada, pero cuando adoptes esta práctica de programación verás que esos casos son bastante raros. La mayoría de las cosas se pueden refactorizar para que no tengan efectos secundarios.

2. Clonar objetos grandes puede ser muy costoso en términos de rendimiento. Afortunadamente, esto no es un gran problema en la práctica porque hay [grandes librerías](https://facebook.github.io/immutable-js/) que permiten que este tipo de enfoque de programación sea rápido y no tan intensivo en memoria como lo sería para ti clonar manualmente objetos y arrays.

**Incorrecto:**

```javascript
const addItemToCart = (cart, item) => {
	cart.push({ item, date: Date.now() });
};
```

**Correcto:**

```javascript
const addItemToCart = (cart, item) => {
	return [...cart, { item, date: Date.now() }];
};
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### No escribas en funciones globales

Contaminar las funciones globales es una mala práctica en JavaScript porque podrías chocar con otra librería y el usuario de tu API no se daría cuenta hasta que se produjera una excepción en producción. Pensemos en un ejemplo: ¿qué pasaría si quisieras extender el método nativo Array de JavaScript para tener un método `diff` que pudiera mostrar la diferencia entre dos arrays? Podrías escribir tu nueva función en `Array.prototype`, pero podría chocar con otra librería que intentara hacer lo mismo. ¿Y si esa otra biblioteca sólo utilizara `diff` para encontrar la diferencia entre el primer y el último elemento de un array? Por eso sería mucho mejor utilizar las clases ES2015/ES6 y simplemente extender el `Array` global.

**Incorrecto:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
	const hash = new Set(comparisonArray);
	return this.filter(elem => !hash.has(elem));
};
```

**Correcto:**

```javascript
class SuperArray extends Array {
	diff(comparisonArray) {
		const hash = new Set(comparisonArray);
		return this.filter(elem => !hash.has(elem));
	}
}
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Favorece la programación funcional frente a la imperativa

JavaScript no es un lenguaje funcional como lo es Haskell, pero tiene un sabor funcional. Los lenguajes funcionales pueden ser más limpios y fáciles de probar. Favorece este estilo de programación siempre que puedas.

**Incorrecto:**

```javascript
const programmerOutput = [
	{
		name: 'Uncle Bobby',
		linesOfCode: 500,
	},
	{
		name: 'Suzie Q',
		linesOfCode: 1500,
	},
	{
		name: 'Jimmy Gosling',
		linesOfCode: 150,
	},
	{
		name: 'Gracie Hopper',
		linesOfCode: 1000,
	},
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
	totalOutput += programmerOutput[i].linesOfCode;
}
```

**Correcto:**

```javascript
const programmerOutput = [
	{
		name: 'Uncle Bobby',
		linesOfCode: 500,
	},
	{
		name: 'Suzie Q',
		linesOfCode: 1500,
	},
	{
		name: 'Jimmy Gosling',
		linesOfCode: 150,
	},
	{
		name: 'Gracie Hopper',
		linesOfCode: 1000,
	},
];

const totalOutput = programmerOutput.reduce(
	(totalLines, output) => totalLines + output.linesOfCode,
	0
);
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Encapsula los condicionales

**Incorrecto:**

```javascript
if (fsm.state === 'fetching' && isEmpty(listNode)) {
	// ...
}
```

**Correcto:**

```javascript
function shouldShowSpinner(fsm, listNode) {
	return fsm.state === 'fetching' && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
	// ...
}
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Evita los condicionales negativos

**Incorrecto:**

```javascript
function isDOMNodeNotPresent(node) {
	// ...
}

if (!isDOMNodeNotPresent(node)) {
	// ...
}
```

**Correcto:**

```javascript
function isDOMNodePresent(node) {
	// ...
}

if (isDOMNodePresent(node)) {
	// ...
}
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Evita los condicionales

Esto parece una tarea imposible. Al escuchar esto por primera vez, la mayoría de la gente dice: "¿cómo se supone que voy a hacer algo sin una declaración `if`?". La respuesta es que puedes usar el polimorfismo para lograr la misma tarea en muchos casos. La segunda pregunta suele ser: "bueno, eso es genial, pero ¿por qué querría hacer eso?". La respuesta es un concepto anterior de código limpio que aprendimos: una función sólo debe hacer una cosa. Cuando tienes clases y funciones que tienen declaraciones `if`, le estás diciendo a tu usuario que tu función hace más de una cosa. Recuerda, haz sólo una cosa.

**Incorrecto:**

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

**Correcto:**

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

**[⬆ volver arriba](#tabla-de-contenidos)**

### Evita la verificación de tipos (parte 1)

JavaScript es no tipado, lo que significa que tus funciones pueden tomar cualquier tipo de argumento. A veces, esta libertad te hace caer en la tentación de hacer comprobaciones de tipo en tus funciones. Hay muchas maneras de evitar tener que hacer esto. La primera cosa a considerar es la consistencia de las APIs.

**Incorrecto:**

```javascript
function travelToTexas(vehicle) {
	if (vehicle instanceof Bicycle) {
		vehicle.pedal(this.currentLocation, new Location('texas'));
	} else if (vehicle instanceof Car) {
		vehicle.drive(this.currentLocation, new Location('texas'));
	}
}
```

**Correcto:**

```javascript
function travelToTexas(vehicle) {
	vehicle.move(this.currentLocation, new Location('texas'));
}
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Evita la verificación de tipos (parte 2)

Si estás trabajando con valores primitivos básicos como strings y enteros, y no puedes usar el polimorfismo pero aún sientes la necesidad de comprobar el tipo, deberías considerar usar TypeScript. Esta es una excelente alternativa al JavaScript normal, ya que proporciona tipado estático sobre la sintaxis estándar de JavaScript. El problema de verificar manualmente el tipado en JavaScript normal es que hacerlo bien requiere tanta palabrería extra que la falsa "seguridad de tipado" que se obtiene no compensa la pérdida de legibilidad. Mantén tu JavaScript limpio, escribe buenas pruebas y haz buenas revisiones de código. Si no, haz todo eso pero con TypeScript (que, como he dicho, es una gran alternativa).

**Incorrecto:**

```javascript
function combine(val1, val2) {
	if (
		(typeof val1 === 'number' && typeof val2 === 'number') ||
		(typeof val1 === 'string' && typeof val2 === 'string')
	) {
		return val1 + val2;
	}

	throw new Error('Debe ser de tipo String o Number');
}
```

**Correcto:**

```javascript
function combine(val1, val2) {
	return val1 + val2;
}
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### No sobreoptimizar

Los navegadores modernos realizan una gran cantidad de optimizaciones en tiempo de ejecución. Muchas veces, si estás optimizando, estás perdiendo el tiempo. [Hay buenos recursos](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) para ver dónde falta optimización. Mientras tanto, hay que centrarse en ellos, hasta que se solucionen, si es posible.

**Incorrecto:**

```javascript
// En los navegadores antiguos, cada iteración con `list.length` sin caché sería costosa
// debido al recuento de `list.length`. En los navegadores modernos, esto está optimizado.
for (let i = 0, len = list.length; i < len; i++) {
	// ...
}
```

**Correcto:**

```javascript
for (let i = 0; i < list.length; i++) {
	// ...
}
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Eliminar el código muerto

El código muerto es tan malo como el código duplicado. No hay razón para mantenerlo en tu código base. Si no está siendo llamado, ¡deshazte de él! Seguirá estando a salvo en tu historial de versiones si todavía lo necesitas.

**Incorrecto:**

```javascript
function oldRequestModule(url) {
	// ...
}

function newRequestModule(url) {
	// ...
}

const req = newRequestModule;
inventoryTracker('manzanas', req, 'www.inventory-awesome.io');
```

**Correcto:**

```javascript
function newRequestModule(url) {
	// ...
}

const req = newRequestModule;
inventoryTracker('manzanas', req, 'www.inventory-awesome.io');
```

**[⬆ volver arriba](#tabla-de-contenidos)**

## **Objectos y Estructuras de Datos**

### Usa getters y setters

Utilizar getters y setters para acceder a los datos de los objetos podría ser mejor que simplemente buscar una propiedad en un objeto. "¿Por qué?", te preguntarás. Pues bien, aquí tienes una lista no ordenada de razones:

- Cuando quieres hacer algo más que obtener una propiedad de un objeto, no tienes que buscar y cambiar cada accesorio en tu código base.
- Hace que añadir validación sea sencillo cuando se hace un `set`.
- Encapsula la representación interna.
- Fácil de añadir registro y manejo de errores al hacer getting y setting.
- Puedes cargar las propiedades de tu objeto de forma perezosa, digamos que lo obtienes de un servidor.

**Incorrecto:**

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

**Correcto:**

```javascript
function makeBankAccount() {
	// este es privado
	let balance = 0;

	// un "getter", hecho público a través del objeto devuelto a continuación
	function getBalance() {
		return balance;
	}

	// un "setter", hecho público a través del objeto devuelto a continuación
	function setBalance(amount) {
		// ... validar antes de actualizar el saldo
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

**[⬆ volver arriba](#tabla-de-contenidos)**

### Haz que los objetos tengan elementos privados

Esto se puede lograr a través de closures (para ES5 e inferiores).

**Incorrecto:**

```javascript
const Employee = function (name) {
	this.name = name;
};

Employee.prototype.getName = function getName() {
	return this.name;
};

const employee = new Employee('John Doe');
console.log(`Nombre del empleado: ${employee.getName()}`); // Nombre del empleado: John Doe
delete employee.name;
console.log(`Nombre del empleado: ${employee.getName()}`); // Nombre del empleado: undefined
```

**Correcto:**

```javascript
function makeEmployee(name) {
	return {
		getName() {
			return name;
		},
	};
}

const employee = makeEmployee('John Doe');
console.log(`Nombre del empleado: ${employee.getName()}`); // Nombre del empleado: John Doe
delete employee.name;
console.log(`Nombre del empleado: ${employee.getName()}`); // Nombre del empleado: John Doe
```

**[⬆ volver arriba](#tabla-de-contenidos)**

## **Clases**

### Prefiere las clases de ES2015/ES6 a las funciones simples de ES5

Es muy difícil conseguir una herencia de clases, una construcción y unas definiciones de métodos legibles para las clases clásicas de ES5. Si necesitas herencia (y ten en cuenta que puede que no la necesites), entonces prefiere las clases ES2015/ES6. Sin embargo, prefiere las funciones pequeñas a las clases hasta que te encuentres con la necesidad de objetos más grandes y complejos.

**Incorrecto:**

```javascript
const Animal = function (age) {
	if (!(this instanceof Animal)) {
		throw new Error('Instanciar Animal con `new`');
	}

	this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function (age, furColor) {
	if (!(this instanceof Mammal)) {
		throw new Error('Instanciar Mamífero con `new`.');
	}

	Animal.call(this, age);
	this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function (age, furColor, languageSpoken) {
	if (!(this instanceof Human)) {
		throw new Error('Instanciar Humano con `new`.');
	}

	Mammal.call(this, age, furColor);
	this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**Correcto:**

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

**[⬆ volver arriba](#tabla-de-contenidos)**

### Usa el encadenamiento de métodos

Este patrón es muy útil en JavaScript y se ve en muchas librerías como jQuery y Lodash. Permite que tu código sea expresivo y menos verboso. Por esa razón, digo, utiliza el encadenamiento de métodos y fíjate en lo limpio que será tu código. En tus funciones de clase, simplemente devuelve `this` al final de cada función, y puedes encadenar más métodos de clase sobre ella.

**Incorrecto:**

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

const car = new Car('Ford', 'F-150', 'red');
car.setColor('pink');
car.save();
```

**Correcto:**

```javascript
class Car {
	constructor(make, model, color) {
		this.make = make;
		this.model = model;
		this.color = color;
	}

	setMake(make) {
		this.make = make;
		// NOTA: Devuelve esto para el encadenamiento
		return this;
	}

	setModel(model) {
		this.model = model;
		// NOTA: Devuelve esto para el encadenamiento
		return this;
	}

	setColor(color) {
		this.color = color;
		// NOTA: Devuelve esto para el encadenamiento
		return this;
	}

	save() {
		console.log(this.make, this.model, this.color);
		// NOTA: Devuelve esto para el encadenamiento
		return this;
	}
}

const car = new Car('Ford', 'F-150', 'red').setColor('pink').save();
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Prefiera la composición a la herencia

Como se afirma en el famoso libro [_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns) de Gang of Four, debes preferir la composición a la herencia siempre que puedas. Hay muchas buenas razones para usar la herencia y muchas buenas razones para usar la composición. El punto más importante de esta máxima es que si tu mente se inclina instintivamente por la herencia, intenta pensar si la composición podría modelar mejor tu problema. En algunos casos puede hacerlo.

Te preguntarás entonces, "¿cuándo debo usar la herencia?". Depende de tu problema en cuestión, pero esta es una lista decente de cuando la herencia tiene más sentido que la composición:

1. Tu herencia representa una relación "es-un" y no una relación "tiene-un" (Human->Animal vs. User->UserDetails).
2. Puedes reutilizar código de las clases base (Los humanos pueden moverse como todos los animales).
3. Quieres hacer cambios globales en las clases derivadas cambiando una clase base. (Cambiar el gasto calórico de todos los animales cuando se mueven).

**Incorrecto:**

```javascript
class Employee {
	constructor(name, email) {
		this.name = name;
		this.email = email;
	}

	// ...
}

// Incorrecto porque Employees "tiene" datos fiscales. EmployeeTaxData no es un tipo de Employee
class EmployeeTaxData extends Employee {
	constructor(ssn, salary) {
		super();
		this.ssn = ssn;
		this.salary = salary;
	}

	// ...
}
```

**Correcto:**

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

**[⬆ volver arriba](#tabla-de-contenidos)**

## **SOLID**

### Principio de la Responsabilidad Individual (SRP)

Como se dice en Clean Code, "Nunca debería haber más de una razón para que una clase cambie". Es tentador llenar una clase con mucha funcionalidad, como cuando sólo puedes llevar una maleta en tu vuelo. El problema con esto es que la clase no será conceptualmente cohesiva y le dará muchas razones para cambiar. Es importante minimizar la cantidad de veces que tiene que cambiar una clase. Es importante porque si hay demasiada funcionalidad en una clase y modificas una parte de ella, puede ser difícil entender cómo eso afectará a otros módulos dependientes en tu código base.

**Incorrecto:**

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

**Correcto:**

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

**[⬆ volver arriba](#tabla-de-contenidos)**

### Principio de Apertura/Cierre (OCP)

Según Bertrand Meyer, "las entidades de software (clases, módulos, funciones, etc.) deben estar abiertas para su ampliación, pero cerradas para su modificación". ¿Pero qué significa esto? Este principio establece básicamente que se debe permitir a los usuarios añadir nuevas funcionalidades sin cambiar el código existente.

**Incorrecto:**

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
			return makeAjaxCall(url).then(response => {
				// transformar la respuesta y retornarla
			});
		} else if (this.adapter.name === 'nodeAdapter') {
			return makeHttpCall(url).then(response => {
				// transformar la respuesta y retornarla
			});
		}
	}
}

function makeAjaxCall(url) {
	// solicitar y retornar la promesa
}

function makeHttpCall(url) {
	// solicitar y retornar la promesa
}
```

**Correcto:**

```javascript
class AjaxAdapter extends Adapter {
	constructor() {
		super();
		this.name = 'ajaxAdapter';
	}

	request(url) {
		// solicitar y retornar la promesa
	}
}

class NodeAdapter extends Adapter {
	constructor() {
		super();
		this.name = 'nodeAdapter';
	}

	request(url) {
		// solicitar y retornar la promesa
	}
}

class HttpRequester {
	constructor(adapter) {
		this.adapter = adapter;
	}

	fetch(url) {
		return this.adapter.request(url).then(response => {
			// transformar la respuesta y retornarla
		});
	}
}
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Principio de Sustitución de Liskov (LSP)

Se trata de un término aterrador para un concepto muy simple. Se define formalmente como "Si S es un subtipo de T, entonces los objetos del tipo T pueden ser sustituidos por objetos del tipo S (es decir, los objetos del tipo S pueden sustituir a los del tipo T) sin alterar ninguna de las propiedades deseables de ese programa (corrección, tarea realizada, etc.)". Esa es una definición aún más aterradora.

La mejor explicación para esto es que si tienes una clase padre y una clase hija, entonces la clase base y la clase hija pueden ser usadas indistintamente sin obtener resultados incorrectos. Esto puede seguir siendo confuso, así que echemos un vistazo al clásico ejemplo del cuadrado y el rectángulo. Matemáticamente, un cuadrado es un rectángulo, pero si se modela utilizando la relación "es-un" a través de la herencia, rápidamente se entra en problemas.

**Incorrecto:**

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
		const area = rectangle.getArea(); // MAL: Retorna 25 por Cuadrado. Debería ser 20.
		rectangle.render(area);
	});
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Correcto:**

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

**[⬆ volver arriba](#tabla-de-contenidos)**

### Principio de Segregación de Interfaces (ISP)

JavaScript no tiene interfaces, por lo que este principio no se aplica tan estrictamente como otros. Sin embargo, es importante y relevante incluso con la falta de sistema de tipos de JavaScript.

El ISP establece que "los clientes no deben ser forzados a depender de interfaces que no utilizan". Las interfaces son contratos implícitos en JavaScript debido a la escritura de tipos.

Un buen ejemplo que demuestra este principio en JavaScript es el de las clases que requieren grandes objetos de configuración. No exigir a los clientes que configuren grandes cantidades de opciones es beneficioso, porque la mayoría de las veces no necesitarán todas las configuraciones. Hacerlos opcionales ayuda a evitar tener una "interfaz pesada".

**Incorrecto:**

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
	rootNode: document.getElementsByTagName('body'),
	animationModule() {}, // La mayoría de las veces, no necesitaremos animar durante el desplazamiento.
	// ...
});
```

**Correcto:**

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
		animationModule() {},
	},
});
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Principio de Inversión de la Dependencia (DIP)

Este principio establece dos cosas esenciales

1. Los módulos de alto nivel no deben depender de los de bajo nivel. Ambos deben depender de abstracciones.
2. Las abstracciones no deben depender de los detalles. Los detalles deberían depender de las abstracciones.

Esto puede ser difícil de entender al principio, pero si has trabajado con AngularJS, habrás visto una implementación de este principio en forma de Inyección de Dependencia (DI). Aunque no son conceptos idénticos, DI evita que los módulos de alto nivel conozcan los detalles de sus módulos de bajo nivel y los configuren. Se puede conseguir esto a través de DI. Un gran beneficio de esto es que reduce el emparejamiento entre módulos. El emparejamiento es un patrón de desarrollo muy malo porque hace que tu código sea difícil de refactorizar.

Como se ha dicho anteriormente, JavaScript no tiene interfaces por lo que las abstracciones de las que se depende son contratos implícitos. Es decir, los métodos y propiedades que un objeto/clase expone a otro objeto/clase. En el ejemplo siguiente, el contrato implícito es que cualquier módulo Request para un `InventoryTracker` tendrá un método `requestItems`.

**Incorrecto:**

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

		// MAL: Hemos creado una dependencia en la implementación de una solicitud específica.
		// Deberíamos hacer que los requestItems dependan de un método de petición: `request`.
		this.requester = new InventoryRequester();
	}

	requestItems() {
		this.items.forEach(item => {
			this.requester.requestItem(item);
		});
	}
}

const inventoryTracker = new InventoryTracker(['manzanas', 'bananas']);
inventoryTracker.requestItems();
```

**Correcto:**

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

// Construyendo nuestras dependencias externamente e inyectándolas, podemos fácilmente
// sustituir nuestro módulo request por uno nuevo que utilice WebSockets.
const inventoryTracker = new InventoryTracker(
	['manzanas', 'bananas'],
	new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ volver arriba](#tabla-de-contenidos)**

## **Pruebas**

Las pruebas son más importantes que el despliegue. Si no tienes pruebas o una cantidad inadecuada, entonces cada vez que envíes el código no estarás seguro de que no has roto nada. Decidir lo que constituye una cantidad adecuada depende de tu equipo, pero tener una cobertura del 100% (todas las declaraciones y ramas) es la forma de lograr una confianza muy alta y la paz mental del desarrollador. Esto significa que además de tener un gran framework de pruebas, también necesitas usar una [buena herramienta de cobertura](https://gotwarlost.github.io/istanbul/).

No hay excusa para no escribir pruebas. Hay [un montón de buenos frameworks de pruebas JS](https://jstherightway.org/#testing-tools), así que encuentra uno que tu equipo prefiera. Cuando encuentres uno que funcione para tu equipo, entonces intenta escribir siempre pruebas para cada nueva característica/módulo que introduzcas. Si tu método preferido es el Desarrollo Dirigido por Pruebas (TDD), es estupendo, pero el punto principal es simplemente asegurarse de que estás alcanzando tus objetivos de cobertura antes de lanzar cualquier característica, o refactorizar una existente.

### Un solo concepto por prueba

**Incorrecto:**

```javascript
import assert from 'assert';

describe('MomentJS', () => {
	it('handles date boundaries', () => {
		let date;

		date = new MomentJS('1/1/2015');
		date.addDays(30);
		assert.equal('1/31/2015', date);

		date = new MomentJS('2/1/2016');
		date.addDays(28);
		assert.equal('02/29/2016', date);

		date = new MomentJS('2/1/2015');
		date.addDays(28);
		assert.equal('03/01/2015', date);
	});
});
```

**Correcto:**

```javascript
import assert from 'assert';

describe('MomentJS', () => {
	it('handles 30-day months', () => {
		const date = new MomentJS('1/1/2015');
		date.addDays(30);
		assert.equal('1/31/2015', date);
	});

	it('handles leap year', () => {
		const date = new MomentJS('2/1/2016');
		date.addDays(28);
		assert.equal('02/29/2016', date);
	});

	it('handles non-leap year', () => {
		const date = new MomentJS('2/1/2015');
		date.addDays(28);
		assert.equal('03/01/2015', date);
	});
});
```

**[⬆ volver arriba](#tabla-de-contenidos)**

## **Concurrencia**

### Usa promesas, no callbacks

Los callbacks no son limpios y provocan un anidamiento excesivo. Con ES2015/ES6, las promesas son un tipo global integrado. Utilízalas.

**Incorrecto:**

```javascript
import { get } from 'request';
import { writeFile } from 'fs';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', (requestErr, response, body) => {
	if (requestErr) {
		console.error(requestErr);
	} else {
		writeFile('article.html', body, writeErr => {
			if (writeErr) {
				console.error(writeErr);
			} else {
				console.log('Archivo escrito');
			}
		});
	}
});
```

**Correcto:**

```javascript
import { get } from 'request-promise';
import { writeFile } from 'fs-extra';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
	.then(body => {
		return writeFile('article.html', body);
	})
	.then(() => {
		console.log('Archivo escrito');
	})
	.catch(err => {
		console.error(err);
	});
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Async/Await son aún más limpios que las Promesas

Las promesas son una alternativa muy limpia a los callbacks, pero ES2017/ES8 trae async y await que ofrecen una solución aún más limpia. Todo lo que necesitas es una función que esté prefijada en una palabra clave `async`, y entonces puedes escribir tu lógica imperativamente sin una cadena de funciones `then`. ¡Utiliza esto si puedes aprovechar las características de ES2017/ES8 hoy mismo!

**Incorrecto:**

```javascript
import { get } from 'request-promise';
import { writeFile } from 'fs-extra';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
	.then(body => {
		return writeFile('article.html', body);
	})
	.then(() => {
		console.log('Archivo escrito');
	})
	.catch(err => {
		console.error(err);
	});
```

**Correcto:**

```javascript
import { get } from 'request-promise';
import { writeFile } from 'fs-extra';

async function getCleanCodeArticle() {
	try {
		const body = await get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin');
		await writeFile('article.html', body);
		console.log('Archivo escrito');
	} catch (err) {
		console.error(err);
	}
}

getCleanCodeArticle();
```

**[⬆ volver arriba](#tabla-de-contenidos)**

## **Manejo de Errores**

¡Los errores lanzados son algo bueno! Significan que el tiempo de ejecución ha identificado con éxito cuando algo en tu programa ha ido mal y te lo hace saber deteniendo la ejecución de la función en la pila actual, matando el proceso (en Node), y notificándote en la consola con un seguimiento de la pila.

### No ignores los errores detectados

No hacer nada con un error detectado no te da la posibilidad de arreglar o reaccionar ante dicho error. Registrar el error en la consola (`console.log`) no es mucho mejor ya que a menudo puede perderse en un mar de cosas impresas en la consola. Si envuelves cualquier trozo de código en un `try/catch` significa que crees que un error puede ocurrir allí y por lo tanto debes tener un plan, o crear una ruta de código, para cuando ocurra.

**Incorrecto:**

```javascript
try {
	functionThatMightThrow();
} catch (error) {
	console.log(error);
}
```

**Correcto:**

```javascript
try {
	functionThatMightThrow();
} catch (error) {
	// Una opción (más ruidosa que console.log):
	console.error(error);
	// Otra opción:
	notifyUserOfError(error);
	// Otra opción:
	reportErrorToService(error);
	// O hacer las tres cosas.
}
```

### No ignores las promesas rechazadas

Por la misma razón no debes ignorar los errores capturados por `try/catch`.

**Incorrecto:**

```javascript
getdata()
	.then(data => {
		functionThatMightThrow(data);
	})
	.catch(error => {
		console.log(error);
	});
```

**Correcto:**

```javascript
getdata()
	.then(data => {
		functionThatMightThrow(data);
	})
	.catch(error => {
		// Una opción (más ruidosa que console.log):
		console.error(error);
		// Otra opción:
		notifyUserOfError(error);
		// Otra opción:
		reportErrorToService(error);
		// O hacer las tres cosas.
	});
```

**[⬆ volver arriba](#tabla-de-contenidos)**

## **Dando Formato**

El hecho de dar formato es subjetivo. Al igual que muchas otras reglas, no hay una regla rígida que deba seguirse. El punto principal es NO ARGUMENTAR el formato. Hay [toneladas de herramientas](https://standardjs.com/rules.html) para automatizar esto. Utiliza una. Es una pérdida de tiempo y dinero que los ingenieros discutan sobre el formato.

Para las cosas que no entran en el ámbito del formato automático (sangría, tabulaciones frente a espacios, comillas dobles frente a simples, etc.) mira aquí para obtener algunas orientaciones.

### Usa las mayúsculas de forma consistente

JavaScript no tiene tipografía, por lo que las mayúsculas dicen mucho sobre tus variables, funciones, etc. Estas reglas son subjetivas, así que tu equipo puede elegir lo que quiera. El punto es que, no importa lo que elijan, sólo sean consistentes.

**Incorrecto:**

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

**Correcto:**

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

**[⬆ volver arriba](#tabla-de-contenidos)**

### Los llamadores de funciones y las llamadas deben estar cerca

Si una función llama a otra, mantén esas funciones verticalmente cerca en el archivo fuente. Lo ideal es que el llamador esté justo encima del llamado. Tendemos a leer el código de arriba a abajo, como un periódico. Por eso, haz que tu código se lea así.

**Incorrecto:**

```javascript
class PerformanceReview {
	constructor(employee) {
		this.employee = employee;
	}

	lookupPeers() {
		return db.lookup(this.employee, 'colegas');
	}

	lookupManager() {
		return db.lookup(this.employee, 'gerente');
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

**Correcto:**

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
		return db.lookup(this.employee, 'colegas');
	}

	getManagerReview() {
		const manager = this.lookupManager();
	}

	lookupManager() {
		return db.lookup(this.employee, 'gerente');
	}

	getSelfReview() {
		// ...
	}
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**[⬆ volver arriba](#tabla-de-contenidos)**

## **Comentarios**

### Sólo comenta las cosas que tienen lógica de funcionamiento compleja

Los comentarios son una disculpa, no un requisito. El código correcto _casi_ se documenta a sí mismo.

**Incorrecto:**

```javascript
function hashIt(data) {
	// El hash
	let hash = 0;

	// Longitud del string
	const length = data.length;

	// Recorrer cada carácter de los datos
	for (let i = 0; i < length; i++) {
		// Obtener el código de caracteres.
		const char = data.charCodeAt(i);
		// Hacer el hash
		hash = (hash << 5) - hash + char;
		// Convertir a entero de 32 bits
		hash &= hash;
	}
}
```

**Correcto:**

```javascript
function hashIt(data) {
	let hash = 0;
	const length = data.length;

	for (let i = 0; i < length; i++) {
		const char = data.charCodeAt(i);
		hash = (hash << 5) - hash + char;

		// Convertir a entero de 32 bits
		hash &= hash;
	}
}
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### No dejes código comentado en tu base de código

El control de versiones existe por una razón. Deja el código antiguo en tu historial.

**Incorrecto:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Correcto:**

```javascript
doStuff();
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### No tengas un diario de comentarios

Recuerda, ¡utiliza el control de versiones! No hay necesidad de código muerto, código comentado, y especialmente un diario de comentarios. ¡Usa `git log` para obtener el historial!

**Incorrecto:**

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

**Correcto:**

```javascript
function combine(a, b) {
	return a + b;
}
```

**[⬆ volver arriba](#tabla-de-contenidos)**

### Evite los marcadores de posición

Normalmente sólo añaden ruido. Deja que las funciones y los nombres de las variables, junto con la sangría y el formato adecuados, den la estructura visual a tu código.

**Incorrecto:**

```javascript
////////////////////////////////////////////////////////////////////////////////
// Instanciación del modelo de ámbito
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
	menu: 'foo',
	nav: 'bar',
};

////////////////////////////////////////////////////////////////////////////////
// Configuración de la acción
////////////////////////////////////////////////////////////////////////////////
const actions = function () {
	// ...
};
```

**Correcto:**

```javascript
$scope.model = {
	menu: 'foo',
	nav: 'bar',
};

const actions = function () {
	// ...
};
```

**[⬆ volver arriba](#tabla-de-contenidos)**

## Traducción

También está disponible en otros idiomas:

- ![am](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Armenia.png) **Armenio**: [hanumanum/clean-code-javascript/](https://github.com/hanumanum/clean-code-javascript)
- ![bd](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bangladesh.png) **Bangla(বাংলা)**: [InsomniacSabbir/clean-code-javascript/](https://github.com/InsomniacSabbir/clean-code-javascript/)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portugués Brasilero**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chino Simplificado**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chino Tradicional**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **Francés**: [eugene-augier/clean-code-javascript-fr](https://github.com/eugene-augier/clean-code-javascript-fr)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **Alemán**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Indonesio**: [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italiano**: [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japonés**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Coreano**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polaco**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Ruso**:
  - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
  - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Español (España)**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Venezuela.png) **Español Latino** [drfcozapata/clean-code-javascript-es](https://github.com/drfcozapata/clean-code-javascript-es)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Español (Uruguay)**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![rs](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Serbia.png) **Serbio**: [doskovicmilos/clean-code-javascript/](https://github.com/doskovicmilos/clean-code-javascript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turco**: [bsonmez/clean-code-javascript](https://github.com/bsonmez/clean-code-javascript/tree/turkish-translation)
- ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ucraniano**: [mindfr1k/clean-code-javascript-ua](https://github.com/mindfr1k/clean-code-javascript-ua)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamita**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)

**[⬆ volver arriba](#tabla-de-contenidos)**
