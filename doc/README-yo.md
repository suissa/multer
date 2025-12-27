# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

Multer jẹ agbedemeji node.js fun mimu `multipart/form-data`, eyiti o jẹ lilo akọkọ fun ikojọpọ awọn faili. O ti wa ni kikọ
lori oke [busboy] (https://github.com/mscdex/busboy) fun ṣiṣe ti o pọju.

**AKIYESI**: Multer kii yoo ṣe ilana eyikeyi fọọmu eyiti kii ṣe multipart (`multipart/form-data`).

## Awọn itumọ

README yii tun wa ni awọn ede miiran:

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

## Fifi sori ẹrọ

```sh
$ npm install multer
```

## Lilo

Multer ṣe afikun ohun kan 'ara' ati ohun 'faili' tabi ohun 'faili' si nkan 'ibere' naa. Ohun ‘ara’ ni awọn iyeye ti awọn aaye ọrọ fọọmu naa, ‘faili’ tabi ohun ‘faili’ ni awọn faili ti a gbejade nipasẹ fọọmu naa.

Apẹẹrẹ lilo ipilẹ:

Maṣe gbagbe `enctype = "multipart/form-data"` ninu fọọmu rẹ.

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

Ti o ba nilo lati mu fọọmu multipart text-nikan mu, o yẹ ki o lo ọna `.ko si()`:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

Eyi ni apẹẹrẹ lori bi a ṣe lo multer ni fọọmu HTML kan. Ṣe akiyesi pataki ti `enctype = "multipart/form-data"` ati `name=" uploaded_file"` aaye:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

Lẹhinna ninu faili javascript rẹ iwọ yoo ṣafikun awọn laini wọnyi lati wọle si faili mejeeji ati ara. O ṣe pataki ki o lo iye aaye `orukọ` lati inu fọọmu naa ninu iṣẹ ikojọpọ rẹ. Eyi sọ fun aaye wo lori ibeere ti o yẹ ki o wa awọn faili sinu. Ti awọn aaye wọnyi ko ba jẹ kanna ni fọọmu HTML ati lori olupin rẹ, ikojọpọ rẹ yoo kuna:

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

### Alaye faili

Faili kọọkan ni alaye wọnyi ninu:

Bọtini | Apejuwe | Akiyesi
--- | --- | ---
`orukọ aaye` | Orukọ aaye pato ninu fọọmu |
`orukọ atilẹba` | Orukọ faili lori kọnputa olumulo |
`iyipada` | Iru fifi koodu |
`mimetype` | Mime iru faili |
`iwọn` | Iwọn faili ni awọn baiti |
`àwárí` | Awọn folda ti o ti fipamọ faili | 'DiskStorage'
`orukọ faili` | Orukọ faili ti o wa laarin 'ibi-opin' | 'DiskStorage'
`ona` | Ọna kikun si faili ti a gbejade | 'DiskStorage'
`fififa` | A `Buffer` ti gbogbo faili | `Ibi iranti'

### `multer(opts)`

Multer gba ohun awọn aṣayan, ipilẹ julọ eyiti o jẹ 'dest'
ohun ini, eyi ti o sọ Multer ibi ti lati po si awọn faili. Ni irú ti o fi awọn
awọn aṣayan ohun, awọn faili yoo wa ni pa ni iranti ati kò kọ si disk.

Nipa aiyipada, Multer yoo tunrukọ awọn faili naa ki o le yago fun awọn ija lorukọ. Awọn
iṣẹ lorukọmii le jẹ adani gẹgẹbi awọn iwulo rẹ.

Awọn atẹle ni awọn aṣayan ti o le kọja si Multer.

Bọtini | Apejuwe
--- | ---
`deest` tabi `ipamọ` | Nibo ni lati fipamọ awọn faili
`Filter` | Iṣẹ lati ṣakoso iru awọn faili ti o gba
`ipinle' | Awọn ifilelẹ ti awọn Àwọn data
`Path itoju` | Jeki ọna kikun ti awọn faili dipo orukọ ipilẹ nikan

Ninu ohun elo wẹẹbu apapọ, `dest` nikan le nilo, ati tunto bi o ṣe han ninu
apẹẹrẹ atẹle.

```javascript
const upload = multer({ dest: 'uploads/' })
```

Ti o ba fẹ iṣakoso diẹ sii lori awọn ikojọpọ rẹ, iwọ yoo fẹ lati lo `ipamọ' naa
aṣayan dipo `dest`. Multer awọn ọkọ oju omi pẹlu awọn ẹrọ ibi ipamọ `DiskStorage`
ati `MemoryStorage`; Diẹ ẹ sii enjini wa lati ẹni kẹta.

#### `. ẹyọkan (orukọ aaye)`

Gba faili ẹyọkan pẹlu orukọ `orukọ aaye`. Faili ẹyọkan yoo wa ni ipamọ
ninu `req.faili`.

#### `.array(orukọ aaye[, maxCount])`

Gba ọpọlọpọ awọn faili, gbogbo wọn pẹlu orukọ `orukọ aaye'. Iyan aṣiṣe jade ti o ba ti
diẹ ẹ sii ju `maxCount` awọn faili ti wa ni ikojọpọ. Eto awọn faili yoo wa ni ipamọ
`req.faili`.

#### `.awọn aaye(awọn aaye)`

Gba akojọpọ awọn faili, ti a tọka nipasẹ `awọn aaye`. Ohun kan pẹlu awọn akojọpọ awọn faili
yoo wa ni ipamọ ni `req.files`.

`awọn aaye` yẹ ki o jẹ oniruuru awọn nkan pẹlu `orukọ` ati ni yiyan `maxCount`.
Apeere:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.kò si()`

Gba awọn aaye ọrọ nikan. Ti o ba ṣe ikojọpọ faili eyikeyi, aṣiṣe pẹlu koodu
"LIMIT\_UNEXPECTED\_FILE" ni yoo jade.

#### `.eyikeyi()`

Gba gbogbo awọn faili ti o wa lori okun waya. Awọn akojọpọ awọn faili yoo wa ni ipamọ
`req.faili`.

** IKILO: ** Rii daju pe o nigbagbogbo mu awọn faili ti olumulo kan gbejade.
Maṣe ṣafikun multer bi agbedemeji agbaye nitori olumulo irira le gbejade
awọn faili si ipa ọna ti o ko nireti. Lo iṣẹ yii nikan ni awọn ipa-ọna
nibi ti o ti n mu awọn faili ti o gbejade.

### `ipamọ'

#### `DiskStorage`

Ẹrọ ibi ipamọ disiki naa fun ọ ni iṣakoso ni kikun lori titoju awọn faili si disk.

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

Awọn aṣayan meji wa ti o wa, `ibi-ọna` ati `orukọ faili`. Awon mejeeji ni
awọn iṣẹ ti o pinnu ibi ti faili yẹ ki o wa ni ipamọ.

A lo `ibi-afẹde` lati pinnu laarin iru folda ti awọn faili ti o gbejade yẹ ki o pinnu
wa ni ipamọ. Eyi tun le fun ni bi `okun` (fun apẹẹrẹ `'/tmp/awọn agberuwo'). Ti ko ba si
A fun ni ‘ibi-afẹde’, itọsọna aiyipada ẹrọ ẹrọ fun igba diẹ
awọn faili ti wa ni lilo.

**Akiyesi:** O ni iduro fun ṣiṣẹda liana nigbati o pese
`Abode` bi iṣẹ kan. Nigbati o ba n kọja okun kan, multer yoo rii daju pe
awọn liana ti wa ni da fun o.

`filename` ni a lo lati pinnu kini faili yẹ ki o wa ni orukọ ninu folda naa.
Ti ko ba si 'orukọ faili' ti a fun, faili kọọkan yoo fun ni orukọ laileto ti kii ṣe
pẹlu eyikeyi itẹsiwaju faili.

**Akiyesi:** Multer kii yoo ṣafikun eyikeyi itẹsiwaju faili fun ọ, iṣẹ rẹ
yẹ ki o da orukọ faili pada ni pipe pẹlu itẹsiwaju faili kan.

Iṣẹ kọọkan ti kọja mejeeji ibeere (`req`) ati alaye diẹ nipa
faili (`faili`) lati ṣe iranlọwọ pẹlu ipinnu.

Ṣakiyesi pe `req.body` le ko ti kun ni kikun sibẹsibẹ. O da lori awọn
paṣẹ pe alabara gbejade awọn aaye ati awọn faili si olupin naa.

Fun agbọye apejọ pipe ti a lo ninu ipe pada (nilo lati kọja
asan bi param akọkọ), tọka si
[Imudani aṣiṣe Node.js](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### `Ibi Iranti Ibi-ipamọ'

Ẹrọ ibi-itọju iranti n tọju awọn faili sinu iranti bi awọn nkan 'Buffer'. O
ko ni awọn aṣayan.

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

Nigbati o ba nlo ibi ipamọ iranti, alaye faili yoo ni aaye ti a npe ni
`fifipamọ' ti o ni gbogbo faili ninu.

**IKILO**: Ikojọpọ awọn faili ti o tobi pupọ, tabi awọn faili kekere jo ni titobi
awọn nọmba ni yarayara, o le fa ki ohun elo rẹ pari ni iranti nigbati
ipamọ iranti ti lo.

### `awọn ifilelẹ lọ'

Ohun kan ti n ṣalaye awọn opin iwọn ti awọn ohun-ini yiyan atẹle. Multer n gba nkan yii lọ sinu ọkọ ayọkẹlẹ taara, ati awọn alaye ti awọn ohun-ini ni a le rii lori [oju-iwe busboy] (https://github.com/mscdex/busboy#busboy-methods).

Awọn iye odidi wọnyi wa:

Bọtini | Apejuwe | Aiyipada
--- | --- | ---
` aayeOrukọ Iwon` | Max aaye orukọ iwọn | 100 baiti
`Iwon aaye` | Max aaye iye iwọn (ni awọn baiti) | 1MB
`awọn aaye` | Nọmba ti o pọju ti awọn aaye ti kii ṣe faili | Ailopin
`Iwon faili` | Fun multipart fọọmu, awọn max faili iwọn (ni awọn baiti) | Ailopin
`awọn faili` | Fun awọn fọọmu multipart, awọn max nọmba ti awọn aaye faili | Ailopin
`awọn ẹya` | Fun multipart fọọmu, awọn max nọmba ti awọn ẹya ara (awọn aaye + awọn faili) | Ailopin
`headerPairs` | Fun awọn fọọmu multipart, nọmba ti o pọju bọtini akọsori=>awọn orisii iye lati ṣe itupalẹ | 2000

Pato awọn opin le ṣe iranlọwọ lati daabobo aaye rẹ lodi si kiko iṣẹ (DoS) kọlu.

### `Filter'

Ṣeto eyi si iṣẹ kan lati ṣakoso iru awọn faili yẹ ki o gbejade ati eyiti
yẹ ki o fo. Iṣẹ naa yẹ ki o dabi eyi:

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

## Aṣiṣe mimu

Nigbati o ba pade aṣiṣe kan, Multer yoo fi aṣiṣe naa ranṣẹ si KIAKIA. O le
ṣe afihan oju-iwe aṣiṣe to wuyi nipa lilo [ọna kiakia ti o ṣe deede] (http://expressjs.com/guide/error-handling.html).

Ti o ba fẹ lati yẹ awọn aṣiṣe pataki lati Multer, o le pe awọn
middleware iṣẹ nipa ara rẹ. Paapaa, ti o ba fẹ mu nikan [awọn aṣiṣe Multer](https://github.com/expressjs/multer/blob/main/lib/multer-error.js), o le lo kilaasi `MulterError` ti o so mọ ohun `multer` funrararẹ (fun apẹẹrẹ `err instanceof multer.MulterError`).

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

## Aṣa ipamọ engine

Fun alaye lori bi o ṣe le kọ ẹrọ ibi ipamọ tirẹ, wo [Multer Storage Engine](https://github.com/expressjs/multer/blob/main/StorageEngine.md).

## Iwe-aṣẹ

[MI](IWE-aṣẹ)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
