# clean-code-javascript

## içindekiler

1. [Giriş](#giriş)
2. [Değişkenler](#değişkenler)
3. [Fonksiyonlar](#fonksiyonlar)
4. [Nesneler Ve Veri Yapıları](#nesneler-ve-veri-yapıları)
5. [Classes](#classes)
6. [SOLID](#solid)
7. [Testing](#testing)
8. [Concurrency](#concurrency)
9. [Error Handling](#error-handling)
10. [Formatting](#formatting)
11. [Comments](#comments)
12. [Translation](#translation)

## Giriş

![Yazılım kalitesi tahmininin kaç höykürme sayısı olarak komik görüntüsü
okurken siz de höyküreceksiniz](https://www.osnews.com/images/comics/wtfm.jpg)

Yazlım mühendisliği prensipleri, Robert C. Martin'in kitabından 
[_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
JavaScript için uyarlandı. Bu belge bir kod yazma semantik rehberi değildir. Bu belge JavaScript ile [okunabilir, yeniden kullanılabilir ve elden geçirilebilir](https://github.com/ryanmcdermott/3rs-of-software-architecture) yazılım üretebilmek için kullanılabilecek bir rehber oluşturmak için yazıldı.

Buradaki her ilkeye kesinlikle uyulması gerekmiyor ve hatta bir çoğu evrensel 
olarak kabul edilmeyebilir. Bunlar yönergelerdir, daha fazlası değil, 
ama uzun yıllar boyunca edindikleri toplu tecrübe ile 
_Clean Code_ kitabı yazarlarınca derlenmiş olanlardır.

Yazılım mühendisliği zanaatı 50 yaşın biraz üzerinde ve hala çok şey öğreniyoruz. 
Yazılım mimarisi mimarlığın kendisi kadar eskidiğinde, belki o zaman uyması gereken 
daha sağlam kurallar olacaktır. Şimdilik, bu kuralların sizin ve ekibinizin ürettiği 
JavaScript kodunun kalitesini değerlendirmek için bir mihenk taşı olarak hizmet 
etmesine izin verin.

Bir şey daha var: bunları bilmek sizi hemen daha iyi bir yazılım geliştiricisi yapmaz 
ve bunlarla yıllarca çalışmış olmak hiç hata yapmayacağınız anlamına da gelmez. 
Her kod parçası, şekillendirilip son haline çevirilen ıslak kilin gibi ilk taslak 
olarak başlar. Son olarak, akranlarımızla birlikte gözden geçirdiğimiz zaman kusurları 
gideririz. İyileştirilmesi gereken ilk taslaklar için kendinize eziyet etmeyin. 
Bunun yerine kodu yorun!

## **Değişkenler**

### Anlamlı ve telafuz edilebilir değişken isimleri kullanın

**Yanlış:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Doğru:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ başa dön](#içindekiler)**

### Aynı tür değişken için aynı kelimeleri kullanın

**Yanlış:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Doğru:**

```javascript
getUser();
```

**[⬆ başa dön](#içindekiler)**

### Aranabilecek isimler kullanın

Yazdığımızdan daha çok kod satırı okuruz. Dolayısıyla yazdığımız kodun
okunabilir ve aranabilir olması önemlidir. Değişkenlerimize programımızın ne yapmaya çalıştığını 
anlatacak anlamlı isimler vermezsek kodumuzu okumaya çalışanlar çok üzülecektir. 
Verdiğiniz isimlerin kolayca aranabilir olmasını sağlayın. Bunun için
[buddy.js](https://github.com/danielstjules/buddy.js) ve
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
gibi araçlar size isimlendirme hatalarınızı gösterek yardımcı olabilir.

**Yanlış:**

```javascript
// Bu 86400000 ne anlama geliyor?
setTimeout(blastOff, 86400000);
```

**Doğru:**

```javascript
// Büyük harfli constant olarak tanımlayın.
const MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

**[⬆ başa dön](#içindekiler)**

### Açıklayıcı değişkenler kullanın

**Yanlış:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**Doğru:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ başa dön](#içindekiler)**

### Zihinsel Haritalamadan Kaçının

Açık olmak kapalı olmaktan daha iyidir.

**Yanlış:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Dur, bu l ne demekti?
  dispatch(l);
});
```

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

### Gereksiz bağlam ekleme

Eğer sınıf ya da nesne ne yaptığını söylüyprsa, değişken isminde tekrar 
belirtmenize gerek yok.

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

### Kısa kontrol (||) ya da kontrol kullanmak yerine ön tanımlı parametre kullanın

Ön tanımlı parametre kullanmak kısa kontrol yapılarında genellikle daha temiz kod oluşturur. Bu şekilde
kullandığınızda, kodunuzun sadece `undefined` olan parametreler için ön tanımlı değer sağlayacağını 
unutmayın. `''`, `""`, `false`, `null`, `0`, ve `NaN` gibi "hatalı" değerler ön tanımlı değer ile 
değiştirilmeyecektir.

**Yanlış:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**Doğru:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ başa dön](#içindekiler)**

## **Fonksiyonlar**

### Fonksiyon parametreleri (ideal olan 2 veya daha az olması)

Fonksiyon parametrelerinin sayısının sınırlandırılması, fonksiyonunuzun test 
edilmesini kolaylaştırdığı için inanılmaz derecede önemlidir. Üçten fazlaya 
sahip olmak, her bir ayrı argümanla tonlarca farklı durumu test etmeniz gereken 
bir kombinasyon patlamasına yol açar.

Bir veya iki argüman ideal durumdur ve mümkünse üçten kaçınılmalıdır.
Bundan fazlası tekrar düşünülmelidir. Çoğunlukla, 2 den fazla parametreye
sahip bir fonksiyonunuz varsa, yapması gerektiğinden fazla iş yapıyordur. 
Gerçekten gerekli olduğu durumda, çoğu zaman bir üst seviye nesne argüman 
olarak yeterli olacaktır.

JavaScript, çok fazla sınıf tanımlamadan anında nesneler oluşturmanıza izin 
verdiğinden, çok fazla argümana ihtiyaç duyduğunuzu tespit ediyorsanız, 
anlık oluşturduğunuz bir nesneyi kullanabilirsiniz..

Fonksiyonun hangi özellikleri beklediğini açıkça belirtmek için, ES2015/ES6 
imha sözdizimini ("destructuring syntax") kullanabilirsiniz. bunun bazı avantajları vardır:

1. Biri fonksiyonun tanımına baktığında, nelerin kullanıldığı oldukça
   açıktır.
2. İmha yöntemi, fonksiyona geçilen parametre nesnesinin belirtilen ilkel değerlerini 
   de klonlar. Bu, yan etkilerin önlenmesine yardımcı olabilir. 
   Not: parametre nesnesinden imha edilen nesneler ve diziler klonlanmaz.
3. Linter kontrolleri kullanılmayan özellikler hakkında sizi uyarır, ki bu imha yöntemi olmadan
   mümkün değildir.

**Yanlış:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

### Fonksiyonlar sadece bir iş yapmalı

Bu, yazılım mühendisliğinde belki de en önemli kuraldır. Eğer bir fonksiyon 
birden fazla iş yapıyorsa, bu fonksiyonu oluşturmaki test etmek ve anlamlandırmak zordur. 
Eğer bir fonksiyonu sadece bir işe yapacak şekilde sınırlandırırsanız, kolayca elden 
geçirilebilir ve kodunuz daha okunaklı olur. Bu rehberden birtek bunu alsanız bile, 
birçok geliştiriciden önde olacaksınız.

**Yanlış:**

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

**Doğru:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ başa dön](#içindekiler)**

### Fonksiyon isimleri fonksiyonun yaptığı işi anlatmalı

**Yanlış:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// Fonksiyon isminden neyin eklendiği anlaşılmıyor
addToDate(date, 1);
```

**Doğru:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ başa dön](#içindekiler)**

### Fonksiyonlar yalnızca bir seviye soyutlama olmalıdır

Birden fazla soyutlama seviyesine sahipseniz, fonksiyon genellikle çok 
fazla şey yapar. Fonksiyonları bölmek yeniden kullanılabilirliğe ve daha 
kolay testlere neden olur.

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

### Çoklanmış kodları kaldırın

Kod çoklanmasını engellemek için elinizden geleni yapın. Çoklanmış kod kötüdür çünkü 
bir sorun olduğunda ya da değiştirilmesi gerektiğinde ilgilenilmesi gereken birden fazla 
yer var demektir.

Bir restoran işlettiğinizi ve envanterinizi takip ettiğinizi düşünün: tüm domatesleriniz, 
soğanlarınız, sarımsaklarınız, baharatlarınız vb. onların içinde. Eğer birden fazla 
listeniz varsa, bir yemek yaptığınızda hepsini güncellemeniz gerekecektir. Yalnızca bir 
listeniz varsa, güncellenecek tek bir yer vardır!

Çoğunlukla, yinelenen kodunuz vardır, çünkü bir ortaklığı paylaşan iki veya 
daha fazla farklı işlem vardır, ancak farklılıkları sizi aynı şeyleri yapan 
iki veya daha fazla ayrı fonksiyona sahip olmaya zorlar. Çift kodun kaldırılması, 
bu farklı şeyleri tek bir işlev/modül/sınıfla işleyebilecek bir soyutlama 
oluşturmak anlamına gelir.

Soyutlamayı doğru yapmak çok önemlidir, bu yüzden _Classes_ bölümünde 
belirtilen SOLID ilkelerine uymalısınız. Kötü soyutlamalar yinelenen 
kodlardan daha kötü olabilir, bu yüzden dikkatli olun! Bunu söyledikten sonra, 
iyi bir soyutlama yapabilirseniz yapın! Kendinizi tekrar etmeyin, aksi halde, 
bir şeyi değiştirmek istediğinizde kendinizi birden çok yeri güncellerken bulacaksınız.

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

### Object.assign ile varsayılan nesneleri ayarlama

**Yanlış:**

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

**Doğru:**

```javascript
const menuConfig = {
  title: "Order",
  // Kullanıcı 'body' değerini atamamış
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

  // config artık bu şekilde: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ başa dön](#içindekiler)**

### Karar verici fonksiyon parametreleri kullanmayın

Karar verici parametreler bir fonksiyonun birden fazla iş yaptığını gösterir. Fonksiyonlar tek iş yapmalılar. Boolean bir değer üzerinden yapacağı işe karar veren fonksiyonları birden fazla fonksiyona bölün.

**Yanlış:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Doğru:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ başa dön](#içindekiler)**

### Yan etkilerden kaçınma (bölüm 1)

Bir fonksiyon, bir değeri almak ve başka bir değer veya değerler döndürmekten 
başka bir şey yaparsa, bir yan etki oluşturur. Bu yan etki bir dosyaya yazmak, 
bazı global değişkenleri değiştirmek veya yanlışlıkla tüm paranızı bir yabancıya 
bağlamak olabilir.

Zaman zaman bir programda yan etkilere ihtiyacınız olur. Önceki örnekte olduğu gibi, 
bir dosyaya yazmanız gerekebilir. Yapmak istediğiniz şey, bunu yaptığınız yeri 
merkezileştirmektir. Belirli bir dosyaya yazan çok sayıda fonksiyon ve sınıfa sahip olmayın. 
Bunu yapan bir servis oluşturun. Sadece bir tane.

Ana nokta, herhangi bir yapıya sahip olmadan nesneler arasında durum paylaşımı, herhangi 
bir şey tarafından yazılabilen değişken yanları kullanmak ve yan etkilerin ortaya çıktığı 
yerleri merkezileştirmemek gibi çok yapılan hatalarda kaçınmaktır. Bunu yapabilirseniz, diğer 
programcıların büyük çoğunluğundan daha mutlu olursunuz.

**Yanlış:**

```javascript
// Global değişken aşağıdaki fonksiyon tarafındna kullanılıyor.
// Eğer başka bir fonksiyon da bunu kullanıyorsa, bu artık array olduğu için sorun çıkarabilir..
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Doğru:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ başa dön](#içindekiler)**

### Yan etkilerden kaçınma (bölüm 1)

JavaScript'te, ilkel değerler değer olarak ve nesneler/diziler referans 
olarak  iletilir. Nesneler ve diziler söz konusu olduğunda, işleviniz bir 
alışveriş sepeti dizisinde bir değişiklik yaparsa, örneğin satın almak 
için bir öğe eklerse, bu `cart` dizisini kullanan diğer işlevler bu eklemeden 
etkilenir. Bu iyi olabilir, ancak kötü de olabilir. Bunu anlamak için aşağıdaki 
örneği düşünelim:

Kullanıcı, bir ağ isteğini başlatan ve `cart` dizisini sunucuya gönderen bir 
`satın alma` fonksiyonunu çağıran "Purchase" düğmesini tıklar. Kötü bir ağ 
bağlantısı nedeniyle, `satın alma` işlevinin isteği yeniden denemeye devam 
etmesi gerekir. Şimdi, bu arada kullanıcı yanlışlıkla ağ isteği başlamadan önce 
istemediği bir öğe üzerinde "Sepete Ekle" düğmesini yanlışlıkla tıklarsa ne olur? 
Bu gerçekleşirse ve ağ isteği başlarsa, o zaman satın alma işlevi yanlışlıkla 
eklenen öğeyi gönderir, çünkü istenmeyen bir öğe eklenerek değiştirilmiş bir 
`addItemToCart` işlevinin değiştirdiği bir alışveriş sepeti dizisine bir başvuru 
yapar.

`AddItemToCart` için her zaman `cart`'ı klonlamak, düzenlemek ve klonu geri göndermek 
için harika bir çözüm olacaktır. Bu, alışveriş sepetinin referansını tutan başka hiçbir 
fonksiyonun herhangi bir değişiklikten etkilenmemesini sağlar.

Bu yaklaşımda hatırlanması gereken iki uyarı:

1. Giriş nesnesini gerçekten değiştirmek istediğiniz durumlar olabilir, ancak bu programlama 
   yaklaşımını uyguladığınızda, bu durumların oldukça nadir olduğunu göreceksiniz. Çoğu şey yan 
   etkisi olmayacak şekilde yeniden yapılandırılabilir!

2. Büyük nesneleri klonlamak, performans açısından çok pahalı olabilir. Neyse ki, 
   bu uygulamada büyük bir sorun değil çünkü bu tür bir programlama yaklaşımının 
   hızlı ve hafıza yoğun olmamasına izin veren ve nesneleri ve dizileri elle 
   klonlamanızdan daha [verimli yapan kütüphaneler](https://facebook.github.io/immutable-js/) 
   var.

**Yanlış:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Doğru:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ başa dön](#içindekiler)**

### Global fonksiyonları değiştirmeyin

Global kapsamı kirletmek JavaScript'te kötü bir uygulamadır çünkü başka bir 
kütüphaneyle çakışabilirsiniz ve API'nizin kullanıcısı canlıda bir istisna 
ile karşılaşana bundan haberdar olmaz. Bir örnek düşünelim: ya JavaScript'in 
yerel `Array` yöntemini iki dizi arasındaki farkı gösterebilecek bir `diff` 
yöntemine sahip olarak genişletmek istiyorsanız? Yeni işlevinizi Array.prototype 
dosyasına yazabilirsiniz, ancak aynı şeyi yapmaya çalışan başka bir kitaplıkla 
çakışabilir. Ya bu diğer kütüphane dizinin ilk ve son elemanları arasındaki farkı 
bulmak için sadece `diff` kullanıyorsa? Bu yüzden sadece ES2015/ES6 sınıflarını kullanmak 
ve sadece `Array` sınıfını genişletmek daha iyi olur.

**Yanlış:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Doğru:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ başa dön](#içindekiler)**

### Imperative programlama yerine fonksiyonel programlamayı tercih edin

JavaScript, Haskell'in olduğu gibi fonksiyonel bir dil değildir, ancak fonksiyonel 
bir tadı vardır. Fonksiyonel diller daha temiz ve test edilmesi daha kolay olabilir. 
Yapabildiğiniz zaman bu programlama stilini tercih edin.

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

### Koşullamaları örtülü hale getirin (encapsulate)

**Yanlış:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**Doğru:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ başa dön](#içindekiler)**

### Ter koşullandırmalardan kaçının

**Yanlış:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Doğru:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ başa dön](#içindekiler)**

### Avoid conditionals

Bu imkansız bir iş gibi görünür. Bunu ilk duyduklarında, çoğu insan 
"bir if ifadesi olmadan nasıl bir şey yapabilirim?" der. Cevap, birçok 
durumda aynı görevi gerçekleştirmek için polimorfizmi kullanabileceğinizdir. 
İkinci soru genellikle, "peki bu harika ama neden bunu yapmak isteyeyim?", 
bu cevap ise daha önce öğrendiğimiz bir temiz kod kavramıdır: bir işlev yalnızca 
bir şey yapmalıdır. `İf` deyimine sahip sınıf ve işlevleriniz olduğunda, 
kullanıcınıza işlevinizin birden fazla şey yaptığını söylersiniz. Unutmayın, 
sadece bir şey yapın.

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

### Tip kontrolünden kaçınma (bölüm 1)

JavaScript'te veri tiplerini tanımlamak zorunlu değildir, yani işlevleriniz herhangi 
bir tipte parametre alabilir. Bazen bu özgürlükten sorun çıkarabiliyor ve fonksiyonlarınızda 
tip kontrolü yapmak cazip hale gelebiliyor. Bunu yapmaktan kaçınmanın birçok yolu vardır. 
Dikkate alınacak ilk şey tutarlı API'ler.

**Yanlış:**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**Doğru:**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ başa dön](#içindekiler)**

### Tip kontrolünden kaçınma (bölüm 2)

Dizeler ve tam sayılar gibi temel ilkel değerlerle çalışıyorsanız,ve polimorfizmi 
kullanamıyorsanız ancak hala tip kontrolü yapma ihtiyacı hissederseniz, TypeScript 
kullanmayı düşünmelisiniz. Standart JavaScript sözdiziminin üstüne statik yazma 
özelliği sağladığından normal JavaScript'e mükemmel bir alternatiftir. Normal 
JavaScript'i manuel olarak kontrol etmekdeki sorun, bunu iyi yapmanın, aldığınız 
sahte "tür güvenliği" nin kayda değer okunabilirlik için telafi etmeyecek kadar 
fazladan ek yük gerektirmesidir. JavaScript'inizi temiz tutun, iyi testler yazın 
ve iyi kod incelemeleri yapın. Aksi takdirde, hepsini yapın ama TypeScript ile 
(dediğim gibi, harika bir alternatif!).

**Yanlış:**

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

**Doğru:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ başa dön](#içindekiler)**

### Gereksiz optimizasyon yapmayın

Modern tarayıcılar çalışma zamanında bir çok optimizasyon ön tanımlı olarak yaparlar. 
Çoğu zaman, kendiniz optimize etmeye çalışıyorsanız, zamanınızı boşa harcıyorsunuz 
demektir. Nerelerde optimizasyonun eksik olduğunu görmek için 
[iyi kaynaklar](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) var. 
Buradaki sorunlara çözülene kadar optimizasyon yapmayı hedefleyin.

**Yanlış:**

```javascript
// Eski tarayıcılarda döngünün her turunda `list.length` ön belleğe alınmadığı için pahalıydı
// çünkğ her defa `list.length` tekrardan hesaplanıyordu. Modern tarayıcılarda bu sorun giderildi.
for (let i = 0; len = list.length; i < len; i++) {
  // ...
}
```

**Doğru:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ başa dön](#içindekiler)**

### Ölü kodları silin

Ölü kod çoklanmış kod kadar kötüdür. Kod yığınınızda tutulmaları için bir sebep 
yoktur. Eğer kullanılmıyorsa, ondan kurtulun! Sürüm kontrol sistemlerinde geriye 
dönebildiğiniz için lazım olduğunda bulabilirsiniz.

**Yanlış:**

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

**Doğru:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ başa dön](#içindekiler)**

## **Nesneler ve Veri Yapıları**

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

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

### Make objects have private members

This can be accomplished through closures (for ES5 and below).

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

## **Classes**

### Prefer ES2015/ES6 classes over ES5 plain functions

It's very difficult to get readable class inheritance, construction, and method
definitions for classical ES5 classes. If you need inheritance (and be aware
that you might not), then prefer ES2015/ES6 classes. However, prefer small functions over
classes until you find yourself needing larger and more complex objects.

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

### Use method chaining

This pattern is very useful in JavaScript and you see it in many libraries such
as jQuery and Lodash. It allows your code to be expressive, and less verbose.
For that reason, I say, use method chaining and take a look at how clean your code
will be. In your class functions, simply return `this` at the end of every function,
and you can chain further class methods onto it.

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

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

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

## **SOLID**

### Single Responsibility Principle (SRP)

As stated in Clean Code, "There should never be more than one reason for a class
to change". It's tempting to jam-pack a class with a lot of functionality, like
when you can only take one suitcase on your flight. The issue with this is
that your class won't be conceptually cohesive and it will give it many reasons
to change. Minimizing the amount of times you need to change a class is important.
It's important because if too much functionality is in one class and you modify
a piece of it, it can be difficult to understand how that will affect other
dependent modules in your codebase.

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

### Open/Closed Principle (OCP)

As stated by Bertrand Meyer, "software entities (classes, modules, functions,
etc.) should be open for extension, but closed for modification." What does that
mean though? This principle basically states that you should allow users to
add new functionalities without changing existing code.

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

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

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

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

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

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

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

## **Testing**

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

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

## **Concurrency**

### Use Promises, not callbacks

Callbacks aren't clean, and they cause excessive amounts of nesting. With ES2015/ES6,
Promises are a built-in global type. Use them!

**Yanlış:**

```javascript
import { get } from "request";
import { writeFile } from "fs";

get(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  (requestErr, response) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      writeFile("article.html", response.body, writeErr => {
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

**Doğru:**

```javascript
import { get } from "request";
import { writeFile } from "fs";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(response => {
    return writeFile("article.html", response);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**[⬆ başa dön](#içindekiler)**

### Async/Await are even cleaner than Promises

Promises are a very clean alternative to callbacks, but ES2017/ES8 brings async and await
which offer an even cleaner solution. All you need is a function that is prefixed
in an `async` keyword, and then you can write your logic imperatively without
a `then` chain of functions. Use this if you can take advantage of ES2017/ES8 features
today!

**Yanlış:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-promise";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(response => {
    return writeFile("article.html", response);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**Doğru:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-promise";

async function getCleanCodeArticle() {
  try {
    const response = await get(
      "https://en.wikipedia.org/wiki/Robert_Cecil_Martin"
    );
    await writeFile("article.html", response);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}
```

**[⬆ başa dön](#içindekiler)**

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

**Yanlış:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Doğru:**

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

**Yanlış:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

## **Formatting**

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

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

### Function callers and callees should be close

If a function calls another, keep those functions vertically close in the source
file. Ideally, keep the caller right above the callee. We tend to read code from
top-to-bottom, like a newspaper. Because of this, make your code read that way.

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

## **Comments**

### Only comment things that have business logic complexity.

Comments are an apology, not a requirement. Good code _mostly_ documents itself.

**Yanlış:**

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

**Doğru:**

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

**[⬆ başa dön](#içindekiler)**

### Don't leave commented out code in your codebase

Version control exists for a reason. Leave old code in your history.

**Yanlış:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Doğru:**

```javascript
doStuff();
```

**[⬆ başa dön](#içindekiler)**

### Don't have journal comments

Remember, use version control! There's no need for dead code, commented code,
and especially journal comments. Use `git log` to get history!

**Yanlış:**

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

**Doğru:**

```javascript
function combine(a, b) {
  return a + b;
}
```

**[⬆ başa dön](#içindekiler)**

### Avoid positional markers

They usually just add noise. Let the functions and variable names along with the
proper indentation and formatting give the visual structure to your code.

**Yanlış:**

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

**Doğru:**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```

**[⬆ başa dön](#içindekiler)**

## Translation

This is also available in other languages:

- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**:
  [GavBaros/clean-code-javascript-fr](https://github.com/GavBaros/clean-code-javascript-fr)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Spanish**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
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
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Indonesian**:
  [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**:
  [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)

**[⬆ başa dön](#içindekiler)**
