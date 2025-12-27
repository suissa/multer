# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

Multer என்பது `மல்டிபார்ட்/ஃபார்ம்-டேட்டா` கையாளுவதற்கான ஒரு node.js மிடில்வேர் ஆகும், இது முதன்மையாக கோப்புகளைப் பதிவேற்றப் பயன்படுகிறது. எழுதப்பட்டிருக்கிறது
அதிகபட்ச செயல்திறனுக்காக [busboy] (https://github.com/mscdex/busboy) மேல்.

**குறிப்பு**: மல்டிபார்ட் (`மல்டிபார்ட்/ஃபார்ம்-டேட்டா`) இல்லாத எந்தப் படிவத்தையும் மல்டர் செயல்படுத்தாது.

## மொழிபெயர்ப்புகள்

இந்த README பிற மொழிகளிலும் கிடைக்கிறது:

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

## நிறுவல்

```sh
$ npm install multer
```

## பயன்பாடு

மல்டர் ஒரு `உடல்` பொருளையும் `கோப்பு` பொருளில் `கோப்பு` அல்லது `கோப்புகள்` பொருளையும் சேர்க்கிறது. `பாடி` ஆப்ஜெக்ட்டில் படிவத்தின் உரைப் புலங்களின் மதிப்புகள் உள்ளன, `கோப்பு` அல்லது `கோப்புகள்` ஆப்ஜெக்ட்டில் படிவத்தின் வழியாகப் பதிவேற்றப்பட்ட கோப்புகள் உள்ளன.

அடிப்படை பயன்பாட்டு உதாரணம்:

உங்கள் படிவத்தில் உள்ள `enctype="multipart/form-data"` ஐ மறந்துவிடாதீர்கள்.

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

நீங்கள் உரை-மட்டுமே மல்டிபார்ட் படிவத்தைக் கையாள வேண்டும் என்றால், நீங்கள் `.none()` முறையைப் பயன்படுத்த வேண்டும்:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

HTML வடிவத்தில் மல்ட்டர் எவ்வாறு பயன்படுத்தப்படுகிறது என்பதற்கான எடுத்துக்காட்டு இங்கே. `enctype="multipart/form-data"` மற்றும் `name="uploaded_file"` புலங்களைச் சிறப்பாகக் கவனிக்கவும்:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

உங்கள் ஜாவாஸ்கிரிப்ட் கோப்பில் கோப்பு மற்றும் உடல் இரண்டையும் அணுக இந்த வரிகளைச் சேர்ப்பீர்கள். உங்கள் பதிவேற்றச் செயல்பாட்டில் உள்ள படிவத்திலிருந்து `பெயர்` புல மதிப்பைப் பயன்படுத்துவது முக்கியம். கோரிக்கையில் எந்தப் புலத்தில் உள்ள கோப்புகளைத் தேட வேண்டும் என்பதை இது மல்ட்டருக்குச் சொல்கிறது. இந்தப் புலங்கள் HTML படிவத்திலும் உங்கள் சேவையகத்திலும் ஒரே மாதிரியாக இல்லாவிட்டால், உங்கள் பதிவேற்றம் தோல்வியடையும்:

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

### கோப்பு தகவல்

ஒவ்வொரு கோப்பிலும் பின்வரும் தகவல்கள் உள்ளன:

திறவுகோல் | விளக்கம் | குறிப்பு
--- | --- | ---
`புலம் பெயர்` | படிவத்தில் குறிப்பிடப்பட்ட புலத்தின் பெயர் |
`அசல் பெயர்` | பயனரின் கணினியில் உள்ள கோப்பின் பெயர் |
`குறியீடு` | கோப்பின் குறியாக்க வகை |
`மைம் வகை` | கோப்பின் மைம் வகை |
`அளவு` | பைட்டுகளில் கோப்பின் அளவு |
`இலக்கு` | கோப்பு சேமிக்கப்பட்ட கோப்புறை | `DiskStorage`
`கோப்பு பெயர்` | கோப்பின் பெயர் `இலக்கு` | `DiskStorage`
`பாதை` | பதிவேற்றிய கோப்பிற்கான முழு பாதை | `DiskStorage`
`தடை` | முழு கோப்பின் ஒரு `Buffer` | `மெமரி ஸ்டோரேஜ்`

### `multer(opts)`

Multer ஒரு விருப்பப் பொருளை ஏற்றுக்கொள்கிறது, அதில் மிக அடிப்படையானது `டெஸ்ட்` ஆகும்
சொத்து, கோப்புகளை எங்கு பதிவேற்ற வேண்டும் என்று Multer க்கு சொல்கிறது. நீங்கள் தவிர்த்துவிட்டால்
விருப்பங்கள் பொருள், கோப்புகள் நினைவகத்தில் வைக்கப்படும் மற்றும் வட்டில் எழுதப்படாது.

முன்னிருப்பாக, பெயரிடும் முரண்பாடுகளைத் தவிர்ப்பதற்காக Multer கோப்புகளை மறுபெயரிடும். தி
மறுபெயரிடுதல் செயல்பாடு உங்கள் தேவைகளுக்கு ஏற்ப தனிப்பயனாக்கலாம்.

பின்வருபவை Multer க்கு அனுப்பக்கூடிய விருப்பங்கள்.

திறவுகோல் | விளக்கம்
--- | ---
`டெஸ்ட்` அல்லது `ஸ்டோரேஜ்` | கோப்புகளை எங்கே சேமிப்பது
`fileFilter` | எந்த கோப்புகள் ஏற்றுக்கொள்ளப்படுகின்றன என்பதைக் கட்டுப்படுத்தும் செயல்பாடு
`வரம்புகள்` | பதிவேற்றிய தரவின் வரம்புகள்
`பாதுகாக்கும் பாதை` | அடிப்படை பெயருக்கு பதிலாக கோப்புகளின் முழு பாதையையும் வைத்திருங்கள்

சராசரி இணையப் பயன்பாட்டில், `dest` மட்டுமே தேவைப்படலாம் மற்றும் காட்டப்பட்டுள்ளபடி உள்ளமைக்கப்படும்
பின்வரும் உதாரணம்.

```javascript
const upload = multer({ dest: 'uploads/' })
```

உங்கள் பதிவேற்றங்களின் மீது கூடுதல் கட்டுப்பாட்டை நீங்கள் விரும்பினால், நீங்கள் `சேமிப்பகத்தைப்' பயன்படுத்த வேண்டும்
`dest` என்பதற்குப் பதிலாக விருப்பம். சேமிப்பு இயந்திரங்கள் `டிஸ்க் ஸ்டோரேஜ்` கொண்ட பல கப்பல்கள்
மற்றும் `மெமரி ஸ்டோரேஜ்`; மூன்றாம் தரப்பினரிடமிருந்து அதிக இன்ஜின்கள் கிடைக்கின்றன.

#### `.ஒற்றை(புலப்பெயர்)`

`fieldname` என்ற பெயரில் ஒரு கோப்பை ஏற்கவும். ஒற்றை கோப்பு சேமிக்கப்படும்
`req.file` இல்.

#### `.array(fieldname[, maxCount])`

கோப்புகளின் வரிசையை ஏற்கவும், அனைத்தும் `ஃபீல்ட்நேம்` என்ற பெயருடன். விருப்பமாக பிழை இருந்தால்
`maxCount` க்கும் அதிகமான கோப்புகள் பதிவேற்றப்பட்டன. கோப்புகளின் வரிசை சேமிக்கப்படும்
`req.files`.

#### `.fields(fields)`

`புலங்கள்` மூலம் குறிப்பிடப்பட்ட கோப்புகளின் கலவையை ஏற்கவும். கோப்புகளின் வரிசைகளைக் கொண்ட ஒரு பொருள்
`req.files` இல் சேமிக்கப்படும்.

`புலங்கள்` என்பது `பெயர்` மற்றும் விருப்பமாக `maxCount` உள்ள பொருள்களின் வரிசையாக இருக்க வேண்டும்.
எடுத்துக்காட்டு:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.இல்லை()`

உரை புலங்களை மட்டும் ஏற்கவும். ஏதேனும் கோப்பு பதிவேற்றம் செய்யப்பட்டால், குறியீட்டில் பிழை
"எல்லை\_எதிர்பாராத\_FILE" வழங்கப்படும்.

#### `. any()`

கம்பி வழியாக வரும் அனைத்து கோப்புகளையும் ஏற்றுக்கொள்கிறது. ஒரு வரிசையில் கோப்புகள் சேமிக்கப்படும்
`req.files`.

**எச்சரிக்கை:** ஒரு பயனர் பதிவேற்றும் கோப்புகளை நீங்கள் எப்போதும் கையாளுகிறீர்கள் என்பதை உறுதிப்படுத்திக் கொள்ளுங்கள்.
தீங்கிழைக்கும் பயனர் பதிவேற்றலாம் என்பதால், மல்ட்டரை உலகளாவிய மிடில்வேராக ஒருபோதும் சேர்க்க வேண்டாம்
நீங்கள் எதிர்பார்க்காத ஒரு வழிக்கான கோப்புகள். வழிகளில் மட்டுமே இந்தச் செயல்பாட்டைப் பயன்படுத்தவும்
நீங்கள் பதிவேற்றிய கோப்புகளை எங்கே கையாளுகிறீர்கள்.

### `சேமிப்பு`

#### `DiskStorage`

வட்டு சேமிப்பக இயந்திரமானது கோப்புகளை வட்டில் சேமிப்பதில் உங்களுக்கு முழு கட்டுப்பாட்டையும் வழங்குகிறது.

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

இரண்டு விருப்பங்கள் உள்ளன, `இலக்கு` மற்றும் `கோப்பு பெயர்`. அவர்கள் இருவரும்
கோப்பு எங்கு சேமிக்கப்பட வேண்டும் என்பதை தீர்மானிக்கும் செயல்பாடுகள்.

பதிவேற்றிய கோப்புகள் எந்த கோப்புறைக்குள் இருக்க வேண்டும் என்பதை தீர்மானிக்க `destination` பயன்படுத்தப்படுகிறது
சேமிக்கப்படும். இதை `ஸ்ட்ரிங்` ஆகவும் கொடுக்கலாம் (எ.கா. `'/tmp/uploads'`). இல்லை என்றால்
`destination` கொடுக்கப்பட்டுள்ளது, தற்காலிகத்திற்கான இயக்க முறைமையின் இயல்புநிலை அடைவு
கோப்புகள் பயன்படுத்தப்படுகின்றன.

**குறிப்பு:** வழங்கும் போது கோப்பகத்தை உருவாக்குவதற்கு நீங்கள் பொறுப்பு
ஒரு செயல்பாடாக `இலக்கு`. ஒரு சரத்தை கடக்கும்போது, மல்ட்டர் அதை உறுதி செய்யும்
அடைவு உங்களுக்காக உருவாக்கப்பட்டது.

கோப்புறைக்குள் கோப்பு என்ன பெயரிடப்பட வேண்டும் என்பதைத் தீர்மானிக்க `கோப்பின் பெயர்` பயன்படுத்தப்படுகிறது.
`கோப்பின் பெயர்` வழங்கப்படவில்லை எனில், ஒவ்வொரு கோப்பிற்கும் ஒரு சீரற்ற பெயர் கொடுக்கப்படும்
எந்த கோப்பு நீட்டிப்பும் அடங்கும்.

**குறிப்பு:** உங்களுக்கான கோப்பு நீட்டிப்பை Multer சேர்க்காது
கோப்பு நீட்டிப்புடன் முழுமையான கோப்புப் பெயரைத் திருப்பி அனுப்ப வேண்டும்.

ஒவ்வொரு செயல்பாடும் கோரிக்கை (`req`) மற்றும் சில தகவல்கள் இரண்டையும் நிறைவேற்றும்
முடிவெடுக்க உதவும் கோப்பு (`கோப்பு`).

கவனத்தில் கொள்ளவும், `req.body` இன்னும் முழுமையாக நிரப்பப்படவில்லை. இது சார்ந்துள்ளது
கிளையன்ட் புலங்கள் மற்றும் கோப்புகளை சேவையகத்திற்கு அனுப்புமாறு கட்டளையிடவும்.

அழைப்பில் பயன்படுத்தப்படும் அழைப்பு மரபைப் புரிந்துகொள்வதற்கு (பாஸ் செய்ய வேண்டும்
முதல் பாராவாக null), பார்க்கவும்
[Node.js பிழை கையாளுதல்](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### `மெமரி ஸ்டோரேஜ்`

நினைவக சேமிப்பக இயந்திரமானது நினைவகத்தில் உள்ள கோப்புகளை `Buffer` ஆப்ஜெக்ட்களாக சேமிக்கிறது. அது
எந்த விருப்பமும் இல்லை.

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

நினைவக சேமிப்பிடத்தைப் பயன்படுத்தும் போது, கோப்புத் தகவலில் ஒரு புலம் இருக்கும்
முழு கோப்பையும் கொண்டிருக்கும் `buffer`.

**எச்சரிக்கை**: மிகப் பெரிய கோப்புகள் அல்லது ஒப்பீட்டளவில் சிறிய கோப்புகளை பெரிய அளவில் பதிவேற்றுதல்
எண்கள் மிக விரைவாக, உங்கள் பயன்பாடு நினைவகம் தீர்ந்துவிடும்
நினைவக சேமிப்பு பயன்படுத்தப்படுகிறது.

### `வரம்புகள்`

பின்வரும் விருப்பப் பண்புகளின் அளவு வரம்புகளைக் குறிப்பிடும் ஒரு பொருள். Multer இந்த பொருளை நேரடியாக busboy க்குள் அனுப்புகிறது, மேலும் பண்புகளின் விவரங்களை [busboy's page](https://github.com/mscdex/busboy#busboy-methods) இல் காணலாம்.

பின்வரும் முழு எண் மதிப்புகள் கிடைக்கின்றன:

திறவுகோல் | விளக்கம் | இயல்புநிலை
--- | --- | ---
`fieldNameSize` | அதிகபட்ச புலம் பெயர் அளவு | 100 பைட்டுகள்
`புல அளவு` | அதிகபட்ச புல மதிப்பு அளவு (பைட்டுகளில்) | 1எம்பி
`வயல்கள்` | கோப்பு அல்லாத புலங்களின் அதிகபட்ச எண்ணிக்கை | முடிவிலி
`கோப்பின் அளவு` | மல்டிபார்ட் படிவங்களுக்கு, அதிகபட்ச கோப்பு அளவு (பைட்டுகளில்) | முடிவிலி
`கோப்புகள்` | மல்டிபார்ட் படிவங்களுக்கு, கோப்பு புலங்களின் அதிகபட்ச எண்ணிக்கை | முடிவிலி
`பாகங்கள்` | மல்டிபார்ட் படிவங்களுக்கு, அதிகபட்ச பாகங்களின் எண்ணிக்கை (புலங்கள் + கோப்புகள்) | முடிவிலி
`தலைச்சட்டிகள்` | மல்டிபார்ட் படிவங்களுக்கு, தலைப்பு விசையின் அதிகபட்ச எண்=>மதிப்பு ஜோடிகளை அலச வேண்டும் | 2000

வரம்புகளைக் குறிப்பிடுவது, சேவை மறுப்பு (DoS) தாக்குதல்களுக்கு எதிராக உங்கள் தளத்தைப் பாதுகாக்க உதவும்.

### `கோப்பு வடிகட்டி`

எந்தக் கோப்புகள் பதிவேற்றப்பட வேண்டும், எந்தெந்தக் கோப்புகளைக் கட்டுப்படுத்த வேண்டும் என்பதைச் செயல்பாட்டிற்கு அமைக்கவும்
தவிர்க்கப்பட வேண்டும். செயல்பாடு இப்படி இருக்க வேண்டும்:

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

## கையாளுவதில் பிழை

ஒரு பிழையை எதிர்கொள்ளும் போது, ​​Multer பிழையை எக்ஸ்பிரஸுக்கு வழங்கும். உங்களால் முடியும்
[நிலையான எக்ஸ்பிரஸ் வழி](http://expressjs.com/guide/error-handling.html) ஐப் பயன்படுத்தி ஒரு நல்ல பிழைப் பக்கத்தைக் காட்டவும்.

நீங்கள் Multer இலிருந்து பிழைகளைப் பிடிக்க விரும்பினால், நீங்கள் அழைக்கலாம்
மிடில்வேர் செயல்பாடு நீங்களே. மேலும், நீங்கள் [மல்ட்டர் பிழைகளை] (https://github.com/expressjs/multer/blob/main/lib/multer-error.js) மட்டும் பிடிக்க விரும்பினால், `multer` பொருளுடன் இணைக்கப்பட்டுள்ள `MulterError` வகுப்பைப் பயன்படுத்தலாம் (எ.கா. `err instance of Multer`).

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

## தனிப்பயன் சேமிப்பு இயந்திரம்

உங்கள் சொந்த சேமிப்பக இயந்திரத்தை எவ்வாறு உருவாக்குவது என்பது பற்றிய தகவலுக்கு, [மல்டர் ஸ்டோரேஜ் எஞ்சின்](https://github.com/expressjs/multer/blob/main/StorageEngine.md) ஐப் பார்க்கவும்.

## உரிமம்

[MY](உரிமம்)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
