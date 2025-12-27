# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

Multer हे `multipart/form-data` हाताळण्यासाठी node.js मिडलवेअर आहे, जे प्रामुख्याने फाइल अपलोड करण्यासाठी वापरले जाते. असे लिहिले आहे
कमाल कार्यक्षमतेसाठी [busboy](https://github.com/mscdex/busboy) वर.

**टीप**: मल्टीपार्ट (`मल्टीपार्ट/फॉर्म-डेटा`) नसलेल्या कोणत्याही फॉर्मवर मल्टर प्रक्रिया करणार नाही.

## भाषांतरे

हा README इतर भाषांमध्ये देखील उपलब्ध आहे:

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

## स्थापना

```sh
$ npm install multer
```

## वापर

मल्टर एक `बॉडी` ऑब्जेक्ट आणि `विनंती` ऑब्जेक्टमध्ये `फाइल` किंवा `फाइल्स` ऑब्जेक्ट जोडते. `बॉडी` ऑब्जेक्टमध्ये फॉर्मच्या मजकूर फील्डची मूल्ये असतात, `फाइल` किंवा `फाइल्स` ऑब्जेक्टमध्ये फॉर्मद्वारे अपलोड केलेल्या फायली असतात.

मूलभूत वापर उदाहरण:

तुमच्या फॉर्ममध्ये `enctype="multipart/form-data"` विसरू नका.

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

तुम्हाला केवळ-मल्टीपार्ट फॉर्म हाताळण्याची आवश्यकता असल्यास, तुम्ही `.none()` पद्धत वापरावी:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

HTML फॉर्ममध्ये मल्टर कसे वापरले जाते याचे येथे एक उदाहरण आहे. `enctype="multipart/form-data"` आणि `name="uploaded_file"` फील्डची विशेष नोंद घ्या:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

नंतर तुमच्या जावास्क्रिप्ट फाइलमध्ये तुम्ही फाइल आणि मुख्य भाग दोन्हीमध्ये प्रवेश करण्यासाठी या ओळी जोडाल. तुमच्या अपलोड फंक्शनमधील फॉर्ममधील `नाव` फील्ड मूल्य वापरणे महत्त्वाचे आहे. हे मल्टरला विनंती करते की कोणत्या फील्डमध्ये फाइल्स शोधल्या पाहिजेत

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

### फाइल माहिती

प्रत्येक फाईलमध्ये खालील माहिती असते:

की | वर्णन | नोंद
--- | --- | ---
`फील्डनाव` | फॉर्ममध्ये निर्दिष्ट क्षेत्राचे नाव |
`मूळ नाव` | वापरकर्त्याच्या संगणकावरील फाइलचे नाव |
`एनकोडिंग` | फाइलचा एन्कोडिंग प्रकार |
`mimetype` | फाइलचा माइम प्रकार |
`आकार` | फाइलचा आकार बाइट्समध्ये |
`गंतव्य` | फाइल ज्या फोल्डरमध्ये सेव्ह केली गेली आहे 'डिस्क स्टोरेज'
`फाइलनाव` | `गंतव्य` मधील फाइलचे नाव | 'डिस्क स्टोरेज'
`पथ` | अपलोड केलेल्या फाइलचा संपूर्ण मार्ग | 'डिस्क स्टोरेज'
`बफर` | संपूर्ण फाइलचा एक `बफर` | `मेमरी स्टोरेज`

### `multer(opts)`

मल्टर ऑप्शन्स ऑब्जेक्ट स्वीकारतो, त्यातील सर्वात मूलभूत म्हणजे `डेस्ट`
मालमत्ता, जे मल्टरला फाइल्स कोठे अपलोड करायचे ते सांगते. आपण वगळल्यास
पर्याय ऑब्जेक्ट, फायली मेमरीमध्ये ठेवल्या जातील आणि डिस्कवर कधीही लिहिल्या जातील.

डीफॉल्टनुसार, मल्टर फाइल्सचे नाव बदलेल जेणेकरुन नामकरण विवाद टाळता येईल. द
नाव बदलण्याचे कार्य आपल्या गरजेनुसार सानुकूलित केले जाऊ शकते.

खालील पर्याय मल्टरला दिले जाऊ शकतात.

की | वर्णन
--- | ---
`dest` किंवा `स्टोरेज` | फाइल्स कुठे साठवायच्या
`फाइलफिल्टर` | कोणत्या फायली स्वीकारल्या जातात हे नियंत्रित करण्यासाठी कार्य
`मर्यादा` | अपलोड केलेल्या डेटाची मर्यादा
`preservePath` | फक्त बेस नावाऐवजी फाईल्सचा संपूर्ण मार्ग ठेवा

सरासरी वेब ॲपमध्ये, फक्त `dest` आवश्यक असू शकते आणि मध्ये दाखवल्याप्रमाणे कॉन्फिगर केले जाऊ शकते
खालील उदाहरण.

```javascript
const upload = multer({ dest: 'uploads/' })
```

तुम्हाला तुमच्या अपलोडवर अधिक नियंत्रण हवे असल्यास, तुम्ही `स्टोरेज` वापरू इच्छित असाल
`dest` ऐवजी पर्याय. स्टोरेज इंजिन `DiskStorage` सह मल्टर जहाजे
आणि 'मेमरी स्टोरेज'; तृतीय पक्षांकडून अधिक इंजिन उपलब्ध आहेत.

#### `.एकल(फील्डनाव)`

`फील्डनेम` नावाची एकच फाइल स्वीकारा. एकल फाइल संग्रहित केली जाईल
`req.file` मध्ये.

#### `.ॲरे(फील्डनाव[, maxCount])`

फाइल्सचा ॲरे स्वीकारा, सर्व `फील्डनेम` नावाने. वैकल्पिकरित्या त्रुटी असल्यास
`maxCount` पेक्षा जास्त फायली अपलोड केल्या आहेत. फाइल्सचा ॲरे मध्ये संग्रहित केला जाईल
`req.files`.

#### `.फील्ड्स(फील्ड्स)`

`फील्ड` द्वारे निर्दिष्ट केलेल्या फायलींचे मिश्रण स्वीकारा. फाइल्सच्या ॲरेसह एक ऑब्जेक्ट
`req.files` मध्ये साठवले जाईल.

`फील्ड` `नाव` आणि वैकल्पिकरित्या `maxCount` असलेल्या ऑब्जेक्ट्सचा ॲरे असावा.
उदाहरण:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.none()`

फक्त मजकूर फील्ड स्वीकारा. कोणतीही फाइल अपलोड केली असल्यास, कोडसह त्रुटी
"LIMIT\_UNEXPECTED\_FILE" जारी केली जाईल.

#### `.any()`

वायरवर येणाऱ्या सर्व फाईल्स स्वीकारते. फाइल्सचा एक ॲरे मध्ये संग्रहित केला जाईल
`req.files`.

**चेतावणी:** वापरकर्त्याने अपलोड केलेल्या फाइल्स तुम्ही नेहमी हाताळत असल्याची खात्री करा.
एक दुर्भावनापूर्ण वापरकर्ता अपलोड करू शकत असल्याने मल्टरला ग्लोबल मिडलवेअर म्हणून कधीही जोडू नका
आपण अपेक्षित नसलेल्या मार्गावरील फायली. हे कार्य फक्त मार्गांवर वापरा
जिथे तुम्ही अपलोड केलेल्या फाइल्स हाताळत आहात.

### `स्टोरेज`

#### `डिस्क स्टोरेज`

डिस्क स्टोरेज इंजिन तुम्हाला डिस्कवर फाइल्स स्टोअर करण्यावर पूर्ण नियंत्रण देते.

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

दोन पर्याय उपलब्ध आहेत, `गंतव्य` आणि `फाइलनाव`. ते दोघे आहेत
फंक्शन्स जी फाइल कुठे साठवायची हे ठरवतात.

अपलोड केलेल्या फाईल्स कोणत्या फोल्डरमध्ये असावी हे निर्धारित करण्यासाठी `गंतव्य` वापरले जाते
साठवले जावे. हे `स्ट्रिंग` म्हणून देखील दिले जाऊ शकते (उदा. `'/tmp/uploads'`). जर नाही
`गंतव्य` दिले आहे, तात्पुरत्यासाठी ऑपरेटिंग सिस्टमची डीफॉल्ट निर्देशिका
फाइल्स वापरल्या जातात.

**टीप:** प्रदान करताना निर्देशिका तयार करण्यासाठी तुम्ही जबाबदार आहात
फंक्शन म्हणून `गंतव्य`. स्ट्रिंग पास करताना, मल्टर याची खात्री करेल
निर्देशिका तुमच्यासाठी तयार केली आहे.

फोल्डरमध्ये फाइलचे नाव काय असावे हे निर्धारित करण्यासाठी `फाइलनेम` वापरले जाते.
कोणतेही `फाइलनाव` दिलेले नसल्यास, प्रत्येक फाइलला एक यादृच्छिक नाव दिले जाईल जे नाही
कोणत्याही फाइल विस्ताराचा समावेश करा.

**टीप:** मल्टर तुमच्यासाठी, तुमच्या कार्यासाठी कोणताही फाईल विस्तार जोडणार नाही
फाईल एक्स्टेंशनसह पूर्ण फाइल नाव परत केले पाहिजे.

प्रत्येक फंक्शन विनंती (`req`) आणि काही माहिती दोन्ही पास केले जाते
फाईल (`फाइल`) निर्णयात मदत करण्यासाठी.

लक्षात ठेवा की `req.body` अद्याप पूर्णपणे भरलेले नसावे. यावर अवलंबून आहे
क्लायंटने फील्ड आणि फाइल्स सर्व्हरवर पाठविण्याचा क्रम.

कॉलबॅकमध्ये वापरलेले कॉलिंग कन्व्हेन्शन समजून घेण्यासाठी (पास करणे आवश्यक आहे
प्रथम परम म्हणून नल), पहा
[Node.js त्रुटी हाताळणी](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### `मेमरी स्टोरेज`

मेमरी स्टोरेज इंजिन फायली मेमरीमध्ये `बफर` ऑब्जेक्ट्स म्हणून संग्रहित करते. ते
कोणतेही पर्याय नाहीत.

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

मेमरी स्टोरेज वापरताना, फाइल माहितीमध्ये एक फील्ड असेल
'बफर' ज्यामध्ये संपूर्ण फाइल आहे.

**चेतावणी**: खूप मोठ्या फाईल्स किंवा तुलनेने लहान फाईल्स मोठ्या प्रमाणात अपलोड करणे
संख्या खूप लवकर, जेव्हा तुमचा अनुप्रयोग मेमरी संपुष्टात येऊ शकते
मेमरी स्टोरेज वापरले जाते.

### `मर्यादा`

खालील पर्यायी गुणधर्मांच्या आकार मर्यादा निर्दिष्ट करणारी ऑब्जेक्ट. Multer हा ऑब्जेक्ट थेट बसबॉयमध्ये पास करतो आणि गुणधर्मांचे तपशील [busboy's page](https://github.com/mscdex/busboy#busboy-methods) वर आढळू शकतात.

खालील पूर्णांक मूल्ये उपलब्ध आहेत:

की | वर्णन | डीफॉल्ट
--- | --- | ---
`fieldNameSize` | कमाल फील्ड नाव आकार | 100 बाइट्स
`फील्डआकार` | कमाल फील्ड मूल्य आकार (बाइट्समध्ये) | 1MB
`फील्ड` | फाइल नसलेल्या फील्डची कमाल संख्या | अनंत
`fileSize` | मल्टीपार्ट फॉर्मसाठी, कमाल फाइल आकार (बाइट्समध्ये) | अनंत
`फाईल्स` | मल्टीपार्ट फॉर्मसाठी, फाइल फील्डची कमाल संख्या | अनंत
`भाग` | मल्टीपार्ट फॉर्मसाठी, भागांची कमाल संख्या (फील्ड + फाइल्स) | अनंत
`हेडरपेअर्स` | मल्टीपार्ट फॉर्मसाठी, हेडर की =>विश्लेषणासाठी मूल्य जोड्यांची कमाल संख्या | 2000

मर्यादा निर्दिष्ट केल्याने सेवा नाकारणे (DoS) हल्ल्यांपासून तुमच्या साइटचे संरक्षण करण्यात मदत होऊ शकते.

### `फाइलफिल्टर`

कोणत्या फाइल अपलोड करायच्या आणि कोणत्या हे नियंत्रित करण्यासाठी हे फंक्शनवर सेट करा
वगळले पाहिजे. फंक्शन यासारखे दिसले पाहिजे:

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

## हाताळणी त्रुटी

एरर समोर आल्यावर, मल्टर एरर एक्सप्रेसला सोपवेल. आपण करू शकता
[मानक एक्सप्रेस मार्ग](http://expressjs.com/guide/error-handling.html) वापरून एक छान त्रुटी पृष्ठ प्रदर्शित करा.

जर तुम्हाला विशेषत: मल्टरकडून चुका पकडायच्या असतील, तर तुम्ही कॉल करू शकता
मिडलवेअर फंक्शन स्वतःहून. तसेच, तुम्हाला फक्त [मुल्टर एरर] (https://github.com/expressjs/multer/blob/main/lib/multer-error.js) पकडायचे असल्यास, तुम्ही `multer` ऑब्जेक्टशी संलग्न असलेला `MulterError` क्लास वापरू शकता (उदा. `Errr instance of multer' or`Multer).

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

## कस्टम स्टोरेज इंजिन

तुमचे स्वतःचे स्टोरेज इंजिन कसे तयार करायचे याच्या माहितीसाठी, [Multer Storage Engine](https://github.com/expressjs/multer/blob/main/StorageEngine.md) पहा.

## परवाना

[माझे](परवाना)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
