# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

Multer, esas olarak dosya yüklemek için kullanılan 'multipart/form-data'yı işlemeye yönelik bir node.js ara yazılımıdır. yazılı
Maksimum verimlilik için [busboy'a](https://github.com/mscdex/busboy) ek olarak.

**NOT**: Multer, çok parçalı olmayan ("çok parçalı/form-veri") hiçbir formu işlemeyecektir.

## Çeviriler

Bu README başka dillerde de mevcuttur:

|                                                                                |                 |
| ------------------------------------------------------------------------------ | --------------- |
| [العربية](https://github.com/expressjs/multer/blob/main/doc/README-ar.md)      | Arabic          |
| [简体中文](https://github.com/expressjs/multer/blob/main/doc/README-zh-cn.md)  | Chinese         |
| [Français](https://github.com/expressjs/multer/blob/main/doc/README-fr.md)     | French          |
| [한국어](https://github.com/expressjs/multer/blob/main/doc/README-ko.md)       | Korean          |
| [Português](https://github.com/expressjs/multer/blob/main/doc/README-pt-br.md) | Portuguese (BR) |
| [Русский язык](https://github.com/expressjs/multer/blob/main/doc/README-ru.md) | Russian         |
| [Español](https://github.com/expressjs/multer/blob/main/doc/README-es.md)      | Spanish         |
| [O'zbek tili](https://github.com/expressjs/multer/blob/main/doc/README-uz.md)  | Uzbek           |
| [Việt Nam](https://github.com/expressjs/multer/blob/main/doc/README-vi.md)     | Vietnamese      |
| [فارسی / Persian](https://github.com/expressjs/multer/blob/main/doc/README-fa.md) | Farsi / Persian |
| [हिन्दी](https://github.com/expressjs/multer/blob/main/doc/README-hi.md)       | Hindi           |
| [বাংলা](https://github.com/expressjs/multer/blob/main/doc/README-bn.md)        | Bengali         |
| [ਪੰਜਾਬੀ](https://github.com/expressjs/multer/blob/main/doc/README-pa.md)       | Punjabi         |
| [Basa Jawa](https://github.com/expressjs/multer/blob/main/doc/README-jv.md)    | Javanese        |
| [తెలుగు](https://github.com/expressjs/multer/blob/main/doc/README-te.md)       | Telugu          |
| [मराठी](https://github.com/expressjs/multer/blob/main/doc/README-mr.md)        | Marathi         |
| [தமிழ்](https://github.com/expressjs/multer/blob/main/doc/README-ta.md)        | Tamil           |
| [اردو](https://github.com/expressjs/multer/blob/main/doc/README-ur.md)         | Urdu            |
| [ગુજરાતી](https://github.com/expressjs/multer/blob/main/doc/README-gu.md)      | Gujarati        |
| [Bahasa Melayu](https://github.com/expressjs/multer/blob/main/doc/README-ms.md) | Malay / Bahasa Melayu |
| [Hausa](https://github.com/expressjs/multer/blob/main/doc/README-ha.md)        | Hausa           |
| [Yorùbá](https://github.com/expressjs/multer/blob/main/doc/README-yo.md)       | Yoruba          |
| [Igbo](https://github.com/expressjs/multer/blob/main/doc/README-ig.md)         | Igbo            |
| [አማርኛ](https://github.com/expressjs/multer/blob/main/doc/README-am.md)        | Amharic         |
| [Türkçe](https://github.com/expressjs/multer/blob/main/doc/README-tr.md)       | Turkish         |
| [Italiano](https://github.com/expressjs/multer/blob/main/doc/README-it.md)     | Italian         |
| [Deutsch](https://github.com/expressjs/multer/blob/main/doc/README-de.md)      | German          |
| [Polski](https://github.com/expressjs/multer/blob/main/doc/README-pl.md)       | Polish          |
| [Українська](https://github.com/expressjs/multer/blob/main/doc/README-uk.md)   | Ukrainian       |

## Kurulum

```sh
$ npm install multer
```

## Kullanım

Multer, 'request' nesnesine bir 'body' nesnesi ve bir 'file' veya 'files' nesnesi ekler. 'body' nesnesi formun metin alanlarının değerlerini içerir, 'file' veya 'files' nesnesi ise form aracılığıyla yüklenen dosyaları içerir.

Temel kullanım örneği:

Formunuzdaki `enctype="multipart/form-data"`yı unutmayın.

```html
<form action="/profile" method="post" enctype="multipart/form-data">
  <input type="file" name="avatar" />
</form>
```

```javascript
const express = require('express')
const multer  = require('multer')
const upload = multer({ dest: 'uploads/' })

const app = express()

app.post('/profile', upload.single('avatar'), function (req, res, next) {
  // req.file is the `avatar` file
  // req.body will hold the text fields, if there were any
})

app.post('/photos/upload', upload.array('photos', 12), function (req, res, next) {
  // req.files is array of `photos` files
  // req.body will contain the text fields, if there were any
})

const uploadMiddleware = upload.fields([{ name: 'avatar', maxCount: 1 }, { name: 'gallery', maxCount: 8 }])
app.post('/cool-profile', uploadMiddleware, function (req, res, next) {
  // req.files is an object (String -> Array) where fieldname is the key, and the value is array of files
  //
  // e.g.
  //  req.files['avatar'][0] -> File
  //  req.files['gallery'] -> Array
  //
  // req.body will contain the text fields, if there were any
})
```

Salt metinden oluşan çok parçalı bir formla uğraşmanız gerekiyorsa `.none()` yöntemini kullanmalısınız:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

Multer'ın HTML formunda nasıl kullanıldığına dair bir örneği burada bulabilirsiniz. `enctype="multipart/form-data"` ve `name="uploaded_file"` alanlarına özellikle dikkat edin:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

Daha sonra javascript dosyanıza hem dosyaya hem de gövdeye erişmek için bu satırları eklersiniz. Yükleme işlevinizde formdaki "ad" alanı değerini kullanmanız önemlidir. Bu, multer'a dosyaları istekte hangi alanda araması gerektiğini söyler. Bu alanlar HTML formunda ve sunucunuzda aynı değilse yükleme işleminiz başarısız olur:

```javascript
const multer  = require('multer')
const upload = multer({ dest: './public/data/uploads/' })
app.post('/stats', upload.single('uploaded_file'), function (req, res) {
  // req.file is the name of your file in the form above, here 'uploaded_file'
  // req.body will hold the text fields, if there were any
  console.log(req.file, req.body)
});
```



## API

### Dosya bilgileri

Her dosya aşağıdaki bilgileri içerir:

Anahtar | Açıklama | Not
--- | --- | ---
'alan adı' | Formda belirtilen alan adı |
'orijinal ad' | Kullanıcının bilgisayarındaki dosyanın adı |
'kodlama' | Dosyanın kodlama türü |
'mime türü' | Dosyanın Mime türü |
'boyut' | Dosyanın bayt cinsinden boyutu |
`hedef` | Dosyanın kaydedildiği klasör | 'Disk Depolama'
'dosya adı' | 'Hedef' içindeki dosyanın adı | 'Disk Depolama'
'yol' | Yüklenen dosyanın tam yolu | 'Disk Depolama'
'tampon' | Tüm dosyanın 'Arabelleği' | 'HafızaDepolama'

### `multer(seçenekler)`

Multer, en temel olanı 'dest' olan bir seçenekler nesnesini kabul eder.
Multer'a dosyaları nereye yükleyeceğini söyleyen özellik. Şunu atlamanız durumunda:
options nesnesini kullanarak dosyalar bellekte tutulacak ve hiçbir zaman diske yazılmayacaktır.

Varsayılan olarak Multer, adlandırma çakışmalarını önleyecek şekilde dosyaları yeniden adlandıracaktır.
yeniden adlandırma işlevi ihtiyaçlarınıza göre özelleştirilebilir.

Multer'a aktarılabilecek seçenekler aşağıdadır.

Anahtar | Açıklama
--- | ---
'hedef' veya 'depolama' | Dosyaların nerede saklanacağı
'dosya Filtresi' | Hangi dosyaların kabul edildiğini kontrol etme işlevi
'limitler' | Yüklenen verilerin sınırları
'Yolu koru' | Yalnızca temel ad yerine dosyaların tam yolunu koruyun

Ortalama bir web uygulamasında yalnızca "hedef" gerekli olabilir ve şekilde gösterildiği gibi yapılandırılabilir.
aşağıdaki örnek.

```javascript
const upload = multer({ dest: 'uploads/' })
```

Yüklemeleriniz üzerinde daha fazla kontrol sahibi olmak istiyorsanız "depolama"yı kullanmak isteyeceksiniz
'hedef' yerine seçenek. Multer, 'DiskStorage' depolama motorlarıyla birlikte gönderilir
ve 'Bellek Depolama'; Üçüncü taraflardan daha fazla motor mevcuttur.

#### `.single(alan adı)`

'Alan adı' adlı tek bir dosyayı kabul edin. Tek dosya saklanacak
'req.file'de.

#### `.array(alanadı[, maxCount])`

Tamamı "alanadı" adında olan bir dizi dosyayı kabul edin. İsteğe bağlı olarak hata durumunda
'maxCount'tan fazla dosya yüklendi. Dosya dizisi şurada saklanacak:
'req.files'.

#### `.fields(fields)`

'Alanlar' tarafından belirtilen karışık dosyaları kabul edin. Dosya dizileri içeren bir nesne
'req.files'ta saklanacak.

'fields', 'name' ve isteğe bağlı olarak 'maxCount' içeren bir nesne dizisi olmalıdır.
Örnek:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.none()`

Yalnızca metin alanlarını kabul edin. Herhangi bir dosya yüklemesi yapılmışsa kod hatası
"LIMIT\_UNEXPECTED\_FILE" düzenlenecek.

#### `.any()`

Kablo üzerinden gelen tüm dosyaları kabul eder. Bir dizi dosya saklanacak
'req.files'.

**UYARI:** Kullanıcının yüklediği dosyaları her zaman kullandığınızdan emin olun.
Kötü niyetli bir kullanıcı yükleme yapabileceği için multer'ı asla küresel bir ara yazılım olarak eklemeyin
dosyaları beklemediğiniz bir rotaya yönlendirir. Bu işlevi yalnızca rotalarda kullanın
Yüklenen dosyaları nerede yönetiyorsunuz?

### `depolama`

#### `DiskDepolama`

Disk depolama motoru, dosyaları diske depolama konusunda size tam kontrol sağlar.

```javascript
const storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, '/tmp/my-uploads')
  },
  filename: function (req, file, cb) {
    const uniqueSuffix = Date.now() + '-' + Math.round(Math.random() * 1E9)
    cb(null, file.fieldname + '-' + uniqueSuffix)
  }
})

const upload = multer({ storage: storage })
```

İki seçenek mevcuttur; 'hedef' ve 'dosya adı'. İkisi de
Dosyanın nerede saklanacağını belirleyen işlevler.

'hedef', yüklenen dosyaların hangi klasörde olması gerektiğini belirlemek için kullanılır
saklanmalıdır. Bu aynı zamanda bir "dize" olarak da verilebilir (ör. `'/tmp/uploads'`). Hayır ise
`hedef` verilir, işletim sisteminin geçici olarak varsayılan dizini
dosyalar kullanılır.

**Not:** Sağlarken dizini oluşturma sorumluluğu size aittir.
Bir işlev olarak 'hedef'. Bir dizeyi geçirirken multer şunları sağlayacak:
dizin sizin için oluşturulur.

'dosyaadı', dosyanın klasör içinde ne şekilde adlandırılması gerektiğini belirlemek için kullanılır.
Herhangi bir "dosya adı" belirtilmezse, her dosyaya rastgele bir ad verilecektir.
herhangi bir dosya uzantısını ekleyin.

**Not:** Multer sizin veya işleviniz için herhangi bir dosya uzantısı eklemez
dosya uzantısıyla birlikte bir dosya adı döndürmelidir.

Her fonksiyona hem istek (`req`) hem de onunla ilgili bazı bilgiler iletilir.
Karara yardımcı olmak için dosya ("dosya").

'req.body'nin henüz tam olarak doldurulmamış olabileceğini unutmayın. bağlıdır
istemcinin alanları ve dosyaları sunucuya iletmesini sağlayın.

Geri aramada kullanılan arama kuralını anlamak için (geçilmesi gereken
ilk parametre olarak null), bkz.
[Node.js hata işleme](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/prodüksiyon/design/errors)

#### `Bellek Depolama`

Bellek depolama motoru, dosyaları bellekte 'Buffer' nesneleri olarak saklar. o
hiçbir seçeneği yok.

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

Bellek depolamayı kullanırken, dosya bilgisi adı verilen bir alan içerecektir.
Dosyanın tamamını içeren 'tampon'.

**UYARI**: Çok büyük dosyalar veya nispeten küçük dosyaların büyük boyutta yüklenmesi
sayıların çok hızlı olması, uygulamanızın hafızasının dolmasına neden olabilir.
Bellek depolama alanı kullanılır.

### `sınırlar'

Aşağıdaki isteğe bağlı özelliklerin boyut sınırlarını belirten bir nesne. Multer bu nesneyi doğrudan komiye aktarır ve özelliklerin ayrıntıları [komi çocuğunun sayfasında](https://github.com/mscdex/busboy#busboy-methods) bulunabilir.

Aşağıdaki tam sayı değerleri mevcuttur:

Anahtar | Açıklama | Varsayılan
--- | --- | ---
'alanAdıBoyut' | Maksimum alan adı boyutu | 100 bayt
'alanBoyutu' | Maksimum alan değeri boyutu (bayt cinsinden) | 1MB
'alanlar' | Maksimum dosya dışı alan sayısı | Sonsuzluk
'dosyaBoyutu' | Çok parçalı formlar için maksimum dosya boyutu (bayt cinsinden) | Sonsuzluk
'dosyalar' | Çok parçalı formlar için maksimum dosya alanı sayısı | Sonsuzluk
'parçalar' | Çok parçalı formlar için maksimum parça sayısı (alanlar + dosyalar) | Sonsuzluk
'başlıkÇiftleri' | Çok parçalı formlar için ayrıştırılacak maksimum başlık anahtarı=>değer çifti sayısı | 2000

Sınırların belirlenmesi, sitenizin hizmet reddi (DoS) saldırılarına karşı korunmasına yardımcı olabilir.

### `dosya Filtresi`

Hangi dosyaların yüklenmesi gerektiğini ve hangilerinin yüklenmesi gerektiğini kontrol etmek için bunu bir işleve ayarlayın.
atlanmalıdır. Fonksiyon şu şekilde görünmelidir:

```javascript
function fileFilter (req, file, cb) {

  // The function should call `cb` with a boolean
  // to indicate if the file should be accepted

  // To reject this file pass `false`, like so:
  cb(null, false)

  // To accept the file pass `true`, like so:
  cb(null, true)

  // You can always pass an error if something goes wrong:
  cb(new Error('I don\'t have a clue!'))

}
```

## Hata işleme

Bir hatayla karşılaştığında Multer, hatayı Express'e devredecektir. Yapabilirsin
[standart ekspres yolu](http://expressjs.com/guide/error-handling.html) kullanarak hoş bir hata sayfası görüntüleyin.

Özellikle Multer'dan gelen hataları yakalamak istiyorsanız,
ara yazılım işlevini kendiniz gerçekleştirin. Ayrıca, yalnızca [Multer hatalarını](https://github.com/expressjs/multer/blob/main/lib/multer-error.js) yakalamak istiyorsanız, "multer" nesnesinin kendisine eklenen "MulterError" sınıfını kullanabilirsiniz (ör. "err exampleof multer.MulterError").

```javascript
const multer = require('multer')
const upload = multer().single('avatar')

app.post('/profile', function (req, res) {
  upload(req, res, function (err) {
    if (err instanceof multer.MulterError) {
      // A Multer error occurred when uploading.
    } else if (err) {
      // An unknown error occurred when uploading.
    }

    // Everything went fine.
  })
})
```

## Özel depolama motoru

Kendi depolama motorunuzu nasıl oluşturacağınız hakkında bilgi için bkz. [Multer Depolama Motoru](https://github.com/expressjs/multer/blob/main/StorageEngine.md).

## Lisans

[BENİM](LİSANS)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
