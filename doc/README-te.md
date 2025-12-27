# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

Multer అనేది `మల్టీపార్ట్/ఫారమ్-డేటా`ని నిర్వహించడానికి ఒక node.js మిడిల్‌వేర్, ఇది ప్రధానంగా ఫైల్‌లను అప్‌లోడ్ చేయడానికి ఉపయోగించబడుతుంది. అని రాసి ఉంది
గరిష్ట సామర్థ్యం కోసం [busboy](https://github.com/mscdex/busboy) పైన.

**గమనిక**: మల్టీపార్ట్ (`మల్టీపార్ట్/ఫారమ్-డేటా`) లేని ఫారమ్‌ను మల్టర్ ప్రాసెస్ చేయదు.

## అనువాదాలు

ఈ README ఇతర భాషలలో కూడా అందుబాటులో ఉంది:

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

## సంస్థాపన

```sh
$ npm install multer
```

## వినియోగం

Multer `అభ్యర్థన` ఆబ్జెక్ట్‌కి `బాడీ` ఆబ్జెక్ట్ మరియు `ఫైల్` లేదా `ఫైల్స్` ఆబ్జెక్ట్‌ని జోడిస్తుంది. `బాడీ` ఆబ్జెక్ట్ ఫారమ్ యొక్క టెక్స్ట్ ఫీల్డ్‌ల విలువలను కలిగి ఉంటుంది, `ఫైల్` లేదా `ఫైల్స్` ఆబ్జెక్ట్ ఫారమ్ ద్వారా అప్‌లోడ్ చేయబడిన ఫైల్‌లను కలిగి ఉంటుంది.

ప్రాథమిక వినియోగ ఉదాహరణ:

మీ ఫారమ్‌లో `enctype="multipart/form-data"`ని మర్చిపోవద్దు.

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

మీరు టెక్స్ట్-ఓన్లీ మల్టీపార్ట్ ఫారమ్‌ను హ్యాండిల్ చేయాల్సిన అవసరం ఉన్నట్లయితే, మీరు `.none()` పద్ధతిని ఉపయోగించాలి:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

HTML రూపంలో మల్టర్ ఎలా ఉపయోగించబడుతుందో ఇక్కడ ఒక ఉదాహరణ ఉంది. `enctype="multipart/form-data"` మరియు `name="uploaded_file"` ఫీల్డ్‌లను ప్రత్యేకంగా గమనించండి:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

ఆపై మీ జావాస్క్రిప్ట్ ఫైల్‌లో మీరు ఫైల్ మరియు బాడీ రెండింటినీ యాక్సెస్ చేయడానికి ఈ పంక్తులను జోడిస్తారు. మీరు మీ అప్‌లోడ్ ఫంక్షన్‌లోని ఫారమ్ నుండి `పేరు` ఫీల్డ్ విలువను ఉపయోగించడం ముఖ్యం. అభ్యర్థనలో ఏ ఫీల్డ్‌లో ఫైల్‌ల కోసం వెతకాలి అని ఇది మల్టర్‌కు తెలియజేస్తుంది. ఈ ఫీల్డ్‌లు HTML ఫారమ్‌లో మరియు మీ సర్వర్‌లో ఒకేలా లేకుంటే, మీ అప్‌లోడ్ విఫలమవుతుంది:

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

### ఫైల్ సమాచారం

ప్రతి ఫైల్ కింది సమాచారాన్ని కలిగి ఉంటుంది:

కీ | వివరణ | గమనిక
--- | --- | ---
`ఫీల్డ్‌నేమ్` | ఫారమ్‌లో పేర్కొన్న ఫీల్డ్ పేరు |
`అసలు పేరు` | వినియోగదారు కంప్యూటర్‌లోని ఫైల్ పేరు |
`ఎన్‌కోడింగ్` | ఫైల్ యొక్క ఎన్‌కోడింగ్ రకం |
`మైమ్ టైప్` | ఫైల్ యొక్క మైమ్ రకం |
`పరిమాణం` | ఫైలు పరిమాణం బైట్‌లలో |
`గమ్యం` | ఫైల్ సేవ్ చేయబడిన ఫోల్డర్ | `డిస్క్‌స్టోరేజ్`
`ఫైల్ పేరు` | `గమ్యం`లో ఉన్న ఫైల్ పేరు | `డిస్క్‌స్టోరేజ్`
`మార్గం` | అప్‌లోడ్ చేసిన ఫైల్‌కి పూర్తి మార్గం | `డిస్క్‌స్టోరేజ్`
`బఫర్` | మొత్తం ఫైల్ యొక్క `బఫర్` | `మెమరీ స్టోరేజ్`

### `మల్టర్(ఎంపికలు)`

Multer ఆప్షన్స్ ఆబ్జెక్ట్‌ను అంగీకరిస్తుంది, అందులో అత్యంత ప్రాథమికమైనది `డెస్ట్`
ఆస్తి, ఇది ఫైల్‌లను ఎక్కడ అప్‌లోడ్ చేయాలో Multerకి తెలియజేస్తుంది. ఒకవేళ మీరు తప్పిస్తే
ఎంపికలు ఆబ్జెక్ట్, ఫైల్‌లు మెమరీలో ఉంచబడతాయి మరియు డిస్క్‌కు ఎప్పుడూ వ్రాయబడవు.

డిఫాల్ట్‌గా, పేరు పెట్టే వైరుధ్యాలను నివారించడానికి Multer ఫైల్‌ల పేరును మారుస్తుంది. ది
పేరు మార్చడం ఫంక్షన్ మీ అవసరాలకు అనుగుణంగా అనుకూలీకరించవచ్చు.

మల్టర్‌కు పంపబడే ఎంపికలు క్రిందివి.

కీ | వివరణ
--- | ---
`డెస్ట్` లేదా `స్టోరేజ్` | ఫైల్‌లను ఎక్కడ నిల్వ చేయాలి
`ఫైల్ ఫిల్టర్` | ఏ ఫైల్‌లు ఆమోదించబడతాయో నియంత్రించడానికి ఫంక్షన్
`పరిమితులు` | అప్‌లోడ్ చేసిన డేటా పరిమితులు
`ప్రిజర్వ్‌పాత్` | కేవలం బేస్ పేరుకు బదులుగా ఫైల్‌ల పూర్తి మార్గాన్ని ఉంచండి

సగటు వెబ్ యాప్‌లో, కేవలం `dest` మాత్రమే అవసరం కావచ్చు మరియు చూపిన విధంగా కాన్ఫిగర్ చేయబడుతుంది
క్రింది ఉదాహరణ.

```javascript
const upload = multer({ dest: 'uploads/' })
```

మీ అప్‌లోడ్‌లపై మీకు మరింత నియంత్రణ కావాలంటే, మీరు `నిల్వ`ని ఉపయోగించాలి
`dest`కి బదులుగా ఎంపిక. నిల్వ ఇంజిన్‌లు `డిస్క్‌స్టోరేజ్`తో మల్టీ షిప్‌లు
మరియు `మెమరీ స్టోరేజ్`; థర్డ్ పార్టీల నుండి మరిన్ని ఇంజన్లు అందుబాటులో ఉన్నాయి.

#### `.సింగిల్(ఫీల్డ్ పేరు)`

`ఫీల్డ్‌నేమ్` పేరుతో ఒకే ఫైల్‌ని ఆమోదించండి. ఒకే ఫైల్ నిల్వ చేయబడుతుంది
`req.file`లో.

#### `.శ్రేణి(ఫీల్డ్‌నేమ్[, maxCount])`

ఫైల్‌ల శ్రేణిని ఆమోదించండి, అన్నీ `ఫీల్డ్‌నేమ్` పేరుతో ఉంటాయి. ఐచ్ఛికంగా లోపం ఉంటే
`maxCount` కంటే ఎక్కువ ఫైల్‌లు అప్‌లోడ్ చేయబడ్డాయి. ఫైల్‌ల శ్రేణి నిల్వ చేయబడుతుంది
`req.files`.

#### `.ఫీల్డ్‌లు(ఫీల్డ్‌లు)`

`ఫీల్డ్‌లు` ద్వారా పేర్కొన్న ఫైల్‌ల మిశ్రమాన్ని ఆమోదించండి. ఫైల్‌ల శ్రేణులతో ఒక వస్తువు
`req.files`లో నిల్వ చేయబడుతుంది.

`ఫీల్డ్‌లు` అనేది `పేరు` మరియు ఐచ్ఛికంగా `maxCount` ఉన్న వస్తువుల శ్రేణి అయి ఉండాలి.
ఉదాహరణ:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `. none()`

టెక్స్ట్ ఫీల్డ్‌లను మాత్రమే ఆమోదించండి. ఏదైనా ఫైల్ అప్‌లోడ్ చేయబడితే, కోడ్‌తో లోపం ఏర్పడుతుంది
"LIMIT\_UNEXPECTED\_FILE" జారీ చేయబడుతుంది.

#### `.ఏదైనా()`

వైర్ ద్వారా వచ్చే అన్ని ఫైల్‌లను అంగీకరిస్తుంది. ఫైల్‌ల శ్రేణి నిల్వ చేయబడుతుంది
`req.files`.

**హెచ్చరిక:** వినియోగదారు అప్‌లోడ్ చేసే ఫైల్‌లను మీరు ఎల్లప్పుడూ హ్యాండిల్ చేశారని నిర్ధారించుకోండి.
హానికరమైన వినియోగదారు అప్‌లోడ్ చేయగలిగినందున మల్టర్‌ను గ్లోబల్ మిడిల్‌వేర్‌గా ఎప్పుడూ జోడించవద్దు
మీరు ఊహించని మార్గానికి ఫైల్‌లు. మార్గాల్లో మాత్రమే ఈ ఫంక్షన్‌ని ఉపయోగించండి
మీరు అప్‌లోడ్ చేసిన ఫైల్‌లను ఎక్కడ నిర్వహిస్తున్నారు.

### `నిల్వ`

#### `డిస్క్‌స్టోరేజ్`

డిస్క్ స్టోరేజ్ ఇంజిన్ మీకు ఫైల్‌లను డిస్క్‌లో నిల్వ చేయడంపై పూర్తి నియంత్రణను ఇస్తుంది.

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

రెండు ఎంపికలు అందుబాటులో ఉన్నాయి, `గమ్యం` మరియు `ఫైల్ పేరు`. వారిద్దరూ
ఫైల్ ఎక్కడ నిల్వ చేయబడాలో నిర్ణయించే విధులు.

అప్‌లోడ్ చేసిన ఫైల్‌లు ఏ ఫోల్డర్‌లో ఉండాలో నిర్ణయించడానికి `గమ్యం` ఉపయోగించబడుతుంది
నిల్వ చేయబడుతుంది. దీనిని `స్ట్రింగ్`గా కూడా ఇవ్వవచ్చు (ఉదా. `'/tmp/uploads'`). కాకపోతే
`గమ్యం` ఇవ్వబడింది, తాత్కాలికం కోసం ఆపరేటింగ్ సిస్టమ్ డిఫాల్ట్ డైరెక్టరీ
ఫైల్స్ ఉపయోగించబడతాయి.

**గమనిక:** అందించేటప్పుడు డైరెక్టరీని సృష్టించడానికి మీరు బాధ్యత వహిస్తారు
విధిగా `గమ్యం`. స్ట్రింగ్‌ను పాస్ చేస్తున్నప్పుడు, మల్టర్ దానిని నిర్ధారిస్తుంది
డైరెక్టరీ మీ కోసం సృష్టించబడింది.

ఫోల్డర్‌లో ఫైల్‌కి ఏ పేరు పెట్టాలో నిర్ణయించడానికి `ఫైల్ పేరు` ఉపయోగించబడుతుంది.
`ఫైల్ పేరు` ఇవ్వకపోతే, ప్రతి ఫైల్‌కు రాండమ్ పేరు ఇవ్వబడుతుంది
ఏదైనా ఫైల్ పొడిగింపును చేర్చండి.

**గమనిక:** Multer మీ కోసం, మీ ఫంక్షన్ కోసం ఏ ఫైల్ పొడిగింపును జోడించదు
ఫైల్ పొడిగింపుతో పూర్తి చేసిన ఫైల్ పేరును తిరిగి ఇవ్వాలి.

ప్రతి ఫంక్షన్ అభ్యర్థన (`req`) మరియు దాని గురించి కొంత సమాచారం రెండింటినీ ఆమోదించింది
నిర్ణయానికి సహాయం చేయడానికి ఫైల్ (`ఫైల్`).

`req.body` ఇంకా పూర్తిగా నిండి ఉండకపోవచ్చని గమనించండి. ఇది ఆధారపడి ఉంటుంది
క్లయింట్ ఫీల్డ్‌లు మరియు ఫైల్‌లను సర్వర్‌కు ప్రసారం చేసేలా ఆదేశించండి.

కాల్‌బ్యాక్‌లో ఉపయోగించిన కాలింగ్ కన్వెన్షన్‌ను అర్థం చేసుకోవడం కోసం (పాస్ కావాలి
మొదటి పారామ్‌గా శూన్యం), చూడండి
[Node.js ఎర్రర్ హ్యాండ్లింగ్](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### `మెమరీ స్టోరేజ్`

మెమరీ స్టోరేజ్ ఇంజిన్ మెమొరీలోని ఫైల్‌లను `బఫర్` ఆబ్జెక్ట్‌లుగా నిల్వ చేస్తుంది. ఇది
ఏ ఎంపికలు లేవు.

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

మెమరీ నిల్వను ఉపయోగిస్తున్నప్పుడు, ఫైల్ సమాచారం అనే ఫీల్డ్‌ని కలిగి ఉంటుంది
మొత్తం ఫైల్‌ని కలిగి ఉన్న `బఫర్`.

**హెచ్చరిక**: చాలా పెద్ద ఫైల్‌లు లేదా సాపేక్షంగా చిన్న ఫైల్‌లను పెద్దవిగా అప్‌లోడ్ చేయడం
సంఖ్యలు చాలా త్వరగా, మీ అప్లికేషన్ మెమరీ అయిపోవచ్చు
మెమరీ నిల్వ ఉపయోగించబడుతుంది.

### `పరిమితులు`

కింది ఐచ్ఛిక లక్షణాల పరిమాణ పరిమితులను పేర్కొనే వస్తువు. Multer ఈ వస్తువును నేరుగా బస్‌బాయ్‌కి పంపుతుంది మరియు లక్షణాల వివరాలను [busboy's page](https://github.com/mscdex/busboy#busboy-methods)లో చూడవచ్చు.

కింది పూర్ణాంకాల విలువలు అందుబాటులో ఉన్నాయి:

కీ | వివరణ | డిఫాల్ట్
--- | --- | ---
`ఫీల్డ్‌నేమ్‌సైజ్` | గరిష్ఠ ఫీల్డ్ పేరు పరిమాణం | 100 బైట్లు
`ఫీల్డ్ సైజ్` | గరిష్ఠ ఫీల్డ్ విలువ పరిమాణం (బైట్‌లలో) | 1MB
`క్షేత్రాలు` | నాన్-ఫైల్ ఫీల్డ్‌ల గరిష్ట సంఖ్య | అనంతం
`ఫైల్‌సైజ్` | మల్టీపార్ట్ ఫారమ్‌ల కోసం, గరిష్ట ఫైల్ పరిమాణం (బైట్‌లలో) | అనంతం
`ఫైళ్లు` | మల్టీపార్ట్ ఫారమ్‌ల కోసం, ఫైల్ ఫీల్డ్‌ల గరిష్ట సంఖ్య | అనంతం
`భాగాలు` | మల్టీపార్ట్ ఫారమ్‌ల కోసం, భాగాల గరిష్ట సంఖ్య (ఫీల్డ్‌లు + ఫైల్‌లు) | అనంతం
`హెడర్ పెయిర్స్` | మల్టీపార్ట్ ఫారమ్‌ల కోసం, హెడర్ కీ యొక్క గరిష్ట సంఖ్య=>విలువ జతలు అన్వయించాలి | 2000

పరిమితులను పేర్కొనడం వలన సేవా నిరాకరణ (DoS) దాడుల నుండి మీ సైట్‌ను రక్షించడంలో సహాయపడుతుంది.

### `ఫైల్ ఫిల్టర్`

ఏ ఫైల్‌లను అప్‌లోడ్ చేయాలి మరియు ఏది అప్‌లోడ్ చేయాలి అనేదానిని నియంత్రించడానికి దీన్ని ఫంక్షన్‌కి సెట్ చేయండి
దాటవేయాలి. ఫంక్షన్ ఇలా ఉండాలి:

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

## నిర్వహణ లోపం

లోపాన్ని ఎదుర్కొన్నప్పుడు, Multer ఆ లోపాన్ని ఎక్స్‌ప్రెస్‌కి అప్పగిస్తుంది. మీరు చెయ్యగలరు
[ప్రామాణిక ఎక్స్‌ప్రెస్ మార్గం](http://expressjs.com/guide/error-handling.html) ఉపయోగించి చక్కని ఎర్రర్ పేజీని ప్రదర్శించండి.

మీరు Multer నుండి ప్రత్యేకంగా లోపాలను గుర్తించాలనుకుంటే, మీరు కాల్ చేయవచ్చు
మిడిల్‌వేర్ ఫంక్షన్ మీరే. అలాగే, మీరు [మల్టర్ ఎర్రర్‌లను] (https://github.com/expressjs/multer/blob/main/lib/multer-error.js) మాత్రమే క్యాచ్ చేయాలనుకుంటే, మీరు `multer` ఆబ్జెక్ట్‌కు జోడించబడిన `MulterError` క్లాస్‌ని ఉపయోగించవచ్చు (ఉదా. `ఎర్రర్ ఇన్‌స్టాన్స్ ఆఫ్ multer`).

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

## కస్టమ్ స్టోరేజ్ ఇంజిన్

మీ స్వంత స్టోరేజ్ ఇంజిన్‌ని ఎలా నిర్మించాలనే దాని గురించి సమాచారం కోసం, [మల్టర్ స్టోరేజ్ ఇంజిన్](https://github.com/expressjs/multer/blob/main/StorageEngine.md) చూడండి.

## లైసెన్స్

[నా](లైసెన్స్)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
