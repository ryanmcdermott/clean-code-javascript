# clean-code-javascript

## İçindekiler

1. [Giriş](#Giriş)
2. [Değişkenler](#Değişkenler)
3. [Fonksiyonlar](#Fonksiyonlar)
4. [Nesneler ve Veri Yapıları](#nesneler-ve-veri-yapıları)
5. [Sınıflar (Class)](#sınıflar)
6. [SOLID](#solid)
7. [Testler](#testler)
8. [Tutarlılık](#Tutarlılık)
9. [Hata Yönetimi](#Hata-Yönetimi)
10. [Biçimlendirme](#Biçimlendirme)
11. [Yorumlar](#yorumlar)
12. [Çeviriler](#çeviriler)

## Giriş

![Kod okurken kaç defa bağıracağınızın bir sayısı olarak yazılım kalite tahmininin görüntüsü](https://www.osnews.com/images/comics/wtfm.jpg)

Robert C. Martin'nın kitabı olan ve yazılım mühendisliği ilkerini barındıran [_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)'un JavaScript için uyarlanmış hali. Bu bir tarz rehberi değil. JavaScript'te [okunabilir, yeniden kullanılabilir ve yeniden düzenlenebilir](https://github.com/ryanmcdermott/3rs-of-software-architecture) yazılımlar üretmeye yönelik bir kılavuzdur.

Buradaki her ilkeye kesinlikle uyulmak zorunda değildir ve daha azı evrensel olarak kabul edilecektir. Bunlar önerilerden başka bir şey değildir, fakat bunlar _Clean Code_ yazarları tarafından uzun yıllar süren tecrübeler ile kodlandı.

Yazılım mühendisliği zanaatımız 50 yaşın biraz üzerinde ve hala çok şey öğreniyoruz. Yazılım mimarisi, mimarinin kendisi kadar eski olduğunda, belki de takip edilmesi daha zor kurallarımız olacak. Şimdi, bu önerilerin sizin ve ekibinizin ürettiği JavaScript kodunun kalitesini değerlendirmek için bir mihenk taşı işlevi görmesine izin verin.

Bir şey daha: bunların bilinmesi sizi hemen daha iyi bir yazılım geliştiricisi yapmaz ve uzun yıllar onlarla çalışmak hata yapmayacağınız anlamına gelmez.

Her kod parçası ilk taslak olarak başlar, örneğin ıslak kil son halini alır. Son olarak, akranlarımızla gözden geçirdiğimizde kusurları kesiyoruz. İyileştirilmesi gereken ilk taslaklar için kendinize yüklenmeyin. Onun yerine kodunuza yüklenin.

## **Değişkenler**

### Anlamlı ve belirgin değişken adları kullanın

**Kötü:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**İyi:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ Başa dön](#İçindekiler)**

### Aynı değişken türü için aynı kelimeleri kullanın

**Kötü:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**İyi:**

```javascript
getUser();
```

**[⬆ Başa dön](#İçindekiler)**

### Aranabilir isimler kullanın

Yazacağımızdan daha fazla kod okuyacağız. Yazdığımız kodun okunabilir ve aranabilir olması önemlidir. Değişkenleri anlamlı ve anlaşılabilir _isimlendirmemekle_, okuyucuya zarar veriyoruz.
Adlarınızı aranabilir hale getirin. [buddy.js](https://github.com/danielstjules/buddy.js) ve
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) gibi araçlar isimlendirilmemiş sabitlerinizi belirlemenize yardımcı olacaktır.

**Kötü:**

```javascript
// 86400000 neydi ?
setTimeout(blastOff, 86400000);
```

**İyi:**

```javascript
// Bunları büyük harfle adlandırılmış sabitler olarak bildirin.
const MILLISECONDS_IN_A_DAY = 86_400_000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

**[⬆ Başa dön](#İçindekiler)**

### Açıklayıcı değişkenler kullan

**Kötü:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**İyi:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ Başa dön](#İçindekiler)**

### Kafadan planmaktan kaçının

Açık olmak, ima etmekten iyidir.

**Kötü:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Bir dakika, 'l' neydi?
  dispatch(l);
});
```

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

### Gereksiz içerikler eklemeyin

Sınıf / nesne adınız size bir şey söylüyorsa, bunu değişken adınızda tekrarlamayın.

**Kötü:**

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

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

### Kısa devre veya şartlar yerine önceden tanımlanmış argümanlar kullanın

Önceden tanımlanan argümanlar genellikle kısa devrelere göre daha temizdir. Bunları kullanırsanız şunun farkında olun, fonksiyonunuz sadece `tanımsız` _(undefined)_ değerler için önceden tanımlanmış argümana kullanacaktır.  `''`, `""`, `false`, `null`, `0`, ve `NaN` gibi "yanlış denilebilecek" değerler önceden tanımlanmış bir değerle değiştirilmez.

**Kötü:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**İyi:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ Başa dön](#İçindekiler)**

## **Fonksiyonlar**

### Fonksiyonlar argümanları (Tercihen 2 veya daha az)

Fonksiyon parametrelerinin sayısını sınırlamak inanılmaz derecede önemlidir, çünkü fonksiyonunuzu test etmeyi kolaylaştırır. Üçten fazlasına sahip olmak, her bir argümanla tonlarca farklı vakayı test etmeniz gereken bir kombinasyonel patlamaya yol açar.

Bir veya iki argüman ideal durumdur ve mümkünse üç tanesinden kaçınılmalıdır. Bundan daha fazlası birleştirilmeldir. Genellikle,
ikiden fazla argüman sonra fonksiyonunuz çok işlem yapmaya çalışıyor. Olmadığı durumlarda, çoğu zaman daha üst düzey bir nesne argüman olarak yeterli olacaktır.

JavaScript havada nesne yapmanıza olanak sağladığı için, bir çok sınıf yapısına gerek kalmadan,  bir nesneyi birden fazla nesne kullanmadan kullanabilirsiniz.

Fonksiyonun hangi özellikleri beklediğini netleştirmek için ES2015 / ES6 ayrıştırma sintaksını kullanabilirsiniz. Bunun birkaç avantajı vardır:

1. Birisi fonksiyonun imzasına baktığında, hangi özelliklerin kullanıldığını hemen anlar..
2. Adlandırılmış parametreleri simüle etmek için kullanılabilir.
3. Ayrıştırma işlemi ayrıca fonksiyona iletilen argüman nesnesinin belirtilen ilk değerlerini de klonlar. Bu, yan etkilerin önlenmesine yardımcı olabilir. Not: Argüman nesnelerinden ayrıştırılan nesneler ve diziler klonlanmaz!
4. Linters, sizi kullanılmayan değerler için uyarabilir bu da ayrıştırmadan imkansız olurdu.

**Kötü:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

### Fonksiyonlar bir şey yapmalı

Bu, yazılım mühendisliğinde açık ara en önemli kuraldır. Fonksiyonlar birden fazla şey yaptığında, birleştirmesi, test etmesi ve anlamdırılması daha zordur. Bir fonksiyonu yalnızca bir eyleme ayırabildiğinizde, kolayca yeniden düzenlenebilir ve kodunuz çok daha temiz okunur. Bu yazıdan başka bir şey almazsanız dahi sadece bununla birçok geliştiricinin önünde olacaksınız.

**Kötü:**

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

**İyi:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ Başa dön](#İçindekiler)**

### Fonksiyon isimleri ne yaptıklarını anlatmalı

**Kötü:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// Fonksiyonun adından ne eklendiğini söylemek zor
addToDate(date, 1);
```

**İyi:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ Başa dön](#İçindekiler)**

### Fonksiyonlar soyutlaştırmadan sadece bir seviye uzak olmalıdır

Birden fazla soyutlama seviyeniz olduğunda fonksiyonunuz genellikle çok fazla şey yapar. Fonksiyonların bölünmesi, yeniden kullanılabilirliği ve daha kolay test yapılmasını sağlayacak.

**Kötü:**

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

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

### Tekrarlanan kodu sil

Tekrarlanan kodlardan kaçınmak için elinizden geleni yapın. Tekrarlanan kod kötü çünkü bazı mantığı değiştirmeniz gerekirse bir şeyi değiştirmek için birden fazla yer olduğu anlamına geliyor.

Bir restoran işlettiğinizi ve stoklarınızı takip ettiğinizi düşünün: tüm domates, soğan, sarımsak, baharat, vb. onlara. Yalnızca bir listeniz varsa, güncellenecek tek bir yer vardır!

Çoğunlukla yinelenen kodunuz vardır, çünkü çoğu şeyi ortak paylaşsalar dahi çok küçük bir kaç farklılık vardır, ancak farklılıkları sizi aynı şeylerin çoğunu yapan iki veya daha fazla ayrı fonksiyona sahip olmaya zorlar. Tekrarlanan kodun kaldırılması, bu farklı şeyleri tek bir fonksiyon / modül / sınıfla işleyebilecek bir soyutlama oluşturmak anlamına gelir.

Soyutlamayı doğru yapmak kritik öneme sahiptir, bu yüzden _Sınıflar_ bölümünde belirtilen SOLID ilkelerini izlemelisiniz. Kötü soyutlamalar yinelenen koddan daha kötü olabilir, bu yüzden dikkatli olun! Bunu söyledikten sonra, eğer iyi bir soyutlama yapabilirseniz, yapın! Kendinizi tekrarlamayın, aksi takdirde bir şeyi değiştirmek istediğinizde kendinizi birden fazla yeri güncellerken bulacaksınız.

**Kötü:**

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

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

### Object.assign ile varsayılan nesneleri ayarlama

**Kötü:**

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

**İyi:**

```javascript
const menuConfig = {
  title: "Order",
  // Kullanıcı 'body' eklemedi
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

  // config çıktısı şimdi : {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ Başa dön](#İçindekiler)**

### Fonksiyon parametrelerinde işaretleme kullanma

İşaretlemeler fonksiyonunuzun birden fazla şey yaptığını gösterir. Fonkisyonlar sadece tek şey yapmalılar. Eğer aşağıdakine benzer değişiklere ve mantıksal operatorlere sahip fonksiyonunuz varsa  fonksiyonlarınızı ayırın.

**Kötü:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**İyi:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ Başa dön](#İçindekiler)**

### Yan Etkilerden Kaçının (Bölüm 1)

Bir fonksiyon, bir değeri alıp başka bir değer veya değer döndürmekten başka bir şey yaparsa yan etki üretir. Bir yan etki, bir dosyaya yazma, bazı global değişkeni değiştirme veya yanlışlıkla tüm paranızı bir yabancıya gönderme olabilir.

Şimdi, zaman zaman bir programda yan etkilere sahip olmanız gerekir. Önceki örnekte olduğu gibi, bir dosyaya yazmanız gerekebilir. Yapmak istediğiniz, bunu yapacağınız yeri merkezileştirmektir. Birden fazla yazma işlemi yapan fonksiyonlar veya sınıflar yapmayın. Bunu yapan bir servisiniz olsun. Bir ve sadece bir.

Buradaki ana nokta, herhangi bir yapıya sahip olmayan nesneler arasında durumu(state) paylaşmak, herhangi bir şeyle yazılabilen değiştirilebilir(mutable) veri türlerini kullanmak ve yan etkilerinizin nerede ortaya çıktığını merkezileştirmemek gibi yaygın tuzaklardan kaçınmaktır. Bunu yapabilirseniz, diğer programcıların büyük çoğunluğundan daha mutlu olacaksınız.

**Kötü:**

```javascript
// Aşağıdaki fonksiyon Global değişkeni refere alıyor
// Bu adı kullanan başka bir fonksiyonumuz olsaydı, şimdi bir dizi olurdu ve onu bozacaktı.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**İyi:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ Başa dön](#İçindekiler)**

### Yan Etkilerden Kaçının (Bölüm 2)

JavaScript'te, temel öğeler değerlerle aktarılır (passed by value) ve objeler/diziler referans ile aktarılır (passed by reference).

Bu durumda, nesnelerde ve dizilerde fonksiyonunuz bir değişiklik yaparsa
örneğin, bir alışveriş sepeti dizisinde, satın almak için bir öğe ekleyerek,
bu `cart` dizisini kullanan diğer fonksiyonlar bu eklemeden etkilenecektir.
Bu harika olabilir, ancak kötü de olabilir. Kötü bir hayal edelim
durum:

Kullanıcı, 'Satın Al' butonuna basar ve bu `satınal` fonksiyonu sunucuya bir istek atarak `sepet` dizisini sunucuya gönderir. Kötü bir ağ bağlantısı nedeniyle, `satınal` işlevi isteği yeniden denemeye devam etmelidir. Şimdi, bu arada kullanıcı yanlışlıkla ağ isteği başlamadan istemedikleri bir öğenin üzerine "Sepete Ekle" düğmesini tıklarsa ne olur? Bu olursa ve ağ isteği başlarsa, yanlışlıkla satın alınan öğeyi gönderir çünkü alışveriş sepeti dizisine, `ürünüSepeteEkle` işlevinin istenmeyen bir öğe ekleyerek değiştirdiği bir referansı vardır. `ürünüSepeteEkle` ın her zaman `sepeti` klonlaması, düzenlemesi ve klonu döndürmesi için harika bir çözüm olacaktır. Bu, alışveriş sepetinin referansını tutan başka hiçbir işlevin herhangi bir değişiklikten etkilenmemesini sağlar.

Bu yaklaşıma değinecek iki uyarı:

1. Girilen nesnesini gerçekten değiştirmek istediğiniz durumlar olabilir, ancak bunu uyguladığınızda, bu vakaların oldukça nadir olduğunu göreceksiniz. Çoğu şeyin yan etkisi olmayacak şekilde yeniden düzenlenebilir!

2. Büyük nesneleri klonlamak performans açısından çok pahalı olabilir.  Neyse ki, bu pratikte büyük bir sorun değildir, çünkü  bu tür programlama yaklaşımlarını manuel yapmaktansa daha hızlı olmasını ve büyük nesneleri ve dizileri klonlanlarken daha az bellek harcayan [harika kütüphaneler](https://facebook.github.io/immutable-js/) vardır.

**Kötü:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**İyi:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ Başa dön](#İçindekiler)**

### Global fonksiyonlar yazma

Globalleri kirletmek JavaScript'te kötü bir uygulamadır, çünkü başka bir kütüphaneyle çakıştırabilirsiniz ve API'lerinizi kullananlar kişiler canlıya çıkana kadar bu aksi durumların farkında olmayabilir.

Bir örnek düşünelim: JavaScript'in yerel kütüphanesindeki dizi `diff` methodunu genişletmek ve iki dizinin farklılıklarını göstermek istediğinizi var sayalım. JavaScript'in yerel Array yöntemini iki dizi arasındaki farkı gösterebilecek bir "diff" yöntemine genişletmek mi istiyorsunuz?

Yeni fonksiyonunuzu `Array.prototype`'e yazabilirsiniz, ancak aynı şeyi yapmaya çalışan başka bir kütüphane ile çakışabilir. Ya diğer kütüphane bir dizinin ilk ve son elemanları arasındaki farkı bulmak için sadece `diff` kullanıyorsa? Bu yüzden sadece ES2015 / ES6 sınıflarını kullanmak ve `Array` globalini genişletmek çok daha iyi olurdu.

**Kötü:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**İyi:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ Başa dön](#İçindekiler)**

### Zorunlu programlama yerine fonksiyonel programlamayı tercih edin

JavaScript, Haskell'in olduğu gibi işlevsel bir dil değildir, ancak işlevsel bir tadı vardır. İşlevsel diller daha temiz ve test edilmesi daha kolay olabilir. Yapabildiğinizde bu tarz bir programlama yapın.

**Kötü:**

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

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

### Koşulları kapsamak

**Kötü:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**İyi:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ Başa dön](#İçindekiler)**

### Avoid negative conditionals

**Kötü:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**İyi:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ Başa dön](#İçindekiler)**

### Koşullardan kaçının

Bu imkansız bir görev gibi görünüyor. Bunu ilk kez duyduktan sonra, çoğu kişi "`if` ifadesi olmadan nasıl bir şey yapmam gerekiyor?" Cevap şu ki
birçok durumda aynı görevi yerine getirmek için polimorfizm kullanabilirsiniz. İkinci soru genellikle, "peki bu harika ama neden bunu yapmak isteyeyim ki?" Cevap, daha önce öğrendiğimiz temiz bir kod kavramıydı: bir fonksiyon sadece bir şey yapmalı. `If` ifadeleri olan sınıflarınız ve fonksiyonlarınız olduğunda, kullanıcılara işlevinizin birden fazla şey yaptığını söylüyor. Unutmayın, sadece bir şey yapın.

**Kötü:**

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

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

### Yazım denetiminden kaçının (Bölüm 1)

JavaScript türsüzdür, yani işlevleriniz her türlü argümanı alabilir. Bazen bu özgürlük bizi oltaya getirebilir ve fonksiyonlarınızda tip kontrolü yapmak cazip hale gelir. Bunu yapmaktan kaçınmanın birçok yolu vardır. Dikkate alınması gereken ilk şey tutarlı API'lardır.

**Kötü:**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**İyi:**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ Başa dön](#İçindekiler)**

### Yazım denetiminden kaçının (Bölüm 2)

Dizeler ve tamsayılar gibi temel ilkel değerlerle çalışıyorsanız,
ve polimorfizm kullanamazsınız, ancak yine de yazım denetimi yapma gereğini hissediyorsanız, TypeScript kullanmayı düşünmelisiniz. Standart JavaScript sentaks üstünde statik yazım sağladığı için normal JavaScript'e mükemmel bir alternatiftir. Manuel olarak normal JavaScript'i kontrol etmeyle ilgili sorun, iyi yapmak o kadar fazla ayrıntılı gerektirir ki, elde ettiğiniz sahte "tip güvenliği" kaybettiğiniz okunabilirliği telafi etmez. JavaScript'inizi temiz tutun, iyi testler yazın ve iyi kod incelemelerine sahip olun. Aksi takdirde, hepsini TypeScript ile yapın (dediğim gibi harika bir alternatif!).

**Kötü:**

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

**İyi:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ Başa dön](#İçindekiler)**

### Aşırı optimizasyon yapma

Modern tarayıcılar çalışma zamanında çok sayıda optimizasyon yapar. Çoğu zaman, eğer optimize ediyorsanız, sadece zamanınızı boşa harcıyorsunuz demektir.

Optimizasyonun nerede olmadığını görmek için [iyi kaynaklar](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) vardır. Bunları optimize edene kadar işaretleyebilirsiniz.

**Kötü:**

```javascript
// Eski tarayıcılarda, önbelleğe alınmamış "list.length" içeren her yineleme maliyetli olacaktır
// list.length yeniden hesaplanması nedeniyle. Modern tarayıcılarda bu optimize edilmiştir.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**İyi:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ Başa dön](#İçindekiler)**

### Kullanılmayan kodları silin

Ölü kod, yinelenen kod kadar kötü. Kod tabanınızda tutmak için bir neden yoktur. Eğer çağrılmazsa, ondan kurtulun! Hala ihtiyacınız varsa sürüm geçmişinizde güvende olacaktır.

**Kötü:**

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

**İyi:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ Başa dön](#İçindekiler)**

## **Nesneler ve Veri Yapıları**

### Alıcıları ve ayarlayıcıları kullanma (Getters & Setters)

Nesnelerdeki verilere erişmek için alıcıları ve ayarlayıcıları kullanmak, bir nesnedeki bir özelliği aramaktan daha iyi olabilir. "Neden?" sorabilirsiniz. Pekala, işte organize edilmeden nedenlerin bir listesi:

- Bir nesne özelliği almanın ötesinde daha fazlasını yapmak istediğinizde, kod tabanınızdaki her erişimciye bakmanız ve değiştirmeniz gerekmez.
- `set`yaparken doğrulama basitçe yapılabilir.
- Dahili gösterimi içine alır.
- Alma ve ayarlama sırasında kayıtlamayı(logging) ve hata yönetimi(error handling) eklemek kolaydır.
- Diyelimki sunucudan alıyorsunuz, nesnenizin özelliklerini tembel(lazy load) olarak yükleyebilirsiniz.

**Kötü:**

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

**İyi:**

```javascript
function makeBankAccount() {
  // Bu fonksiyon özelinde (private)
  let balance = 0;

  // "alıcı"yı, genel(public) objeyi döndürerek yap
  function getBalance() {
    return balance;
  }
  
  // "ayarlayıcı"yı, genel(public) objeyi döndürerek yap
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

**[⬆ Başa dön](#İçindekiler)**

### Nesnelerin özel üyeleri olmasını sağlama

This can be accomplished through closures (for ES5 and below).

**Kötü:**

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

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

## **Sınıflar**

### ES5 düz fonksiyonlar yerine ES2015 / ES6 sınıflarını tercih et

Klasik ES5 sınıfları için okunabilir sınıf mirası, yapısı ve yöntem tanımları almak çok zordur. Miras almaya(inheritance) ihtiyacınız varsa (ve gerekmeyebileceğini unutmayın), ES2015 / ES6 sınıflarını tercih edin. Bununla birlikte, kendinizi daha büyük ve daha karmaşık nesnelere ihtiyaç duyana kadar küçük işlevlere tercih edin.

**Kötü:**

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

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

### Yöntem zincirini kullan

Bu desen JavaScript'te çok kullanışlıdır ve jQuery ve Lodash gibi birçok kütüphanede görürsünüz. Kodunuzun etkileyici ve daha az detaylı olmasını sağlar. Bu nedenle, diyorum ki, yöntem zincirleme kullanın ve kodunuzun ne kadar temiz olacağını göreceksiniz. Sınıf fonksiyonlarınızda, her fonksiyonun sonunda `this`'i döndürmeniz yeterlidir ve daha fazla sınıf yöntemini buna zincirleyebilirsiniz.

**Kötü:**

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

**İyi:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // NOT: Zincirleme için `this` döndür
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOT: Zincirleme için `this` döndür
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOT: Zincirleme için `this` döndür
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOT: Zincirleme için `this` döndür
    return this;
  }
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

**[⬆ Başa dön](#İçindekiler)**

### Miras yerine kompozisyon tercih et

Dörtlü çete tarafından başlatılan ünlü [_Tasarım Desenleri_](https://en.wikipedia.org/wiki/Design_Patterns) gibi, siz de kompozisyonu, miras bırakmaya yerine göre tercih etmelisiniz. Miras kullanmak için birçok iyi neden ve kompozisyon kullanmak için birçok iyi neden vardır. Bu maksimum nokta için ana nokta, zihniniz içgüdüsel olarak miras kullanma için giderse, kompozisyon sorununuzu daha iyi modelleyip değiştiremeyeceğini düşünmeye çalışın. Bazı durumlarda olabilir.

O zaman "ne zaman miras kullanmalıyım?" diye merak ediyor olabilirsiniz. O eldeki probleminize bağlıdır, ancak bu mirasın kompozisyondan daha mantıklı olduğu iyi bir listedir:

1. Mirasınız "has-a" değil, "is-a" ilişkisini temsil eder ilişki (İnsan-> Hayvan ve Kullanıcı-> KullanıcıAyrıntıları).
2. Temel sınıflardan kodu yeniden kullanabilirsiniz (İnsanlar tüm hayvanlar gibi hareket edebilir).
3. Temel sınıfı değiştirerek türetilmiş sınıflarda genel değişiklikler yapmak istiyorsunuz. (Hareket ettiklerinde tüm hayvanların kalori harcamalarını değiştirin).

**Kötü:**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Kötü çünkü Çalışanların(Employees) vergi bilgisi 'var'. ÇalışanVergiBilgisi(EmployeeTaxData) bir çeşit Çalışan(Employee) değil.
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

## **SOLID**

### Tek Sorumluluk İlkesi (SRP)

Temiz Kod'da belirtildiği gibi, "Bir sınıfın değişmesi için asla birden fazla sebep olmamalıdır". Bir sınıfı tıklım tıklım bir çok işlevsellikle doldurmak çekici gelebilir, tıpkı uçuşlarda yanına alabileceğiniz bir valiz gibi. Bununla ilgili sorun, sınıfınızın kavramsal olarak uyumlu olmayacağı ve değişmesi için birçok neden vereceği yönündedir. Bir sınıfı değiştirmek için ihtiyaç duyduğunuz sayıyı en aza indirmek önemlidir. Bir sınıfta çok fazla işlevsellik varsa ve bir parçasını değiştirirseniz, bunun kod tabanınızdaki diğer bağımlı modülleri nasıl etkileyeceğini anlamak zor olabilir.

**Kötü:**

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

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

### Açık / Kapalı Prensibi (OCP)

Bertrand Meyer tarafından belirtildiği gibi, "yazılım varlıkları (sınıflar, modüller, işlevler, vb.) Genişletme için açık, ancak değişiklik için kapalı olmalıdır." Bu ne anlama geliyor? Bu ilke, temel olarak kullanıcıların mevcut kodu değiştirmeden yeni işlevler eklemelerine izin vermeniz gerektiğini belirtir.

**Kötü:**

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

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

### Liskov’un Yerine Geçme Prensibi (LSP)

Bu çok basit bir kavram için korkutucu bir terimdir.Resmi olarak "S, T'nin bir alt tipiyse, o zaman T tipi nesnelerin yerine S tipi nesneler (yani, S tipi nesneler T programındaki nesnelerin yerine geçebilir), bu programın istenen özelliklerini değiştirmeden değiştirilebilir (doğruluk, yapılan görev vb.) " Bu daha da korkunç bir tanım.

Bunun için en iyi açıklama, bir üst sınıfınız ve bir alt sınıfınız varsa, temel sınıf ve alt sınıf yanlış sonuçlar elde etmeden birbirinin yerine kullanılabilir.Bu hala kafa karıştırıcı olabilir, bu yüzden klasik Kare Dikdörtgen örneğine bakalım. Matematiksel olarak, bir kare bir dikdörtgendir, ancak miras yoluyla "is-a" ilişkisini kullanarak model verirseniz, hızlı bir şekilde sorun yaşarsınız.

**Kötü:**

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
    const area = rectangle.getArea(); // KÖTÜ: Kare için 25 değerini döndürür. 20 olmalı.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

### Arayüzlerin Ayrımı Prensibi (ISP)

JavaScript'in arayüzleri yoktur, bu nedenle bu ilke diğerleri kadar kesin olarak geçerli değildir. Bununla birlikte, JavaScript'in tür sistemi eksikliğinde bile önemli ve alâkalıdır.

ISP, "kullanıcılar kullanmadığı arabirimlere bağımlı olmaya zorlanmamalıdır." der. Arabirimler, `Duck Typing` yüzünden JavaScript'de üstü kapalı anlaşmalardır.

Bu ilkeyi JavaScript'te gösteren iyi bir örnek, sınıflar büyük ayar nesneleri gerektirir. Kullanıcıların büyük miktarda seçenek ayarlamalarını istememek gerekli değildir, çünkü çoğu zaman tüm ayarlara ihtiyaç duymazlar. Bunları isteğe bağlı yapmak, bir "büyük arayüzü" olmasını önlemeye yardımcı olur.

**Kötü:**

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
  animationModule() {} // Coğunlukla, bunu canlandırmamız gerekmeyecek
  // ...
});
```

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

### Bağlılığı Tersine Çevirme Prensibi (DIP)

Bu ilke iki temel şeyi ifade eder:

1. Yüksek seviyeli modüller, düşük seviyeli modüllere bağlı olmamalıdır. Her ikisi de soyutlamalara bağlı olmalıdır.
2. Soyutlamalar detaylara bağlı olmamalıdır. Ayrıntılar soyutlamalara bağlı olmalıdır.

İlk başta bunu anlamak zor olabilir, ancak AngularJS ile çalıştıysanız, bu prensibin Bağımlılık Enjeksiyonu (DI) şeklinde bir uygulamasını gördünüz. Aynı kavramlar olmasalar da, DIP yüksek seviyede modüllerin düşük seviyeli modüllerinin detaylarını bilmelerini ve ayarlamalarını sağlar.
Bunu DI ile başarabilir. Bunun büyük bir yararı, modüller arasındaki bağlantıyı azaltmasıdır. Eşleştirme(Coupling) çok kötü bir gelişme modelidir çünkü
kodunuzu yeniden düzenleme zorlaştırır.

Daha önce belirtildiği gibi, JavaScript'in arayüzleri yoktur, bu nedenle soyutlamalar örtük sözleşmelere bağlıdır.
Yani, bir nesnenin / sınıfın başka bir nesneye / sınıfa maruz bıraktığı yöntemler ve özellikler. 
Aşağıdaki örnekte, örtük sözleşme, bir `InventoryTracker` için herhangi bir Request modülünün `requestItems` yöntemine sahip olacağıdır.

**Kötü:**

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

    // KÖTÜ: Belirli bir istek uygulamasına bağımlılık yarattık.
    // requestItems sadece `request` 'e bağlımlı olmalıdır.
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

**İyi:**

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

// Bağımlılıklarımızı harici olarak yapılandırarak ve enjekte ederek
// istek modülümüzü WebSockets kullanan yeni ve havalı bir modülle kolayca değiştirebiliriz.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ Başa dön](#İçindekiler)**

## **Testler**

Test etmek canlıya çıkmaktan bile önemlidir. Eğer testiniz yoksa  veya yetersiz bir miktardaysa, kodu her gönderdiğinizde hiçbir şeyin bozulmadığına emin olmazsınız. Neyin yeterli bir miktar oluşturduğuna karar vermek takımınıza bağlıdır, %100 kapsama sahip olmak (tüm ifadeler ve şubeler) size güven ve gönül rahatlığı sağlar. Bu, harika bir test framework'üne sahip olmanın yanı sıra, [iyi bir kapsama aracı](https://gotwarlost.github.io/istanbul/) kullanmanız gerektiği anlamına gelir

Test yazmamanın mazereti yoktur. Çok sayıda [iyi JS test framework'ü](https://jstherightway.org/#testing-tools) vardır, bu yüzden ekibinize uyan birini bulun.

Ekibiniz için uygun olanı bulduğunuzda, kullanılan her yeni özellik / modül için daima testler yazmayı hedefleyin. Tercih ettiğiniz yöntem Test Odaklı Geliştirme (TDD) ise, bu harika, ancak asıl mesele, herhangi bir özelliği başlatmadan veya mevcut bir özelliği yeniden düzenlemeden önce kapsama hedeflerinize ulaştığınızdan emin olmaktır.

### Her test için tek konsept

**Kötü:**

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

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

## **Tutarlılık**

### Promises kullanın, callback değil

'Callback'ler temiz değildir ve aşırı miktarda iç içe geçmeye neden olurlar. ES2015 / ES6 ile Promise'ler yerleşik bir global tiptir. Onları kullan!

**Kötü:**

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

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

### Async/Await kullanmak Promis kullanmaktan bile daha temiz

Promise'ler callback'lere göte çok daha temiz alternatiflerdir, ama ES2017/ES8 async ve await getirdi ve bu daha temiz bir çözüm sunuyor. Tek yapmanız gereken fonksiyonun başına `async` eklemek ve sonrasında mantıksal kullanımızı `then` zinciri kulanmadan yazabilirsiniz. Bugün ES2017 / ES8 özelliklerinden yararlanabiliyorsanız bunu kullanın!

**Kötü:**

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

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

## **Hata Yönetimi**

Atılan hatalar iyi bir şey! Bunlar, programınızdaki bir şey yanlış gittiğinde çalışma zamanının başarıyla tanımlandığı anlamına gelir ve sizi geçerli yığında (stack) fonksiyonu çalıştırmayı, işlevi dururup size konsolda yığın izlemede haber vererek yapar.

--- HERE

### Yakalanmış hataları görmemezlikten gelmeyin

Yakalanan bir hatayla hiçbir şey yapmamanız size söz konusu hatayı düzeltebilme veya tepki gösterme yeteneği vermez. Hatayı konsola (`console.log`) kaydetmek, konsola yazdırılan bir şey denizinde kaybolabileceği sıklıkta daha iyi değildir.

Herhangi bir kod parçasını  `try/catch` içerisinde kullanıyorsanız, orada bir hata olabileceğini düşündüğünüz anlamına gelir ve bu nedenle gerçekleştiği zaman için bir planınız olması veya bir kod yolu oluşturmanız gerekir.

**Kötü:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**İyi:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  //  Bir secenek (console.log'dan daha dikkat çekici)
  console.error(error);
  // Bir secenek daha
  notifyUserOfError(error);
  // Bir secenek daha
  reportErrorToService(error);
  // veya hepsini bir yapın
}
```

### Reddedilmiş promisleri görmemezlikten gelmeyin

Aynı sebeplerden dolayı, `try/catch`'de oluşan hataları yok saymamalısınız

**Kötü:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**İyi:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    // Bir secenek (console.log'dan daha dikkat çekici)
    console.error(error);
    // Bir secenek daha
    notifyUserOfError(error);
    // Bir secenek daha
    reportErrorToService(error);
    // veya hepsini bir yapın
  });
```

**[⬆ Başa dön](#İçindekiler)**

## **Biçimlendirme**

Biçimlendirme özneldir. Buradaki birçok kural gibi, uygulamanız gereken zor ve hızlı bir kural yoktur. Ana nokta biçimlendirme üzerinde tartışma DEĞİLDİR. Bunu otomatikleştirmek için [tonlarca araç](https://standardjs.com/rules.html) vardır. Birini kullan! Mühendislerin biçimlendirme konusunda tartışmaları zaman ve para kaybıdır.

Otomatik biçimlendirme (girintileme, sekmeler ve boşluklar, çift veya tek tırnak işaretleri vb.) Kapsamına girmeyen şeyler için bazı rehberlik için buraya bakın.

### Tutarlı büyük harf kullanımı

JavaScript türsüzdür, bu nedenle büyük / küçük harf kullanımı değişkenleriniz, işlevleriniz vb. Hakkında çok şey anlatır. Bu kurallar özneldir, böylece ekibiniz istediklerini seçebilir. Mesele şu ki, ne seçerseniz seçin, tutarlı olun.

**Kötü:**

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

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

### Fonksion çağıranları ve çağrılanları yakın olmalı

Eğer bir fonksiyon diğer fonksiyonu çağırıyorsa, dikey olarak bu fonksiyonları kaynak dosyasında yakın tutun. İdeal olan, fonksiyonu kullanan kullandığı fonksiyonun hemen üstünde olmasıdır. We tend to read code from top-to-bottom, like a newspaper.Bu nedenle, kodunuzu bu şekilde okuyun.

**Kötü:**

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

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

## **Yorumlar**

### Yalnızca iş mantığı karmaşıklığı olan şeyleri yorumlayın

Yorumlar aslında bir özür, şart değil. İyi kod _çoğunlukla_ kendini belgelemektedir.

**Kötü:**

```javascript
function hashIt(data) {
  // Karma
  let hash = 0;

  // String uzunluğu
  const length = data.length;

  // Verilerdeki her karakteri gözden geçirin
  for (let i = 0; i < length; i++) {
    // Karakter kodunu al
    const char = data.charCodeAt(i);
    // Karıştır
    hash = (hash << 5) - hash + char;
    // 32-bit tam sayıya dönüştür
    hash &= hash;
  }
}
```

**İyi:**

```javascript
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // 32-bit tam sayıya dönüştür
    hash &= hash;
  }
}
```

**[⬆ Başa dön](#İçindekiler)**

### Kodlarınızı yorum olarak bırakmayın

Versiyon kontrol'un var olmasının bir sebebi var. Eski kodlarınızı tarihin tozlu sayfalarında bırakın.

**Kötü:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**İyi:**

```javascript
doStuff();
```

**[⬆ Başa dön](#İçindekiler)**

### Günlük yorumları yapmayın

Hatırlayın, versiyon kontrol kullanın! Kullanılmayan koda, yoruma alınmış koda ve özellikle günlük kodlarına gerek yok. Geçmiş için `git log` kullanın.

**Kötü:**

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

**İyi:**

```javascript
function combine(a, b) {
  return a + b;
}
```

**[⬆ Başa dön](#İçindekiler)**

### Yer belirleyicilerden kaçının

Bunlar genellikle kirlilik yaratır. Fonksiyonların ve değişken adlarının yanı sıra uygun girinti ve biçimlendirme kodunuza görsel yapı kazandırsın.

**Kötü:**

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

**İyi:**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```

**[⬆ Başa dön](#İçindekiler)**

## Çeviriler

Ayrıca diğer dillerde de:

- ![am](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Armenia.png) **Armenian**: [hanumanum/clean-code-javascript/](https://github.com/hanumanum/clean-code-javascript)
- ![bd](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bangladesh.png) **Bangla(বাংলা)**: [InsomniacSabbir/clean-code-javascript/](https://github.com/InsomniacSabbir/clean-code-javascript/)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Simplified Chinese**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Traditional Chinese**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [GavBaros/clean-code-javascript-fr](https://github.com/GavBaros/clean-code-javascript-fr)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Indonesia**: [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**:
  - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
  - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Spanish**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [bsonmez/clean-code-javascript](https://github.com/bsonmez/clean-code-javascript)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)

**[⬆ Başa dön](#İçindekiler)**
