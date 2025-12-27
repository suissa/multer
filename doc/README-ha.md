# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

Multer shine node.js middleware don sarrafa `multipart/form-data`, wanda aka fi amfani dashi don loda fayiloli. An rubuta
a saman [busboy](https://github.com/mscdex/busboy) don iyakar inganci.

**NOTE**: Multer ba zai aiwatar da kowane nau'i wanda ba multiparts ba (`multipart/form-data`).

## Fassarorin

Hakanan ana samun wannan README a cikin wasu harsuna:

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

## Shigarwa

```sh
$ npm install multer
```

## Amfani

Multer yana ƙara wani abu na 'jiki' da abun 'fayil' ko 'fayal' abu zuwa abun 'buƙatun'. Abun 'jiki' yana ƙunshe da ƙimar filayen rubutu na sigar, abin 'fayil' ko 'files' ya ƙunshi fayilolin da aka ɗora ta hanyar sigar.

Misalin amfani na asali:

Kar a manta `enctype = "multipart/form-data"' a cikin sigar ku.

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

Idan kana buƙatar sarrafa nau'i mai yawa na rubutu-kawai, ya kamata ka yi amfani da hanyar `.none()':

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

Ga misali kan yadda ake amfani da multer a cikin sigar HTML. Ɗauki bayanin kula na musamman na filayen `enctype = "multipart/form-data"' da ''name=" uploaded_file"` filayen:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

Sannan a cikin fayil ɗin javascript ɗinku zaku ƙara waɗannan layin don samun damar duka fayil ɗin da jiki. Yana da mahimmanci ka yi amfani da ƙimar filin 'suna' daga sigar da ke cikin aikin lodawa. Wannan yana bayyana wanne filin akan buƙatun ya kamata ya nemi fayilolin a ciki. Idan waɗannan filayen ba iri ɗaya bane a cikin sigar HTML kuma akan sabar ku, ɗorawar ku zata gaza:

```javascript
const multer  = require('multer')
const upload = multer({ dest: './public/data/uploads/' })
app.post('/stats', upload.single('uploaded_file'), function (req, res) {
  // req.file is the name of your file in the form above, here 'uploaded_file'
  // req.body will hold the text fields, if there were any
  console.log(req.file, req.body)
});
```



# API

### Fayilolin fayil

Kowane fayil ya ƙunshi bayanai masu zuwa:

Maɓalli | Bayani | Lura
--- | --- | ---
'sunan filin' | Sunan filin da aka ƙayyade a cikin tsari |
`asali sunan` | Sunan fayil ɗin akan kwamfutar mai amfani |
'encoding' | Nau'in rikodi na fayil |
`mimetype` | Nau'in Mime na fayil |
`Size` | Girman fayil ɗin a cikin bytes |
'makoma' | Babban fayil ɗin da aka ajiye fayil ɗin zuwa gare shi | 'DiskStorage'
'filename' | Sunan fayil ɗin da ke cikin 'makomar' | 'DiskStorage'
'hanyar' | Cikakken hanyar zuwa fayil ɗin da aka ɗora | 'DiskStorage'
'buffer' | A `Buffer` na dukan fayil | 'Ma'ajiyar Ƙwaƙwalwa'

### `Multer(opts)`

Multer yana karɓar abu na zaɓi, mafi mahimmancin su shine 'wuri'
dukiya, wanda ke gaya wa Multer inda za a loda fayilolin. Idan kun bar abin
abubuwan zažužžukan, fayilolin za a adana su cikin ƙwaƙwalwar ajiya kuma ba za a taɓa rubuta su zuwa faifai ba.

Ta hanyar tsohuwa, Multer zai sake suna fayilolin don guje wa rikice-rikicen suna. The
Ana iya daidaita aikin sake suna bisa ga bukatun ku.

Wadannan su ne zaɓuɓɓukan da za a iya wuce zuwa Multer.

Maɓalli | Bayani
--- | ---
'dest' ko 'ajiya' | Inda za a adana fayilolin
'Filter' | Ayyuka don sarrafa waɗanne fayiloli aka karɓa
'iyaka' | Iyaka na bayanan da aka ɗora
'Tsarin Hanya' | Rike cikakken hanyar fayiloli maimakon sunan tushe kawai

A cikin matsakaicin ƙa'idar gidan yanar gizo, 'dest' kawai za a iya buƙata, kuma an saita shi kamar yadda aka nuna a ciki
misali mai zuwa.

```javascript
const upload = multer({ dest: 'uploads/' })
```

Idan kuna son ƙarin iko akan abubuwan da kuke lodawa, kuna son amfani da 'ajiya'
zaɓi maimakon 'dest'. Multer jiragen ruwa tare da injunan ajiya 'DiskStorage'
da 'MemoryStorage'; Akwai ƙarin injuna daga ɓangare na uku.

#### `. guda (sunan filin)`

Karɓi fayil guda ɗaya tare da sunan 'filin sunan'. Za a adana fayil ɗin guda ɗaya
a cikin 'req.file'.

#### `.array(sunan filin[, maxCount])`

Karɓar jeri na fayiloli, duk tare da sunan `fieldname`. Optionally kuskure fita idan
fiye da 'maxCount' fayiloli ana loda su. Za a adana tsararrun fayiloli a ciki
'req.files'.

#### `.filaye(filaye)`

Karɓar cakuɗen fayiloli, ƙayyadaddun ta 'filaye'. Wani abu mai jeri na fayiloli
za a adana a cikin 'req.files'.

Ya kamata 'filaye' su zama tsararrun abubuwa tare da 'suna' da zaɓin 'maxCount'.
Misali:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.babu()`

Karɓar filayen rubutu kawai. Idan an yi wani loda fayil, kuskure tare da code
"LIMIT\_UNEXPECTED\_FILE" za a fitar.

#### `.kowane()`

Yana karɓar duk fayilolin da suka zo akan waya. Za a adana tarin fayiloli a ciki
'req.files'.

**GARGADI:** Tabbatar cewa koyaushe kuna sarrafa fayilolin da mai amfani ke lodawa.
Kada a taɓa ƙara multer azaman tsakiya na duniya tunda mai amfani da mugunta zai iya lodawa
fayiloli zuwa hanyar da ba ku yi tsammani ba. Yi amfani da wannan aikin akan hanyoyi kawai
inda kake sarrafa fayilolin da aka ɗora.

### 'ajiya'

#### 'DiskStorage'

Injin ajiyar diski yana ba ku cikakken iko akan adana fayiloli zuwa faifai.

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

Akwai zaɓuɓɓuka guda biyu da ake samu, 'makowa' da 'sunan fayil'. Dukansu ne
ayyuka waɗanda ke ƙayyade inda ya kamata a adana fayil ɗin.

Ana amfani da `manufa' don tantance a cikin wanne babban fayil ɗin fayilolin da aka ɗorawa ya kamata
a adana. Hakanan ana iya bayar da wannan azaman 'string' (misali `'/tmp/uploads'). Idan babu
An bayar da `manufa', tsohowar tsarin aiki na wucin gadi
ana amfani da fayiloli.

**Lura:** Kuna da alhakin ƙirƙirar kundin adireshi lokacin samarwa
'makoma' a matsayin aiki. Lokacin wucewa kirtani, multer zai tabbatar da hakan
an ƙirƙira muku littafin.

Ana amfani da `filename' don tantance abin da ya kamata a sanya wa fayil suna a cikin babban fayil ɗin.
Idan ba a bayar da 'filename' ba, kowane fayil za a ba da sunan bazuwar da ba haka ba
hada da kowane tsawo na fayil.

** Lura:** Multer ba zai ƙara maka wani tsawo na fayil ba, aikinka
yakamata ya dawo da sunan fayil cikakke tare da tsawo na fayil.

Kowane aiki yana wucewa duka buƙatun (`req`) da wasu bayanai game da
fayil ɗin ('file') don taimakawa tare da yanke shawara.

Lura cewa 'req.body' mai yiwuwa ba a cika yawan jama'a ba tukuna. Ya dogara da
odar cewa abokin ciniki ya aika filaye da fayiloli zuwa uwar garken.

Don fahimtar al'adar kira da aka yi amfani da ita a cikin kiran baya (yana buƙatar wucewa
null a matsayin farkon param), koma zuwa
[Kuskuren Node.js](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### 'Ajiye Ajiye'

Injin ajiyar ƙwaƙwalwar ajiya yana adana fayilolin a cikin ƙwaƙwalwar ajiya azaman abubuwan 'Buffer'. Yana
bashi da wani zabi.

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

Lokacin amfani da ma'ajiyar žwažwalwar ajiya, bayanin fayil zai ƙunshi filin da ake kira
'buffer' wanda ya ƙunshi dukan fayil ɗin.

**GARGADI**: Ana loda manyan fayiloli, ko ƙananan fayiloli a cikin manya
lambobi da sauri, na iya sa aikace-aikacen ku ya ƙare ƙwaƙwalwar ajiya lokacin
Ana amfani da ma'ajiyar ƙwaƙwalwar ajiya.

### 'iyaka'

Abu mai ƙayyadaddun girman iyakoki na zaɓuɓɓukan zaɓi masu zuwa. Multer yana shigar da wannan abu zuwa cikin busboy kai tsaye, kuma ana iya samun cikakkun bayanan kaddarorin akan [shafin busboy](https://github.com/mscdex/busboy#busboy-methods).

Ana samun ƙimar lamba masu zuwa:

Maɓalli | Bayani | Default
--- | --- | ---
'Filin Sunan Girman' | Girman sunan filin | 100 bytes
'Girbin filin' | Matsakaicin ƙimar ƙimar filin (a cikin bytes) | 1MB
'filaye' | Matsakaicin adadin filayen marasa fayil | Rashin iyaka
'Fayil Girma' | Don nau'ikan sassa da yawa, girman fayil ɗin max (a cikin bytes) | Rashin iyaka
'fiyiloli' | Don siffofin ɓangarori da yawa, max adadin filayen fayil | Rashin iyaka
'bangarori' | Don nau'ikan sassa da yawa, max adadin sassa (filaye + fayiloli) | Rashin iyaka
`headPairs` | Don nau'ikan sassa da yawa, max adadin maɓalli na kai=>darajar nau'i-nau'i don tantancewa | 2000

Ƙayyade iyakoki na iya taimakawa kare rukunin yanar gizonku daga hare-haren hana sabis (DoS).

### 'Filter'

Saita wannan zuwa aiki don sarrafa fayilolin da ya kamata a loda da wanne
ya kamata a tsallake. Aikin ya kamata yayi kama da haka:

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

## Kuskuren kulawa

Lokacin cin karo da kuskure, Multer zai wakilta kuskuren zuwa Express. Za ka iya
nuna kyakkyawan shafin kuskure ta amfani da [daidaitaccen hanyar bayyanawa](http://expressjs.com/guide/error-handling.html).

Idan kuna son kama kurakurai musamman daga Multer, zaku iya kiran su
aikin tsakiya da kanka. Hakanan, idan kuna son kama kawai [kuskuren Multer](https://github.com/expressjs/multer/blob/main/lib/multer-error.js), za ku iya amfani da ajin `MulterError` wanda ke haɗe zuwa abin 'multer' kanta (misali `eerr exampleof multer.MulterError`).

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

## Injin ajiya na musamman

Don bayani kan yadda ake gina injin ajiyar ku, duba [Multer Storage Engine](https://github.com/expressjs/multer/blob/main/StorageEngine.md).

## Lasisi

[MY] (LASIS)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
