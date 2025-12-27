# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

Multer በዋናነት ፋይሎችን ለመስቀል የሚያገለግለውን `multipart/form-data`ን ለመቆጣጠር node.js መካከለኛ ዌር ነው። ተብሎ ተጽፏል
ለከፍተኛ ውጤታማነት በ[busboy](https://github.com/mscdex/busboy) ላይ።

**ማስታወሻ**፡ ሙልተር ባለብዙ ክፍል (ባለብዙ ክፍል/ቅጽ-ውሂብ) ያልሆነውን ማንኛውንም ቅጽ አያስኬድም።

## ትርጉሞች

ይህ README በሌሎች ቋንቋዎችም ይገኛል።

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

## መጫን

```sh
$ npm install multer
```

##አጠቃቀም

ማልተር የ'አካል' ነገርን እና 'ፋይል' ወይም 'ፋይሎችን ወደ'ጥያቄ' ነገር ያክላል። የ‹አካል› ነገር የቅጹን የጽሑፍ መስኮች እሴቶችን ይይዛል፣ የ«ፋይል» ወይም የ«ፋይሎች» ነገር በቅጹ የተሰቀሉ ፋይሎችን ይይዛል።

መሰረታዊ የአጠቃቀም ምሳሌ፡-

በእርስዎ ቅጽ ውስጥ ያለውን `enctype="multipart/form-data"`ን አይርሱ።

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

የጽሑፍ-ብቻ ባለብዙ ክፍል ቅፅን መያዝ ካስፈለገዎት የ`.none()` ዘዴን መጠቀም አለብዎት፡-

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

ሙልተር በኤችቲኤምኤል ቅጽ ውስጥ እንዴት ጥቅም ላይ እንደሚውል የሚያሳይ ምሳሌ እዚህ አለ። የ`enctype="multipart/form-data"` እና `name="የተሰቀለ_ፋይል"` መስኮችን ልዩ ማስታወሻ ይያዙ፡

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

ከዚያ በጃቫስክሪፕት ፋይልዎ ውስጥ ፋይሉን እና አካሉን ለመድረስ እነዚህን መስመሮች ይጨምራሉ። በሰቀላ ተግባርዎ ውስጥ የ‹ስም› መስክ ዋጋን ከቅጹ ላይ መጠቀምዎ አስፈላጊ ነው። ይህ በጥያቄው ላይ የትኛውን መስክ ፋይሎቹን መፈለግ እንዳለበት ይነግርዎታል። እነዚህ መስኮች በኤችቲኤምኤል ቅጽ እና በአገልጋዩ ላይ ተመሳሳይ ካልሆኑ ሰቀላዎ አይሳካም።

```javascript
const multer  = require('multer')
const upload = multer({ dest: './public/data/uploads/' })
app.post('/stats', upload.single('uploaded_file'), function (req, res) {
  // req.file is the name of your file in the form above, here 'uploaded_file'
  // req.body will hold the text fields, if there were any
  console.log(req.file, req.body)
});
```



## ኤፒአይ

### የፋይል መረጃ

እያንዳንዱ ፋይል የሚከተሉትን መረጃዎች ይይዛል።

ቁልፍ | መግለጫ | ማስታወሻ
--- | --- | ---
'የመስክ ስም' | በመስክ ስም የተገለፀው በቅጹ |
'የመጀመሪያ ስም' | በተጠቃሚው ኮምፒውተር ላይ ያለው የፋይል ስም |
`ኢንኮዲንግ' | የፋይሉ ኢንኮዲንግ አይነት |
`mimetype` | የፋይሉ ሚሜ አይነት |
`መጠን` | የፋይሉ መጠን በባይት |
'መዳረሻ' | ፋይሉ የተቀመጠበት አቃፊ | 'DiskStorage'
'የፋይል ስም' | በ‹መዳረሻ› ውስጥ ያለው የፋይሉ ስም | 'DiskStorage'
'መንገድ' | ወደ የተሰቀለው ፋይል ሙሉ ዱካ | 'DiskStorage'
'ማቆያ' | የሙሉ ፋይል 'ማቋቋ' | 'የማህደረ ትውስታ ማከማቻ'

### `multer(opts)`

ማልተር የአማራጭ ነገርን ይቀበላል፣ በጣም መሠረታዊው 'dest' ነው።
ፋይሎቹን የት እንደሚሰቅሉ ለ Multer የሚነግረን ንብረት። ከተውትህ
አማራጮች ነገር፣ ፋይሎቹ በማህደረ ትውስታ ውስጥ ይቀመጣሉ እና በጭራሽ ወደ ዲስክ አይፃፉም።

በነባሪ፣ Multer ግጭቶችን ከመሰየም ለመዳን የፋይሎቹን ስም ይለውጣል። የ
ዳግም መሰየም ተግባር እንደ ፍላጎቶችዎ ሊበጅ ይችላል።

ወደ ሙልተር የሚተላለፉ አማራጮች የሚከተሉት ናቸው።

ቁልፍ | መግለጫ
--- | ---
`dest` ወይም `ማከማቻ` | ፋይሎችን የት እንደሚከማቹ
`ፋይል ማጣሪያ` | የትኛዎቹ ፋይሎች ተቀባይነት እንዳላቸው የመቆጣጠር ተግባር
'ገደቦች' | የተሰቀለው ውሂብ ገደቦች
'ተጠባባቂ መንገድ' | ከመሠረታዊ ስም ይልቅ የፋይሎችን ሙሉ መንገድ ያቆዩ

በአማካይ የድር መተግበሪያ 'dest' ብቻ ሊያስፈልግ ይችላል እና እንደሚታየው ሊዋቀር ይችላል።
የሚከተለው ምሳሌ.

```javascript
const upload = multer({ dest: 'uploads/' })
```

በሰቀላዎችዎ ላይ ተጨማሪ ቁጥጥር ከፈለጉ `ማከማቻውን» መጠቀም ይፈልጋሉ
ከ`dest` ይልቅ አማራጭ። መርከቦችን በማከማቻ ሞተሮች 'DiskStorage' ይቀያይሩ
እና 'Memory Storage'; ተጨማሪ ሞተሮች ከሶስተኛ ወገኖች ይገኛሉ.

#### `.ነጠላ(የመስክ ስም)`

'የመስክ ስም' የሚል ነጠላ ፋይል ተቀበል። ነጠላ ፋይሉ ይከማቻል
በ`req.file` ውስጥ።

#### `.array(የመስክ ስም[፣ maxCount])»

የፋይሎች ድርድር ተቀበል፣ ሁሉም በ‹የመስክ ስም› ስም። እንደ አማራጭ ስህተት ከሆነ
ከ`maxCount` በላይ ፋይሎች ተሰቅለዋል። የፋይሎች ድርድር በ ውስጥ ይከማቻል
`req.files`

#### `.መስኮች(መስኮች)`

በ«መስኮች» የተገለጸውን የፋይሎች ቅልቅል ተቀበል። የፋይል ድርድር ያለው ነገር
በ`req.files` ውስጥ ይከማቻል።

'መስኮች' 'ስም' ያላቸው እና እንደ አማራጭ 'maxCount' ያላቸው የነገሮች ድርድር መሆን አለባቸው።
ለምሳሌ፥

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.ምንም()`

የጽሑፍ መስኮችን ብቻ ተቀበል። ማንኛውም ፋይል ሰቀላ ከተሰራ፣ በኮድ ስህተት
"LIMIT\_UNEXPECTED\_FILE" ይወጣል።

#### `.ማንኛውም()`

በሽቦው ላይ የሚመጡትን ሁሉንም ፋይሎች ይቀበላል። የፋይሎች ድርድር ይከማቻል
`req.files`

** ማስጠንቀቂያ፡** ተጠቃሚው የሚሰቅላቸውን ፋይሎች ሁልጊዜ መያዝዎን ያረጋግጡ።
ተንኮል አዘል ተጠቃሚ ሊሰቀል ስለሚችል ሙልተርን እንደ አለምአቀፍ መካከለኛ ዌር በጭራሽ አይጨምሩ
ወደ ማትገምተው መንገድ ፋይሎች። ይህንን ተግባር በመንገዶች ላይ ብቻ ይጠቀሙ
የተጫኑትን ፋይሎች የሚይዙበት.

### `ማከማቻ»

#### 'ዲስክ ማከማቻ'

የዲስክ ማከማቻ ሞተር ፋይሎችን ወደ ዲስክ በማከማቸት ላይ ሙሉ ቁጥጥር ይሰጥዎታል።

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

ሁለት አማራጮች አሉ፡ `መዳረሻ` እና `የፋይል ስም`። ሁለቱም ናቸው።
ፋይሉ የት መቀመጥ እንዳለበት የሚወስኑ ተግባራት.

'መዳረሻ' የተሰቀሉት ፋይሎች በየትኛው አቃፊ ውስጥ መሆን እንዳለባቸው ለመወሰን ይጠቅማል
ተከማችቷል ። ይህ እንደ `string` (ለምሳሌ `'/tmp/ሰቀላዎች») ሊሰጥ ይችላል። አይደለም ከሆነ
የስርዓተ ክወናው ነባሪ ማውጫ ለጊዜውያዊ 'መድረሻ' ተሰጥቷል።
ፋይሎች ጥቅም ላይ ይውላሉ.

**ማስታወሻ:** ሲያቀርቡ ማውጫውን የመፍጠር ሃላፊነት አለብዎት
እንደ ተግባር 'መዳረሻ' ሕብረቁምፊን በሚያልፉበት ጊዜ ሙልተር ያንን ያረጋግጣል
ማውጫው ለእርስዎ ተፈጥሯል።

‹ፋይል ስም› ፋይሉ በአቃፊው ውስጥ ምን መሰየም እንዳለበት ለመወሰን ይጠቅማል።
ምንም `የፋይል ስም` ካልተሰጠ፣ እያንዳንዱ ፋይል የማይሰጥ የዘፈቀደ ስም ይሰጠዋል
ማንኛውንም የፋይል ቅጥያ ያካትቱ።

**ማስታወሻ:** ሙልተር ለእርስዎ ተግባር የትኛውንም የፋይል ቅጥያ አይጨምርም።
የተሟላ የፋይል ስም ከፋይል ቅጥያ ጋር መመለስ አለበት።

እያንዳንዱ ተግባር ሁለቱንም ጥያቄ (`req`) እና ስለ አንዳንድ መረጃ ያልፋል
ለውሳኔው የሚረዳው ፋይል (`ፋይል`)።

'req.body' ገና ሙሉ በሙሉ ተሞልቶ ላይሆን እንደሚችል ልብ ይበሉ። በ ላይ ይወሰናል
ደንበኛው መስኮችን እና ፋይሎችን ወደ አገልጋዩ እንዲያስተላልፍ ማዘዝ.

በመልሶ ጥሪው ውስጥ ጥቅም ላይ የዋለውን የጥሪ ስምምነት ለመረዳት (ማለፍ ያስፈልጋል
null እንደ መጀመሪያው ፓራም) ይመልከቱ
[Node.js የስህተት አያያዝ](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### 'የማስታወሻ ማከማቻ'

የማህደረ ትውስታ ማከማቻ ሞተር ፋይሎቹን በማህደረ ትውስታ ውስጥ እንደ “ቋት” ነገሮች ያከማቻል። እሱ
ምንም አማራጮች የሉትም።

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

የማህደረ ትውስታ ማከማቻ ሲጠቀሙ የፋይሉ መረጃ የሚባል መስክ ይይዛል
ሙሉውን ፋይል የያዘ 'ማቋቋ'።

**ማስጠንቀቂያ**: በጣም ትልቅ ፋይሎችን በመስቀል ላይ, ወይም ትልቅ ውስጥ በአንጻራዊ ትናንሽ ፋይሎች
ቁጥሮች በጣም በፍጥነት፣ መቼ መተግበሪያዎ ማህደረ ትውስታ እንዲያልቅ ሊያደርግ ይችላል።
የማህደረ ትውስታ ማከማቻ ጥቅም ላይ ይውላል.

### 'ገደቦች'

የሚከተሉት የአማራጭ ንብረቶች የመጠን ገደቦችን የሚገልጽ ዕቃ። ማልተር ይህን ነገር በቀጥታ ወደ ቡስቦይ ያስተላልፋል፣ እና የንብረቶቹ ዝርዝሮች በ [busboy's page](https://github.com/mscdex/busboy#busboy-methods) ላይ ይገኛሉ።

የሚከተሉት የኢንቲጀር እሴቶች ይገኛሉ፡-

ቁልፍ | መግለጫ | ነባሪ
--- | --- | ---
`የመስክ ስም መጠን` | ከፍተኛው የመስክ ስም መጠን | 100 ባይት
'የመስክ መጠን' | ከፍተኛው የመስክ ዋጋ መጠን (በባይት) | 1 ሜባ
'መስኮች' | ከፍተኛው የፋይል ያልሆኑ መስኮች ብዛት | ማለቂያ የሌለው
'የፋይል መጠን' | ለባለብዙ ክፍል ቅጾች፣ ከፍተኛው የፋይል መጠን (በባይት) | ማለቂያ የሌለው
`ፋይሎች` | ለባለብዙ ክፍል ቅጾች፣ የፋይል መስኮች ከፍተኛው ቁጥር | ማለቂያ የሌለው
'ክፍሎች' | ለባለብዙ ክፍል ቅጾች፣ ከፍተኛው የክፍሎች ብዛት (መስኮች + ፋይሎች) | ማለቂያ የሌለው
`headerPairs` | ለባለብዙ ክፍል ቅጾች፣ የሚተነተንበት ከፍተኛው የራስጌ ቁልፍ=>የእሴት ጥንዶች | 2000

ገደቦቹን መግለጽ ጣቢያዎን ከአገልግሎት መከልከል (DoS) ጥቃቶች ለመጠበቅ ይረዳል።

### `ፋይል ማጣሪያ`

የትኞቹ ፋይሎች መሰቀል እንዳለባቸው እና የትኛውን ለመቆጣጠር ይህንን ወደ ተግባር ያቀናብሩት።
መዝለል አለበት. ተግባሩ ይህን ይመስላል።

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

## አያያዝ ላይ ስህተት

ስህተት ሲያጋጥመው፣ Multer ስህተቱን ወደ ኤክስፕረስ ይልካል። ትችላለህ
[መደበኛው ኤክስፕረስ መንገድ](http://expressjs.com/guide/error-handling.html) በመጠቀም ጥሩ የስህተት ገጽ አሳይ።

በተለይ ከ Multer ስህተቶችን ለመያዝ ከፈለጉ፣ ወደዚህ መደወል ይችላሉ።
የመሃል ዌር ተግባር በራስዎ። እንዲሁም [The Multer errors](https://github.com/expressjs/multer/blob/main/lib/multer-error.js) ብቻ ለመያዝ ከፈለጉ ከራሱ ከ`multerError` ጋር የተያያዘውን የ`MulterError` ክፍልን መጠቀም ትችላለህ (ለምሳሌ፦ “err instanceof multer.MulterError`)።

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

## ብጁ የማጠራቀሚያ ሞተር

የራስዎን የማከማቻ ሞተር እንዴት እንደሚገነቡ ላይ መረጃ ለማግኘት [Multer Storage Engine](https://github.com/expressjs/multer/blob/main/StorageEngine.md) ይመልከቱ።

## ፍቃድ

[የእኔ] (ፍቃድ)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
