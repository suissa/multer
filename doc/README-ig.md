# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

Multer bụ node.js middleware maka ijikwa `multipart/form-data`, nke a na-ejikarị maka ibugo faịlụ. Edere ya
n'elu [busboy](https://github.com/mscdex/busboy) maka oke arụmọrụ.

**IHE**: Multer agaghị ahazi ụdị ọ bụla nke na-abụghị multipart (`multipart/form-data`).

## Ntụgharị asụsụ

README a dịkwa n'asụsụ ndị ọzọ:

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

## Nwụnye

```sh
$ npm install multer
```

## Ojiji

Multer na-agbakwunye ihe 'ahụ' yana ihe 'faịlụ' ma ọ bụ 'faịlụ' na ihe 'arịrịọ'. Ihe 'ahụ' nwere ụkpụrụ nke mpaghara ederede nke ụdị ahụ, ihe 'faịlụ' ma ọ bụ 'faịlụ' nwere faịlụ ndị ebugoro site na ụdị.

Ihe atụ eji eme ihe:

Echefula `enctype="multipart/form-data"' n'ụdị gị.

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

Ọ bụrụ na ịchọrọ ijikwa fọm multipart naanị ederede, ị ga-eji usoro `.enweghị()':

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

Nke a bụ ọmụmaatụ otu esi eji multer eme ihe n'ụdị HTML. Deba ama pụrụ iche nke mpaghara `enctype="multipart/form-data" na 'name=" uploaded_file"':

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

Mgbe ahụ na faịlụ javascript gị, ị ga-agbakwunye ahịrị ndị a iji nweta ma faịlụ ma ahụ. Ọ dị mkpa ka ị jiri uru ubi 'aha' sitere na ụdị dị na ọrụ mbulite gị. Nke a na-agwa multer nke ubi na arịrịọ ọ kwesịrị ịchọ faịlụ na. Ọ bụrụ na ubi ndị a abụghị otu na HTML ụdị na gị na ihe nkesa, gị bulite ga-ada:

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

### Ozi faịlụ

Faịlụ ọ bụla nwere ozi ndị a:

Igodo | Nkọwa | Rịba ama
--- | --- | ---
'aha ubi' | Aha mpaghara akọwapụtara n'ụdị |
'aha mbụ' | Aha faịlụ na kọmputa onye ọrụ |
'nkọwapụta' | Ụdị ntinye faịlụ |
'mimetype' | Ụdị mime nke faịlụ |
'nha' | Ogo nke faịlụ na bytes |
'ebe' | Ebe nchekwa nke echekwara faịlụ | 'Storage'
'aha faịlụ' | Aha faịlụ n'ime 'ebe' | 'Storage'
'ụzọ' | Uzo zuru oke na faịlụ ebugoro | 'Storage'
'buffer' | Ihe nchekwa 'nke faịlụ niile | 'Nchekwa Nchekwa'

### `multer(nhọrọ)`

Multer na-anabata ihe nhọrọ, nke kachasị bụ 'dest'
ihe onwunwe, nke na-agwa Multer ebe ọ ga-ebugo faịlụ. Ọ bụrụ na ị hapụ ya
nhọrọ ihe, faịlụ ga-edobe na ebe nchekwa na ọ dịghị mgbe e dere na disk.

Site na ndabara, Multer ga-atụgharị aha faịlụ ndị ahụ ka ịzenarị esemokwu ịkpọ aha. Nke
Enwere ike ịhazi ọrụ renaming dịka mkpa gị si dị.

Ndị a bụ nhọrọ ndị enwere ike ịfefe na Multer.

Igodo | Nkọwa
--- | ---
'dest' ma ọ bụ 'nchekwa' | Ebe ị ga-echekwa faịlụ
'Filter' | Ọrụ iji jikwaa faịlụ ndị anabatara
'oke' | Oke data ebugoro
'Chekwa Ụzọ' | Debe faịlụ zuru oke kama naanị aha ntọala

