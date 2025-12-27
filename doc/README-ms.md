# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

Multer ialah perisian tengah node.js untuk mengendalikan `multipart/form-data`, yang digunakan terutamanya untuk memuat naik fail. Ia tertulis
di atas [busboy](https://github.com/mscdex/busboy) untuk kecekapan maksimum.

**NOTA**: Multer tidak akan memproses sebarang bentuk yang bukan berbilang (`multipart/form-data`).

## Terjemahan

README ini juga tersedia dalam bahasa lain:

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

## Pemasangan

```sh
$ npm install multer
```

## Penggunaan

Multer menambah objek `body` dan objek `fail` atau `fail` pada objek `permintaan`. Objek `body` mengandungi nilai medan teks borang, objek `fail` atau `fail` mengandungi fail yang dimuat naik melalui borang.

Contoh penggunaan asas:

Jangan lupa `enctype="multipart/form-data"` dalam borang anda.

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

Sekiranya anda perlu mengendalikan borang berbilang bahagian teks sahaja, anda harus menggunakan kaedah `.none()`:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

Berikut ialah contoh tentang cara multer digunakan dalam bentuk HTML. Ambil perhatian khusus medan `enctype="multipart/form-data"` dan `name="uploaded_file"`:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

Kemudian dalam fail javascript anda, anda akan menambah baris ini untuk mengakses kedua-dua fail dan badan. Adalah penting anda menggunakan nilai medan `nama` daripada borang dalam fungsi muat naik anda. Ini memberitahu multer medan mana pada permintaan yang harus dicari untuk fail tersebut. Jika medan ini tidak sama dalam bentuk HTML dan pada pelayan anda, muat naik anda akan gagal:

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

### Maklumat fail

Setiap fail mengandungi maklumat berikut:

Kunci | Penerangan | Nota
--- | --- | ---
`fieldname` | Nama medan dinyatakan dalam borang |
`nama asal` | Nama fail pada komputer pengguna |
`pengekodan` | Pengekodan jenis fail |
`mimetype` | Jenis mime bagi fail |
`saiz` | Saiz fail dalam bait |
`destinasi` | Folder tempat fail telah disimpan | `DiskStorage`
`nama fail` | Nama fail dalam `destinasi` | `DiskStorage`
`laluan` | Laluan penuh ke fail yang dimuat naik | `DiskStorage`
`penampan` | `Penimbal` bagi keseluruhan fail | `MemoryStorage`

### `multer(opts)`

Multer menerima objek pilihan, yang paling asas ialah `dest`
harta, yang memberitahu Multer tempat untuk memuat naik fail. Sekiranya anda meninggalkan
objek pilihan, fail akan disimpan dalam ingatan dan tidak pernah ditulis ke cakera.

Secara lalai, Multer akan menamakan semula fail untuk mengelakkan konflik penamaan. The
fungsi penamaan semula boleh disesuaikan mengikut keperluan anda.

Berikut adalah pilihan yang boleh dihantar kepada Multer.

Kunci | Penerangan
--- | ---
`dest` atau `storage` | Di mana untuk menyimpan fail
`penapis fail` | Berfungsi untuk mengawal fail mana yang diterima
`had` | Had data yang dimuat naik
`preservePath` | Simpan laluan penuh fail dan bukannya nama asas sahaja

Dalam apl web biasa, hanya `destinasi` mungkin diperlukan dan dikonfigurasikan seperti yang ditunjukkan dalam
contoh berikut.

```javascript
const upload = multer({ dest: 'uploads/' })
```

Jika anda mahukan lebih kawalan ke atas muat naik anda, anda perlu menggunakan `storan`
pilihan dan bukannya `dest`. Multer dihantar dengan enjin storan `DiskStorage`
dan `MemoryStorage`; Lebih banyak enjin boleh didapati daripada pihak ketiga.

#### `.single(fieldname)`

Terima satu fail dengan nama `fieldname`. Fail tunggal akan disimpan
dalam `req.file`.

#### `.array(fieldname[, maxCount])`

Terima tatasusunan fail, semuanya dengan nama `fieldname`. Pilihan ralat keluar jika
lebih daripada fail `maxCount` dimuat naik. Tatasusunan fail akan disimpan dalam
`req.files`.

#### `.medan(medan)`

Terima gabungan fail, yang ditentukan oleh `medan`. Objek dengan tatasusunan fail
akan disimpan dalam `req.files`.

`fields` hendaklah susunan objek dengan `name` dan secara pilihan `maxCount`.
Contoh:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.tiada()`

Terima medan teks sahaja. Jika sebarang muat naik fail dibuat, ralat dengan kod
"LIMIT\_UNEXPECTED\_FILE" akan dikeluarkan.

#### `.mana-mana()`

Menerima semua fail yang datang melalui wayar. Tatasusunan fail akan disimpan dalam
`req.files`.

**AMARAN:** ​​Pastikan anda sentiasa mengendalikan fail yang dimuat naik oleh pengguna.
Jangan sekali-kali menambah multer sebagai perisian tengah global kerana pengguna berniat jahat boleh memuat naik
fail ke laluan yang anda tidak jangkakan. Hanya gunakan fungsi ini pada laluan
tempat anda mengendalikan fail yang dimuat naik.

### `storan`

#### `DiskStorage`

Enjin storan cakera memberi anda kawalan penuh untuk menyimpan fail ke cakera.

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

Terdapat dua pilihan yang tersedia, `destination` dan `filename`. Mereka berdua
fungsi yang menentukan di mana fail harus disimpan.

`destination` digunakan untuk menentukan dalam folder mana fail yang dimuat naik harus
disimpan. Ini juga boleh diberikan sebagai `rentetan` (cth. `'/tmp/uploads'`). Jika tidak
`destination` diberikan, direktori lalai sistem pengendalian untuk sementara
fail digunakan.

**Nota:** Anda bertanggungjawab untuk mencipta direktori apabila menyediakan
`destinasi` sebagai fungsi. Apabila menghantar rentetan, multer akan memastikannya
direktori dicipta untuk anda.

`filename` digunakan untuk menentukan nama fail yang sepatutnya di dalam folder.
Jika tiada `nama fail` diberikan, setiap fail akan diberi nama rawak yang tidak
sertakan sebarang sambungan fail.

**Nota:** Multer tidak akan menambahkan sebarang sambungan fail untuk anda, fungsi anda
harus mengembalikan nama fail yang lengkap dengan sambungan fail.

Setiap fungsi akan diluluskan kedua-dua permintaan (`req`) dan beberapa maklumat tentang
fail (`fail`) untuk membantu keputusan.

Harap maklum bahawa `req.body` mungkin belum diisi sepenuhnya. Ia bergantung kepada
agar klien menghantar medan dan fail ke pelayan.

Untuk memahami konvensyen panggilan yang digunakan dalam panggilan balik (perlu lulus
null sebagai param pertama), rujuk
[Pengendalian ralat Node.js](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### `MemoryStorage`

Enjin storan memori menyimpan fail dalam ingatan sebagai objek `Buffer`. Ia
tidak mempunyai sebarang pilihan.

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

Apabila menggunakan storan memori, maklumat fail akan mengandungi medan yang dipanggil
`buffer` yang mengandungi keseluruhan fail.

**AMARAN**: Memuat naik fail yang sangat besar, atau fail yang agak kecil dalam besar
nombor dengan sangat cepat, boleh menyebabkan aplikasi anda kehabisan memori apabila
storan memori digunakan.

### `had`

Objek yang menyatakan had saiz sifat pilihan berikut. Multer menghantar objek ini ke busboy secara langsung dan butiran sifat boleh didapati di [halaman busboy](https://github.com/mscdex/busboy#busboy-methods).

Nilai integer berikut tersedia:

Kunci | Penerangan | Lalai
--- | --- | ---
`fieldNameSize` | Saiz nama medan maksimum | 100 bait
`fieldSize` | Saiz nilai medan maksimum (dalam bait) | 1MB
`medan` | Bilangan maksimum medan bukan fail | Infiniti
`Saiz fail` | Untuk borang berbilang bahagian, saiz fail maksimum (dalam bait) | Infiniti
`fail` | Untuk borang berbilang bahagian, bilangan maksimum medan fail | Infiniti
`bahagian` | Untuk borang berbilang bahagian, bilangan maksimum bahagian (medan + fail) | Infiniti
`headerPairs` | Untuk borang berbilang bahagian, bilangan maksimum kekunci pengepala => pasangan nilai untuk dihuraikan | 2000

Menentukan had boleh membantu melindungi tapak anda daripada serangan penafian perkhidmatan (DoS).

### `Penapis fail`

Tetapkan ini kepada fungsi untuk mengawal fail yang harus dimuat naik dan yang mana
patut dilangkau. Fungsi sepatutnya kelihatan seperti ini:

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

## Ralat pengendalian

Apabila menghadapi ralat, Multer akan mewakilkan ralat itu kepada Express. awak boleh
paparkan halaman ralat yang bagus menggunakan [cara ekspres standard](http://expressjs.com/guide/error-handling.html).

Jika anda ingin menangkap ralat secara khusus daripada Multer, anda boleh menghubungi
fungsi middleware sendiri. Selain itu, jika anda hanya ingin menangkap [ralat Multer](https://github.com/expressjs/multer/blob/main/lib/multer-error.js), anda boleh menggunakan kelas `MulterError` yang dilampirkan pada objek `multer` itu sendiri (cth. `err instanceof multer.MulterError`).

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

## Enjin storan tersuai

Untuk mendapatkan maklumat tentang cara membina enjin storan anda sendiri, lihat [Enjin Storan Berbilang](https://github.com/expressjs/multer/blob/main/StorageEngine.md).

## Lesen

[SAYA](LESEN)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
