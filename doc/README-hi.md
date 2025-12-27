# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

मल्टर `मल्टीपार्ट/फॉर्म-डेटा` को संभालने के लिए एक नोड.जेएस मिडलवेयर है, जिसका उपयोग मुख्य रूप से फ़ाइलें अपलोड करने के लिए किया जाता है। यह लिखा है
अधिकतम दक्षता के लिए [busboy](https://github.com/mscdex/busboy) के शीर्ष पर।

**ध्यान दें**: मल्टर ऐसे किसी भी फॉर्म को प्रोसेस नहीं करेगा जो मल्टीपार्ट (`मल्टीपार्ट/फॉर्म-डेटा`) नहीं है।

## अनुवाद

यह README अन्य भाषाओं में भी उपलब्ध है:

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

## उपयोग

मल्टर 'अनुरोध' ऑब्जेक्ट में एक 'बॉडी' ऑब्जेक्ट और एक 'फ़ाइल' या 'फ़ाइलें' ऑब्जेक्ट जोड़ता है। `बॉडी` ऑब्जेक्ट में फ़ॉर्म के टेक्स्ट फ़ील्ड के मान शामिल हैं, `फ़ाइल` या `फ़ाइलें` ऑब्जेक्ट में फ़ॉर्म के माध्यम से अपलोड की गई फ़ाइलें शामिल हैं।

मूल उपयोग उदाहरण:

अपने फॉर्म में `enctype='multipart/form-data'` को न भूलें।

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

यदि आपको केवल-टेक्स्ट मल्टीपार्ट फॉर्म को संभालने की आवश्यकता है, तो आपको `.none()` विधि का उपयोग करना चाहिए:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

यहां एक उदाहरण दिया गया है कि HTML फॉर्म में मल्टर का उपयोग कैसे किया जाता है। `enctype='multipart/form-data'` और `name='uploaded_file'` फ़ील्ड का विशेष ध्यान रखें:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

फिर अपनी जावास्क्रिप्ट फ़ाइल में आप फ़ाइल और बॉडी दोनों तक पहुँचने के लिए इन पंक्तियों को जोड़ेंगे। यह महत्वपूर्ण है कि आप अपने अपलोड फ़ंक्शन में फ़ॉर्म से `नाम` फ़ील्ड मान का उपयोग करें। यह मल्टर को बताता है कि अनुरोध पर उसे किस फ़ील्ड में फ़ाइलें ढूंढनी चाहिए। यदि ये फ़ील्ड HTML फॉर्म और आपके सर्वर पर समान नहीं हैं, तो आपका अपलोड विफल हो जाएगा:

```javascript
const multer  = require('multer')
const upload = multer({ dest: './public/data/uploads/' })
app.post('/stats', upload.single('uploaded_file'), function (req, res) {
  // req.file is the name of your file in the form above, here 'uploaded_file'
  // req.body will hold the text fields, if there were any
  console.log(req.file, req.body)
});
```



## एपीआई

### फ़ाइल जानकारी

प्रत्येक फ़ाइल में निम्नलिखित जानकारी होती है:

कुंजी | विवरण | नोट
--- | --- | ---
`फ़ील्डनाम` | प्रपत्र में निर्दिष्ट फ़ील्ड नाम |
`मूल नाम` | उपयोगकर्ता के कंप्यूटर पर फ़ाइल का नाम |
`एन्कोडिंग` | फ़ाइल का एन्कोडिंग प्रकार |
`माइमटाइप` | फ़ाइल का माइम प्रकार |
'आकार' | फ़ाइल का आकार बाइट्स में |
`गंतव्य` | वह फ़ोल्डर जिसमें फ़ाइल सहेजी गई है | `डिस्कस्टोरेज`
`फ़ाइल नाम` | `गंतव्य` के अंतर्गत फ़ाइल का नाम | `डिस्कस्टोरेज`
`पथ` | अपलोड की गई फ़ाइल का पूरा पथ | `डिस्कस्टोरेज`
'बफ़र' | संपूर्ण फ़ाइल का एक `बफ़र` | `मेमोरीस्टोरेज`

### `मल्टर(ऑप्ट्स)`

मल्टर एक विकल्प ऑब्जेक्ट स्वीकार करता है, जिसमें से सबसे बुनियादी 'डेस्ट' है
संपत्ति, जो मुल्टर को बताती है कि फ़ाइलें कहाँ अपलोड करनी हैं। यदि आप इसे छोड़ देते हैं
विकल्प ऑब्जेक्ट, फ़ाइलें मेमोरी में रखी जाएंगी और डिस्क पर कभी नहीं लिखी जाएंगी।

डिफ़ॉल्ट रूप से, मल्टर फ़ाइलों का नाम बदल देगा ताकि नामकरण संबंधी विवादों से बचा जा सके। द
नाम बदलने के कार्य को आपकी आवश्यकताओं के अनुसार अनुकूलित किया जा सकता है।

निम्नलिखित विकल्प हैं जिन्हें मुल्टर को दिया जा सकता है।

कुंजी | विवरण
--- | ---
`नियति` या `भंडारण` | फ़ाइलें कहाँ संग्रहित करें
`फ़ाइलफ़िल्टर` | यह नियंत्रित करने का कार्य कि कौन सी फ़ाइलें स्वीकार की जाती हैं
'सीमाएँ' | अपलोड किए गए डेटा की सीमाएं
`प्रिज़र्वपाथ` | केवल आधार नाम के बजाय फ़ाइलों का पूरा पथ रखें

एक औसत वेब ऐप में, केवल `dest` की आवश्यकता हो सकती है, और दिखाए गए अनुसार कॉन्फ़िगर किया जा सकता है
निम्नलिखित उदाहरण.

```javascript
const upload = multer({ dest: 'uploads/' })
```

यदि आप अपने अपलोड पर अधिक नियंत्रण चाहते हैं, तो आप `स्टोरेज` का उपयोग करना चाहेंगे
`dest` के बजाय विकल्प। मुल्टर स्टोरेज इंजन `डिस्कस्टोरेज` के साथ जहाज करता है
और `मेमोरीस्टोरेज`; तृतीय पक्षों से अधिक इंजन उपलब्ध हैं।

#### `.एकल(फ़ील्डनाम)`

`फ़ील्डनाम` नाम से एकल फ़ाइल स्वीकार करें। एकल फ़ाइल संग्रहीत की जाएगी
`req.file` में।

#### `.array(fieldname[, maxCount])`

फ़ाइलों की एक श्रृंखला स्वीकार करें, सभी `फ़ील्डनाम` नाम से। यदि वैकल्पिक रूप से त्रुटि हो
`maxCount` से अधिक फ़ाइलें अपलोड की गई हैं। फ़ाइलों की श्रृंखला संग्रहित की जाएगी
`req.files`।

#### `.फ़ील्ड्स(फ़ील्ड्स)`

`फ़ील्ड्स` द्वारा निर्दिष्ट फ़ाइलों का मिश्रण स्वीकार करें। फ़ाइलों की सरणियों वाला एक ऑब्जेक्ट
`req.files` में संग्रहीत किया जाएगा।

`फ़ील्ड` `नाम` और वैकल्पिक रूप से `मैक्सकाउंट` के साथ वस्तुओं की एक सरणी होनी चाहिए।
उदाहरण:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.none()`

केवल टेक्स्ट फ़ील्ड स्वीकार करें. यदि कोई फ़ाइल अपलोड की जाती है, तो कोड में त्रुटि होती है
"LIMIT\_UNEXPECTED\_FILE" जारी किया जाएगा।

#### `.कोई()`

वायर पर आने वाली सभी फ़ाइलों को स्वीकार करता है। फ़ाइलों की एक श्रृंखला संग्रहित की जाएगी
`req.files`।

**चेतावनी:** सुनिश्चित करें कि आप हमेशा उन फ़ाइलों को संभालें जो उपयोगकर्ता अपलोड करता है।
मल्टर को कभी भी वैश्विक मिडलवेयर के रूप में न जोड़ें क्योंकि कोई दुर्भावनापूर्ण उपयोगकर्ता अपलोड कर सकता है
फ़ाइलें उस रूट पर ले जाती हैं जिसका आपने अनुमान नहीं लगाया था। इस फ़ंक्शन का उपयोग केवल मार्गों पर करें
जहां आप अपलोड की गई फ़ाइलों को संभाल रहे हैं।

### `भंडारण`

#### `डिस्कस्टोरेज`

डिस्क स्टोरेज इंजन आपको फ़ाइलों को डिस्क पर संग्रहीत करने पर पूर्ण नियंत्रण देता है।

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

दो विकल्प उपलब्ध हैं, `गंतव्य` और `फ़ाइल नाम`। वे दोनों हैं
फ़ंक्शंस जो यह निर्धारित करते हैं कि फ़ाइल को कहाँ संग्रहीत किया जाना चाहिए।

`गंतव्य` का उपयोग यह निर्धारित करने के लिए किया जाता है कि अपलोड की गई फ़ाइलें किस फ़ोल्डर में होनी चाहिए
संग्रहित किया जाए. इसे `स्ट्रिंग` (जैसे ``/tmp/uploads'`) के रूप में भी दिया जा सकता है। यदि नहीं
`गंतव्य` दिया गया है, अस्थायी के लिए ऑपरेटिंग सिस्टम की डिफ़ॉल्ट निर्देशिका
फ़ाइलों का उपयोग किया जाता है.

