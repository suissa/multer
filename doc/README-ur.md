# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

ملٹر `ملٹی پارٹ/فارم ڈیٹا` کو سنبھالنے کے لئے ایک نوڈ. جے ایس مڈل ویئر ہے ، جو بنیادی طور پر فائلوں کو اپ لوڈ کرنے کے لئے استعمال ہوتا ہے۔ یہ لکھا ہوا ہے
زیادہ سے زیادہ کارکردگی کے لئے [بس بوائے] (https://github.com/mscdex/busboy) کے اوپر۔

** نوٹ **: مولٹر کسی بھی فارم پر کارروائی نہیں کرے گا جو ملٹی پارٹ نہیں ہے (`ملٹی پارٹ/فارم ڈیٹا)۔

## ترجمے

یہ README دوسری زبانوں میں بھی دستیاب ہے:

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

## تنصیب

```sh
$ npm install multer
```

## استعمال

مولٹر نے `درخواست` آبجیکٹ میں ایک `باڈی` آبجیکٹ اور ایک `فائل یا` فائلوں `آبجیکٹ کا اضافہ کیا ہے۔ `باڈی` آبجیکٹ میں فارم کے ٹیکسٹ فیلڈز کی قدر ہوتی ہے ، `فائل یا` فائلوں `آبجیکٹ میں فارم کے ذریعے اپ لوڈ کردہ فائلیں شامل ہوتی ہیں۔

بنیادی استعمال کی مثال:

اپنی شکل میں `enctype =" ملٹی پارٹ/فارم ڈیٹا "` کو مت بھولنا۔

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

اگر آپ کو صرف ایک ٹیکسٹ ملٹی پارٹ فارم کو سنبھالنے کی ضرورت ہے تو ، آپ کو `. کوئی ()` طریقہ استعمال کرنا چاہئے:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

یہاں ایک مثال ہے کہ کس طرح مولٹر کو HTML شکل میں استعمال کیا جاتا ہے۔ `encType =" ملٹی پارٹ/فارم ڈیٹا "` اور `نام =" اپ لوڈ کردہ_فائل "` فیلڈز کا خصوصی نوٹ لیں: فیلڈز:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

پھر اپنی جاوا اسکرپٹ فائل میں آپ فائل اور جسم دونوں تک رسائی کے ل these ان لائنوں کو شامل کریں گے۔ یہ ضروری ہے کہ آپ اپنے اپلوڈ فنکشن میں فارم سے `نام` فیلڈ ویلیو استعمال کریں۔ یہ مولٹر کو بتاتا ہے کہ درخواست پر کون سا فیلڈ فائلوں کو تلاش کرنا چاہئے۔ اگر یہ فیلڈ HTML فارم میں اور آپ کے سرور پر ایک جیسے نہیں ہیں تو ، آپ کا اپ لوڈ ناکام ہوجائے گا:

```javascript
const multer  = require('multer')
const upload = multer({ dest: './public/data/uploads/' })
app.post('/stats', upload.single('uploaded_file'), function (req, res) {
  // req.file is the name of your file in the form above, here 'uploaded_file'
  // req.body will hold the text fields, if there were any
  console.log(req.file, req.body)
});
```



## api

### فائل کی معلومات

ہر فائل میں درج ذیل معلومات شامل ہیں:

کلید | تفصیل | نوٹ
--- | --- | ---
`فیلڈ نام` | فارم میں مخصوص فیلڈ کا نام |
`اصلی نام` | صارف کے کمپیوٹر پر فائل کا نام |
`انکوڈنگ` | فائل کی انکوڈنگ کی قسم |
`mimeType` | فائل کی مائم قسم |
`سائز` | بائٹس میں فائل کا سائز |
`منزل` | وہ فولڈر جس میں فائل محفوظ کی گئی ہے | `ڈسک اسٹوریج`
`فائل نام` | `منزل کے اندر فائل کا نام` | `ڈسک اسٹوریج`
`راستہ` | اپ لوڈ کردہ فائل کا پورا راستہ | `ڈسک اسٹوریج`
`بفر` | پوری فائل کا a `بفر | `میموری اسٹوریج`

### `مولٹر (آپٹ)`

مولٹر ایک آپشن آبجیکٹ کو قبول کرتا ہے ، جس میں سب سے بنیادی `تقدیر ہے
پراپرٹی ، جو مولٹر کو بتاتی ہے کہ فائلیں کہاں اپ لوڈ کریں۔ اگر آپ کو چھوڑ دیں
اختیارات آبجیکٹ ، فائلوں کو میموری میں رکھا جائے گا اور کبھی ڈسک پر نہیں لکھا جائے گا۔

پہلے سے طے شدہ طور پر ، مولٹر فائلوں کا نام تبدیل کرے گا تاکہ تنازعات کا نام لینے سے بچ سکے۔
نام تبدیل کرنے کی تقریب کو آپ کی ضروریات کے مطابق اپنی مرضی کے مطابق بنایا جاسکتا ہے۔

مندرجہ ذیل اختیارات ہیں جو مولٹر کو منتقل ہوسکتے ہیں۔

کلید | تفصیل
--- | ---
`تقدیر یا` اسٹوریج` | فائلوں کو کہاں اسٹور کریں
`فائل فلٹر` | کون سی فائلوں کو قبول کیا جاتا ہے اس پر قابو پانے کے لئے فنکشن
`حدود` | اپ لوڈ کردہ ڈیٹا کی حدود
`پریزروپیتھ | صرف بیس نام کے بجائے فائلوں کا پورا راستہ رکھیں

اوسط ویب ایپ میں ، صرف `ڈسٹ` کی ضرورت ہوسکتی ہے ، اور جیسا کہ دکھایا گیا ہے اس کی تشکیل کی جاسکتی ہے
مندرجہ ذیل مثال

```javascript
const upload = multer({ dest: 'uploads/' })
```

اگر آپ اپنے اپ لوڈوں پر زیادہ کنٹرول چاہتے ہیں تو ، آپ `اسٹوریج` استعمال کرنا چاہیں گے
`تقدیر کے بجائے آپشن۔ اسٹوریج انجنوں کے ساتھ ملٹر جہاز `ڈسک اسٹوریج`
اور `میموری اسٹوریج` ؛ تیسرے فریق سے مزید انجن دستیاب ہیں۔

#### `.Single (فیلڈ نام)`

کسی ایک فائل کو `فیلڈ نام` کے ساتھ قبول کریں۔ ایک فائل کو ذخیرہ کیا جائے گا
`req.file` میں۔

#### `.array (فیلڈ نام [، میکسکاؤنٹ])`

فائلوں کی ایک صف کو قبول کریں ، سبھی نام `فیلڈ نام` کے ساتھ۔ اختیاری طور پر غلطی ختم کردی گئی ہے
max میکسکاؤنٹ سے زیادہ فائلیں اپ لوڈ کی گئیں۔ فائلوں کی صف میں ذخیرہ کیا جائے گا
`req.files`.

#### `. فیلڈز (فیلڈز)`

فائلوں کا مرکب قبول کریں ، جو `فیلڈز کے ذریعہ بیان کیا گیا ہے۔ فائلوں کی صفوں کے ساتھ ایک شے
`req.files` میں محفوظ کیا جائے گا۔

`فیلڈز` نام اور اختیاری طور پر ایک `میکسکاؤنٹ` کے ساتھ اشیاء کی ایک صف ہونی چاہئے۔
مثال:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.none ()`

صرف ٹیکسٹ فیلڈز کو قبول کریں۔ اگر کوئی فائل اپ لوڈ کی گئی ہے تو ، کوڈ کے ساتھ غلطی
"حد \ _ غیر متوقع \ _File" جاری کی جائے گی۔

#### `.any ()`

تار کے اوپر آنے والی تمام فائلوں کو قبول کرتا ہے۔ فائلوں کی ایک صف میں ذخیرہ کیا جائے گا
`req.files`.

** انتباہ: ** یقینی بنائیں کہ آپ ہمیشہ فائلوں کو سنبھالتے ہیں جو صارف اپ لوڈ کرتے ہیں۔
کبھی بھی مولٹر کو عالمی مڈل ویئر کے طور پر شامل نہ کریں کیونکہ کوئی بدنیتی پر مبنی صارف اپ لوڈ کرسکتا ہے
فائلوں کو ایک ایسے راستے پر جس کا آپ نے اندازہ نہیں کیا تھا۔ صرف اس فنکشن کو راستوں پر استعمال کریں
جہاں آپ اپ لوڈ کردہ فائلوں کو سنبھال رہے ہیں۔

### `اسٹوریج`

#### `ڈسک اسٹوریج`

ڈسک اسٹوریج انجن آپ کو فائلوں کو ڈسک پر اسٹور کرنے پر مکمل کنٹرول فراہم کرتا ہے۔

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

یہاں دو اختیارات دستیاب ہیں ، `منزل اور` فائل نام ہے۔ وہ دونوں ہیں
افعال جو طے کرتے ہیں کہ فائل کو کہاں محفوظ کیا جانا چاہئے۔

`منزل کا استعمال اس بات کا تعین کرنے کے لئے کیا جاتا ہے کہ اپ لوڈ کردہ فائلوں کو کس فولڈر میں ہونا چاہئے
ذخیرہ کریں۔ اسے بطور `سٹرنگ` (جیسے` '/TMP/اپ لوڈز') کے طور پر بھی دیا جاسکتا ہے۔ اگر نہیں
`منزل مقصود دی جاتی ہے ، عارضی طور پر آپریٹنگ سسٹم کی ڈیفالٹ ڈائرکٹری
فائلیں استعمال ہوتی ہیں۔

** نوٹ: ** فراہم کرتے وقت آپ ڈائریکٹری بنانے کے ذمہ دار ہیں
`منزل کے طور پر ایک فنکشن کے طور پر۔ جب تار گزرتے ہو تو ، مولٹر اس بات کو یقینی بنائے گا
ڈائریکٹری آپ کے لئے بنائی گئی ہے۔

`فائل نام` اس بات کا تعین کرنے کے لئے استعمال کیا جاتا ہے کہ فولڈر کے اندر فائل کا نام کیا ہونا چاہئے۔
اگر کوئی `فائل نام نہیں دیا گیا ہے تو ، ہر فائل کو بے ترتیب نام دیا جائے گا جو نہیں کرتا ہے
کسی بھی فائل کی توسیع شامل کریں۔

** نوٹ: ** مولٹر آپ کے لئے ، آپ کے فنکشن کے ل any کسی فائل کی توسیع کو شامل نہیں کرے گا
فائل توسیع کے ساتھ مکمل فائل کا نام واپس کرنا چاہئے۔

ہر فنکشن درخواست (`req`) اور اس کے بارے میں کچھ معلومات دونوں سے گزر جاتا ہے
فیصلے میں مدد کے لئے فائل (`فائل)۔