Na nkezi ngwa weebụ, ọ bụ naanị 'dest' ka a ga-achọ, ma hazie ya dịka egosiri na
ihe atụ na-esonụ.

```javascript
const upload = multer({ dest: 'uploads/' })
```

Ọ bụrụ na ịchọrọ njikwa karịa na ebugo gị, ị ga-achọ iji 'nchekwa'
nhọrọ kama 'dest'. Mgbanwe ụgbọ mmiri nwere igwe nchekwa 'DiskStorage'
na 'Nchekwa Nchekwa'; Injin ndị ọzọ dị site na ndị ọzọ.

#### `. otu (aha ubi)`

Nabata otu faịlụ nwere aha 'fieldname'. A ga-echekwa otu faịlụ ahụ
na 'req.file'.

#### `.array(aha ubi[, maxCount])`

Nabata ọtụtụ faịlụ, ha niile nwere aha 'fieldname'. Mebie nke nhọrọ ma ọ bụrụ
A na-ebugo karịa faịlụ 'maxCount'. A ga-echekwa ụdị faịlụ n'ime
'req.faịlụ'.

#### `.ubi(ubi)`

Nabata ngwakọta faịlụ, nke 'ubi' akọwapụtara. Ihe nwere ọtụtụ faịlụ
a ga-echekwa na 'req.files'.

'ubi' kwesịrị ịbụ ọtụtụ ihe nwere 'aha' yana nhọrọ 'maxCount'.
Ọmụmaatụ:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.ọ dịghị onye()`

Nabata naanị mpaghara ederede. Ọ bụrụ na ebugo faịlụ ọ bụla, njehie na koodu
"LIMIT\_UNEXPECTED\_FILE" ga-ewepụta.

#### `. ọ bụla ()`

Nabata faịlụ niile na-abịa n'elu waya. A ga-echekwa ọtụtụ faịlụ n'ime
'req.faịlụ'.

** ịdọ aka ná ntị:** Gbaa mbọ hụ na ị na-ejikwa faịlụ ndị onye ọrụ na-ebugo mgbe niile.
E tinyela multer dị ka etiti etiti ụwa ebe ọ bụ na onye ọrụ ọjọọ nwere ike bulite
faịlụ gaa n'ụzọ nke ị na-atụghị anya ya. Jiri naanị ọrụ a n'ụzọ
ebe ị na-ejizi faịlụ ebugoro.

### 'nchekwa'

#### 'DiskStorage'

Igwe nchekwa diski na-enye gị njikwa zuru oke na ịchekwa faịlụ na diski.

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

Enwere nhọrọ abụọ dị, 'ebe' na 'filename'. Ha abụọ bụ
ọrụ na-ekpebi ebe a ga-echekwa faịlụ ahụ.

A na-eji 'ebe' iji chọpụta n'ime folda faịlụ ebugoro kwesịrị
echekwabara. Enwere ike ịnye nke a dịka 'string' (dịka '''/tmp/bulite''). Ọ bụrụ mba
Enyere 'ebe', ndekọ ndabere nke sistemụ arụmọrụ maka nwa oge
a na-eji faịlụ .

** Mara:** Ọ bụ gị na-ahụ maka imepụta ndekọ mgbe ị na-enye
'ebe' dị ka ọrụ. Mgbe ị na-agafe eriri, multer ga-ahụ na nke ahụ
a na-emepụta ndekọ aha maka gị.

A na-eji `filename' chọpụta ihe ekwesịrị ịkpọ aha faịlụ n'ime folda ahụ.
Ọ bụrụ na enyeghị 'filename', faịlụ ọ bụla ga-enye aha na-enweghị usoro
tinye ndọtị faịlụ ọ bụla.

**Mara:** Multer agaghị etinye ndọtị faịlụ ọ bụla maka gị, ọrụ gị
kwesịrị iweghachi aha faịlụ zuru oke yana ndọtị faịlụ.

