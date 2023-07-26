# clean-code-javascript

## İçindəkilər

1. [Giriş](#giriş)
2. [Dəyişənlər](#dəyişənlər)
3. [Funksiyalar](#funksiyalar)
4. [Obyekt və Data Sturkturlar](#obyekt-və-data-sturkturlar)
5. [Siniflər](#siniflər)
6. [SOLID](#solid)
7. [Testlər](#testlər)
8. [Asinxrom](#asinxrom)
9. [Xəta İdaraetmə](#xəta-İdaraetmə)
10. [Formatlaşdırma](#formatlaşdırma)
11. [Şərhlər](#şərhlər)
12. [Tərcümələr](#tərcümələr)

## Giriş

![İki fərqli tip kodu oxuyarkən verəcəyiniz reaksiyanın yumorostik təsviri.](https://www.osnews.com/images/comics/wtfm.jpg)


Robert C. Martinin proqram mühəndisliyi prinsiplərini özündə birləşdirən [_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) kitabının JavaScript versiyası. Bu üslub bələdçisi deyil. JavaScript-də [oxuna bilən, təkrar istifadə edilə bilən və yenidən redaktə edilə bilən](https://github.com/ryanmcdermott/3rs-of-software-architecture) kodların  yaradılması üçün bələdçidir.


Burada hər bir prinsipə ciddi şəkildə əməl edilməməlidir və daha azı hamılıqla qəbul ediləcəkdir. Bunlar təlimatlardır başqa bir şey deyil, lakin onlar _Clean Code_ müəllifləri tərəfindən uzun illər təcrübə əsasında kodlaşdırılıb.



Proqram mühəndisliyi sənətimizin 50 ildən bir qədər artıq yaşı var və biz hələ də çox şey öyrənirik. Proqram arxitekturası arxitekturanın özü qədər köhnə olduqda, bəlkə də o zaman riayət etməli olduğumuz daha çətin qaydalarımız olacaq. Hələlik bu təlimatlar sizin və komandanızın hazırladığı JavaScript kodunun keyfiyyətini artırmaq üçün rol oynayacaq.


Daha bir şey: bunları bilmək sizi dərhal daha yaxşı proqram tərtibatçısı etməyəcək və uzun illər onlarla işləmək səhv etməyəcəksiniz mənasına gəlmir.Hər bir kod parçası nəm gilin son formaya çevrilməsi kimi ilk qaralama olaraq başlayır.Nəhayət, həmkarlarımızla birlikdə nəzərdən keçirərkən qüsurları aradan qaldırırıq. Təkmilləşdirməyə ehtiyacı olan ilk qaralamalar üçün özünüzü incitməyin. Bunun əvəzinə kodu incidin!


## **Dəyişənlər**

### Mənalı və anlaşıqlı dəyişən adlarından istifadə edin

**Pis:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Yaxşı:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Eyni növ dəyişən üçün eyni metoddan istifadə edin

**Pis:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Yaxşı:**

```javascript
getUser();
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Axtarıla bilən adlardan istifadə edin

Biz yazacağımızdan daha çox kod oxuyacağıq. Yazdığımız kodun oxuna bilən və axtarıla bilən olması vacibdir. Dəyişənləri mənalı və axtarıla bilən adlarla _adlandırmamaqla_ biz oxucularımızı incidirik .
[buddy.js](https://github.com/danielstjules/buddy.js) və
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
adsız sabitləri müəyyən etməyə kömək edə bilər.

**Pis:**

```javascript
// 86400000 nəyə lazımdır?
setTimeout(blastOff, 86400000);
```

**Yaxşı:**

```javascript
//Onları böyük hərflə yazılmış sabitlər kimi adlandırın.
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

**[⬆ Başa qayıt](#İçindəkilər)**

### İzahedici dəyişənlərdən istifadə edin

**Pis:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**Yaxşı:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Ağlınızda planlamaqdan çəkinin

Açıq olmaq eyham etməkdən yaxşıdır.

**Pis:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  //Bir dəqiqə, l nə idi ?
  dispatch(l);
});
```

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

### Lazımsız kontekst əlavə etməyin

Sinif/obyekt adınız sizə bir şey deyirsə, bunu dəyişən adınızda təkrarlamayın.

**Pis:**

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

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

### Qısa yol və şərti parametrlər əvəzinə standart parametrlərdən istifadə edin

Standart arqumentlər ümumiyyətlə qısa dövrələrdən daha təmizdir. Nəzərə alın ki, onlardan istifadə etsəniz, funksiyanız yalnız `undefined` arqumentlər üçün standart dəyərlər təmin edəcək. `''`, `""`, `false`, `null`, `0`, və
`NaN` kimi , "saxta" dəyərlər standart bir deyerlə dəyiştirilə bilməz.



**Pis:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**Yaxşı:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ Başa qayıt](#İçindəkilər)**

## **Funksiyalar**

### Funksiya arqumentləri (ideal olaraq 2 və ya daha az)

Funksiya parametrlərinin miqdarının məhdudlaşdırılması olduqca vacibdir, çünki bu, funksiyanızı sınamağı asanlaşdırır. Üçdən çox olması kombinator partlayışına gətirib çıxarır ki, burada hər bir ayrı arqumentlə tonlarla fərqli işi sınamalısan.

Bir və ya iki arqument ideal haldır və mümkünsə üçdən qaçınmaq lazımdır. Bundan çoxu birləşdirilməlidir. Adətən, ikidən çox arqumentiniz varsa, funksiyanız çox şey etməyə çalışır. Olmadığı hallarda, çox vaxt daha yüksək səviyyəli bir obyekt arqument kimi kifayət edəcəkdir.

JavaScript sizə obyektlər yaratmağa imkan verdiyi üçün, bir çox obyektdən istifadə etmədən, bir çox sinif konstruksiyalarına ehtiyac olmadan bir obyektdən istifadə edə bilərsiniz.

Funksiyanın hansı özəllikləri daşıdığını aydınlaşdırmaq üçün ES2015 / ES6 təhlil sintaksisindən istifadə edə bilərsiniz. Bunun bir sıra üstünlükləri var:

1. Kimsə funksiya imzasına baxdıqda, hansı xüsusiyyətlərin istifadə edildiyi dərhal aydın olur.
2. Adlandırılmış parametrləri simulyasiya etmək üçün istifadə edilə bilər.
3. Təhlil prosesi həmçinin funksiyaya ötürülən arqument obyektinin müəyyən edilmiş ilkin qiymətlərini klonlaşdırır. Bu, yan təsirlərin qarşısını almağa kömək edə bilər. Qeyd: Arqument obyektlərindən təhlil edilən obyektlər və massivlər klonlaşdırılmır!
4. Linters sizi istifadə olunmamış xassələr barədə xəbərdar edə bilər ki, bu da sökülmədən qeyri-mümkündür.

**Pis:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

### Funksiyalar bir şeyi etməlidir

Bu proqram mühəndisliyində ən vacib qaydadır. Funksiyalar birdən çox şeyi yerinə yetirdikdə, onları tərtib etmək, test etmək və əsaslandırmaq daha çətindir. Bir funksiyanı yalnız bir hərəkətə cəlb etdiyiniz zaman, onu asanlıqla redaktə etmək olar və kodunuz daha təmiz oxunacaqdır.Bu məqalədən başqa bir şey almasanız belə, tək bununla bir çox tərtibatçıdan öndə olacaqsınız.

**Pis:**

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

**Yaxşı:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Funksiya adları nə etdiklərini bildirməlidir

**Pis:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// Funksiyanın adından onun nəyi əlavə etdiyini anlamaq çətindir.
addToDate(date, 1);
```

**Yaxşı:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Funksiyalar mücərrədləşdirmədən yalnız bir səviyyə uzaqda olmalıdır  

Birdən çox mücərrədləşdirmə səviyyəsinə malik olduğunuz zaman funksiyanız adətən çox iş görür. Funksiyaların bölünməsi təkrar istifadəyə və asan test edilməsinə səbəb olur.

**Pis:**

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

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

### Təkrarlanan kodu silin

Təkrarlanan kodun qarşısını almaq üçün əlinizdən gələni edin. Təkrarlanan kod pisdir, çünki bu o deməkdir ki, bəzi məntiqi dəyişməlisinizsə nəyisə dəyişdirmək üçün birdən çox yerdə dəyişdirməlisiniz.

Təsəvvür edin ki, siz restoran işlədirsinizsə və stoklarınızı izləyirsiniz: bütün pomidorlarınız, soğanlarınız və s. Əgər bunu saxladığınız bir çox siyahılarınız varsa, içərisində pomidor olan yeməyi təqdim edərkən hamısı yenilənməlidir. Yalnız bir siyahınız varsa, yeniləmək üçün yalnız bir yer var!

Çox vaxt Təkrarlanan kodunuz olur, çünki sizin iki və ya daha çox bir qədər fərqli şeylər, lakin çoxlu ortaq cəhətləri olan səhifələriniz olur. Təkrarlanan kodun silinməsi bu müxtəlif şeylər toplusunu yalnız bir funksiya/modul/siniflə idarə edə biləcək bir təsvir yaratmaq deməkdir.

Təsviri düzgün əldə etmək çox vacibdir, buna görə də siz _Siniflər_ bölməsində verilmiş SOLID prinsiplərinə əməl etməlisiniz. Pis təsvirlər təkrarlanan koddan daha pis ola bilər, ona görə də diqqətli olun! Bunu bildikdən sonra yaxşı bir təsvir edə bilirsinizsə, bunu edin! Kodunuzu təkrarlamayın, əks halda bir şeyi dəyişmək istədiyiniz zaman birdən çox yeri yeniləyəcəksiniz.

**Pis:**

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

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

### Object.assign ilə standart obyektləri təyin edin

**Pis:**

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

**Yaxşı:**

```javascript
const menuConfig = {
  title: "Order",
  // İstifadəçi 'body' əlavə etmədi.
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
  // config nəticəsi: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Funksiya parametirlərində işarələmə istifadə etmə

İşarələmələr funksiyanızın birdən çox iş gördüyünü göstərir. Funksiyalar yalnız bir şeyi etməlidir. Aşağıdakı kimi dəyişiklikləri və məntiqi operatorları olan funksiyalarınız varsa, funksiyalarınızı ayırın.


**Pis:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Yaxşı:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Yan təsirlərdən qaçın (1-ci hissə)

Funksiya bir dəyər götürüb başqa bir dəyər və ya dəyərləri qaytarmaqdan başqa bir şey edərsə, yan təsir yaradır. Yan təsiri  fayla yazmaq, bəzi qlobal dəyişəni dəyişdirmək və ya təsadüfən bütün pulunuzu bir qəribə ötürmək ola bilər.

Bu mühüm bir məsələdir. Zaman-zaman bir proqramda yan təsirlərə sahib olmağınız tələb oluna bilər. Əvvəlki nümunədə olduğu kimi, bir fayla yazmağınız lazım ola bilər. Sizə tövsiyə etdiyimiz, bu əməliyyatı mərkəzləşdirməkdir. Birdən çox yazma əməliyyatı yerinə yetirən funksiya və ya siniflər yaratmayın. Bunun üçün yalnız və yalnız bir xidmətiniz olsun.

Burada əsas məqam heç bir strukturu olmayan obyektlər arasında vəziyyəti paylaşmaq, hər hansı bir şeyə yazıla bilən dəyişkən məlumat növlərindən istifadə etmək və yan təsirlərinizin baş verdiyi yerləri mərkəzləşdirməmək kimi ümumi tələlərdən qaçmaqdır. Əgər bunu bacarsanız, digər proqramçıların böyük əksəriyyətindən daha xoşbəxt olacaqsınız.

**Pis:**

```javascript
// Aşağıdakı funksiya Qlobal dəyişənə istinad edir
// Bu addan istifadə edən başqa bir funksiyamız olsaydı, indi massiv olardı və onu pozardı.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Yaxşı:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Yan təsirlərdən qaçın (2-ci hissə)


JavaScript'de, bəzi dəyərlər dəyişdirilə bilməz (immutable) və bəziləri dəyişdirilə bilər (mutable). Obyektlər və sıralamalar (arrays) iki növ dəyişən dəyərdir, bu səbəbdən onları bir funksiyaya parametr kimi ötürdüyünüzdə diqqətli olmaq vacibdir. Bir JavaScript funksiyası obyektin xüsusiyyətlərini dəyişdirə və sıralamanın içindəki məzmunu dəyişdirə bilər ki, bu da başqa yerlərdə asanlıqla səhvlər yaradabilir.

Tutaq ki, alış-veriş səbətini təmsil edən massiv parametrini qəbul edən funksiya var. Əgər funksiya həmin alış-veriş səbəti massivində dəyişiklik edərsə - məsələn, almaq üçün element əlavə etməklə - o zaman həmin `səbət` massivdən istifadə edən hər hansı digər funksiya bu əlavədən təsirlənəcək. Bu əla ola bilər, amma pis də ola bilər. Gəlin pis bir vəziyyəti təsəvvür edək:

İstifadəçi `Al` düyməsini sıxır və bu alış funksiyası serverə sorğu göndərərək `səbət` massivini serverə göndərir. Zəif şəbəkə bağlantısı səbəbindən `Al` funksiyası sorğunu təkrar etməyə davam etməlidir.
İndi, bu arada, əgər istifadəçi şəbəkə sorğusu başlamazdan əvvəl istəmədiyi elementdə təsadüfən `Səbətə əlavə et` düyməsini klikləsə nə olacaq? 

Əgər bu baş verərsə və şəbəkə sorğusu başlayarsa, o, təsadüfən alınmış elementi göndərəcək, çünki onun `səbətə əlavə et` funksiyasının arzuolunmaz elementi əlavə etməklə dəyişdirdiyi `səbət` massivinə istinad var. Səbətə məhsul əlavə etmək həmişə səbəti klonlaşdırmaq, redaktə etmək və klonu qaytarmaq üçün əla həll olacaq. Bu, alış-veriş səbətinə istinadı saxlayan heç bir digər funksiyaların hər hansı dəyişiklikdən təsirlənməyəcəyini təmin edir.



Bu yanaşmaya dair iki xəbərdarlıq:

1. Giriş obyektini həqiqətən dəyişdirmək istədiyiniz hallar ola bilər, lakin bu proqramlaşdırma təcrübəsini qəbul etdiyiniz zaman bu halların olduqca nadir olduğunu görəcəksiniz. Əksər şeylər heç bir yan təsir göstərməmək üçün yenidən düzəldilə bilər!

2. Böyük obyektlərin klonlanması performans baxımından çox bahalı ola bilər. Xoşbəxtlikdən, praktikada bu böyük problem deyil, çünki bu cür proqramlaşdırma yanaşmasının sürətli olmasına və obyektləri və massivləri əl ilə klonlamaq üçün yaddaş tutumunu tələb etməyən [böyük kitabxanalar](https://facebook.github.io/immutable-js/) var .


**Pis:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Yaxşı:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Qlobal funksiyalara yazmayın


Qlobalları çirkləndirmək JavaScript-də pis bir təcrübədir, çünki siz başqa kitabxana ilə toqquşa bilərsiniz və API istifadəçisi proyekt istifadəyə verilənə qədər bu halın fərqinə varmıya bilər.

Məsələn: Tutaq ki, siz JavaScript-in doğma kitabxanasında massiv `diff` metodunu genişləndirmək və iki massivin fərqlərini göstərmək istəyirsiniz.
JavaScript-in yerli Array metodunu iki massiv arasındakı fərqi göstərə bilən "diff" metoduna genişləndirmək istəyirsiniz?
Siz yeni funksiyanızı `Array.prototype`-da yaza bilərsiniz, lakin o, eyni şeyi etməyə çalışan başqa kitabxana ilə ziddiyyət təşkil edə bilər. Bəs əgər digər kitabxana massivin birinci və sonuncu elementləri arasındakı fərqi tapmaq üçün sadəcə olaraq `diff` istifadə edirsə? Beləliklə, sadəcə ES2015 / ES6 siniflərindən istifadə etmək və Array qlobalı genişləndirmək daha yaxşı olardı.


**Pis:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Yaxşı:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ Başa qayıt](#İçindəkilər)**

### İmperativ proqramlaşdırmadan daha çox funksional proqramlaşdırmaya üstünlük verin

JavaScript Haskell kimi funksional bir dil deyil, lakin onun funksional ləzzəti var. Funksional dillər daha təmiz və sınaqdan keçirilməsi asan ola bilər. Bacardığınız zaman bu proqramlaşdırma tərzinə üstünlük verin.

**Pis:**

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

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

### Şərtiləri əhatə edin

**Pis:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**Yaxşı:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Mənfi şərtlərdən çəkinin

**Pis:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Yaxşı:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Şərtlərdən çəkinin

Bu, qeyri-mümkün bir iş kimi görünür. Bunu ilk dəfə eşidəndə əksəriyyət “ifadəsiz mən necə bir iş görüm `if`?” deyir. Cavab budur ki, bir çox hallarda eyni tapşırığa nail olmaq üçün polimorfizmdən istifadə edə bilərsiniz. İkinci sual adətən, "yaxşı, bu əladır, amma niyə bunu etmək istərdim?" Cavab öyrəndiyimiz əvvəlki təmiz kod anlayışıdır: funksiya yalnız bir işi görməlidir. İfadələri olan siniflər və funksiyalarınız olduqda `if`, istifadəçiyə funksiyanızın birdən çox şey etdiyini söyləyirsiniz. Unutma, yalnız bir şey et.

**Pis:**

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

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

### Növ yoxlamasından çəkinin (1-ci hissə)

JavaScript tipsizdir, yəni funksiyalarınız istənilən növ arqument qəbul edə bilər. Bəzən bu azadlıq sizi dişləyir və funksiyalarınızda tip yoxlaması etmək cazibədar olur. Bunu etmək məcburiyyətində qalmamağın bir çox yolu var. Nəzərə alınacaq ilk şey ardıcıl API-lərdir.

**Pis:**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**Yaxşı:**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Növ yoxlamasından çəkinin (2-ci hissə)

Əgər siz sətirlər və tam ədədlər kimi əsas primitiv dəyərlərlə işləyirsinizsə və polimorfizmdən istifadə edə bilmirsinizsə, lakin hələ də yazın yoxlanılması ehtiyacı hiss edirsinizsə, TypeScript-dən istifadə etməyi düşünməlisiniz. Bu, adi JavaScript-ə əla alternativdir, çünki standart JavaScript sintaksisinin üstündə statik yazmağı təmin edir. Normal JavaScript-ni əl ilə yazın yoxlanılması ilə bağlı problem ondan ibarətdir ki, bunu yaxşı etmək o qədər əlavə söz tələb edir ki, əldə etdiyiniz saxta "tip-təhlükəsizlik" itirilmiş oxunaqlılığı kompensasiya etmir. JavaScript-inizi təmiz saxlayın, yaxşı testlər yazın və yaxşı kod nəzərdən keçirin. Əks halda, TypeScript-dən başqa bütün bunları edin (bu, dediyim kimi, əla alternativdir!).



**Pis:**

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

**Yaxşı:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Həddindən artıq optimallaşdırmayın

Müasir brauzerlər iş zamanı başlıq altında çoxlu optimallaşdırmalar edirlər. Çox vaxt, əgər siz optimallaşdırırsınızsa, sadəcə vaxtınızı itirirsiniz. Optimallaşdırmanın çatışmadığı yerləri görmək üçün [yaxşı mənbələr](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers) var . Mümkünsə, düzələnə qədər onları hədəfləyin.



**Pis:**

```javascript
//Köhnə brauzerlərdə “list.length”  olmayan hər bir yeniləmə baha başa gələcək
// Çünki "list.length" yenidən hesablanması. Müasir brauzerlərdə optimallaşdırılıb.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Yaxşı:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ Başa qayıt](#İçindəkilər)**

### İstifadə olunmayan kodları silin

Ölü kod dublikat kod qədər pisdir. Onu kod bazanızda saxlamaq üçün heç bir səbəb yoxdur. Əgər çağırılmırsa, ondan qurtulun! Sizə hələ də ehtiyacınız varsa, o, versiya tarixçənizdə hələ də təhlükəsiz olacaq.

**Pis:**

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

**Yaxşı:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ Başa qayıt](#İçindəkilər)**

## **Obyekt və Data Sturkturlar**

### Alıcı(getters) və təyinedici(setters) istifadə edin

Obyektlərdəki məlumatlara daxil olmaq üçün alıcılardan və təyinedicilərdən istifadə obyektdə xassə axtarmaqdan daha yaxşı ola bilər. "Niyə?" soruşa bilərsən. Yaxşı, buda səbəblərin siyahısı:

- Obyekt xassəsini əldə etməkdən daha çox şey etmək istədiyiniz zaman kod bazanızdakı hər bir aksessuarı axtarıb dəyişmək lazım deyil.
  to look up and change every accessor in your codebase.
- `set` sadə şəkildə həyata keçirilə bilər.
- Daxili təmsili əhatə edir.
- Alarkən və qurarkən giriş və səhvlərin idarə edilməsini(error handling) əlavə etmək asandır.
- Siz obyektinizin xassələrini tənbəl(lazy load) yükləyə bilərsiniz, deyək ki, onu serverdən əldə edə bilərsiniz.
**Pis:**

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

**Yaxşı:**

```javascript
function makeBankAccount() {
  // Bu funksiya özəldir (private)
  let balance = 0;

  //  "Alıcı",  İctimai(public) obyekti qaytarmaqla et
  function getBalance() {
    return balance;
  }

  //  "Təyinedici",  İctimai(public) obyekti qaytarmaqla et
  function setBalance(amount) {
    // ... balansı yeniləməzdən əvvəl doğrulayın
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

**[⬆ Başa qayıt](#İçindəkilər)**

### Obyektlərin şəxsi üzvlərinə sahib olun

Bu, bağlamalar vasitəsilə həyata keçirilə bilər (ES5 və aşağıda).

**Pis:**

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

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

## **Siniflər**

### ES5 sadə funksiyalarındansa ES2015/ES6 siniflərinə üstünlük verin

Klassik ES5 sinifləri üçün oxunaqlı sinif irsi, quruluşu və metod təriflərini əldə etmək çox çətindir. Əgər varisliyə(inheritance) ehtiyacınız varsa (Nəzərə alın ki, ehtiyacınız olmaya bilər), o zaman ES2015/ES6 siniflərinə üstünlük verin. Bununla belə, daha böyük və daha mürəkkəb obyektlərə ehtiyac duyduğunuzu görənə qədər siniflər üzərində kiçik funksiyalara üstünlük verin.
**Pis:**

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

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

### Zəncirləmə metodundan istifadə edin

Bu nümunə JavaScript-də çox faydalıdır və siz onu jQuery və Lodash kimi bir çox kitabxanada görürsünüz. Bu, kodunuzun ifadəli və daha az təfərrüatlı olmasına imkan verir. Bu səbəbdən deyirəm ki, zəncirləmə metodundan istifadə edin və kodunuzun nə qədər təmiz olacağına baxın. Sinif funksiyalarınızda sadəcə olaraq `this` hər funksiyanın sonunda qayıdın və siz ona əlavə sinif metodlarını birləşdirə bilərsiniz.

**Pis:**

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

**Yaxşı:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // QEYD: Zəncirləmə üçün "this" qaytarın
    return this;
  }

  setModel(model) {
    this.model = model;
       // QEYD: Zəncirləmə üçün "this" qaytarın
    return this;
  }

  setColor(color) {
    this.color = color;
       // QEYD: Zəncirləmə üçün "this" qaytarın
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
       // QEYD: Zəncirləmə üçün "this" qaytarın
    return this;
  }
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Vərəsəlikdən kompozisiyaya üstünlük verin

Dördlər Dəstəsinin [Dizayn Nümunələrində](https://en.wikipedia.org/wiki/Design_Patterns) deyildiyi kimi , bacardığınız yerdə mirasdan daha çox kompozisiyaya üstünlük verməlisiniz. Mirasdan istifadə etmək üçün çoxlu yaxşı səbəblər və kompozisiyadan istifadə etmək üçün çoxlu yaxşı səbəblər var. Bu maksim üçün əsas məqam ondan ibarətdir ki, əgər ağlınız instinktiv olaraq miras almağa gedirsə, kompozisiyanın probleminizi daha yaxşı modelləşdirə biləcəyini düşünməyə çalışın. Bəzi hallarda ola bilər.




Bəs, "mən mirasdan nə vaxt istifadə etməliyəm?" Bu, əlinizdə olan probleminizdən asılıdır, lakin bu, mirasın kompozisiyadan daha mənalı olduğu zamanların layiqli siyahısıdır:

1. Sizin mirasınız "has-a" əlaqəsini deyil, "is-a" əlaqəsini təmsil edir (İnsan->Heyvan vs. İstifadəçi->İstifadəçi təfərrüatları).
2. Siz əsas siniflərdən kodu təkrar istifadə edə bilərsiniz (İnsanlar bütün heyvanlar kimi hərəkət edə bilər).
3. Siz əsas sinfi dəyişdirərək törəmə siniflərə qlobal dəyişikliklər etmək istəyirsiniz. (Hərəkət edərkən bütün heyvanların kalori xərclərini dəyişdirin).

**Pis:**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Pisdir, çünki İşçilər vergi məlumatlarına malikdirlər. İşçilərinvergiməlumatları(EmployeeTaxData) bir növ İşçi (İşçi) deyil.
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

## **SOLID**

### Vahid Məsuliyyət Prinsipi (SRP)

Təmiz Kodeksdə deyildiyi kimi, "Sinifin dəyişməsi üçün heç vaxt birdən çox səbəb olmamalıdır". Uçuşunuza yalnız bir çamadan götürə bildiyiniz zaman kimi bir çox funksionallığı olan bir sinfi yığmaq cazibədardır. Bununla bağlı problem ondadır ki, sinifiniz konseptual olaraq birləşməyəcək və bu, ona dəyişmək üçün bir çox səbəb verəcək. Bir sinfi dəyişmək üçün lazım olan vaxtların sayını minimuma endirmək vacibdir. Bu vacibdir, çünki bir sinifdə çox funksionallıq varsa və siz onun bir hissəsini dəyişdirsəniz, bunun kod bazanızdakı digər asılı modullara necə təsir edəcəyini anlamaq çətin ola bilər.

**Pis:**

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

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

### Açıq/Qapalı Prinsip (OCP)

Bertrand Meyerin qeyd etdiyi kimi, “proqram təminatı obyektləri (siniflər, modullar, funksiyalar və s.) genişləndirmək üçün açıq, lakin modifikasiya üçün bağlanmalıdır”. Bəs bu nə deməkdir? Bu prinsip əsasən bildirir ki, siz istifadəçilərə mövcud kodu dəyişmədən yeni funksiyalar əlavə etməyə icazə verməlisiniz.

**Pis:**

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
        // cavabı çevirmək və qayıtmaq
      });
    } else if (this.adapter.name === "nodeAdapter") {
      return makeHttpCall(url).then(response => {
        // cavabı çevirmək və qayıtmaq
      });
    }
  }
}

function makeAjaxCall(url) {
  // tələb və geri qaytarma 
}

function makeHttpCall(url) {
  // tələb və geri qaytarma 
}
```

**Yaxşı:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }

  request(url) {
    // tələb və geri qaytarma 
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }

  request(url) {
    // tələb və geri qaytarma 
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

**[⬆ Başa qayıt](#İçindəkilər)**

### Liskov Əvəzetmə Prinsipi (LSP)

Bu, çox sadə bir konsepsiya üçün qorxulu bir termindir. Formal olaraq "S T-nin alt növüdürsə, T tipli obyektlər S tipli obyektlərlə əvəz edilə bilər (yəni, S tipli obyektlər T tipli obyektləri əvəz edə bilər) bu proqramın istənilən xassələrini dəyişdirmədən (düzgünlük, yerinə yetirilən tapşırıq və s.)." Bu daha qorxulu tərifdir.

Bunun ən yaxşı izahı odur ki, əgər valideyn(parent) sinifiniz və uşaq(child) sinifiniz varsa, o zaman əsas sinif və uşaq sinif səhv nəticələr əldə etmədən bir-birini əvəz edə bilər. Bu hələ də çaşdırıcı ola bilər, ona görə də klassik Kvadrat-Dördbucaqlı nümunəsinə nəzər salaq. Riyazi olaraq kvadrat düzbucaqlıdır, lakin siz onu miras yolu ilə “is-a” münasibətindən istifadə edərək modelləşdirsəniz, tez bir zamanda problem yaranır.

**Pis:**

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
    const area = rectangle.getArea(); // Pis: kıvadırat üçün 25 qaytarır.20 olmalıdır.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

### İnterfeys Ayrılma Prinsipi (ISP)

JavaScript-in interfeysləri yoxdur, ona görə də bu prinsip digərləri kimi ciddi şəkildə tətbiq edilmir. Bununla belə, JavaScript-in tip sisteminin olmaması ilə belə vacib və aktualdır.

ISP bildirir ki, "Müştərilər istifadə etmədikləri interfeyslərdən asılı olmağa məcbur edilməməlidirlər." İnterfeyslər `Duck Typing` səbəbindən JavaScript-də gizli müqavilələrdir.

JavaScript-də bu prinsipi nümayiş etdirən yaxşı nümunə böyük parametrlər obyektləri tələb edən siniflər üçündür. Müştərilərdən böyük miqdarda seçimlər qurmağı tələb etməmək faydalıdır, çünki çox vaxt onlar bütün parametrlərə ehtiyac duymayacaqlar. Onları isteğe bağlı etmək "yağ interfeysi"nin qarşısını almağa kömək edir.

**Pis:**

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
  animationModule() {} // Çox vaxt bunu canlandırmaq lazım olmayacaq
  // ...
});
```

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

### Asılılığın inversiya prinsipi (DIP)

Bu prinsip iki əsas şeyi ifadə edir:

1. Yüksək səviyyəli modullar aşağı səviyyəli modullardan asılı olmamalıdır. Hər ikisi abstraksiyalardan asılı olmalıdır.
2. Abstraksiyalar təfərrüatlardan asılı olmamalıdır. Detallar abstraksiyalardan asılı olmalıdır.

Əvvəlcə bunu başa düşmək çətin ola bilər, lakin siz AngularJS ilə işləmisinizsə, bu prinsipin Dependency Injection (DI) şəklində həyata keçirildiyini görmüsünüz. Eyni anlayışlar olmasa da, DIP yüksək səviyyəli modulları aşağı səviyyəli modullarının təfərrüatlarını bilməkdən və onları qurmaqdan saxlayır. Bunu DI vasitəsilə həyata keçirə bilər. Bunun böyük bir faydası modullar arasındakı əlaqəni azaltmasıdır. Birləşmə çox pis inkişaf nümunəsidir, çünki kodunuzun yenidən qurulmasını çətinləşdirir.

Daha əvvəl qeyd edildiyi kimi, JavaScript-in interfeysləri yoxdur, ona görə də asılı olan abstraksiyalar gizli müqavilələrdir. Yəni, bir obyektin/sinfin başqa bir obyektə/sinifa məruz qoyduğu metodlar və xüsusiyyətlər. Aşağıdakı misalda gizli müqavilə ondan ibarətdir ki, `InventoryTracker` üçün hər hansı Sorğu modulunda `requestItems` metodu olacaq.

**Pis:**

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

    // PİS: Biz xüsusi sorğunun həyata keçirilməsindən asılılıq yaratdıq.
    // Sadəcə olaraq, sorğu elementlərinin(requestItems) sorğu metodundan asılı olması lazımdır: `request`
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

**Yaxşı:**

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

// Asılılığımızı xaricdən qurmaqla və onları inyeksiya etməklə asanlıqla edə bilərik
// Sorğu modulumuzu WebSockets istifadə edən dəbdəbəli yenisi ilə əvəz edin.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ Başa qayıt](#İçindəkilər)**

## **Testlər**

Test canlı yayımdan daha vacibdir. Əgər testiniz yoxdursa, kodu hər dəfə təqdim edəndə nəyinsə pozulduğuna əmin olmayacaqsınız. 100% əhatəyə malik olmaq (bütün bəyanatlar və filiallar) sizə inam və dinclik bəxş edən kifayət qədər nədən ibarət olduğuna qərar vermək sizin komandanızdan asılıdır.
Bu o deməkdir ki, əla sınaq çərçivəsinə malik olmaqla yanaşı, [yaxşı əhatə aləti](https://gotwarlost.github.io/istanbul/) istifadə etməlisiniz.


Testləri yazmamaq üçün heç bir bəhanə yoxdur. [Çoxlu yaxşı JS test çərçivələri](https://jstherightway.org/#testing-tools) var, ona görə də komandanızın üstünlük verdiyi birini tapın. Komandanız üçün işləyən birini tapdığınız zaman, təqdim etdiyiniz hər yeni funksiya/modul üçün həmişə testlər yazmağı hədəfləyin. Tercih etdiyiniz metod Test Əsaslı İnkişafdırsa (TDD), bu əladır, lakin əsas məqam hər hansı bir funksiyanı işə salmazdan və ya mövcud funksiyanı yenidən nəzərdən keçirməzdən əvvəl əhatə dairəsi məqsədlərinizə çatdığınızdan əmin olmaqdır.

### Hər test üçün tək konsepsiya

**Pis:**

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

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

## **Asinxrom**

### Promise istifadə et, callback deyil.

callback səliqəli deyil və həddindən artıq miqdarda qarışıqlığa səbəb olur. ES2015/ES6 ilə Promises daxili qlobal tipdir. Onlardan istifadə edin!

**Pis:**

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

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

### Async/Await Promisedən daha təmizdir

Promise'lər callback'lərə görə çox daha təmiz alternativdir.Lakin ES2017/ES8 daha təmiz həll təklif edir. Sadəcə funksiyanın başına `async`və sonra siz məntiqinizi `then` funksiyalar zənciri olmadan yaza bilərsiniz.Bu gün ES2017/ES8 xüsusiyyətlərindən yararlana bilsəniz, bundan istifadə edin!

**Pis:**

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

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

## **Xəta İdaraetmə**

Atılan səhvlər yaxşı bir şeydir! Onlar o deməkdir ki, icra müddətiniz proqramınızda nəyinsə səhv getdiyini uğurla müəyyən edib və o, cari yığında funksiyanın icrasını dayandırmaqla, prosesi (Node-da) dayandırmaqla və yığın izi ilə konsolda sizi xəbərdar etməklə sizə xəbər verir.

### Səhvləri görməzdən gəlməyin

Tutulan xəta ilə heç nə etməmək sizə qeyd olunan xətanı düzəltmək və ya ona reaksiya vermək imkanı vermir.Konsolda(`console.log`)  qeyd olunan səhvlər konsolda qeyd olunan şeylər arasında itə bilər.Hər hansı bir kodu `try/catch` bloku ilə bürümək deməkdir ki, o yerdə bir səhv baş verə biləcəyinizi düşünürsünüz və bu səhvin baş verdiyi vaxt üçün bir planınız, ya da onun baş verdiyi hal üçün bir kod yolunuz olmalıdır.

**Pis:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Yaxşı:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // Bir seçim (console.log-dan daha nəzərə çarpan)
  console.error(error);
  // Başqa seçim:
  notifyUserOfError(error);
  // Başqa seçim:
  reportErrorToService(error);
  // Ya da hamısını edin!
}
```

### Rədd edilmiş promisləri görməzdən gəlməyin

Eyni səbəblərə görə siz `try/catch` zamanı baş verən xətaları nəzərdən qaçırmamalısınız

**Pis:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**Yaxşı:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    // Bir seçim (console.log-dan daha nəzərə çarpan)
    console.error(error);
    //  Başqa seçim:
    notifyUserOfError(error);
    //  Başqa seçim:
    reportErrorToService(error);
    // Ya da hamısını edin!
  });
```

**[⬆ Başa qayıt](#İçindəkilər)**

## **Formatlaşdırma**

Formatlaşdırma subyektivdir. Buradakı bir çox qaydalar kimi, riayət etməli olduğunuz çətin və sürətli qayda yoxdur. Əsas odur ki, formatla bağlı MÜBAHİSƏ ETMƏYİN. Bunu avtomatlaşdırmaq üçün [çoxlu alətlər](https://standardjs.com/rules.html) var . Birini istifadə edin! Mühəndislərin formatlama üzərində mübahisə etməsi vaxt və pul itkisidir.

Avtomatik formatlaşdırmanın səlahiyyətlərinə aid olmayan şeylər üçün (gizinti, nişanlar və boşluqlar, cüt və tək dırnaq işarələri və s.) bəzi təlimatlar üçün buraya baxın.

### Ardıcıl böyük hərflərdən istifadə edin

JavaScript tipsizdir, ona görə də böyük hərf sizə dəyişənləriniz, funksiyalarınız və s. haqqında çox şey deyir. Bu qaydalar subyektivdir, ona görə də komandanız istədiklərini seçə bilər. Məsələ ondadır ki, hamınız nə seçsəniz də, sadəcə ardıcıl olun.

**Pis:**

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

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

### Funksiya çağıranlar  və çağırılanlar yaxın olmalıdır

Əgər funksiya digərini çağırırsa, mənbə faylında həmin funksiyaları şaquli olaraq yaxın saxlayın. İdeal olaraq, zəng edəni zəng edənin üstündə saxlayın. Biz qəzet kimi kodu yuxarıdan aşağıya oxumağa meylli oluruq. Buna görə kodunuzun oxunmasını təmin edin.

**Pis:**

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

**Yaxşı:**

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

**[⬆ Başa qayıt](#İçindəkilər)**

## **Şərhlər**

### Yalnız iş məntiqi mürəkkəbliyi olan şeyləri şərh edin.

Şərhlər üzrxahlıqdır, tələb deyil. Yaxşı kod _əsasən_ özünü sənədləşdirir.



**Pis:**

```javascript
function hashIt(data) {
  // Karma
  let hash = 0;

  // Sətrin uzunluğu
  const length = data.length;

  // Məlumatdakı hər bir simvolu nəzərdən keçirin
  for (let i = 0; i < length; i++) {
    // Simvol kodunu əldə edin
    const char = data.charCodeAt(i);
    // Qarışdır
    hash = (hash << 5) - hash + char;
    // 32 bitlik tam ədədə çevirin
    hash &= hash;
  }
}
```

**Yaxşı:**

```javascript
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // 32 bitlik tam ədədə çevirin
    hash &= hash;
  }
}
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Kodunuzu  şərh edilmiş kod olaraq tərk etməyin

Versiya nəzarəti bir səbəbdən mövcuddur. Köhnə kodu tarixçənizdə buraxın.

**Pis:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Yaxşı:**

```javascript
doStuff();
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Gündəlik şərhlər verməyin

Unutmayın, versiya nəzarətindən(`GİT`) istifadə edin! İstifadə edilməmiş koda, şərh edilmiş kodlara və xüsusən də log kodlarına ehtiyac yoxdur. Tarix üçün `git log` istifadə edin.

**Pis:**

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

**Yaxşı:**

```javascript
function combine(a, b) {
  return a + b;
}
```

**[⬆ Başa qayıt](#İçindəkilər)**

### Mövqe işarələrindən çəkinin

Bunlar çox vaxt çirklənmə yaradır. Düzgün girinti və formatlaşdırma, həmçinin funksiyalar və dəyişən adlar kodunuza vizual quruluş versin.

**Pis:**

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

**Yaxşı:**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```

**[⬆ Başa qayıt](#İçindəkilər)**

## Tərcümələr

Bu digər dillərdə də mövcuddur:
- ![aze](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Azerbaijan.png) **Azerbaijani**: [huseynovelmir/clean-code-javascript/](https://github.com/huseynovelmir/clean-code-javascript)
- ![am](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Armenia.png) **Armenian**: [hanumanum/clean-code-javascript/](https://github.com/hanumanum/clean-code-javascript)
- ![bd](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bangladesh.png) **Bangla(বাংলা)**: [InsomniacSabbir/clean-code-javascript/](https://github.com/InsomniacSabbir/clean-code-javascript/)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Simplified Chinese**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Traditional Chinese**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [eugene-augier/clean-code-javascript-fr](https://github.com/eugene-augier/clean-code-javascript-fr)
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
- ![rs](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Serbia.png) **Serbian**: [doskovicmilos/clean-code-javascript/](https://github.com/doskovicmilos/clean-code-javascript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [bsonmez/clean-code-javascript](https://github.com/bsonmez/clean-code-javascript/tree/turkish-translation)
- ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ukrainian**: [mindfr1k/clean-code-javascript-ua](https://github.com/mindfr1k/clean-code-javascript-ua)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)
- ![ir](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Iran.png) **Persian**: [hamettio/clean-code-javascript](https://github.com/hamettio/clean-code-javascript)

**[⬆ Başa qayıt](#İçindəkilər)**