نوٹ کریں کہ `req.body` شاید ابھی تک مکمل طور پر آباد نہیں ہوا ہوگا۔ یہ منحصر ہے
آرڈر کریں کہ کلائنٹ فیلڈ اور فائلوں کو سرور میں منتقل کرے۔

کال بیک میں استعمال ہونے والے کالنگ کنونشن کو سمجھنے کے لئے (گزرنے کی ضرورت ہے
پہلے پیرام کے طور پر NULL) ، حوالہ دیں
.

#### `میموری اسٹوریج`

میموری اسٹوریج انجن فائلوں کو میموری میں `بفر` آبجیکٹ کے بطور اسٹور کرتا ہے۔ یہ
کوئی آپشن نہیں ہے۔

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

میموری اسٹوریج کا استعمال کرتے وقت ، فائل کی معلومات میں ایک فیلڈ شامل ہوگا جس کا نام ہے
`بفر` جس میں پوری فائل شامل ہے۔

** انتباہ **: بہت بڑی فائلیں ، یا بڑی تعداد میں نسبتا small چھوٹی فائلیں اپ لوڈ کرنا
تعداد بہت جلد ، جب آپ کی درخواست کو میموری ختم ہونے کا سبب بن سکتا ہے
میموری اسٹوریج استعمال ہوتا ہے۔

