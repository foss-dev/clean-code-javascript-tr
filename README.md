# clean-code-javascript-tr

## İçindekiler
  1. [Giriş](#giriş)
  2. [Değişkenler](#değişkenler)
  3. [Fonksiyonlar](#fonksiyonlar)
  4. [Nesneler ve Veri Yapıları](#nesneler-ve-veri-yapıları)
  5. [Sınıflar](#sınıflar)
  6. [SOLID](#solid)
  7. [Test](#test)
  8. [Eşzamanlılık](#eşzamanlılık)
  9. [Hata Yakalama](#hata-yakalama)
  10. [Yazım Şekli](#yazım-şekli)
  11. [Yorumlar](#yorumlar)
  12. [Çeviriler](#translation)

## Giriş
![Yazılım kalitesi hakkında "Bu ne ola ki" sorularından oluşan komik bir görsel.](http://www.osnews.com/images/comics/wtfm.jpg)

Yazılım mühendisliği prensipleri, Robert C. Martin'in
[*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) isimli kitabından alınmış, JavaScript için uyarlanmıştır. Bu bir stil rehber değildir. Bu JavaScript ile
[readable, reusable, ve refactorable](https://github.com/ryanmcdermott/3rs-of-software-architecture) yazılımlar üretmek için bir rehberdir..

Burada yazılan prensiplere sıkı bir şekilde uymanız gerekmez belki bazı kurallar herkesçe kabul edilecektir. Burada yazanlar kurallar ve daha fazlası değil. Ancak bu kurallar *Clean Code* yazarlarının uzun yıllara dayanan deneyimleri sonucu ortaya çıktığı için dikkate almanız iyi olabilir.

Yazılım mühendisliği konusundaki çalışmalarımız 50 yılın biraz üzerinde ve hala çok fazla şey öğreniyoruz. Yazılım mimarisi, mimarlığın kendisi kadar eski olduğunda belki de uyulması gereken daha zor kurallara sahip olacağız. Şimdilik, bu kılavuzların sizin ve ekibinizin ürettiği JavaScript kodunun kalitesini değerlendirmek için bir mihenk taşı olarak hizmet etmesine izin verin.

Son bir şey daha: Bunları biliyor olmak sizi hemen mükemmel bir yazılım geliştirici yapmaz ve bu kuralları bilerek geçirdiğiniz yıllar hata yapmayacağınız anlamına da gelmez. Her kod parçası bir taslak olarak başlar, tıpkı ıslak bir kilin son halini alması gibi de devam eder. Son olarak takım arkadaşlarımızla incelemeler yaparken kötü görünen kısımları yok eder. Gelişime ihtiyacı olan kodun ilk hali için kendinize kızmayın. Bunun yerine kodu dövün :)

## **Değişkenler**
### Anlamlı ve belirli değişken isimleri kullanın

**Kötü:**
```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
```

**İyi:**
```javascript
const mevcutTarih = moment().format('YYYY/MM/DD');
```
**[⬆ en başa dön](#içindekiler)**

### Aynı türde değişkenler için aynı kelimeleri kullanın

**Kötü:**
```javascript
kullaniciBilgisiGetir();
musteriVerisiGetir();
musteriKayitlariGetir();
```

**İyi:**
```javascript
kullaniciGetir();
```
**[⬆ en başa dön](#içindekiler)**

### Aranabilir isimler kullanın
Yazacağımızdan daha fazla kod okuyacağız. Bu yazdığımız kodun okunabilir ve aranabilir olması açısından önemlidir. Değişkenleri kötü bir şekilde adlandırmayarak programımızı anlamaya çalışan kod okuyucularına zarar vermeyiz. İsimleri aranabilir yapın. [buddy.js](https://github.com/danielstjules/buddy.js) ve [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) gibi araçlar tanımlanmamış sabit değerleri constant olarak tanımlamanıza yardımcı olabilir.

**Kötü:**
```javascript
// Bu 86400000 ne olabilir??
setTimeout(havalandirmayiCalistir, 86400000);

```

**İyi:**
```javascript
// Bu türden tanımlamaları büyük harfler ve alt çizgiler içerecek şekilde belirtin.
const BIR_GUNDEKI_MILISANIYELER = 86400000;

setTimeout(havalandirmayiCalistir, BIR_GUNDEKI_MILISANIYELER);

```
**[⬆ en başa dön](#içindekiler)**

### Açıklayıcı değişkenler kullanın
**Kötü:**
```javascript
const adres = 'Kireç Burnu Sahili, Sarıyer 34400';
const sehirPostaKoduRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
sehirPostaKodunuKaydet(adres.match(sehirPostaKoduRegex)[1], adres.match(sehirPostaKoduRegex)[2]);
```

**İyi:**
```javascript
const adres = 'Kireç Burnu Sahili, Sarıyer 34400';
const sehirPostaKoduRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, sehir, postaKodu] = adres.match(sehirPostaKoduRegex) || [];
sehirPostaKodunuKaydet(sehir, postaKodu);
```
**[⬆ en başa dön](#içindekiler)**

### Zihinsel Haritalamadan Kaçının
Açık olan kapalı olandan daha iyidir

**Kötü:**
```javascript
const lokasyonlar = ['Ankara', 'İstanbul', 'İzmir'];
lokasyonlar.forEach((l) => {
  birSeylerYap();
  baskaBirSeyYap();
  // ...
  // ...
  // ...
  // Bekle bi dk.. Bu l nedir?
  oneCikar(l);
});
```

**İyi:**
```javascript
const lokasyonlar = ['Ankara', 'İstanbul', 'İzmir'];
lokasyonlar.forEach((lokasyon) => {
  birSeylerYap();
  baskaBirSeyYap();
  // ...
  // ...
  // ...
  oneCikar(lokasyon);
});
```
**[⬆ en başa dön](#içindekiler)**

### Gereksiz içerik eklemeyin
Eğer sınıf ya da nesne adından ne yaptığı anlaşılıyorsa, tekrar olarak değişkenler içinde onu anlatan isimlendirmeler yapmayın.

**Kötü:**
```javascript
const Araba = {
  arabaUret: 'Honda',
  arabaModeli: 'Accord',
  arabaRengi: 'Mavi'
};

function arabayiBoya(araba) {
  araba.arabaRengi = 'Kırmızı';
}
```

**İyi:**
```javascript
const Araba = {
  uret: 'Honda',
  model: 'Accord',
  renk: 'Mavi'
};

function arabayiBoya(araba) {
  araba.renk = 'Kırmızı';
}
```
**[⬆ en başa dön](#içindekiler)**

### Kısa Mantıksal İfadeler ya da Koşullar Yerine Varsayılan Argümanlar Kullanın
Varsayılan argümanlar çoğunlukla kısa mantıksa ifadelerden daha temiz bir kullanıma sahiptir. Varsayılan argümanların sadece undefined argümanlar geçerliyse çalışacağını unutmayın. Diğer falsy olarak kabul edilen değerler varsayılan argümanı değiştirecektir. Bunlar `''`, `""`, `false`, `null`, `0`, ve `NaN` olarak gösterilebilir.

**Kötü:**
```javascript
function fabrikaOlustur(isim) {
  const fabrikaAdi = isim || 'Önceki Yazılımcı AŞ';
  // ...
}

```

**İyi:**
```javascript
function fabrikaOlustur(isim = 'Önceki Yazılımcı AŞ') {
  // ...
}

```
**[⬆ en başa dön](#içindekiler)**

## **Fonksiyonlar**
### Fonksiyon Argümanları (İdeal olanı 2 ya da daha az)
Fonksiyonların aldığı argümanları sınırlandırmak fonksiyonun test edilebilirliği
açısından oldukça önemlidir. Üçten fazla argümana sahip bir fonksiyonu test 
etmeniz gerektiğinde, her bir durumu her bir argümanla ayrı ayrı test
edeceğinizden dolayı tonlarca teste maruz kalabilirsiniz.

Bir veya iki argüman normal olan durumdur, mümkün olduğunca üçüncüden kaçınılmadılır.
Bundan daha fazla olanlar iyileştirilmelidir. Eğer fonksiyonunuz ikiden fazla
argüman alıyorsa, muhtemelen yapması gerekenden fazla işi yapmaya çalışıyordur.
Daha fazla argümana ihtiyacınız olduğunuz durumlarda daha kapsamlı bir nesne
kullanmak yeterli olacaktır.

Javascript size anında nesne oluşturma kabiliyetini verdiğinden dolayı, daha fazla
argümana ihtiyaç duyduğunuz durumlarda; herhangi bir sınıf üretmeye gerek kalmadan
nesneler içerisinde argümanlarınızı gönderebilirsiniz.

Fonksiyonun beklediği argümanları garantilemek için ES2015/ES6 ile gelen
yıkım işlemi (destructuring) sözdizimini kullanabilirsiniz. Bunun birkaç avantajı var:

1. Dışarıdan birisi fonksiyon iskeletine baktığı zaman, fonksiyonun dışarıdan aldığı
özellikleri kolayca anlayabilir.
2. Yıkım işlemi (destructuring) aynı zamanda nesne içerisinde gönderilen ilkel
değerleri klonlar. Bu yan etkilerin engellenmesinde yardımcı olur. Not: Argüman
nesneleri tarafından yıkıma uğratılmış (destruct edilmiş) nesne ve dizi değerleri klonlanmaz.
3. Linterlar sizi kullanılmayan değerler için uyarabilir, ki bu durumu yıkım ("destruct") işlemi olmadan
yapmanız mümkün değildir.

**Kötü:**
```javascript
function menuOlustur(baslik, icerik, butonIcerik, iptalEdilebilir) {
  // ...
}
```

**İyi:**
```javascript
function menuOlustur({ baslik, icerik, butonIcerik, iptalEdilebilir }) {
  // ...
}

menuOlustur({
  baslik: 'Takip Et',
  icerik: 'Kullanıcı takip edilsin mi?',
  butonIcerik: 'TAKİP ET',
  iptalEdilebilir: true
});
```
**[⬆ en başa dön](#içindekiler)**


### Fonksiyonlar Tek Bir Şey Yapmalı
Bu yazılım mühendisliğinde en önemli kuraldır. Fonksiyonlar birden fazla iş yaptığında, onları düzenlemek, test etmek ve hakkında fikir sahibi olmak oldukça zorlaşır. Bir fonksiyonu izole ettiğinizde, daha kolay refactor edilebilir ve daha temiz, okunabilir bir kod haline gelir. Bu kılavuzdan aldığınız tek bilgi bu olsa bile birçok geliştiricinin önde olacaksınız.

**Kötü:**
```javascript
function musterilereMailYolla(musteriler) {
  musteriler.forEach((musteri) => {
    const musteriKaydi = database.sorgula(musteri);
    if (musteriKaydi.aktifMi()) {
      email(musteri);
    }
  });
}
```

**İyi:**
```javascript
function aktifMusterilereEmailGonder(musteriler) {
  musteriler
    .filter(aktifMusteriMi)
    .forEach(email);
}

function aktifMusteriMi(musteri) {
  const musteriKaydi = database.sorgula(musteri);
  return musteriKaydi.aktifMi();
}
```
**[⬆ en başa dön](#içindekiler)**

### Fonksiyon İsimleri Ne Yaptıklarını Söylemeli

**Kötü:**
```javascript
function tariheEkle(tarih, ay) {
  // ...
}

const tarih = new Date();

// Fonkisyon adına bakarak neyin eklendiğini anlamak zor
tariheEkle(tarih, 1);
```

**İyi:**
```javascript
function tariheAyEkle(ay, tarih) {
  // ...
}

const tarih = new Date();
tariheAyEkle(1, tarih);
```
**[⬆ en başa dön](#içindekiler)**

### Fonksiyonlar bir seviye soyutlaştırılmalıdır
Fonkiyonunuz bir seviyeden fazla soyutlaşmış ise, gereğinden fazla
iş yapıyor demektir. Fonksiyonlarınızı görevlerine göre küçük parçalara 
bölmek geri kullanılabilirlik ve kolay test edilebilirlik açısından önemlidir.

**Kötü:**
```javascript
function dahaIyiJSAlternatifineDonustur(kod) {
  const REGEXLER = [
    // ...
  ];

  const kodParcaciklari = kod.split(' ');
  const simgeler = [];
  REGEXLER.forEach((REGEX) => {
    kodParcaciklari.forEach((kodParcacigi) => {
      // ...
    });
  });

  const ast = [];
  simgeler.forEach((simge) => {
    // lex...
  });

  ast.forEach((node) => {
    // dönüstür...
  });
}
```

**İyi:**
```javascript
function dahaIyiJSAlternatifineDonustur(kod) {
  const simgeler = simgelestir(kod);
  const ast = analizEt(simgeler);
  ast.forEach((node) => {
    // dönüstür...
  });
}

function simgelestir(kod) {
  const REGEXLER = [
    // ...
  ];

  const kodParcaciklari = kod.split(' ');
  const simgeler = [];
  REGEXLER.forEach((REGEX) => {
    kodParcaciklari.forEach((kodParcacigi) => {
      simgeler.push( /* ... */ );
    });
  });

  return simgeler;
}

function analizEt(simgeler) {
  const ast = [];
  simgeler.forEach((simge) => {
    ast.push( /* ... */ );
  });

  return ast;
}
```
**[⬆ en başa dön](#içindekiler)**

### Yinelenen kodu kaldırın
Yinelenen kodu kaldırmak için elinizden gelenin en iyisini yapın. Tekrarlanan kodun
kötü olma nedeni, kodunuzda mantıksal bir durumu değiştirmeye çalıştığınızda
bunu birden fazla yerde yapmanızı gerektirmesidir. Bu da oldukça hataya elverişli bir durumdur.

Bir restoran işlettiğinizi ve içinde domates, soğan, biber, sarımsak vs. olan bir deponuz
olduğunu ve deponuzu takip ettiğinizi düşünün. Eğer bu iş için birden fazla liste
tutarsanız, en ufak bir servisinizde tüm listeleri yeniden güncellemeniz gerekecektir.
Eğer tek bir listeniz olursa tek bir noktadan tüm listeyi yönetebilirsiniz

Çoğu zaman kod tekrarına düşersiniz. Çünkü iki veya daha fazla küçük farklılığı
olan ama çoğunlukla aynı özellikleri taşıyan iki fonksiyon sizi bu küçük nedenlerden
dolayı çoğunlukla aynı özelliklere sahip olan ve temelde aynı işi yapan
iki farklı fonksiyon yazmaya zorlar. Tekrarlayan kodu kaldırmak demek; bu farklılıkları
farklı bir yerde yerine getirebilecek soyut fonksiyonlar, modüller, sınıflar yazmak demektir.

Soyutlamayı doğru yapmak çok kritikdir. Bu yüzden devam eden kısımlardan *Sınıflar*
kısmındaki KATI kuralları takip etmelisiniz. Kötü soyutlamalar kod tekrarından da kötüdür.
Bu yüzden dikkatli olmalısınız. İyi bir soyutlama yapabilirim diyorsanız bunu yapın.
Kendinizi tekrar etmeyin, aksi takdirde kendinizi birden fazla yeri güncellerken bulacaksınız.

**Kötü:**
```javascript
function gelistiriciListesiniGoster(gelistiriciler) {
  gelistiriciler.forEach((gelistirici) => {
    const beklenenMaas = gelistirici.beklenenMaasiHesapla();
    const deneyim = gelistirici.deneyimiGetir();
    const githubLink = gelistirici.githubLink();
    const veri = {
      beklenenMaas,
      deneyim,
      githubLink
    };

    yazdir(veri);
  });
}

function showManagerList(yoneticiler) {
  yoneticiler.forEach((yonetici) => {
    const beklenenMaas = yonetici.beklenenMaasiHesapla();
    const deneyim = yonetici.deneyimiGetir();
    const portfolio = yonetici.projeleriniGetir();
    const veri = {
      beklenenMaas,
      experience,
      portfolio
    };

    yazdir(veri);
  });
}
```

**İyi:**
```javascript
function personelListesiniGoster(personeller) {
  personeller.forEach((personel) => {
    const beklenenMaas = personel.beklenenMaasiHesapla();
    const deneyim = personel.deneyimiGetir();

    const veri = {
      beklenenMaas,
      deneyim
    };

    switch (personel.tip) {
      case 'yonetici':
        veri.portfolio = personel.projeleriniGetir();
        break;
      case 'developer':
        veri.githubLink = personel.githubLink();
        break;
    }

    yazdir(veri);
  });
}
```
**[⬆ en başa dön](#içindekiler)**

### Varsayılan Nesneleri Object.assign ile Atayın

**Kötü:**
```javascript
const menuAyari = {
  baslik: null,
  icerik: 'Deneme',
  butonYazisi: null,
  iptalEdilebilir: true
};

function menuOlustur(ayar) {
  ayar.baslik = ayar.baslik || 'Bir Baslik';
  ayar.icerik = ayar.icerik || 'Deneme';
  ayar.butonYazisi = ayar.butonYazisi || 'Kaydet';
  ayar.iptalEdilebilir = ayar.iptalEdilebilir !== undefined ? ayar.iptalEdilebilir : true;
}

menuOlustur(menuAyari);
```

**İyi:**
```javascript
const menuAyari = {
  baslik: 'Bir Baslik',
  // Geliştirici 'icerik' key'ini burada belirtmedi
  butonYazisi: 'Kaydet',
  iptalEdilebilir: true
};

function menuOlustur(ayar) {
  ayar = Object.assign({
    baslik: 'Bir Baslik',
    icerik: 'Deneme',
    butonYazisi: 'Kaydet',
    iptalEdilebilir: true
  }, ayar);

  // ayar simdi: {baslik: "Bir Baslik", icerik: "Deneme", butonYazisi: "Kaydet", iptalEdilebilir: true}
  // ...
}

menuOlustur(menuAyari);
```
**[⬆ en başa dön](#içindekiler)**


### Bayrakları Fonksiyon Argümanları Olarak Kullanmayın
Bayraklar geliştiriciye fonksiyonun birden fazla şey yaptığını söyler. Fonksiyonlar sadece bir iş yapmalıdır. Eğer fonksiyonlarınız boolean değere dayalı olarak farklı kodlar çalıştırıyorsa onları bölün.

**Kötü:**
```javascript
function dosyaOlustur(isim, gecici) {
  if (gecici) {
    fs.create(`./gecici/${isim}`);
  } else {
    fs.create(isim);
  }
}
```

**İyi:**
```javascript
function dosyaOlustur(isim) {
  fs.create(isim);
}

function geciciDosyaOlustur(isim) {
  dosyaOlustur(`./temp/${isim}`);
}
```
**[⬆ en başa dön](#içindekiler)**

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

**Kötü:**
```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**İyi:**
```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

const name = 'Ryan McDermott';
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```
**[⬆ en başa dön](#içindekiler)**

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

**[⬆ en başa dön](#içindekiler)**

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
**[⬆ en başa dön](#içindekiler)**

### Favor functional programming over imperative programming
JavaScript isn't a functional language in the way that Haskell is, but it has
a functional flavor to it. Functional languages can be cleaner and easier to test.
Favor this style of programming when you can.

**Kötü:**
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

**İyi:**
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
**[⬆ en başa dön](#içindekiler)**

### Encapsulate conditionals

**Kötü:**
```javascript
if (fsm.state === 'fetching' && isEmpty(listNode)) {
  // ...
}
```

**İyi:**
```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === 'fetching' && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```
**[⬆ en başa dön](#içindekiler)**

### Negatif Koşullardan Kaçının

**Kötü:**
```javascript
function domYaratilmadi(node) {
  // ...
}

if (!domYaratilmadi(node)) {
  // ...
}
```

**İyi:**
```javascript
function domYaratildi(node) {
  // ...
}

if (domYaratildi(node)) {
  // ...
}
```
**[⬆ en başa dön](#içindekiler)**

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
**[⬆ en başa dön](#içindekiler)**

### Tip Kontrolünden Kaçının (Bölüm 1)
JavaScript tip güvensiz bir dildir, yani fonksiyonlarınız herhangi bir tipte argüman alabilir.
Bazen bu özgürlük can yakıcı olabiliyor haliyle fonksiyonlarınızda tip kontrolü yapmak cazip hale gelebiliyor. Bundan kaçınmanın birçok yolu var.
Dikkate alınması gereken ilk şey tutarlı API'lar yazmanız.

**Kötü:**
```javascript
function nigdeyeZiyaret(arac) {
  if (arac instanceof Bisiklet) {
    arac.pedaliCevir(this.mevcutLokasyon, new Lokasyon('nigde'));
  } else if (arac instanceof Araba) {
    arac.sur(this.mevcutLokasyon, new Lokasyon('nigde'));
  }
}
```

**İyi:**
```javascript
function nigdeyeZiyaret(arac) {
  arac.hareketEt(this.mevcutLokasyon, new Lokasyon('nigde'));
}
```
**[⬆ en başa dön](#içindekiler)**

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
  if (typeof val1 === 'number' && typeof val2 === 'number' ||
      typeof val1 === 'string' && typeof val2 === 'string') {
    return val1 + val2;
  }

  throw new Error('Must be of type String or Number');
}
```

**İyi:**
```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```
**[⬆ en başa dön](#içindekiler)**

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
**[⬆ en başa dön](#içindekiler)**

### Ölü Kodları Silin
Ölü kod da tekrarlı kodlar kadar kötüdür. Kodlarınızda ölü kod saklamanız için herhangi bir neden yoktur.
Eğer herhangi bir yerde çağrılmıyorlarsa onlardan kurtulun. İhtiyacınız olduğunda, versiyon kontrol sisteminde bulabilirsiniz.

**Kötü:**
```javascript
function eskiHttpRequestModulu(url) {
  // ...
}

function yeniHttpRequestModulu(url) {
  // ...
}

const istek = yeniHttpRequestModulu;
envanterTakibi('elmalar', istek, 'www.envantertakibi.com');

```

**İyi:**
```javascript
function yeniHttpRequestModulu(url) {
  // ...
}

const istek = yeniHttpRequestModulu;
envanterTakibi('elmalar', istek, 'www.envantertakibi.com');
```
**[⬆ en başa dön](#içindekiler)**

## **Nesneler ve Veri Yapıları**
### Setter ve Getter kullanın
Nesnelerdeki verilere erişmek için Setter ve Getter kullanmak sadece bir nesnedeki 
özellikleri aramaktan daha iyi olabilir. "Neden?" diye soracaksınız. Peki, işte burada
sebeplerinin bir listesi var:

* Bir nesne özelliğini elde etmekten daha fazla şey yapmak istediğinizde kod tabanınızdaki
her erişimciyi aramanız ve değiştirmeniz gerekmez.
* `set` işlemi yaparken doğrulama eklemeyi kolaylaştırır.
* İç temsili kapsüller.
* Set ve Get işlemlerini gerçekleştirirken kayıt tutmayı (log) ve hata yakalamayı eklemek kolaydır.
* Nesnenizin özelliklerini sunucudan alırken Lazy Load kullanabilirsiniz.


**Kötü:**
```javascript
function bankaHesabiOlustur() {
  // ...

  return {
    bakiye: 0,
    // ...
  };
}

const hesap = bankaHesabiOlustur();
hesap.bakiye = 100;
```

**İyi:**
```javascript
function bankaHesabiOlustur() {
  // bu private
  let bakiye = 0;

  //Getter aşağıda döndürülen nesne aracılığıyla public hale getirildi
  function getBakiye() {
    return bakiye;
  }

  // Setter aşağıda döndürülen nesne aracılığıyla public hale getirildi
  function setBakiye(deger) {
    // ... bakiyeyi güncellemeden önce onaylar
    bakiye = deger;
  }

  return {
    // ...
    getBakiye,
    setBakiye,
  };
}

const hesap = bankaHesabiOlustur();
hesap.setBakiye(100);
```
**[⬆ en başa dön](#içindekiler)**


### Nesnelerin private üyelere sahip olmasını sağlayın.
Bu, kapamalarla(closures) gerçekleştirilebilir. (ES5 ve altı için).

**Kötü:**
```javascript

const Calisan = function(isim) {
  this.isim = isim;
};

Calisan.prototype.getIsim = function getIsim() {
  return this.isim;
};

const calisan = new calisan('John Doe');
console.log(`Calisanin ismi: ${calisan.getIsim()}`); // Calisanin ismi: John Doe
delete calisan.isim;
console.log(`Calisanin ismi: ${calisan.getIsim()}`); // Calisanin ismi: undefined
```

**İyi:**
```javascript
function calisanOlustur(isim) {
  return {
    getIsim() {
      return isim;
    },
  };
}

const calisan = calisanOlustur('John Doe');
console.log(`Calisanin ismi: ${calisan.getIsim()}`); // Calisanin ismi: John Doe
delete calisan.isim;
console.log(`Calisanin ismi: ${calisan.getIsim()}`); // Calisanin ismi: John Doe
```
**[⬆ en başa dön](#içindekiler)**


## **Sınıflar**
### Yalın ES5 fonksiyonları yerine ES2015/ES6 sınıflarını tercih edin
Klasik ES5 sınıfları için okunabilir sınıf kalıtımları, construction ve metod tanımlarını
almak çok zordur. Eğer kalıtıma ihtiyacınız varsa (ihtiyacınızın olmayabileceğinin farkında olun), 
o zaman ES2015/ES6 sınıflarını tercih edin. Ancak, daha büyük ve karmaşık nesnelerle uğraşana
kadar sınıflar yerine küçük fonksiyonları kullanın.

**Kötü:**
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

**İyi:**
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
**[⬆ en başa dön](#içindekiler)**


### Metod zincirleme yöntemini kullanın
Bu yöntem JavaScript'te çok kullanışlıdır ve bunu jQuery ve Lodash gibi birçok kütüphanede görebilirsiniz. 
Kodunuzun daha anlamlı ve daha az detaylı olmasını sağlar.
Bu nedenle, metod zincirleme yöntemini bir kez kullanın ve kodunuzun ne kadar temiz olacağına bir göz atın derim.
Sınıf fonksiyonlarında basitçe her fonksiyon sonunda `this` döndürün,
böylece daha fazla sınıf metodu zincirleyebilirsiniz.

**Kötü:**
```javascript
class Araba {
  constructor(marka, model, renk) {
    this.marka = marka;
    this.model = model;
    this.renk = renk;
  }

  setMarka(marka) {
    this.marka = marka;
  }

  setModel(model) {
    this.model = model;
  }

  setRenk(renk) {
    this.renk = renk;
  }

  kaydet() {
    console.log(this.marka, this.model, this.renk);
  }
}

const araba = new Araba('Ford','F-150','kirmizi');
araba.setRenk('pembe');
araba.kaydet();
```

**İyi:**
```javascript
class Araba {
  constructor(marka, model, renk) {
    this.marka = marka;
    this.model = model;
    this.renk = renk;
  }

  setMarka(marka) {
    this.marka = marka;
    // NOT: Zincirleme için 'this' döndürülüyor
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOT: Zincirleme için 'this' döndürülüyor
    return this;
  }

  setRenk(renk) {
    this.renk = renk;
    // NOT: Zincirleme için 'this' döndürülüyor
    return this;
  }

  kaydet() {
    console.log(this.marka, this.model, this.renk);
    // NOT: Zincirleme için 'this' döndürülüyor
    return this;
  }
}

const araba = new Araba('Ford','F-150','kirmizi')
  .setRenk('pembe')
  .kaydet();
```
**[⬆ en başa dön](#içindekiler)**

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
**[⬆ en başa dön](#içindekiler)**

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
**[⬆ en başa dön](#içindekiler)**

### Açık/Kapalı Prensibi
Bertrand Meyer tarafından belirtildiği gibi, "yazılım varlıkları (classlar, modüller, fonksiyonlar vs.) gelişime açık, değişime kapalı olmalıdır." Peki bu ne anlamaya geliyor? Bu ilke temel olarak, kullanıcıların varolan kodu değiştirmeden yeni işlevler ekleyebilmesini sağlamamız gerektiğini belirtir.

**Kötü:**
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

**İyi:**
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
**[⬆ en başa dön](#içindekiler)**

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
  rectangles.forEach((rectangle) => {
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
  shapes.forEach((shape) => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```
**[⬆ en başa dön](#içindekiler)**

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
  rootNode: document.getElementsByTagName('body'),
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
  rootNode: document.getElementsByTagName('body'),
  options: {
    animationModule() {}
  }
});
```
**[⬆ en başa dön](#içindekiler)**

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

**İyi:**
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
**[⬆ en başa dön](#içindekiler)**

## **Testing**
Testing is more important than shipping. If you have no tests or an
inadequate amount, then every time you ship code you won't be sure that you
didn't break anything. Deciding on what constitutes an adequate amount is up
to your team, but having 100% coverage (all statements and branches) is how
you achieve very high confidence and developer peace of mind. This means that
in addition to having a great testing framework, you also need to use a
[good coverage tool](http://gotwarlost.github.io/istanbul/).

There's no excuse to not write tests. There are [plenty of good JS test frameworks](http://jstherightway.org/#testing-tools), so find one that your team prefers.
When you find one that works for your team, then aim to always write tests
for every new feature/module you introduce. If your preferred method is
Test Driven Development (TDD), that is great, but the main point is to just
make sure you are reaching your coverage goals before launching any feature,
or refactoring an existing one.

### Single concept per test

**Kötü:**
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

**İyi:**
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
**[⬆ en başa dön](#içindekiler)**

## **Eşzamanlılık**
### Promiseleri kullan,Callbackleri değil.
Callbackler kusursuz değildir , ve aşırı miktarda iç içe geçmeye neden olurlar. ES2015/ES6
ile birlikte Promiseler bir yerleşik evrensel tiptir. Onları kullan!

**Kötü:**
```javascript
import { get } from 'request';
import { writeFile } from 'fs';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin', (istekHatasi, cevap) => {
  if (requestErr) {
    console.error(istekHatasi);
  } else {
    writeFile('makale.html', cevap.body, (yazmaHatasi) => {
      if (yazmaHatasi) {
        console.error(yazmaHatasi);
      } else {
        console.log('Dosya yazildi');
      }
    });
  }
});

```

**İyi:**
```javascript
import { get } from 'request';
import { writeFile } from 'fs';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then((cevap) => {
    return writeFile('makale.html', cevap);
  })
  .then(() => {
    console.log('Dosya yazildi');
  })
  .catch((hata) => {
    console.error(hata);
  });

```
**[⬆ en başa dön](#içindekiler)**

### Async/Await ,Promise'den daha temizdir.
Promiseler Callbacklere nazaran daha temizdir, fakat ES2017/ES8 daha 
temiz bir çözüm sunan async await'i getirdi. Tek ihtiyacın `async` önekine sahip bir fonksiyon, 
ve sonrasında `then`li fonksiyonlar zincirini kullanmaksızın 
mantığını zorunlu olarak yazabilirsin. ES2017 / ES8 özelliklerinden yararlanabiliyorsanız bunu
bugün kullanın!.

**Kötü:**
```javascript
import { get } from 'request-promise';
import { writeFile } from 'fs-promise';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then((cevap) => {
    return writeFile('makale.html', cevap);
  })
  .then(() => {
    console.log('Dosya yazildi');
  })
  .catch((hata) => {
    console.error(hata);
  });

```

**İyi:**
```javascript
import { get } from 'request-promise';
import { writeFile } from 'fs-promise';

async function temizKodMakalesiniAl() {
  try {
    const cevap = await get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin');
    await writeFile('makale.html', cevap);
    console.log('Dosya yazildi');
  } catch(hata) {
    console.error(hata);
  }
}
```
**[⬆ en başa dön](#içindekiler)**


## **Hata Yakalama**
Hatalar oluşturmak iyi bir şeydir. Hatalar size programınızda bir şeylerin yolunda olmadığını söylemenin en iyi yoludur.
Çalışan bir kod parçacığı ya da çalışmayı durduran bir fonksiyonun, process'in neden durduğuna dair konsol ekranında sizi bilgilendirirler.

### Yakalanan Hataları Görmezden Gelmeyin
Yakalanan bir hata ile hiçbir şey gerçekleştirmemek, size o hatayı tamamen fixlemiş olma imkanı sunmaz.
Hataları (`console.log`) ile göstermek, tıpkı suya yazı yazmak gibidir. Çoğu zaman yetersizdir.
Eğer kod bölümlerini `try/catch` blokları ile oluşturuyorsanız o bölümde bir hatanın oluşabileceğini düşünüyorsunuzdur.
Bu durumlar için bir planınız olmalı ya da bu durumları yönetebileceğiniz ayrı kod yapılarınız olmalı.

**Kötü:**
```javascript
try {
  hataFirlatabilecekFonksiyon();
} catch (hata) {
  console.log(hata);
}
```

**İyi:**
```javascript
try {
  hataFirlatabilecekFonksiyon();
} catch (hata) {
  // İlk seçenek (console.log'dan daha çok bilgilendirici):
  console.error(hata);
  // Diğer Seçenek:
  kullaniciyaHataGoster(hata);
  // Diğer Seçenek:
  hatayiServiseBildir(hata);
  // Ya da üçünü de yapabilirsiniz!!
}
```

### Promise Hatalarını Görmezden Gelmeyin
Aynı sebepten dolayı `try/catch`'ten kaynaklanan hataları gözardı etmemelisiniz.

**Kötü:**
```javascript
verileriGetir()
  .then((veri) => {
    fonksiyonHataFirlatabilir(veri);
  })
  .catch((hata) => {
    console.log(hata);
  });
```

**İyi:**
```javascript
verileriGetir()
  .then((veri) => {
    fonksiyonHataFirlatabilir(veri);
  })
  .catch((hata) => {
    // İlk seçenek (console.log'dan daha çok bilgilendirici):
    console.error(hata);
    // Diğer Seçenek:
    kullaniciyaHataGoster(hata);
    // Diğer Seçenek:
    hatayiServiseBildir(hata);
    // Ya da üçünü de yapabilirsiniz!!
  });
```

**[⬆ en başa dön](#içindekiler)**


## **Yazım Şekli**
Yazım şekli özneldir. Buradaki birçok kural gibi, uymanız gereken zor ve 
sıkı bir kural yoktur. Yazım şekli üzerinde TARTIŞMAYIN.
Bunları otomatikleştirmek için [binlerce araç](http://standardjs.com/rules.html) vardır.
Birini kullanın! Mühendisler için, yazım şekli üzerinde tartışmak zaman ve para kaybıdır.

Otomatik formatlama kapsamına girmeyen şeyler
(girintileme, tab veya boşluk, çift veya tek tırnak vb.) hakkında rehberlik
için buraya bakın.

### Büyük harf kullanımınız tutarlı olsun
JavaScript'in bir yazım kuralı yoktur, bu yüzden büyük harf kullanımı size değişkenler, 
fonksiyonlar vb. şeyler hakkında birçok bilgi verir. Bu kurallar özneldir, yani ekibiniz istediğini
seçebilir. Önemli olan neyi seçtiğiniz değildir, seçtiğinizde tutarlı olmanızdır.

**Kötü:**
```javascript
const HAFTANIN_GUN_SAYISI = 7;
const ayinGunSayisi = 30;

const sarkilar = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const Sanatcilar = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function veritabaniniSil() {}
function veritabanini_kurtar() {}

class hayvan {}
class Alpaka {}
```

**İyi:**
```javascript
const HAFTANIN_GUN_SAYISI = 7;
const AYIN_GUN_SAYISI = 30;

const SARKILAR = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const SANATCILAR = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function veritabaniniSil() {}
function veritabaniniKurtar() {}

class Hayvan {}
class Alpaka {}
```
**[⬆ en başa dön](#içindekiler)**


### Çağırılan ve çağıran fonksiyonlar birbirine yakın olmalıdır.
Eğer bir fonksiyon diğerini çağırıyorsa, kodda onları dikey olarak birbirine yakın tutun. İdeal olarak, çağıran fonksiyonu çağırılanın hemen üzerinde tutun. Kodları tıpkı gazete okur gibi
yukarıdan aşağıya doğru okuruz. Bu nedenle, kodunuzun bu yolda okunabilmesini sağlayın.

**Kötü:**
```javascript
class PerformansDegerlendirmesi {
  constructor(calisan) {
    this.calisan = calisan;
  }

  benzerleriniGetir() {
    return db.lookup(this.calisan, 'benzer');
  }

  mudurleriGetir() {
    return db.lookup(this.calisan, 'mudur');
  }

  benzerDegerlendirmeler() {
    const benzerler = this.benzerleriniGetir();
    // ...
  }

  performansDegerlendirmesi() {
    this.benzerDegerlendirmeler();
    this.mudurDegerlendirmeleri();
    this.kendiDegerlendirmeleriGetir();
  }

  mudurDegerlendirmeleri() {
    const mudur = this.mudurleriGetir();
  }

  kendiDegerlendirmeleriGetir() {
    // ...
  }
}

const deegrlendirme = new PerformansDegerlendirmesi(calisan);
deegrlendirme.performansDegerlendirmesi();
```

**İyi:**
```javascript
class PerformansDegerlendirmesi {
  constructor(calisan) {
    this.calisan = calisan;
  }

  performansDegerlendirmesi() {
    this.benzerDegerlendirmeler();
    this.mudurDegerlendirmeleri();
    this.kendiDegerlendirmeleriGetir();
  }

  benzerDegerlendirmeler() {
    const benzer = this.benzerleriniGetir();
    // ...
  }

  benzerleriniGetir() {
    return db.lookup(this.calisan, 'benzer');
  }

  mudurDegerlendirmeleri() {
    const mudur = this.mudurleriGetir();
  }

  mudurleriGetir() {
    return db.lookup(this.calisan, 'mudur');
  }

  kendiDegerlendirmeleriGetir() {
    // ...
  }
}

const degerlendirme = new PerformansDegerlendirmesi(calisan);
degerlendirme.performansDegerlendirmesi();
```

**[⬆ en başa dön](#içindekiler)**

## **Yorumlar**
### Sadece iş mantığının karmaşık olduğu durumlarda yorumları kullanın.
Yorumlar lükstür, zorunlu değildir. İyi kod *çoğunlukla* kendini belli eder.

**Kötü:**
```javascript
function ozetCikar(veri) {
  // Özet
  let ozet = 0;

  // data değişkeninin uzunluğu
  const uzunluk = veri.length;

  // veri değişkeninin her karakterini döngüye sok
  for (let i = 0; i < uzunluk; i++) {
  
    // Karakter kodunu getir
    const karakter = veri.charCodeAt(i);
    
    // Özetini çıkar
    ozet = ((ozet << 5) - ozet) + karakter;
    
    // 32-bit'lik sayıya çevir
    ozet &= ozet;
    
  }
}
```

**İyi:**
```javascript

function ozetCikar(veri) {
  let ozet = 0;
  const uzunluk = veri.length;

  for (let i = 0; i < uzunluk; i++) {
    const karakter = veri.charCodeAt(i);
    ozet = ((ozet << 5) - ozet) + karakter;

    // 32-bit'lik sayıya çevir
    ozet &= ozet;
  }
}

```
**[⬆ en başa dön](#içindekiler)**

### Kod tabanınızda yorum satırına alınmış kod bırakmayın.
Sürüm kontrol sistemleri bu nedenle var. Eski kodu geçmişinizde bırakın.

**Kötü:**
```javascript
birSeyYap();
// baskaBirSeyYap();
// birazDahaBirSeyYap();
// dahaFazlaBirSeyYap();
```

**İyi:**
```javascript
birSeyYap();
```
**[⬆ en başa dön](#içindekiler)**

### Yorum satırını günlüğe çevirmeyin
Sürüm kontrol sistemlerini kullanmanız gerektiğini hatırlayın! Ölü koda, yorum satırına alınmış koda ve 
özellikle günlüğe çevrilmiş yorum satırına gerek yok. Önceki yapılanları almak için `git log` komutunu kullanın!

**Kötü:**
```javascript
/**
 * 2016-12-20: Monadları kaldırdım, onları anlamadım (RM)
 * 2016-10-01: Özel monadları kullanarak geliştirdim (JP)
 * 2016-02-03: Tip denetimini kaldırdım (LI)
 * 2015-03-14: Topla fonksiyonunu ekledim (JR)
 */
function topla(a, b) {
  return a + b;
}
```

**İyi:**
```javascript
function topla(a, b) {
  return a + b;
}
```
**[⬆ en başa dön](#içindekiler)**

### Konum işaretleyicilerini kullanmaktan kaçının
Onlar sadece kuru gürültüden ibaret. Fonksiyonlar ve değişkenlerin uygun girintilemeler,
yoluyla kodunuza görsel şeklini vermesine izin verin.

**Kötü:**
```javascript
////////////////////////////////////////////////////////////////////////////////
// Scope Model Örneği
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: 'foo',
  nav: 'bar'
};

////////////////////////////////////////////////////////////////////////////////
// Eylem tanımlanması
////////////////////////////////////////////////////////////////////////////////
const eylemler = function() {
  // ...
};
```

**İyi:**
```javascript
$scope.model = {
  menu: 'foo',
  nav: 'bar'
};

const eylemler = function() {
  // ...
};
```
**[⬆ en başa dön](#içindekiler)**

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
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**:
  [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)

**[⬆ en başa dön](#içindekiler)**
