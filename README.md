# clean-code-javascript

## İçindekiler

1. [Giriş](#giriş)
2. [Değişkenler](#değişkenler)
3. [Fonksiyonlar](#fonksiyonlar)
4. [Nesneler ve Veri Yapıları](#objects-and-data-structures)
5. [Sınıflar](#classes)
6. [SOLID](#solid)
7. [Test](#testing)
8. [Eşzamanlılık](#concurrency)
9. [Hata Denetimi](#error-handling)
10. [Biçimleme](#formatting)
11. [Yorumlar](#comments)
12. [Çeviriler](#translation)

## Giriş

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

Robert C. Martin'in [_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) kitabından alınan yazılım mühendisliği prensiplerinin JavaScript'e uyarlamasıdır. Bu bir stil rehberi değildir. Bu, JavaScript'le [okunabilir, tekrar kullanılabilir, ve tekrar düzenlenebilir](https://github.com/ryanmcdermott/3rs-of-software-architecture) yazılımlar üretmek için bir rehberdir.

Buradaki her prensip kesinlikle uygulanmak zorunda diyemeyiz, hatta çok azında genel bir
mutabakat vardır. Bunlar sadece genel bir kılavuzdan daha fazlası değildir, ama _Clean Code_ yazarlarının yıllardan beri edindiği tecrübelerin bir bileşimidir.

Yazılım mühendisliği sanatımız 50 yıldan biraz daha yaşlıö ve biz hala birçok şey öğreniyoruz.
Yazılım mimarisi, mimarinin kendisi kadar yaşlandığında belki izleyeceğimiz daha keskin kurallar oluşacaktır.
Şimdilik bu kılavuzları, sizin ve takımınınızın yazacağı JavaScript kodunun kalitesini belirlemekteki mihenk taşları olarak kabullenebiliriz.

Bir şey daha: bunları bilmek sizi daha iyi bir yazılım geliştirici yapmaz, ve bunları yıllardır kullanıyor olmak hata yapmayacağınız anlamına gelmez.
Kodun her parçası öncelikle bir taslak olarak başlar, tıpkı ıslak kilin son haline donüşmesi gibi.
Son olarak, ancak eşlerimizle tekrar gözden geçirdiğimizde kusurlardan arınabiliyoruz. O yüzden gelişmeye ihtiyacı olan ilk taslaklar
için kendinizi hırpalamayın, onun yerine kodu hırpalayın.

## **Değişkenler**

### Anlamlı ve kolay okunabilir değişken isimleri kullanın

**Kötü:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**İyi:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ Başa dön](#İçindekiler)**

### Aynı tipdeki değişkenler için aynı kelimeleri kullanın

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

Yazacağımızdan çok daha fazla kodu okumamız gerekecek. Bu nedenle yazdığımız kodun okunabilir ve aranabilir olması önemlidir.
Değişkenleri isimlendirmeyerek okuyucularımızın kodu anlamasını zorlaştırırız. İsimlerinizi aranabilir yapın.

We will read more code than we will ever write. It's important that the code we
do write is readable and searchable. By _not_ naming variables that end up
being meaningful for understanding our program, we hurt our readers.
[buddy.js](https://github.com/danielstjules/buddy.js) ve
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) gibi araçlar isimsiz sabitleri bulmakta size yardımcı olabilir.

**Kötü:**

```javascript
// Neden 86400000?
setTimeout(blastOff, 86400000);
```

**İyi:**

```javascript
// Büyük harfli sabitler olarak tanımlayın
const MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

**[⬆ Başa dön](#İçindekiler)**

### Açıklayıcı isimler kullanın

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
const [, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ Başa dön](#İçindekiler)**

### Zihinsel Haritalamadan Sakının

Açık, belirgin (explicit) üstü kapalıdan (implicit) daha iyidir.

**Kötü:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Bir dakika, `l` ne için kullanılıyordu?
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

### Gereksiz bağlamları eklemeyin

Eğer sınıfınız/nesneniz size adını söylüyorsa, aynı ismi değişken isimlerinizde tekrarlamayın.

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

### Koşul ifadeleri ya da kısa devreleme (short circuiting) yerine varsayılan argüman değerlerini kullanın.

Varsayılan argüman değerleri çoğu zaman kısa devreleme yapmaktan daha temizdir.
Şuna dikkat etmeyi unutmayın, eğer varsayılan değerleri kullanırsanız bu değerler sadece `undefined` argümanlar için geçerli olacaktır.
Diğer "falsy" değerler, örneğin `''`, `""`, `false`, `null`, `0`, `NaN`, varsayılan değer ile değiştirilmeyecektir.

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

### Fonksiyon argümanları (İdeal olarak 2 ya da daha az)

Fonksiyon parametrelerini sınırlamak çok önemlidir, çünkü fonksiyonunuzu test etmeyi daha kola hale getirir.
Üçten fazla parametre olması, kombinasyonların çok artmasına ve onlarca farklı durumun ortaya çıkmasına sebep olur.

Bir ya da iki argüman idealdir, ve mümkünse üç olmasından kaçınmak gerekir.
Bundan daha fazlası birleştirilmelidir. Genellikle eğer iki argümandan daha fazlası varsa,
fonksiyonunuz çok fazla iş yapmaya çalışıyordur. Bunun geçerli olmadığı durumlarda, çoğunlukla üst seviye bir nesne argüman olarak yeterli olacaktır.

JavaScript, size anında nesne yaratma imkanı sunduğundan dolayı, eğer çok fazla argüman yollamaya ihtiyacınız varsa,
birçok sınıf oluşturmaya gerek kalmadan bir nesne içerisinde bu argümanları gönderebilirsiniz.

Fonksiyonun hangi alanlara ihtiyacı olduğunu daha açıkça belirtmek için, ES2015/ES6 nesne ayrıştırma söz dizimin kullanabilirsiniz.
Bunun birkaç avantajı:

1. Birisi fonksiyon imzasına baktığında, hemen hangi özelliklerin kullanıldığı açık bir şekilde görülür.
2. Ayrıştırma aynı zamanda fonksiyona gönderilen argümanların belirtilen ilk değerlerini klonlar.
   Bu, bazı yan etkileri önlemeye yarayabilir. Not: Argüman nesnesinden ayrıştırılan nesneler ve diziler klonlanmaz.
3. Linterlar sizi kullanılmayan alanlar konusunda uyarabilir, nesne ayrıştırması olmasa bu mümkün olmazdı.

**Kötü:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
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

### Fonksiyonlar sadece tek bir iş yapmalıdır

Bu açık arayla yazılım mühendisliğindeki en önemli kuraldır.
Fonksiyonlar birden fazla iş yaptığında onları toparlamak, test etmek ve nedenselleştirmek zorlaşır.
Bir fonksiyonu sadece tek bir iş yapacak şekilde izole ettiğinizde, onu kolayca tekrardan düzenleyebilir ve
kodunuzu daha temiz bir hale getirebilirsiniz. Bu rehberden sadece bu kuralı bile alıp uygulasanız,
birçok geliştiricinin önüne geçebilirsiniz.

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

### Fonksiyon isimleri fonksiyonun ne yaptığını göstermelidir

**Kötü:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
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

### Fonksiyonlar sadece birinci seviye soyutlama yapmalıdır

Eğer bir seviyeden daha fazla soyutlamanız varsa fonksiyonunuz genellikle çok fazla iş yapıyor demektir.
Fonksiyonları bölmek tekrar kullanılabilirliği ve kolay test edilebilirliği artırmayı sağlar.

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

### Tekrar eden kodu silin

Tekrar eden koddan kurtulmak için elinizden gelenin en iyisini yapmaya çalışın. Tekrar eden kod kötüdür çünkü
eğer kod mantığında bir değişiklik yapmanız gerekirse değiştirmeniz gereken birden fazla yer var demektir.

Bir restoran işlettiğinizi ve stoklarınızın izini tuttuğunuzu düşünün: bütün domates, soğan, sarımsak, baharat vs.
Eğer bunları yazdığınız birden fazla liste varsa, içinde domates olan bir yemek servis ettiğinizde bütün bu listeleri güncellemeniz gerekir.
Eğer bir listeniz olursa, sadece onu güncellersiniz ve işiniz hallolur.

Çoğunlukla tekrar eden kodunuz olacaktır, çünkü iki ya da daha fazla birbirinden çok az farklı olan, birçok ortak noktası olan şeyleriniz olacaktır.
Ama aralarındaki farklılıklar sizi benzer işler yapan iki ya da daha fazla fonksiyon yaratmaya itmiş olabilir.
Tekrar eden kodları silmek, bir fonksiyon/module/sınıf ile bu farklılıkların üstesinden gelebilecek bir soyutlama yaratmak demektir.

Bu soyutlamayı doğru yapmak kritik bir önem arz eder, bu yüzden _Sınıflar_ bölümündeki SOLID prensiplerini izlemelisiniz.
Kötü soyutlamalar tekrar eden koddan daha kötü olabilir, o yüzden dikkatli olmalısınız! Bunu söylerken, eğer iyi bir soyutlama
yapabiliyorsanız yapmalısınız! Kendinizi tekrar etmeyin, yoksa kendinizi bir şeyi değiştirmek için birden fazla yeri
güncellerken bulursunuz.

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

### Varsayılan nesneleri Object.assign ile değerleyin

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

**[⬆ Başa dön](#İçindekiler)**

### Bayrakları (Flag) foksiyon parametresi olarak kullanmayın

Bayraklar kullanıcıya bu fonksiyonun birden fazla iş yaptığını söyler. Fonksiyonlar sadece tek bir iş yapmalıdır.
Bir boolean değere göre farklı yollar izleyen fonksiyonlarınızı farklı fonksiyonlara bölün.

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

### Yan Etkilerden Kaçının (bölüm 1)

Eğer bir fonksiyon bir değer alıp bir çıktı üretmekten daha farklı birşey yapıyorsa o fonksiyon bir yan etki üretiyor demektir.
Bir yan etki bir dosyaya yazmak, bir global değişkenin değerini değiştirmek, ya da yanlışlıkla bütün paranızı bir başkasına göndermek olabilir.

Şimdi, bir etkinlikteki bir programda bir yan etkiye ihtiyacınız var. Aynı bir önceki örnekteki gibi, bir dosyaya yazabilirsiniz.
Bu durumda yapmak istediğiniz, bu yaptığınız işi merkezileştirmektir. Bir dosyaya yazan birçok fonksiyon ve sınıfınız olmamalı.
Sadece bu işi yapan bir servisiniz olmalı, sadece bir.

En önemli nokta bir yapı olmaksızın nesneler arası durum paylaşımı, herhangibir şey tarafından değiştirilebilen
veri tipleri kullanımı, ve yan etkilerin oluştuğu yeri merkezileştirmemek gibi yaygın görünmez tehlikelerden kaçınmaktır.
Eğer bunu yapabilirseniz, diğer birçok programcıdan daha mutlu olacaksınız.

**Kötü:**

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

### Yan Etkilerden Kaçının (bölüm 2)

JavaScript'te basit türler değer olarak aktarılır ve nesneler/diziler referans olarak aktarılır.
Nesneler ve dizilere örnek olarak, bir alışveriş sepeti dizisini ele alalım, eğer bir fonksiyon bu sepet dizisine satın almak için yeni bir ürün eklerse
bu diziyi kullanan diğer bir fonksiyon da bu eklemeden etkilenecektir. Bu çok güzel olabilir, ama aynı zamanda kötü de olabilir.
Örneğin kötü bir senaryoyu düşünelim:

Kullanıcı "Satın Al" butonuna tıklar ve bu buton `purchase` adında bir fonksiyonu tetikler ve
`cart` dizisini bir ağ isteği ile sunucuya gönderir. Kötü bir ağ bağlantısı nedeniyle `purchase` fonksiyonunu isteği tekrarlamak zorunda kalır.
Şimdi, eğer bu süre içerisinde yeni istek gönderilmeden önce kullanıcı yanlışlıkla sepetine yeni bir ürün eklerse ne olur?
Bu durumda en son yanlışlıkla eklenen ürün de `purchase` fonksiyonu tarafından sunucuya gönderilir.

Bu probleme en iyi çözüm `addItemToCart` fonksiyonunun `cart` dizisinde değişiklik yapmadan önce klonlayıp, klonladığı dizide değişiklik yapmasıdır.
Bu sayede `cart` dizisine referansı olan başka fonksiyonlar `addItemToCart` fonksiyonu tarafından etkilenmemiş olurlar.

Bu yaklaşımda bahsetmemiz gereken iki önemli uyarı var:

1. Girdi olarak gelen nesneyi değiştirmek istediğiniz durumlar da olabilir,
   ama bu programlama pratiğine alıştığınızda böyle durumların gerçekten çok nadir olduğunu göreceksiniz.
   Birçok kod herhangi bir yan etkisi olmayacak şekilde yeniden yazılabilir!

2. Büyük nesneleri klonlamak performans açısından çok maliyetli olabilir. Neyse ki pratikte bu çok büyük bir problem değil, çünkü
   bu tarz programlama yaklaşımlarının performanslı ve bellek anlamında sorun oluşturmayacak şekilde çalışabilmesi için yaratılmış [muhteşem kütüphaneler](https://facebook.github.io/immutable-js/) var.

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

### Favor functional programming over imperative programming

JavaScript isn't a functional language in the way that Haskell is, but it has
a functional flavor to it. Functional languages can be cleaner and easier to test.
Favor this style of programming when you can.

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

### Encapsulate conditionals

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

### Avoid conditionals

This seems like an impossible task. Upon first hearing this, most people say,
"how am I supposed to do anything without an `if` statement?" The answer is that
you can use polymorphism to achieve the same task in many cases. The second
question is usually, "well that's great but why would I want to do that?" The
answer is a previous clean code concept we learned: a function should only do
one thing. When you have classes and functions that have `if` statements, you
are telling your user that your function does more than one thing. Remember,
just do one thing.

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

### Avoid type-checking (part 1)

JavaScript is untyped, which means your functions can take any type of argument.
Sometimes you are bitten by this freedom and it becomes tempting to do
type-checking in your functions. There are many ways to avoid having to do this.
The first thing to consider is consistent APIs.

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

### Don't over-optimize

Modern browsers do a lot of optimization under-the-hood at runtime. A lot of
times, if you are optimizing then you are just wasting your time. [There are good
resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
for seeing where optimization is lacking. Target those in the meantime, until
they are fixed if they can be.

**Kötü:**

```javascript
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0; len = list.length; i < len; i++) {
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

### Remove dead code

Dead code is just as bad as duplicate code. There's no reason to keep it in
your codebase. If it's not being called, get rid of it! It will still be safe
in your version history if you still need it.

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

## **Objects and Data Structures**

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

**[⬆ Başa dön](#İçindekiler)**

### Make objects have private members

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

## **Classes**

### Prefer ES2015/ES6 classes over ES5 plain functions

It's very difficult to get readable class inheritance, construction, and method
definitions for classical ES5 classes. If you need inheritance (and be aware
that you might not), then prefer ES2015/ES6 classes. However, prefer small functions over
classes until you find yourself needing larger and more complex objects.

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

### Use method chaining

This pattern is very useful in JavaScript and you see it in many libraries such
as jQuery and Lodash. It allows your code to be expressive, and less verbose.
For that reason, I say, use method chaining and take a look at how clean your code
will be. In your class functions, simply return `this` at the end of every function,
and you can chain further class methods onto it.

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

**[⬆ Başa dön](#İçindekiler)**

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

**Kötü:**

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

### Single Responsibility Principle (SRP)

As stated in Clean Code, "There should never be more than one reason for a class
to change". It's tempting to jam-pack a class with a lot of functionality, like
when you can only take one suitcase on your flight. The issue with this is
that your class won't be conceptually cohesive and it will give it many reasons
to change. Minimizing the amount of times you need to change a class is important.
It's important because if too much functionality is in one class and you modify
a piece of it, it can be difficult to understand how that will affect other
dependent modules in your codebase.

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

### Open/Closed Principle (OCP)

As stated by Bertrand Meyer, "software entities (classes, modules, functions,
etc.) should be open for extension, but closed for modification." What does that
mean though? This principle basically states that you should allow users to
add new functionalities without changing existing code.

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
    const area = rectangle.getArea(); // BAD: Returns 25 for Square. Should be 20.
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
  animationModule() {} // Most of the time, we won't need to animate when traversing.
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

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ Başa dön](#İçindekiler)**

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

## **Concurrency**

### Use Promises, not callbacks

Callbacks aren't clean, and they cause excessive amounts of nesting. With ES2015/ES6,
Promises are a built-in global type. Use them!

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

### Async/Await are even cleaner than Promises

Promises are a very clean alternative to callbacks, but ES2017/ES8 brings async and await
which offer an even cleaner solution. All you need is a function that is prefixed
in an `async` keyword, and then you can write your logic imperatively without
a `then` chain of functions. Use this if you can take advantage of ES2017/ES8 features
today!

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
    const body = await get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin");
    await writeFile("article.html", body);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}

getCleanCodeArticle();
```

**[⬆ Başa dön](#İçindekiler)**

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
    // One option (more noisy than console.log):
    console.error(error);
    // Another option:
    notifyUserOfError(error);
    // Another option:
    reportErrorToService(error);
    // OR do all three!
  });
```

**[⬆ Başa dön](#İçindekiler)**

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

### Function callers and callees should be close

If a function calls another, keep those functions vertically close in the source
file. Ideally, keep the caller right above the callee. We tend to read code from
top-to-bottom, like a newspaper. Because of this, make your code read that way.

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

## **Comments**

### Only comment things that have business logic complexity.

Comments are an apology, not a requirement. Good code _mostly_ documents itself.

**Kötü:**

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

**İyi:**

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

**[⬆ Başa dön](#İçindekiler)**

### Don't leave commented out code in your codebase

Version control exists for a reason. Leave old code in your history.

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

### Don't have journal comments

Remember, use version control! There's no need for dead code, commented code,
and especially journal comments. Use `git log` to get history!

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

### Avoid positional markers

They usually just add noise. Let the functions and variable names along with the
proper indentation and formatting give the visual structure to your code.

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

**[⬆ Başa dön](#İçindekiler)**