Ọrụ ọ bụla na-agafe ma arịrịọ (`req`) yana ụfọdụ ozi gbasara ya
faịlụ ('file`) iji nyere aka na mkpebi ahụ.

Mara na o nwere ike ịbụ na 'req.body' enwebeghị ndị mmadụ juru na ya. Ọ dabere na
ịtụ na onye ahịa na-ebufe ubi na faịlụ na ihe nkesa.

Maka ịghọta mgbakọ ọkpụkpọ ejiri na oku azụ (chọrọ ịgafe
null dị ka param mbụ), rụtụ aka
[Njikwa njehie Node.js](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### 'Nchekwa Nchekwa'

Igwe nchekwa ebe nchekwa na-echekwa faịlụ na ebe nchekwa dị ka ihe 'nkwadebe'. Ọ
enweghị nhọrọ ọ bụla.

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

Mgbe ị na-eji nchekwa ebe nchekwa, ozi faịlụ ga-enwe mpaghara akpọrọ
'buffer' nke nwere faịlụ niile.

**Ịdọ aka ná ntị**: Na-ebugote nnukwu faịlụ, ma ọ bụ obere faịlụ na nnukwu
ọnụọgụgụ ngwa ngwa, nwere ike ime ka ngwa gị kwụsị ebe nchekwa mgbe
a na-eji nchekwa ebe nchekwa.

### 'oke'

Ihe na-akọwa oke oke nke njirimara nhọrọ ndị a. Multer na-ebufe ihe a n'ime bọsboy ozugbo, na nkọwapụta nke akụrụngwa nwere ike ịhụ na [busboy's page](https://github.com/mscdex/busboy#busboy-methods).

Ọnụ ahịa integer ndị a dị:

Igodo | Nkọwa | Ọdabara
--- | --- | ---
'Aha aha nha' | Oke ubi aha nha | 100 bytes
'Nha ubi' | Ọnụ ahịa oke oke (na bytes) | 1MB
'ubi' | Ọnụ ọgụgụ kachasị nke mpaghara na-abụghị faịlụ | Enweghị ngwụcha
'Nha faịlụ' | Maka ụdị multipart, nha faịlụ kachasị (na bytes) | Enweghị ngwụcha
'faịlụ' | Maka ụdị multipart, ọnụ ọgụgụ kachasị nke mpaghara faịlụ | Enweghị ngwụcha
'akụkụ' | Maka ụdị multipart, ọnụ ọgụgụ kachasị nke akụkụ (ubi + faịlụ) | Enweghị ngwụcha
'headerPairs' | Maka ụdị akụkụ dị iche iche, ọnụọgụ isi isi => uru ụzọ abụọ iji tụgharịa | 2000

Ikowa oke nwere ike inye aka chedo saịtị gị pụọ na mwakpo ịgọnarị ọrụ (DoS).

### 'Filter'

Tọọ nke a ka ọ bụrụ ọrụ iji jikwaa faịlụ ndị a ga-ebugo na nke
kwesiri ịmafe ya. Ọrụ kwesịrị ịdị ka nke a:

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

## Mmezi njikwa

Mgbe ị na-ezute mperi, Multer ga-enyefe njehie ahụ na Express. Ị nwere ike
Gosipụta ibe njehie mara mma site na iji [ụkpụrụ awara awara](http://expressjs.com/guide/error-handling.html).

Ọ bụrụ na ịchọrọ ijide njehie kpọmkwem site na Multer, ị nwere ike ịkpọ ya
Middleware arụ ọrụ n'onwe gị. Ọzọkwa, ọ bụrụ na ịchọrọ ijide naanị [Mmehie Multer](https://github.com/expressjs/multer/blob/main/lib/multer-error.js), ị nwere ike iji klaasị `MulterError` nke etinyere na ihe `multer` n'onwe ya (dịka `err instanceof multer.MulterError`).

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

## Igwe nchekwa nchekwa omenala

Maka ozi gbasara otu esi arụ igwe nchekwa nke gị, lee [Multer Storage Engine](https://github.com/expressjs/multer/blob/main/StorageEngine.md).

## Ikikere

[MY] (Ikikere)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
