# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

Multer minangka middleware node.js kanggo nangani `multipart/form-data`, sing utamané digunakake kanggo ngunggah file. Iku ditulis
ing ndhuwur [busboy] (https://github.com/mscdex/busboy) kanggo efficiency maksimum.

**CATETAN**: Multer ora bakal ngolah wujud apa wae sing ora multipart (`multipart/form-data`).

## Terjemahan

README iki uga kasedhiya ing basa liya:

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

## Instalasi

```sh
$ npm install multer
```

## Panganggone

Multer nambahake obyek `body` lan obyek `file` utawa `file` menyang obyek `request`. Obyek `body` ngemot nilai kolom teks formulir, obyek `file` utawa `file` ngemot file sing diunggah liwat formulir.

Conto panggunaan dhasar:

Aja lali `enctype="multipart/form-data"` ing formulir sampeyan.

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

Yen sampeyan kudu nangani formulir multipart mung teks, sampeyan kudu nggunakake metode `.none()`:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

Punika conto carane multer digunakake ing wangun HTML. Elinga lapangan `enctype="multipart/form-data"` lan `name="uploaded_file"`:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

Banjur ing file javascript sampeyan bakal nambah garis kasebut kanggo ngakses file lan awak. Penting sampeyan nggunakake nilai kolom `jeneng` saka formulir ing fungsi unggahan sampeyan. Iki ngandhani multer kolom ing panyuwunan sing kudu digoleki file kasebut. Yen kolom kasebut ora padha ing formulir HTML lan ing server sampeyan, unggahan sampeyan bakal gagal:

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

### Informasi file

Saben file ngemot informasi ing ngisor iki:

Kunci | katrangan | Cathetan
--- | --- | ---
`jeneng lapangan` | Jeneng kolom kasebut ing wangun |
`jeneng asli` | Jeneng berkas ing komputer pangguna |
`enkoding` | Jinis enkoding berkas |
`mimetype` | Jinis mime berkas |
`ukuran` | Ukuran berkas ing bita |
`tujuan` | Folder sing wis disimpen file | `DiskStorage`
`jeneng berkas` | Jeneng berkas ing `tujuan` | `DiskStorage`
`path` | Path lengkap menyang berkas sing diunggahaké | `DiskStorage`
`buffer` | A `Buffer` saka kabèh berkas | `MemoriStorage`

### `multer(milih)`

Multer nampa obyek opsi, sing paling dhasar yaiku `dest`
property, kang ngandhani Multer ngendi kanggo ngunggah file. Yen sampeyan ngilangi
obyek opsi, file bakal disimpen ing memori lan ora bakal ditulis ing disk.

Kanthi gawan, Multer bakal ngganti jeneng file supaya ora konflik jeneng. Ing
fungsi ngganti jeneng bisa selaras miturut kabutuhan.

Ing ngisor iki minangka pilihan sing bisa dikirim menyang Multer.

Kunci | Katrangan
--- | ---
`dest` utawa `panyimpenan` | Where kanggo nyimpen file
`Filter' | Fungsi kanggo ngontrol file sing ditampa
`wates` | Watesan data sing diunggah
`preservePath` | Tansah path lengkap file tinimbang mung jeneng dhasar

Ing aplikasi web rata-rata, mung `tujuh` sing dibutuhake, lan dikonfigurasi kaya sing dituduhake ing
tuladha ing ngisor iki.

```javascript
const upload = multer({ dest: 'uploads/' })
```

Yen sampeyan pengin luwih ngontrol unggahan sampeyan, sampeyan pengin nggunakake `panyimpenan`
pilihan tinimbang `dest`. Kapal Multer nganggo mesin panyimpenan `DiskStorage`
lan `MemoryStorage`; Mesin liyane kasedhiya saka pihak katelu.

#### `.single(fieldname)`

Nampa file siji kanthi jeneng `fieldname`. File siji bakal disimpen
ing `req.file`.

#### `.array(fieldname[, maxCount])`

Nampa larik file, kabeh nganggo jeneng `fieldname`. Optionally kesalahan metu yen
luwih saka file `maxCount` sing diunggah. Array file bakal disimpen ing
`req.files`.

#### `.fields(fields)`

Nampa campuran file, sing ditemtokake dening `fields`. Objek kanthi susunan file
bakal disimpen ing `req.files`.

`fields` kudu dadi array obyek kanthi `jeneng` lan opsional `maxCount`.
Tuladha:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.ora ana ()`

Nampa mung kolom teks. Yen ana upload file, kesalahan karo kode
"LIMIT\_UNEXPECTED\_FILE" bakal diterbitake.

#### `.any()`

Nampa kabeh file sing teka liwat kabel. Array file bakal disimpen ing
`req.files`.

**PÈNGET:** Priksa manawa sampeyan tansah nangani file sing diunggahaké pangguna.
Aja nambah multer minangka middleware global amarga pangguna jahat bisa ngunggah
file menyang rute sing ora diantisipasi. Mung nggunakake fungsi iki ing rute
ngendi sampeyan nangani file sing diunggah.

### `panyimpenan`

#### `DiskStorage`

Mesin panyimpenan disk menehi kontrol lengkap kanggo nyimpen file menyang disk.

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

Ana rong pilihan sing kasedhiya, `tujuan` lan `jeneng file`. Loro-lorone
fungsi sing nemtokake ngendi file kudu disimpen.

`tujuan` digunakake kanggo nemtokake ing folder endi file sing diunggah
disimpen. Iki uga bisa diwenehi minangka `string` (contone `'/tmp/uploads'`). Yen ora
`tujuan` diwenehi, direktori standar sistem operasi kanggo sementara
file digunakake.

** Cathetan: ** Sampeyan tanggung jawab kanggo nggawe direktori nalika nyedhiyakake
`tujuan` minangka fungsi. Nalika ngliwati senar, multer bakal nggawe manawa
direktori digawe kanggo sampeyan.

`filename` digunakake kanggo nemtokake file apa sing kudu dijenengi ing folder kasebut.
Yen ora ana `filename` diwenehi, saben file bakal diwenehi jeneng acak sing ora
kalebu ekstensi file apa wae.

** Cathetan: ** Multer ora bakal nambah ekstensi file kanggo sampeyan, fungsi sampeyan
ngirim bali jeneng berkas lengkap karo ekstensi file.

Saben fungsi bakal ngliwati panjalukan (`req`) lan sawetara informasi babagan
file (`file`) kanggo bantuan karo kaputusan.

Elinga yen `req.body` bisa uga durung diisi kanthi lengkap. Iku gumantung ing
supaya klien ngirim lapangan lan file menyang server.

Kanggo mangerteni konvensi panggilan sing digunakake ing callback (perlu lulus
null minangka param pisanan), deleng
[Penanganan kesalahan Node.js](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### `MemoryStorage`

Mesin panyimpenan memori nyimpen file ing memori minangka obyek `Buffer`. Iku
ora duwe pilihan.

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

Nalika nggunakake panyimpenan memori, info file bakal ngemot kolom disebut
`buffer` sing ngemot kabeh file.

**PÈNGET**: Ngunggah file sing gedhé banget, utawa file sing ukurané cilik
nomer cepet banget, bisa nimbulaké aplikasi kanggo mbukak metu saka memori nalika
panyimpenan memori digunakake.

### `wates`

Obyek sing nemtokake watesan ukuran properti opsional ing ngisor iki. Multer ngliwati obyek iki menyang busboy langsung, lan rincian properti bisa ditemokake ing [kaca busboy] (https://github.com/mscdex/busboy#busboy-methods).

Nilai integer ing ngisor iki kasedhiya:

Kunci | katrangan | Default
--- | --- | ---
`fieldNameSize` | Ukuran jeneng lapangan maksimal | 100 bita
`ukuran lapangan` | Ukuran nilai lapangan maksimal (ing bita) | 1 MB
`sawah` | Jumlah maksimum kolom non-berkas | tanpa wates
`Ukuran berkas` | Kanggo formulir multipart, ukuran file maksimal (ing bita) | tanpa wates
`berkas` | Kanggo formulir multipart, jumlah maksimum kolom berkas | tanpa wates
`bagean` | Kanggo wangun multipart, jumlah max bagean (lapangan + file) | tanpa wates
`headerPairs` | Kanggo wangun multipart, jumlah maksimum tombol header => pasangan nilai kanggo parse | 2000

Nemtokake watesan bisa mbantu nglindhungi situs sampeyan saka serangan penolakan layanan (DoS).

### `Filter'

Setel iki menyang fungsi kanggo ngontrol file sing kudu diunggah lan endi
kudu dilewati. Fungsi kasebut kudu katon kaya iki:

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

## Penanganan kesalahan

Nalika nemoni kesalahan, Multer bakal utusan kesalahan kasebut menyang Express. Sampeyan bisa
nampilake kaca kesalahan sing apik nggunakake [cara ekspres standar] (http://expressjs.com/guide/error-handling.html).

Yen sampeyan pengin nyekel kasalahan khusus saka Multer, sampeyan bisa nelpon ing
fungsi middleware dhewe. Uga, yen sampeyan pengin nyekel mung [kesalahan Multer] (https://github.com/expressjs/multer/blob/main/lib/multer-error.js), sampeyan bisa nggunakake kelas `MulterError` sing ditempelake ing obyek `multer` dhewe (contone `err instanceof multer.MulterError`).

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

## Mesin panyimpenan khusus

Kanggo informasi babagan carane nggawe mesin panyimpenan dhewe, deleng [Multer Storage Engine](https://github.com/expressjs/multer/blob/main/StorageEngine.md).

## Lisensi

[MY](LISENSI)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