**ध्यान दें:** प्रदान करते समय निर्देशिका बनाने के लिए आप जिम्मेदार हैं
एक फ़ंक्शन के रूप में `गंतव्य`। एक स्ट्रिंग को पास करते समय, मल्टर यह सुनिश्चित करेगा
निर्देशिका आपके लिए बनाई गई है.

`फ़ाइल नाम` का उपयोग यह निर्धारित करने के लिए किया जाता है कि फ़ाइल को फ़ोल्डर के अंदर क्या नाम दिया जाना चाहिए।
यदि कोई `फ़ाइल नाम` नहीं दिया गया है, तो प्रत्येक फ़ाइल को एक यादृच्छिक नाम दिया जाएगा जो नहीं दिया गया है
कोई फ़ाइल एक्सटेंशन शामिल करें.

**ध्यान दें:** मल्टर आपके, आपके कार्य के लिए कोई फ़ाइल एक्सटेंशन नहीं जोड़ेगा
फ़ाइल एक्सटेंशन के साथ पूर्ण फ़ाइल नाम लौटाना चाहिए।

प्रत्येक फ़ंक्शन अनुरोध (`req`) और कुछ जानकारी दोनों को पारित कर देता है
निर्णय में सहायता के लिए फ़ाइल ('फ़ाइल')।

