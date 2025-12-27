# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

મલ્ટર એ `મલ્ટીપાર્ટ/ફોર્મ-ડેટા`ને હેન્ડલ કરવા માટે નોડ.જેએસ મિડલવેર છે, જેનો ઉપયોગ મુખ્યત્વે ફાઇલો અપલોડ કરવા માટે થાય છે. લખેલું છે
મહત્તમ કાર્યક્ષમતા માટે [busboy](https://github.com/mscdex/busboy) ની ટોચ પર.

**નોંધ**: મલ્ટર કોઈપણ ફોર્મ પર પ્રક્રિયા કરશે નહીં જે મલ્ટિપાર્ટ (`મલ્ટિપાર્ટ/ફોર્મ-ડેટા`) નથી.

## અનુવાદો

આ README અન્ય ભાષાઓમાં પણ ઉપલબ્ધ છે:

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

## ઇન્સ્ટોલેશન

```sh
$ npm install multer
```

## ઉપયોગ

મલ્ટર `વિનંતી` ઑબ્જેક્ટમાં `બોડી` ઑબ્જેક્ટ અને `ફાઇલ` અથવા `ફાઇલ્સ` ઑબ્જેક્ટ ઉમેરે છે. `બોડી` ઑબ્જેક્ટ ફોર્મના ટેક્સ્ટ ફીલ્ડના મૂલ્યો ધરાવે છે, `ફાઇલ` અથવા `ફાઇલ` ઑબ્જેક્ટ ફોર્મ દ્વારા અપલોડ કરેલી ફાઇલો ધરાવે છે.

મૂળભૂત ઉપયોગ ઉદાહરણ:

તમારા ફોર્મમાં `enctype="multipart/form-data"` ભૂલશો નહીં.

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

જો તમારે ટેક્સ્ટ-ઓન્લી મલ્ટિપાર્ટ ફોર્મ હેન્ડલ કરવાની જરૂર હોય, તો તમારે `.none()` પદ્ધતિનો ઉપયોગ કરવો જોઈએ:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

HTML ફોર્મમાં મલ્ટરનો ઉપયોગ કેવી રીતે થાય છે તેનું ઉદાહરણ અહીં છે. `enctype="multipart/form-data"` અને `name="uploaded_file"` ફીલ્ડની ખાસ નોંધ લો:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

પછી તમારી જાવાસ્ક્રિપ્ટ ફાઇલમાં તમે ફાઇલ અને બોડી બંનેને ઍક્સેસ કરવા માટે આ રેખાઓ ઉમેરશો. તે મહત્વનું છે કે તમે તમારા અપલોડ કાર્યમાં ફોર્મમાંથી `નામ` ફીલ્ડ મૂલ્યનો ઉપયોગ કરો. આ મલ્ટરને જણાવે છે કે વિનંતિ પર કયા ફીલ્ડમાં તેણે ફાઇલો શોધવી જોઈએ. જો આ ફીલ્ડ્સ HTML ફોર્મમાં અને તમારા સર્વર પર સમાન ન હોય, તો તમારું અપલોડ નિષ્ફળ જશે:

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

### ફાઇલ માહિતી

દરેક ફાઇલમાં નીચેની માહિતી શામેલ છે:

કી | વર્ણન | નોંધ
--- | --- | ---
`ક્ષેત્રનું નામ` | ફોર્મમાં ઉલ્લેખિત ક્ષેત્રનું નામ |
`મૂળ નામ` | વપરાશકર્તાના કમ્પ્યુટર પર ફાઇલનું નામ |
`એનકોડિંગ` | ફાઇલનો એન્કોડિંગ પ્રકાર |
`mimetype` | ફાઇલનો માઇમ પ્રકાર |
`માપ` | બાઈટમાં ફાઈલનું કદ |
`ગંતવ્ય` | ફોલ્ડર કે જેમાં ફાઇલ સાચવવામાં આવી છે | 'ડિસ્ક સ્ટોરેજ'
`ફાઇલનામ` | `ગંતવ્ય` ની અંદરની ફાઇલનું નામ | 'ડિસ્ક સ્ટોરેજ'
`પાથ` | અપલોડ કરેલી ફાઇલનો સંપૂર્ણ માર્ગ | 'ડિસ્ક સ્ટોરેજ'
`બફર` | આખી ફાઇલનું `બફર` | 'મેમરી સ્ટોરેજ'

### `multer(opts)`

મલ્ટર વિકલ્પો ઑબ્જેક્ટ સ્વીકારે છે, જેમાંથી સૌથી મૂળભૂત `ડેસ્ટ` છે
મિલકત, જે મલ્ટરને કહે છે કે ફાઇલો ક્યાં અપલોડ કરવી. જો તમે છોડી દો
વિકલ્પો ઑબ્જેક્ટ, ફાઇલોને મેમરીમાં રાખવામાં આવશે અને ડિસ્ક પર ક્યારેય લખવામાં આવશે.

મૂળભૂત રીતે, મલ્ટર ફાઇલોનું નામ બદલી નાખશે જેથી નામકરણ તકરાર ટાળી શકાય. આ
નામ બદલવાનું કાર્ય તમારી જરૂરિયાતો અનુસાર કસ્ટમાઇઝ કરી શકાય છે.

નીચેના વિકલ્પો છે જે મલ્ટરને પસાર કરી શકાય છે.

કી | વર્ણન
--- | ---
`ડેસ્ટ` અથવા `સ્ટોરેજ` | ફાઇલો ક્યાં સંગ્રહિત કરવી
`ફાઇલફિલ્ટર` | કઈ ફાઈલો સ્વીકારવામાં આવે છે તે નિયંત્રિત કરવા માટેનું કાર્ય
`મર્યાદા` | અપલોડ કરેલા ડેટાની મર્યાદા
`preservePath` | ફક્ત આધાર નામને બદલે ફાઇલોનો સંપૂર્ણ પાથ રાખો

સરેરાશ વેબ એપમાં, માત્ર `dest` ની જરૂર પડી શકે છે, અને તેમાં બતાવ્યા પ્રમાણે ગોઠવેલ છે
નીચેનું ઉદાહરણ.

```javascript
const upload = multer({ dest: 'uploads/' })
```

જો તમને તમારા અપલોડ્સ પર વધુ નિયંત્રણ જોઈતું હોય, તો તમે `સ્ટોરેજ`નો ઉપયોગ કરવા માગો છો
'dest' ને બદલે વિકલ્પ. સંગ્રહ એન્જિન `ડિસ્કસ્ટોરેજ` સાથે મલ્ટર જહાજો
અને 'મેમરી સ્ટોરેજ'; તૃતીય પક્ષો પાસેથી વધુ એન્જિન ઉપલબ્ધ છે.

#### `.સિંગલ(ફિલ્ડનામ)`

`ફિલ્ડનામ` નામની એક ફાઇલ સ્વીકારો. સિંગલ ફાઇલ સંગ્રહિત કરવામાં આવશે
`req.file` માં.

#### `.એરે(ફિલ્ડનામ[, maxCount])`

ફાઇલોની એરે સ્વીકારો, બધી જ `ફિલ્ડનામ` નામ સાથે. જો વૈકલ્પિક રીતે ભૂલ કરો
`maxCount` કરતાં વધુ ફાઇલો અપલોડ કરવામાં આવી છે. ફાઇલોની એરેમાં સંગ્રહિત કરવામાં આવશે
`req.files`.

#### `.ક્ષેત્રો(ક્ષેત્રો)`

`ફીલ્ડ્સ` દ્વારા ઉલ્લેખિત ફાઇલોના મિશ્રણને સ્વીકારો. ફાઇલોના એરે સાથેનો ઑબ્જેક્ટ
`req.files` માં સંગ્રહિત થશે.

`ફિલ્ડ્સ` `નામ` અને વૈકલ્પિક રીતે `મેક્સકાઉન્ટ` સાથેના ઑબ્જેક્ટની શ્રેણી હોવી જોઈએ.
ઉદાહરણ:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.none()`

ફક્ત ટેક્સ્ટ ફીલ્ડ્સ સ્વીકારો. જો કોઈપણ ફાઇલ અપલોડ કરવામાં આવી હોય, તો કોડ સાથે ભૂલ
"LIMIT\_UNEXPECTED\_FILE" જારી કરવામાં આવશે.

#### `.any()`

વાયર ઉપર આવતી તમામ ફાઇલોને સ્વીકારે છે. ફાઇલોની શ્રેણીમાં સંગ્રહિત કરવામાં આવશે
`req.files`.

**ચેતવણી:** ખાતરી કરો કે તમે હંમેશા વપરાશકર્તા દ્વારા અપલોડ કરેલી ફાઇલોને હેન્ડલ કરો છો.
મલ્ટરને વૈશ્વિક મિડલવેર તરીકે ક્યારેય ઉમેરશો નહીં કારણ કે દૂષિત વપરાશકર્તા અપલોડ કરી શકે છે
તમે ધાર્યું ન હોય તેવા રૂટની ફાઇલો. આ ફંક્શનનો ઉપયોગ ફક્ત રૂટ પર કરો
જ્યાં તમે અપલોડ કરેલી ફાઇલોને હેન્ડલ કરી રહ્યાં છો.

### `સ્ટોરેજ`

#### `ડિસ્ક સ્ટોરેજ`

ડિસ્ક સ્ટોરેજ એન્જિન તમને ડિસ્ક પર ફાઇલોને સ્ટોર કરવા પર સંપૂર્ણ નિયંત્રણ આપે છે.

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

ત્યાં બે વિકલ્પો ઉપલબ્ધ છે, `ગંતવ્ય` અને `ફાઇલનામ`. તેઓ બંને છે
વિધેયો જે નક્કી કરે છે કે ફાઇલ ક્યાં સંગ્રહિત થવી જોઈએ.

અપલોડ કરેલી ફાઇલો કયા ફોલ્ડરમાં હોવી જોઈએ તે નક્કી કરવા માટે `ગંતવ્ય` નો ઉપયોગ થાય છે
સંગ્રહ કરવો. આને `સ્ટ્રિંગ` તરીકે પણ આપી શકાય છે (દા.ત. `'/tmp/uploads'`). જો ના
`ગંતવ્ય` આપેલ છે, કામચલાઉ માટે ઑપરેટિંગ સિસ્ટમની ડિફૉલ્ટ ડિરેક્ટરી
ફાઇલોનો ઉપયોગ થાય છે.

**નોંધ:** પ્રદાન કરતી વખતે તમે ડિરેક્ટરી બનાવવા માટે જવાબદાર છો
કાર્ય તરીકે `ગંતવ્ય`. સ્ટ્રિંગ પસાર કરતી વખતે, મલ્ટર તેની ખાતરી કરશે
ડિરેક્ટરી તમારા માટે બનાવવામાં આવી છે.

ફોલ્ડરની અંદર ફાઇલનું નામ શું હોવું જોઈએ તે નિર્ધારિત કરવા માટે `ફાઇલનામ` વપરાય છે.
જો કોઈ `ફાઇલનામ` આપવામાં આવ્યું નથી, તો દરેક ફાઇલને રેન્ડમ નામ આપવામાં આવશે જે નથી
કોઈપણ ફાઇલ એક્સ્ટેંશન શામેલ કરો.

**નોંધ:** મલ્ટર તમારા માટે, તમારા કાર્ય માટે કોઈપણ ફાઇલ એક્સ્ટેંશનને જોડશે નહીં
ફાઇલ એક્સ્ટેંશન સાથે પૂર્ણ થયેલ ફાઇલનામ પરત કરવું જોઈએ.

દરેક ફંક્શન વિનંતી (`req`) અને તેના વિશે કેટલીક માહિતી બંને પસાર થાય છે
નિર્ણય લેવામાં મદદ કરવા માટે ફાઇલ (`ફાઇલ`).

નોંધ કરો કે `req.body` હજુ સુધી સંપૂર્ણ રીતે ભરાયેલું ન હોઈ શકે. તે પર આધાર રાખે છે
ઓર્ડર કે ક્લાયંટ ફીલ્ડ્સ અને ફાઇલોને સર્વર પર ટ્રાન્સમિટ કરે.

કૉલબેકમાં વપરાયેલ કૉલિંગ કન્વેન્શનને સમજવા માટે (પાસ કરવાની જરૂર છે
પ્રથમ પરમ તરીકે નલ), નો સંદર્ભ લો
[Node.js એરર હેન્ડલિંગ](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### `મેમરી સ્ટોરેજ`

મેમરી સ્ટોરેજ એન્જિન મેમરીમાં ફાઇલોને `બફર` ઑબ્જેક્ટ તરીકે સંગ્રહિત કરે છે. તે
પાસે કોઈ વિકલ્પ નથી.

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

મેમરી સ્ટોરેજનો ઉપયોગ કરતી વખતે, ફાઇલની માહિતીમાં નામનું ક્ષેત્ર હશે
`બફર` જેમાં આખી ફાઇલ હોય છે.

**ચેતવણી**: ખૂબ મોટી ફાઇલો, અથવા પ્રમાણમાં નાની ફાઇલો અપલોડ કરવી
નંબરો ખૂબ જ ઝડપથી, જ્યારે તમારી એપ્લિકેશનની મેમરી સમાપ્ત થઈ શકે છે
મેમરી સ્ટોરેજનો ઉપયોગ થાય છે.

### `મર્યાદા`

નીચેના વૈકલ્પિક ગુણધર્મોની કદ મર્યાદાનો ઉલ્લેખ કરતી ઑબ્જેક્ટ. મલ્ટર આ ઑબ્જેક્ટને સીધા જ બસબોયમાં પસાર કરે છે, અને પ્રોપર્ટીઝની વિગતો [busboy's page](https://github.com/mscdex/busboy#busboy-methods) પર મળી શકે છે.

નીચેના પૂર્ણાંક મૂલ્યો ઉપલબ્ધ છે:

કી | વર્ણન | ડિફૉલ્ટ
--- | --- | ---
`fieldNameSize` | મહત્તમ ક્ષેત્ર નામનું કદ | 100 બાઇટ્સ
`ફિલ્ડસાઇઝ` | મહત્તમ ફીલ્ડ મૂલ્યનું કદ (બાઇટ્સમાં) | 1MB
`ક્ષેત્રો` | નોન-ફાઈલ ફીલ્ડ્સની મહત્તમ સંખ્યા | અનંત
`ફાઇલસાઇઝ` | મલ્ટિપાર્ટ ફોર્મ્સ માટે, મહત્તમ ફાઇલ કદ (બાઇટ્સમાં) | અનંત
`ફાઇલો` | મલ્ટિપાર્ટ ફોર્મ્સ માટે, ફાઇલ ફીલ્ડ્સની મહત્તમ સંખ્યા | અનંત
`ભાગો` | મલ્ટિપાર્ટ ફોર્મ્સ માટે, ભાગોની મહત્તમ સંખ્યા (ક્ષેત્રો + ફાઇલો) | અનંત
`હેડરપેયર્સ` | મલ્ટિપાર્ટ ફોર્મ્સ માટે, હેડર કીની મહત્તમ સંખ્યા =>પર્સ કરવા માટે મૂલ્યની જોડી | 2000

મર્યાદાનો ઉલ્લેખ કરવાથી તમારી સાઇટને ડિનાયલ ઑફ સર્વિસ (DoS) હુમલાઓ સામે સુરક્ષિત કરવામાં મદદ મળી શકે છે.

### `ફાઇલફિલ્ટર`

કઈ ફાઈલો અપલોડ કરવી જોઈએ અને કઈ ફાઈલોને નિયંત્રિત કરવા માટે આને ફંક્શન પર સેટ કરો
છોડવું જોઈએ. કાર્ય આના જેવું દેખાવું જોઈએ:

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

## હેન્ડલિંગમાં ભૂલ

ભૂલનો સામનો કરતી વખતે, મલ્ટર ભૂલને એક્સપ્રેસને સોંપશે. તમે કરી શકો છો
[સ્ટાન્ડર્ડ એક્સપ્રેસ વે](http://expressjs.com/guide/error-handling.html) નો ઉપયોગ કરીને એક સરસ ભૂલ પૃષ્ઠ પ્રદર્શિત કરો.

જો તમે ખાસ કરીને મલ્ટરમાંથી ભૂલો પકડવા માંગતા હો, તો તમે કૉલ કરી શકો છો
તમારા દ્વારા મિડલવેર કાર્ય. ઉપરાંત, જો તમે માત્ર [મલ્ટર ભૂલો](https://github.com/expressjs/multer/blob/main/lib/multer-error.js) પકડવા માંગતા હોવ, તો તમે `MulterError` વર્ગનો ઉપયોગ કરી શકો છો જે `multer` ઑબ્જેક્ટ સાથે જોડાયેલ છે (દા.ત. `Errr instance of multer' orMulter).

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

## કસ્ટમ સ્ટોરેજ એન્જિન

તમારું પોતાનું સ્ટોરેજ એન્જિન કેવી રીતે બનાવવું તેની માહિતી માટે, [Multer Storage Engine](https://github.com/expressjs/multer/blob/main/StorageEngine.md) જુઓ.

## લાઇસન્સ

[મારું](લાઈસન્સ)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