### `حدود`

مندرجہ ذیل اختیاری خصوصیات کی سائز کی حدود کی وضاحت کرنے والا ایک شے۔ مولٹر اس آبجیکٹ کو براہ راست بس بوائے میں منتقل کرتا ہے ، اور جائیدادوں کی تفصیلات [بس بوائے کے صفحے] (https://github.com/mscdex/busboy#busboy-methods) پر مل سکتی ہیں۔

مندرجہ ذیل عددی اقدار دستیاب ہیں:

کلید | تفصیل | پہلے سے طے شدہ
--- | --- | ---
`فیلڈنامائز` | زیادہ سے زیادہ فیلڈ کا نام سائز | 100 بائٹس
`فیلڈسائز` | زیادہ سے زیادہ فیلڈ ویلیو سائز (بائٹس میں) | 1MB
`فیلڈز` | غیر فائل فیلڈز کی زیادہ سے زیادہ تعداد | انفینٹی
`فائلسائز` | ملٹی پارٹ فارموں کے لئے ، زیادہ سے زیادہ فائل کا سائز (بائٹس میں) | انفینٹی
`فائلیں` | ملٹی پارٹ فارموں کے لئے ، فائل فیلڈز کی زیادہ سے زیادہ تعداد | انفینٹی
`حصے` | ملٹی پارٹ فارموں کے لئے ، حصوں کی زیادہ سے زیادہ تعداد (فیلڈز + فائلیں) | انفینٹی
`ہیڈرپیرز | ملٹی پارٹ فارموں کے لئے ، ہیڈر کی کلید کی زیادہ سے زیادہ تعداد => پارس کے لئے قدر کے جوڑے | 2000

حدود کی وضاحت کرنے سے آپ کی سائٹ کو سروس (DOS) کے حملوں سے انکار سے بچانے میں مدد مل سکتی ہے۔

### `فائل فلٹر`

اس کو کنٹرول کرنے کے لئے کسی فنکشن پر سیٹ کریں کہ کون سی فائلیں اپ لوڈ کی جائیں اور کون سا
چھوڑنا چاہئے۔ فنکشن اس طرح نظر آنا چاہئے:

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

## غلطی سے نمٹنے کے

جب کسی غلطی کا سامنا کرنا پڑتا ہے تو ، مولٹر غلطی کو ظاہر کرنے کے لئے غلطی کرے گا۔ آپ کر سکتے ہیں
[معیاری ایکسپریس وے] (http://expressjs.com/guide/error-handling.html) کا استعمال کرتے ہوئے ایک اچھا غلطی والا صفحہ دکھائیں۔

اگر آپ خاص طور پر مولٹر سے غلطیوں کو پکڑنا چاہتے ہیں تو ، آپ کال کرسکتے ہیں
خود ہی مڈل ویئر کا کام۔ نیز ، اگر آپ صرف [مولٹر کی غلطیاں] (https://github.com/expressjs/multer/blob/maine/lib/multer-error.js) کو پکڑنا چاہتے ہیں تو ، آپ `مولٹرر` آبجیکٹ کے ساتھ منسلک `مولٹریرر` کلاس استعمال کرسکتے ہیں۔

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

## کسٹم اسٹوریج انجن

اپنے اسٹوریج انجن کی تعمیر کے بارے میں معلومات کے ل [، [ملٹر اسٹوریج انجن] (https://github.com/expressjs/multer/blob/main/storageengine.md) دیکھیں۔

## لائسنس

[میرا] (لائسنس)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