ध्यान दें कि `req.body` अभी तक पूरी तरह से पॉप्युलेट नहीं हुआ होगा। यह इस पर निर्भर करता है
आदेश दें कि क्लाइंट फ़ील्ड और फ़ाइलों को सर्वर तक पहुंचाए।

कॉलबैक में प्रयुक्त कॉलिंग कन्वेंशन को समझने के लिए (पास करने की आवश्यकता है)।
पहले पैरामीटर के रूप में शून्य), देखें
[Node.js त्रुटि प्रबंधन](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### `मेमोरीस्टोरेज`

मेमोरी स्टोरेज इंजन फ़ाइलों को मेमोरी में `बफ़र` ऑब्जेक्ट के रूप में संग्रहीत करता है। यह
कोई विकल्प नहीं है.

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

मेमोरी स्टोरेज का उपयोग करते समय, फ़ाइल जानकारी में एक फ़ील्ड होगी जिसे कहा जाता है
`बफ़र` जिसमें संपूर्ण फ़ाइल शामिल है।

**चेतावनी**: बहुत बड़ी फ़ाइलें, या अपेक्षाकृत छोटी फ़ाइलें बड़े पैमाने पर अपलोड करना
संख्याएँ बहुत तेज़ी से, आपके एप्लिकेशन की मेमोरी ख़त्म होने का कारण बन सकती हैं
मेमोरी स्टोरेज का उपयोग किया जाता है.

### `सीमाएँ`

निम्नलिखित वैकल्पिक गुणों की आकार सीमा निर्दिष्ट करने वाली एक वस्तु। मुल्टर इस ऑब्जेक्ट को सीधे बसबॉय में भेजता है, और गुणों का विवरण [बसबॉय के पेज] (https://github.com/mscdex/busboy#busboy-methods) पर पाया जा सकता है।

निम्नलिखित पूर्णांक मान उपलब्ध हैं:

कुंजी | विवरण | डिफ़ॉल्ट
--- | --- | ---
`फ़ील्डनाम आकार` | अधिकतम फ़ील्ड नाम आकार | 100 बाइट्स
`फ़ील्डसाइज़` | अधिकतम फ़ील्ड मान आकार (बाइट्स में) | 1एमबी
`फ़ील्ड` | गैर-फ़ाइल फ़ील्ड की अधिकतम संख्या | अनंत
`फ़ाइल आकार` | मल्टीपार्ट फॉर्म के लिए, अधिकतम फ़ाइल आकार (बाइट्स में) | अनंत
`फ़ाइलें` | मल्टीपार्ट फॉर्म के लिए, फ़ाइल फ़ील्ड की अधिकतम संख्या | अनंत
'भाग' | बहुखण्डीय प्रपत्रों के लिए, भागों की अधिकतम संख्या (फ़ील्ड + फ़ाइलें) | अनंत
`हेडरपेयर` | मल्टीपार्ट फॉर्म के लिए, पार्स करने के लिए हेडर कुंजी=>मान जोड़े की अधिकतम संख्या | 2000

सीमाएँ निर्दिष्ट करने से आपकी साइट को सेवा से इनकार (DoS) हमलों से बचाने में मदद मिल सकती है।

### `फ़ाइलफ़िल्टर`

इसे नियंत्रित करने के लिए एक फ़ंक्शन पर सेट करें कि कौन सी फ़ाइलें अपलोड की जानी चाहिए और कौन सी
छोड़ देना चाहिए. फ़ंक्शन इस तरह दिखना चाहिए:

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

## त्रुटि प्रबंधन

किसी त्रुटि का सामना करने पर, मल्टर त्रुटि को एक्सप्रेस को सौंप देगा। आप कर सकते हैं
[मानक एक्सप्रेस वे](http://expressjs.com/guide/error-handling.html) का उपयोग करके एक अच्छा त्रुटि पृष्ठ प्रदर्शित करें।

यदि आप विशेष रूप से मल्टर से त्रुटियों को पकड़ना चाहते हैं, तो आप कॉल कर सकते हैं
मिडलवेयर स्वयं कार्य करता है। इसके अलावा, यदि आप केवल [मल्टर त्रुटियों] (https://github.com/expressjs/multer/blob/main/lib/multer-error.js) को पकड़ना चाहते हैं, तो आप `MulterError` वर्ग का उपयोग कर सकते हैं जो `multer` ऑब्जेक्ट से जुड़ा हुआ है (उदाहरण के लिए `errinstanceof multer.MulterError`)।

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

## कस्टम स्टोरेज इंजन

अपना स्वयं का स्टोरेज इंजन कैसे बनाएं, इसकी जानकारी के लिए, [मल्टर स्टोरेज इंजन](https://github.com/expressjs/multer/blob/main/StorageEngine.md) देखें।

## लाइसेंस

[मेरा](लाइसेंस)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
