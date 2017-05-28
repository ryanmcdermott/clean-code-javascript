# clean-code-javascript (Bahasa Indonesia)

## Daftar Isi
  1. [Kata Pengantar](#kata-pengantar)
  2. [Variabel](#variabel)
  3. [Fungsi](#fungsi)
  4. [Objek dan Struktur Data](#objek-dan-struktur-data)
  5. [Class](#class)
  6. [SOLID](#solid)
  7. [Pengujian](#pengujian)
  8. [Concurrency](#concurrency)
  9. [Penanganan Kesalahan](#penanganan-kesalahan)
  10. [Menyusun Format](#menyusun-format)
  11. [Komentar](#komentar)
  12. [Alih Bahasa](#alih-bahasa)

## Kata Pengantar
![Gambar lucu dari estimasi kualitas perangkat lunak sebagai hitungan berapa
banyak umpatan yang keluar saat membaca kode](http://www.osnews.com/images/comics/wtfm.jpg)

Prinsip Rekayasa Perangkat Lunak, oleh Robert C. Buku
[*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) oleh Martin,
diadaptasi untuk JavaScript. Perlu diingat bahwa panduan ini bukan tentang gaya
penulisan kode. Panduan ini lebih tentang membuat perangkat lunak yg [mudah dibaca,
dapat digunakan kembali, dan mudah direfaktor kembali](https://github.com/ryanmcdermott/3rs-of-software-architecture)
dalam bahasa Javascript.

Tidak semua prinsip disini harus diikuti secara ketat, dan bahkan lebih sedikit
akan lebih mudah disepakati bersama. Ini hanyalah panduan-panduan saja dan tidak
lebih, tetapi panduan-panduan ini dikodifikiasi selama bertahun-tahun dari
pengalaman kolektif penulis buku *Clean Code*.

Rekayasa-rekayasa perangkat lunak kita baru berumur 50 tahun lebih sedikit. dan
kita masih belajar banyak. Ketika arsitektur perangkat lunak setua arsitektur itu
sendiri, mungkin kita memiliki aturan yang lebih ketat untuk diikuti. Untuk saat
ini, biarkan panduan ini berfungsi sebagai batu ujian (touchstone) untuk mengukur
kualitas kode JavaScript yang kamu dan tim kamu buat.

Satu hal lagi: mengetahui hal ini bukan berarti membuat kamu menjadi seorang
pengembang software (developer) yg baik secara tiba-tiba, dan bekerja dengannya
bukan berarti kamu tidak akan membuat kesalahan-kesalahan. Setiap potongan kode
dimulai sebagai draft pertama, seperti tanah liat basah yg terus dibentuk menjadi
bentuk terakhirnya. Akhirnya, kita memahat ketidaksempurnaan ketika kita mereview
dengan teman dalam tim kita. Apabila draft pertamamu butuh perbaikan, jangan
pukul dirimu sendiri. Pukul Kodenya !

## **Variabel**
### Gunakan nama-nama variabel yg bermakna dan mudah diucapkan

**Buruk:**
```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
```

**Baik:**
```javascript
const currentDate = moment().format('YYYY/MM/DD');
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Gunakan kosakata yang sama untuk jenis variabel yang sama

**Buruk:**
```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Baik:**
```javascript
getUser();
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Gunakan nama yg mudah dicari kembali
Kita akan lebih banyak membaca kode daripada menulisnya. Maka penting hukumnya
menulis kode untuk mudah dibaca dan mudah dicari. Dengan *tidak* memberi nama
variabel yang bermakna untuk memahami program kita, kita menyakiti pembaca kode
kita. Buat nama variabel lebih mudah dicari. Alat seperti

[buddy.js](https://github.com/danielstjules/buddy.js) dan
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
dapat membantu mengidentifikasi konstanta yg tidak diberi nama.

**Buruk:**
```javascript
// Nah kan, 86400000 itu untuk apa?
setTimeout(blastOff, 86400000);

```

**Baik:**
```javascript
// tulis `const` dalam huruf kapital dan secara global
const MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);

```
**[⬆ Kembali ke atas](#daftar-isi)**

### Gunakan variabel penjelas
**Buruk:**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(address.match(cityZipCodeRegex)[1], address.match(cityZipCodeRegex)[2]);
```

**Baik:**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Hindari Mental Mapping
Explisit lebih baik daripada implisit.

**Buruk:**
```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((l) => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Sebentar, `l` itu untuk apa lagi?
  dispatch(l);
});
```

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**

### Jangan menambahkan konteks yang tidak dibutuhkan
Jika nama class/object kamu memiliki arti tertentu, jangan mengulanginya di dalam
nama variabel.

**Buruk:**
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

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**

### Gunakan argumen default daripada `short circuiting` dan `kondisional`
Default arguments are often cleaner than short circuiting. Be aware that if you
use them, your function will only provide default values for `undefined`
arguments. Other "falsy" values such as `''`, `""`, `false`, `null`, `0`, and
`NaN`, will not be replaced by a default value.

Argumen default lebih bersih daripada `shoor circuiting`. Sadarilah jika kamu
menggunakannya, fungsimu hanya akan memberikan nilai default untuk argumen `undefined`.
Selain nilai "falsy" seperti `''`, `""`, `false`, `null`, `0`, dan `NaN`, tidak
digantikan oleh nilai default.

**Buruk:**
```javascript
function createMicrobrewery(name) {
  const breweryName = name || 'Hipster Brew Co.';
  // ...
}

```

**Baik:**
```javascript
function createMicrobrewery(breweryName = 'Hipster Brew Co.') {
  // ...
}

```
**[⬆ Kembali ke atas](#daftar-isi)**

## **Fungsi**
### Argumen Fungsi (idealnya hanya 2 atau kurang)
Membatasi jumlah parameter dari fungsi sangatlah penting karena membuat pengujian
fungsi menjadi lebih mudah. Memiliki lebih dari 3 parameter dapat mengakibatkan
ledakan kombinasional dimana kamu harus menguji banyak sekali kasus-kasus yg
berbeda di tiap-tiap argumen yg terpisah.

Satu atau dua argumen adalah kasus yg ideal, jika tiga maka sebaiknya dihindari.
Apapun lebih dari itu harus dikonsolidasikan. Biasanya, jika kamu memiliki lebih
dari dua argumen maka fungsimu melakukan hal yg terlalu banyak. Di beberapa kasus
dimana hal itu tidak terjadi, seringkali higher-level object akan cukup menjadi
sebuah argumen.

JavaScript memperbolehkanmu untuk membuat object secara langsung, tanpa membuat
banyak class terlebih dahulu. kamu dapat menggunakan sebuah objek jika kamu
mendapatibahwa kamu memerlukan banyak argumen.

To make it obvious what properties the function expects, you can use the ES2015/ES6
destructuring syntax. This has a few advantages:

Lebih jelasnya, kamu dapat menggunakan destructuring syntax pada ES2015/ES6. Hal
tersebut memiliki beberapa kelebihan:

1. Ketika seseorang melihat fungsi, fungsi tersebut secara langsung menjelaskan
properti-properti apa saja yg digunakan.
2. Merestrukturisasi juga mengkloning nilai primitif yang ditentukan dari objek
argumen yang dilewatkan ke fungsi. Hal ini dapat membantu mencegah efek samping.
Sebagai Catatan: objek dan array yg telah didestrukturisasi dari argumen objek
tidak di kloning.
3. Linters dapat memberi peringatan kepadamu tentang properti yg tidak digunakan,
yg mana akan lebih tidak mungkin tanpa merestrukturisasi.

**Buruk:**
```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**


### Fungsi harus melakukan satu hal saja
This is by far the most important rule in software engineering. When functions
do more than one thing, they are harder to compose, test, and reason about.
When you can isolate a function to just one action, they can be refactored
easily and your code will read much cleaner. If you take nothing else away from
this guide other than this, you'll be ahead of many developers.

Ini adalah aturan terpenting dalam rekayasa perangkat lunak. Ketika fungsi melakukan
lebih dari satu hal, mereka sulit untuk dibuat, diuji dan dipikirkan. Ketika kamu
dapat mengisolasi sebuah fungsi untuk melakukan satu action saja, mereka dapat
difaktorkan secara mudah dan kodemu akan jauh lebih bersih. Jika kamu tidak mengambil
apapun dari panduan ini selain hal ini, maka anda akan ada di depan banyak pengembang.

**Buruk:**
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

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**

### Nama dari fungsi harus mewakili apa yg dia lakukan

**Buruk:**
```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// sulit untuk mengatakan dari nama fungsi yg ditambahkan
addToDate(date, 1);
```

**Baik:**
```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Fungsi-fungsi seharusnya satu tingkat abstraksi
Ketika kamu memiliki lebih dari satu tingkat abstraksi, fungsimu biasanya melakukan
hal yg terlalu banyak. Memisah-misah fungsi dapat meningkatkan reusabilitas dan
memudahkan pengujian.

**Buruk:**
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

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**

### Hapus kode yg duplikat
Lakukan yg terbaik untuk menghindari kode yg duplikat. Kode yg duplikat itu Buruk
karena berarti terdapat lebih dari satu tempat untuk mengubah sesuatu jika kamu
perlu mengubah beberapa logic.

Bayangkan jika amu memiliki sebuah restoran dan kamu harus terus melacak inventarismu:
tomat, bawang, cabe, dan lain-lain. Jika kamu memiliki beberapa daftar yg kamu
simpan, maka kamu harus mengubah itu semua jika kamu menyajikan hidangan yg ada
tomatnya. Jika kamu hanya memiliki satu daftar saja, maka kamu hanya perlu mengubah
di satu tempat saja.

Seringkali kamu memiliki kode yg duplikat karena kamu memiliki dua atau lebih
hal-hal yg sedikit berbeda, memiliki banyak hal yg sama, namun perbedaan itu
memaksamu untuk memisahkan dua atau lebih fungsi yg berbeda namun kebanyakan
melakukan hal yg sama. Membuang kode yg duplikat berarti membuat sebuah
abstraksi yg dapat menangani hal yg berbeda dengan satu fungsi/modul/class.

Mendapatkan abstraksi yg benar sangat penting, itulah kenapa kamu harus mengikuti
prinsip SOLID yg ada setelah bagian *Class*. Abstraksi yg bruk dapat lebih buruk
daripada kode yg duplikat, jadi berhati-hatilah! Setelah diberitahu hal ini, kamu
dapat membuat abstraksi yg baik, lakukan! Jangan membuat dirimu mengulang (DRY),
Jika tidak, kamu akan mendapati dirimu memperbarui hal dibeberapa tempat setiap
kali kamu ingin mengubah satu hal.

**Buruk:**
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

**Baik:**
```javascript
function showEmployeeList(employees) {
  employees.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    let portfolio;
    switch (employee.type) {
      case 'manager':
        portfolio = employee.getMBAProjects();
        break;
      case 'developer':
        portfolio = employee.getGithubLink();
        break;
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
**[⬆ Kembali ke atas](#daftar-isi)**

### Tetapkan default objects dengan Object.assign

**Buruk:**
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

**Baik:**
```javascript
const menuConfig = {
  title: 'Order',
  // Pengguna tidak memasukkan key 'body'
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

  // config sekarang sama dengan: {title: "Order", body: "Bar",
  // buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```
**[⬆ Kembali ke atas](#daftar-isi)**


### Jangan gunakan flag sebagai parameter fungsi
Flag memberitau penggunamu bahwa fungsi ini melakukan lebih dari satu hal. Fungsi seharusnya melakukan satu hal. Pisahkan fungsimu jika mereka
mengikuti jalur kode yg berbeda berdasarkan boolean.

**Buruk:**
```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Baik:**
```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Hindari Efek Samping (bagian 1)
Sebuah fungsi memproduksi efek samping ketika dia melakukan apapun selain memasukkan
nilai dan mengembalikan nilai. Sebuah efek samping bisa berupa menulis ke sebuah
file, memodifikasi sebuah variabel grobal, atau secara tidak sengaja mentransfer
semua uangmu ke orang yg tidak kamu kenal sebelumnya.

Sekarang, kamu memang perlu memiliki efek samping dalam sebuah program pada
suatu kesempatan. Seperti pada contoh sebelumnya, kamu bisa jadi perlu untuk
menulis sebuah file. Apa yg akan kamu lakukan adalah memusatkan dimana kamu
melakukan ini. Tidak memiliki beberapa fungsi dan class yang menulis ke
file tertentu. Miliki satu service yg melakukan hal itu. Satu dan satu-satunya.

Poin utama untuk menghindari jebakan yg umum seperti berbagi state antara objek
tanpa struktur apapun, menggunakan tipe data yg mutable yg dapat ditulis ke
apapun, dan tidak memusatkan dimana efek samping tersebut muncul. Jika kamu dapat
melakukan hal ini, kamu akan lebih bahagia daripada programmer-programmer.
kebanyakan.

**Buruk:**
```javascript
// Variabel global direferensi oleh beberapa fungsi
// Jika kita mempunyai fungsi lain yg menggunakan nama variabel ini, sekarang akan menjadi array dan dapat merusak ini semua.
let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Baik:**
```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

const name = 'Ryan McDermott';
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Hindari Efek Samping (bagian 2)
Dalam JavaScript, primitif dilewatkan oleh nilai dan objek / array dilewatkan
melalui referensi. Dalam kasus dimana objek dan array, jika fungsimu membuat
perubahan di array keranjang belanja, sebagai contoh, dengan menambahkan
sebuah barang untuk pembelian, maka fungsi lain yg menggunakan array `keranjang`
akan terpengaruh dengan penambahan ini. Hal itu mungkin bagus, namun bisa
jadi hal yg buruk jg. Mari kita mengimajinasikan situasi berikut:

Pengguna menekan "Beli", tombol yg memanggil fungsi `beli` yg memunculkan
permintaan jaringan dan mengirim array `keranjang` ke server. Karena
koneksi jaringan yg buruk, fungsi `beli` harus terus mencoba request kembali.
Sekarang, bagaimana jika di waktu yg sama si pengguna secara tidak sengaja
menekan tombol "Tambahkan ke Keranjang" pada barang yg sedang tidak mereka
inginkan sebelum request jaringan dimulai? Jika hal itu terjadi dan request
jaringan dimulai, maka fungsi beli tersebut akan mengirimkan tambahan barang
secara tidak sengaja karena hal itu mereferensi ke array keranjang belanja
yg mana fungsi `addItemToCart` dimodifikasi dengan menambahkan barang yg
tidak diinginkan.

Solusi yg bagus bisa jadi `addItemToCart` harus selalu mengkloning `cart`,
mengeditnya, dan mengembalikan klonnya. Ini memastikan bahwa tidak ada fungsi
lain yang memegang referensi keranjang belanja akan terpengaruh oleh perubahan
apapun.

Dua keberatan yg disebutkan pada pendekatan ini:
  1. Terdapat beberapa kasus dimana kamu benar-benar ingin memodifikasi objek
   masukan, namun ketika kamu menggunakan praktek pemrograman ini kamu akan
   menemukan bahwa kasus tersebut sangatlah langka. Kebanyakan apapun yg
   refaktorkan tidak memiliki efek samping.
  2. Kloning objek-objek yg besar dapat sangat mahal dalam hal performa.
  Beruntungnya, hal ini bukan isu yg besar dalam prakteknya karena terdapat
  library seperti [great libraries](https://facebook.github.io/immutable-js/)
  yg memperbolehkan pendekatan pemrograman seperti ini lebih cepat dan
  tidak terlalu banyak memakan memory seperti mengkloning objek-objek dan
  array-array secara manual.

**Buruk:**
```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Baik:**
```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date : Date.now() }];
};
```

**[⬆ Kembali ke atas](#daftar-isi)**

### Jangan menulis fungsi global
Polluting globals is a bad practice in JavaScript because you could clash with another
library and the user of your API would be none-the-wiser until they get an
exception in production. Let's think about an example: what if you wanted to
extend JavaScript's native Array method to have a `diff` method that could
show the difference between two arrays? You could write your new function
to the `Array.prototype`, but it could clash with another library that tried
to do the same thing. What if that other library was just using `diff` to find
the difference between the first and last elements of an array? This is why it
would be much better to just use ES2015/ES6 classes and simply extend the `Array` global.

Mencemari hal yg bersifat global adalah praktek yg buruk di JavaScript karena
kamu dapat berbenturan dengan library yg lain dan pengguna dari API-mu tidak
akan menjadi yg bijak sampai mereka mendapatkan exception di production.
Mari berpikir tentang sebuah contoh: Bagaimana jika kamu ingin memperpanjang
metode Array asli JavaScript untuk memiliki metode `diff` yang dapat menunjukkan
perbedaan antara dua array? Kamu dapat menulis fungsi barumu ke `Array.prototype`,
tapi hal tersebut dapat berbenturan dengan library lain yang mencoba melakukan
hal yang sama. Bagaimana jika libraru lainnya sudah menggunakan `diff` untuk
mencari perbedaan antara elemen pertama dan terakhir dalam sebuah array? Inilah
kenapa lebih baiknya menggunakan class ES2015/ES6 dan secara sederhana
memperpanjang global `Array`.

**Buruk:**
```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Baik:**
```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Gunakan functional programming daripada imperative programming
JavaScript bukanlah sebuah bahasa fungsional seperti Haskell, namun dia memiliki
aroma fungsional juga. Bahasa yg fungsional lebih sederhana dan lebih mudah
diuji. Gunakan gaya pemrograman seperti ini sebisa mungkin.

**Buruk:**
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

**Baik:**
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

const INITIAL_VALUE = 0;

const totalOutput = programmerOutput
  .map((programmer) => programmer.linesOfCode)
  .reduce((acc, linesOfCode) => acc + linesOfCode, INITIAL_VALUE);
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Enkapsulasi Kondisional

**Buruk:**
```javascript
if (fsm.state === 'fetching' && isEmpty(listNode)) {
  // ...
}
```

**Baik:**
```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === 'fetching' && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Hindari Kondisional Negatif

**Buruk:**
```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Baik:**
```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Hindari Kondisional
Ini terlihat seperti pekerjaan yg tidak mungkin. Sejak pertama kali mendengar
hal ini, kebanyakan orang berkata "Bagaimana saya bisa bekerja tanpa sebuah
statemen `if` ?" Jawabannya aalah kamu bisa menggunakan polimorfisme untuk
mengerjakan hal-hal di berbagai kasus. Pertanyaan kedua biasanya, "Oke itu bagus
tapi kenapa aku harus melakukannya?" Jawabannya adalah kode yg bersih seperti
apa yg sudah kita pelajari sebelumnya: sebuah fungsi harus melakukan satu hal
saja. Ketika kamu memiliki class dan fungsi yang memiliki statemen `if`, kamu
memberitahu user bahwa fungsimu melakukan hal yg lebih dari satu. Ingat, hanya
lakukan satu hal saja.

**Buruk:**
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

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**

### Hindari type-checking (bagian 1)
JavaScript tidak memiliki tipe data, yang mana fungsimu dapat mengambil tipe
data apapun dari sebuah argumen. Terkadang kamu tergigit karena kebebasan ini
dan menjadi tergoda untuk melakukan type-checking pada fungsimu. Terdapat
banyak jalan untuk terhindar dari hal ini. Hal pertama yg harus disadari adalah
memiliki API yg konsisten.

**Buruk:**
```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location('texas'));
  }
}
```

**Baik:**
```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location('texas'));
}
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Hindari type-checking (bagian 2)
Jika kamu bekerja dengan nilai primitif sederhana seperti string, integer dan
array, dan kamu tidak bisa menggunakan polimorfisme tapi kamu tetap merasa
perlu untuk melakukan type-check, kamu harus mempertimbangkan untuk menggunakan
TypeScript. Itu merupakan alternatif yg sangat baik untuk JavaScript biasa, yg
mana menyediakanmu static typing diatas standar sintaks JavaScript. Masalah
dengan type-checking secara manual di Javascript adalah hal itu memerlukan
banyak kata tambahan palsu yang merusak readibilitas. Pastikan JavaScript-mu
bersih, tulis pengujian yang baik, dan memiliki kode review yang bagus.
Jika tidak, lakukan semuanya dengan TypeScript (yang mana, seperti apa yang
aku katakan, itu adalah sebuah alternatif yang sangat bagus)

**Buruk:**
```javascript
function combine(val1, val2) {
  if (typeof val1 === 'number' && typeof val2 === 'number' ||
      typeof val1 === 'string' && typeof val2 === 'string') {
    return val1 + val2;
  }

  throw new Error('Must be of type String or Number');
}
```

**Baik:**
```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Jangan Optimasi Berlebihan
Modern browsers do a lot of optimization under-the-hood at runtime. A lot of
times, if you are optimizing then you are just wasting your time. [There are good
resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
for seeing where optimization is lacking. Target those in the meantime, until
they are fixed if they can be.

Browser-browser yang modern melakukan banyak optimisasi dibalik layar di runtime.
Seringkali, kamu mengoptimasi dan kamu hanya menghabiskan waktumu. [Terdapat
Sumber yg bagus](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
untuk dilihat dimana optimasi itu kurang. Targetkan mereka sementara,
sampai mereka diperbaiki jika mereka bisa melakukannya.

**Buruk:**
```javascript

// Di Browser lama, setiap iterasi dengan `list.length` yg uncached akan sangat costly
// karena `list.length` menghitung ulang. Di browser yang modern, ini sudah teroptimasi
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Baik:**
```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Hapus kode mati
Kode yg mati akan seburuk kode yang duplikat. Tidak ada alasan untuk menyimpannya
dalam basis kode-mu. Jika tidak dipanggil, maka buang saja! Itu akan tetap amazon
di "version history" jika kamu membutuhkannya lagi.

**Buruk:**
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

**Baik:**
```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```
**[⬆ Kembali ke atas](#daftar-isi)**

## **Objek dan Struktur Data**
### Gunakan getters dan setters
Menggunakan getters dan setter untuk mengakses data di objek akan lebih baik
daripada sekedar mencari sebuah property dari objek, "Kenapa?" kamu mungkin
bertanya. Baik, terdapat daftar yg tidak teratur sebagai alasan:

* Ketika kamu ingin melakukan hal lebih dari sekedar mendapatkan properti objek,
kamu tidak harus mencari dan mengubah setiap aksesor di basis kode-mu.
* Buat menambahkan validasi sederhana ketika melakukan sebuah `set`.
* Enkapsulasi representasi internal.
* Memudahkan untuk tambah catatan dan penanggulangan kesalahan ketika getting
dan setting.
* Kamu bisa lazy load properti-properti dari objek, seperti mendapatkannya dari
server.

**Buruk:**
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

**Baik:**
```javascript
function makeBankAccount() {
  // yg satu ini privat
  let balance = 0;

  // sebuah "getter", dibuat publik via objek yg dikembalikan di bawah ini
  function getBalance() {
    return balance;
  }

  // sebuah "setter", dibuat publik via objek yg dikembalikan di bawah ini
  function setBalance(amount) {
    // ... validasi sebelum memperbarui balance
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
**[⬆ Kembali ke atas](#daftar-isi)**


### Buat objek memiliki private member
Hal ini dapat diselesaikan lewat closures (untuk ES5 serta diatasnya)

**Buruk:**
```javascript

const Employee = function(name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee('John Doe');
console.log(`Employee name: ${employee.getName()}`); // Nama employee: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Nama employee: undefined
```

**Baik:**
```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    },
  };
}

const employee = makeEmployee('John Doe');
console.log(`Employee name: ${employee.getName()}`); // Nama employee: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Nama employee: John Doe
```
**[⬆ Kembali ke atas](#daftar-isi)**


## **Class**
### Gunakan class ES2015/ES6 daripada fungsi ES5
Sangat susah untuk mendapatkan class inheritance, construction, dan definisi
method yg mudah dibaca untuk class ES5 yg terdahulu. Jika kamu memerlukan
inheritance (dan waspadai jika kamu tidak), maka pilihlah class ES2015/ES6.
Akan tetapi, gunakan fungsi yg kecil daripada class sampai kamu mendapati
dirimu memerlukan objek yang lebih besar dan kompleks.

**Buruk:**
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

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**


### Gunakan metode chaining
Pola ini sangat berguna di JavaScript dan kamu akan melihatnya di banyak library
seperti jQuery dan Lodash. Hal ini memperbolehkan kode-mu jadi lebih ekspresif,
dan lebih sedikit bertele-tele. Untuk alasan tersebut, aku katakan, gunakan
metode chainging dan lihatlah betapa bersih kodemu nanti. Di fungsi class,
sederhanaya kembalikan `this` pada akhir setiap fungsi, dan kamu akan merantai
metode class setelahnya.

**Buruk:**
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

**Baik:**
```javascript
class Car {
  constructor() {
    this.make = 'Honda';
    this.model = 'Accord';
    this.color = 'white';
  }

  setMake(make) {
    this.make = make;
    // CATATAN: Mengembalikan this untuk chaining
    return this;
  }

  setModel(model) {
    this.model = model;
    // CATATAN: Mengembalikan this untuk chaining
    return this;
  }

  setColor(color) {
    this.color = color;
    // CATATAN: Mengembalikan this untuk chaining
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // CATATAN: Mengembalikan this untuk chaining
    return this;
  }
}

const car = new Car()
  .setColor('pink')
  .setMake('Ford')
  .setModel('F-150')
  .save();
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Gunakan composition daripada inheritance
Sebagaimana terkenal dinyatakan di [*Design Patterns*](https://en.wikipedia.org/wiki/Design_Patterns)
oleh Gang of Four, kamu sebaiknya memilih composition daripada inheritance
dimanapun kamu bisa. Terdapat banyak alasan baik untuk menggunakan inheritance
dan banyak alasan baik pula untuk menggunakan composition. Poin utamanya untuk
pepatah ini adalah jika pikiranmu secara insting lebih memilih inheritance,
coba untuk memikir jika composition dapat memodelkan problemmu lebih baik. di
beberapa kasus hal tersebut bisa.

Kamu mungkin saja bertanya-tanya, "Kapan aku seharusnya menggunakan inheritance?"
hal itu tergantung pada problemnya, tapi ini adalah alasan yg lumayan dari
kapan inheritence membuat hal lebih daripada composition:

1. Inheritance-mu merepresentasikan sebuah hubungan "adalah-sebuah" dan bukan
hubungan "memiliki-sebuah" (Manusia->Hewan vs. Pengguna->DetailPengguna).
2. Kamu dapat menggunakan kembali kode dari basis class (Manusa dapat bergerak
seperti semua hewan-hewan).
3. Kamu ingin membuat perubahan yg global untuk menurunkan class dengan
mengubah sebuah basis class. (Mengubah pengeluaran kalori dari semua hewan-hewan
ketika ia bergerak)

**Buruk:**
```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Buruk karena Employee "memiliki" data pajak. EmployeeTaxData bukan sebuah tipe dari Employee
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**

## **SOLID**
### Single Responsibility Principle (SRP) / Prinsip Tanggung Jawab Tunggal
Seperti dinyatakan di Clean Code, "Semestinya tidak akan lebih dari satu alasan
sebuah class untuk berubah". Hal itu sangat menggoda untuk memadati class dengan
fungsionalitas yang banyak, seperti ketika kamu hanya dapat mengambil satu koper
dalam penerbanganmu. Masalahnya dengan ini adalah classmu tidak akan secara
konseptual kohesif dan hal hal itu akan memberikan alasan untuk berubah.
Meminimalisir jumlah waktu yang kamu gunakan untuk mengubah class sangat penting.
Hal itu penting karena jika terlalu banyak fungsionalitas dalam sebuah class dan
kamu memodifikasi sebagian saja, hal itu akan sulit untuk dimengerti Bagaimana
itu akan mempengaruhi modul lain yg terhubung dalam basis kode-mu.

**Buruk:**
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

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**

### Open/Closed Principle (OCP) / Prinsip Terbuka/Tertutup
Seperti yang dinyatakan Bertrand Meyer, "entitas perangkat lunak (class, modul,
fungsi, dsb) harus terbuka untuk ekstensi, namun tertutup untuk modifikasi." Apa
maksudnya? Prinsip ini dasarnya menyatakan bahwa kamu harus tetap memperbolehkan
pengguna untuk menambahkan fungsionalitas baru tanpa mengubah kode yg sudah ada.

**Buruk:**
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
        // mengubah respon dan mengembalikan
      });
    } else if (this.adapter.name === 'httpNodeAdapter') {
      return makeHttpCall(url).then((response) => {
        // mengubah respon dan mengembalikan
      });
    }
  }
}

function makeAjaxCall(url) {
  // request dan mengembalikan promise
}

function makeHttpCall(url) {
  // request dan mengembalikan promise
}
```

**Baik:**
```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'ajaxAdapter';
  }

  request(url) {
    // request dan mengembalikan promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'nodeAdapter';
  }

  request(url) {
    // request dan mengembalikan promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then((response) => {
      // request dan mengembalikan promise
    });
  }
}
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Liskov Substitution Principle (LSP) / Prinsip Subtitusi Liskov
Ini merupakan istilah mengerikan untuk sebuah konsep sederhana. Hal ini secara
formal didefinisikan sebagai "jika S adalah sub-tipe dari T, maka objek dari
tipe T mungkin terganti oleh objek tipe S (contohnya, objek tipe S mungkin
pengganti objek dari tipe T) tanpa mengubah tiap properti dari program
(kebenaran, tugas yg dilakukan, dsb)." Hal itu lebih mengerikan daripada
definisinya

Penjelasan terbaik dari hal ini adalah jika kamu memiliki parent class dan
sebuah child class, maka basis class dan child class dapa digunakan secara
bergantian tanpa mendapatkan hasil yg salah. Hal ini mungkin membingungkan,
jadi mari kita lihat pada contoh Kotak-Segi Empat klasik. Secara matematis,
sebuah kotak adalah Segi Empat, namun jika kamu memodelkannya menggunakan
hubungan "adalah-sebuah" via inheritance, maka kamu secara cepat ada
dalam masalah

**Buruk:**
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
    const area = rectangle.getArea(); // Buruk: Mengembalikan 25 untuk kotak. Harusnya 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**

### Interface Segregation Principle (ISP) / Prinsip segregasi antarmuka
JavaScript tidak memiliki interface jadi prinsip ini tidak digunakan secara
ketat seperti yang lainnya. Akan tetapi, hal ini penting dan relevan walaupun
JavaScript tidak memiliki sistem penggolongan (type).

ISP dinyatakan seperti "Klien harus dipaksa untuk bergantung pada interface
yg mereka tidak gunakan." Interface adalah kontrak implisit di JavaScript
karena duck typing.

Sebuah contoh yang baik untuk dilihat adalah demonstrasi prinsip ini di JavaScript
adalah untuk class yang membutuhkan object pengaturan yang besar. Tidak membutuhkan
Klien untuk mengatur banyak opsi adalah keuntungan, karena sebagian besar waktu
mereka tidak memerlukan sebua setting. Membuat mereka opsional mencegah
mendapati sebuah "fat interface"

**Buruk:**
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
  animationModule() {} // Sebagian besar waktu, kita tidak membutuhkan animasi sewaktu tranversing.
  // ...
});

```

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**

### Dependency Inversion Principle (DIP)
Prinsip ini menyatakan dua hal esensial:
1. Modul Tingkat-Tinggi seharusnya tidak tergantung pada modul tingkat-rendah.
Keduanya harus bergantung pada abstraksi.
2. Abstraksi seharusnya tidak bergantung pada detail. Detail harus bergantung
pada abstraksi.

Hal ini dapat menjadi susah untuk dimengerti saat pertama kali, namun jika
kamu telah bekerja menggunakan AngularJS, kamu telah melihat dari implementasi
dari prinsip ini dalam bentuk Dependency Injection (DI). Ketika mereka bukan
konsep yang identik, DIP menjaga modul tingkat-tinggi dari mengetahui detail
dari modul tingkat-rendah-nya dan mengatur mereka. Hal ini dapat diselesaikan
melalui DI. Keuntungan terbesar dari hal ini adalah mengurangi kopel antar
modul. Kopel sangat buruk untuk pola pengembangan karena hal itu membuat
kodemu susah untuk direfaktor.

Seperti dinyatakan sebelumnya, JavaScript tidak memiliki Interface jadi
abstraksi yang tergantung adalah kontrak implisit. Hal itu dapat dikatakan,
metode dan properti yang sebuah objek/class buka pada objek/class lain. Di
contoh dibawah ini, kontrak implisit adalah Request modul apapun untuk
sebuah `InventoryTracker` akan memiliki sebuah metode `requestItems`.

**Buruk:**
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

    // Buruk: Kita telah menciptakan ketergantungan pada penerapan request tertentu.
    // Kita harus memiliki sebuah requestItems yg bergantung pada metode request: `request`
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

**Baik:**
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

// Dengan membangun ketergantungan kita secara eksternal dan menyuntikkannya, kita dapat dengan mudah
// mengganti modul request kita untuk satu hal baru yang menggunakan WebSockets.
const inventoryTracker = new InventoryTracker(['apples', 'bananas'], new InventoryRequesterV2());
inventoryTracker.requestItems();
```
**[⬆ Kembali ke atas](#daftar-isi)**

## **Pengujian**
Testing is more important than shipping. If you have no tests or an
inadequate amount, then every time you ship code you won't be sure that you
didn't break anything. Deciding on what constitutes an adequate amount is up
to your team, but having 100% coverage (all statements and branches) is how
you achieve very high confidence and developer peace of mind. This means that
in addition to having a great testing framework, you also need to use a
[good coverage tool](http://gotwarlost.github.io/istanbul/).

Pengujian itu lebih penting daripada shipping. Jika kamu tidak punya pengujian
atau tidak mencukupunya, maka setiap kali kamu ship kodemu kamu tidak akan
yakin kalau kamu tidak merusak segalanya. Memutuskan berapa jumlah yang cukup
tergantung pada tim-mu, tapi memiliki 100% cakupan (semua statemen dan cabang)
adalah bagaimana kamu meraih percaya diri yg snagat tinggi dan kedamaian
pikiran pengembang. Hal ini berarti selain memiliki kerangka pengujian yg
bagus, kamu juga perlu menggunakan sebuah [alat cakupan yang bagus](http://gotwarlost.github.io/istanbul/).

Tidak ada alasan untuk tidak menulis pengujuan. Terdapat [banyak framework pengujian yg bagus di Javascript](http://jstherightway.org/#testing-tools),
jadi pilih satu yang tim-mu lebih suka. Ketika kamu menemukan satu yang dapat
bekerja dengan tim-mu, lalu arahkan untuk selalu menulis test untuk setiap
fitur/modul baru yang kamu perkenalkan. Jika kamu lebih suka metode TDD (Test
Driven Development), itu hal yang bagus, tapi poin utamanya adalah untuk
selalu memastikan kamu meraih cakupan tujuan-tujuan sebelum meluncurkan
fitur apapun, atau merefaktor satu yg telah ada.

### Satu konsep tiap pengujian

**Buruk:**
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

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**

## **Concurrency**
### Gunakan Promises, jangan callbacks
Callback itu tidak clean, dan mereka memngakibatkan nesting dengan jumlah yang
berlebihan. Dengan ES2015/ES6, promise sudah built-in. Jadi gunakanlah!

**Buruk:**
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

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**

### Async/Await lebih sederhana daripada Promises
Promise adalah alternatif dari callback yang sangat bersih, namun ES2017/ES8 membawa
async dan await yang mana menawarkan solusi yang lebih bersih lagi. Yang kamu perlukan
adalah fungsi yang memiliki prefix sebuah kata kuncu `async`, lalu kamu dapat
menulis logika secara imperatif tanpa rantai sebuah `then` dari fungsi. Gunakan
ini jika kamu dapat mengambil manfaat dari fitur ES2017/ES8 hari ini!

**Buruk:**
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

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**


## **Penanganan Kesalahan**
Dilempari error adalah sebuah hal yang baik! Hal itu berarti di runtime telah
sukses mengidentifikasi ketika sesuatu di programmu ada kesalahan dan hal itu
memberi kesempatan kepadamu untuk menghentikan eksekusi fungsi dari stack yang
sekarang, menghentikan process (di Node) dan memberitahumu di konsol dengan
tumpukan jejak.

### Jangan abaikan kesalahan yg tertangkap
Doing nothing with a caught error doesn't give you the ability to ever fix
or react to said error. Logging the error to the console (`console.log`)
isn't much better as often times it can get lost in a sea of things printed
to the console. If you wrap any bit of code in a `try/catch` it means you
think an error may occur there and therefore you should have a plan,
or create a code path, for when it occurs.

Tidak melakukan apapun dengan error yg tertangkan tidak akan memberikanmu
kemampuan untuk membetulkan atau bereaksi pada error yang telah dikatakan.
Mencatat error untuk konsol (`console.log`) tidak jauh lebih baik seperti
sering kali bisa tersesat di lautan benda-benda yang dicetak ke konsol.
Jika kamu menyelimuti kodemu sedikit-sedikit di sebuah `try/catch` hal itu
berarti kamu memikirkan sebuah error yang mungkin saja muncul disana dan
oleh karena itu kamu harus memiliki sebuah rencana, atau membuat sebuah
jalur kode, untuk kapan hal itu akan muncul.

**Buruk:**
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Baik:**
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // Satu opsi (lebih berisik daripada console.log):
  console.error(error);
  // opsi lainnya:
  notifyUserOfError(error);
  // opsi lainnya:
  reportErrorToService(error);
  // atau lakukan ketiganya!
}
```

### Jangan abaikan promise yg ditolak
Untuk alasan yang sama seperti kamu tidak boleh mengabaikan error yang tertangkap
dari `try/catch`.

**Buruk:**
```javascript
getdata()
  .then((data) => {
    functionThatMightThrow(data);
  })
  .catch((error) => {
    console.log(error);
  });
```

**Baik:**
```javascript
getdata()
  .then((data) => {
    functionThatMightThrow(data);
  })
  .catch((error) => {
    // Satu opsi (lebih berisik daripada console.log):
    console.error(error);
    // Opsi lain:
    notifyUserOfError(error);
    // Opsi lain:
    reportErrorToService(error);
    // Atau lakukan ketiganya!
  });
```

**[⬆ Kembali ke atas](#daftar-isi)**


## **Menyusun Format**
Formatting is subjective. Like many rules herein, there is no hard and fast
rule that you must follow. The main point is DO NOT ARGUE over formatting.
There are [tons of tools](http://standardjs.com/rules.html) to automate this.
Use one! It's a waste of time and money for engineers to argue over formatting.

For things that don't fall under the purview of automatic formatting
(indentation, tabs vs. spaces, double vs. single quotes, etc.) look here
for some guidance.

### Gunakan Kapitalisasi yg konsisten
JavaScript is untyped, so capitalization tells you a lot about your variables,
functions, etc. These rules are subjective, so your team can choose whatever
they want. The point is, no matter what you all choose, just be consistent.

**Buruk:**
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

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**


### Fungsi yg memanggil dan yang dipanggil seharusnya berdekatan
If a function calls another, keep those functions vertically close in the source
file. Ideally, keep the caller right above the callee. We tend to read code from
top-to-bottom, like a newspaper. Because of this, make your code read that way.

**Buruk:**
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

**Baik:**
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

**[⬆ Kembali ke atas](#daftar-isi)**

## **Komentar**
### Komentar hanya untuk hal-hal yang memiliki kompleksitas logika.
Comments are an apology, not a requirement. Good code *mostly* documents itself.

**Buruk:**
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

**Baik:**
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
**[⬆ Kembali ke atas](#daftar-isi)**

### Jangan meninggalkan kode yang dikomentari dalam basis kode anda
Version control exists for a reason. Leave old code in your history.

**Buruk:**
```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Baik:**
```javascript
doStuff();
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Jangan ada komentar tentang jurnal
Remember, use version control! There's no need for dead code, commented code,
and especially journal comments. Use `git log` to get history!

**Buruk:**
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

**Baik:**
```javascript
function combine(a, b) {
  return a + b;
}
```
**[⬆ Kembali ke atas](#daftar-isi)**

### Hindari komentar marker
They usually just add noise. Let the functions and variable names along with the
proper indentation and formatting give the visual structure to your code.

**Buruk:**
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

**Baik:**
```javascript
$scope.model = {
  menu: 'foo',
  nav: 'bar'
};

const actions = function() {
  // ...
};
```
**[⬆ Kembali ke atas](#daftar-isi)**

## Alih Bahasa

Panduan ini juga tersedia di berbagai bahasa:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Bahasa Brazil Portugis**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Bahasa Spanyol**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Bahasa Mandarin**:
    - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
    - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **Bahasa Jerman**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Bahasa Korea**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Bahasa Polandia**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Bahasa Rusia**:
    - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
    - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
  - ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Bahasa Vietnam**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)
  - ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Bahasa Jepang**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)

**[⬆ Kembali ke atas](#daftar-isi)**
