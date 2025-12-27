# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

Multer `multipart/form-data` ਨੂੰ ਸੰਭਾਲਣ ਲਈ ਇੱਕ node.js ਮਿਡਲਵੇਅਰ ਹੈ, ਜੋ ਕਿ ਮੁੱਖ ਤੌਰ 'ਤੇ ਫ਼ਾਈਲਾਂ ਨੂੰ ਅੱਪਲੋਡ ਕਰਨ ਲਈ ਵਰਤਿਆ ਜਾਂਦਾ ਹੈ। ਲਿਖਿਆ ਹੈ
ਵੱਧ ਤੋਂ ਵੱਧ ਕੁਸ਼ਲਤਾ ਲਈ [busboy](https://github.com/mscdex/busboy) ਦੇ ਸਿਖਰ 'ਤੇ।

**ਨੋਟ**: ਮਲਟਰ ਕਿਸੇ ਵੀ ਫਾਰਮ 'ਤੇ ਪ੍ਰਕਿਰਿਆ ਨਹੀਂ ਕਰੇਗਾ ਜੋ ਮਲਟੀਪਾਰਟ (`ਮਲਟੀਪਾਰਟ/ਫਾਰਮ-ਡਾਟਾ`) ਨਹੀਂ ਹੈ।

## ਅਨੁਵਾਦ

ਇਹ README ਹੋਰ ਭਾਸ਼ਾਵਾਂ ਵਿੱਚ ਵੀ ਉਪਲਬਧ ਹੈ:

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

## ਇੰਸਟਾਲੇਸ਼ਨ

```sh
$ npm install multer
```

## ਵਰਤੋਂ

ਮਲਟਰ ਇੱਕ `ਸਰੀਰ` ਆਬਜੈਕਟ ਅਤੇ ਇੱਕ `ਫਾਇਲ` ਜਾਂ `ਫਾਇਲਾਂ` ਆਬਜੈਕਟ ਨੂੰ `ਬੇਨਤੀ` ਵਸਤੂ ਵਿੱਚ ਜੋੜਦਾ ਹੈ। `ਸਰੀਰ` ਵਸਤੂ ਵਿੱਚ ਫਾਰਮ ਦੇ ਟੈਕਸਟ ਖੇਤਰਾਂ ਦੇ ਮੁੱਲ ਸ਼ਾਮਲ ਹੁੰਦੇ ਹਨ, `ਫ਼ਾਈਲ` ਜਾਂ `ਫ਼ਾਈਲਾਂ` ਵਸਤੂ ਵਿੱਚ ਫਾਰਮ ਰਾਹੀਂ ਅੱਪਲੋਡ ਕੀਤੀਆਂ ਫ਼ਾਈਲਾਂ ਸ਼ਾਮਲ ਹੁੰਦੀਆਂ ਹਨ।

ਬੁਨਿਆਦੀ ਵਰਤੋਂ ਉਦਾਹਰਨ:

ਆਪਣੇ ਫਾਰਮ ਵਿੱਚ `enctype="multipart/form-data"` ਨੂੰ ਨਾ ਭੁੱਲੋ।

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

ਜੇਕਰ ਤੁਹਾਨੂੰ ਇੱਕ ਟੈਕਸਟ-ਓਨਲੀ ਮਲਟੀਪਾਰਟ ਫਾਰਮ ਨੂੰ ਹੈਂਡਲ ਕਰਨ ਦੀ ਲੋੜ ਹੈ, ਤਾਂ ਤੁਹਾਨੂੰ `.none()` ਵਿਧੀ ਦੀ ਵਰਤੋਂ ਕਰਨੀ ਚਾਹੀਦੀ ਹੈ:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

ਇੱਥੇ ਇੱਕ ਉਦਾਹਰਨ ਹੈ ਕਿ HTML ਰੂਪ ਵਿੱਚ ਮਲਟਰ ਦੀ ਵਰਤੋਂ ਕਿਵੇਂ ਕੀਤੀ ਜਾਂਦੀ ਹੈ। `enctype="multipart/form-data"` ਅਤੇ `name="uploaded_file"` ਖੇਤਰਾਂ ਦਾ ਵਿਸ਼ੇਸ਼ ਧਿਆਨ ਰੱਖੋ:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

ਫਿਰ ਤੁਹਾਡੀ ਜਾਵਾਸਕ੍ਰਿਪਟ ਫਾਈਲ ਵਿੱਚ ਤੁਸੀਂ ਫਾਈਲ ਅਤੇ ਬਾਡੀ ਦੋਵਾਂ ਤੱਕ ਪਹੁੰਚ ਕਰਨ ਲਈ ਇਹਨਾਂ ਲਾਈਨਾਂ ਨੂੰ ਜੋੜੋਗੇ। ਇਹ ਮਹੱਤਵਪੂਰਨ ਹੈ ਕਿ ਤੁਸੀਂ ਆਪਣੇ ਅੱਪਲੋਡ ਫੰਕਸ਼ਨ ਵਿੱਚ ਫਾਰਮ ਤੋਂ `ਨਾਮ` ਖੇਤਰ ਮੁੱਲ ਦੀ ਵਰਤੋਂ ਕਰੋ। ਇਹ ਮਲਟਰ ਨੂੰ ਦੱਸਦਾ ਹੈ ਕਿ ਬੇਨਤੀ 'ਤੇ ਕਿਸ ਫੀਲਡ ਨੂੰ ਇਸ ਵਿੱਚ ਫਾਈਲਾਂ ਦੀ ਭਾਲ ਕਰਨੀ ਚਾਹੀਦੀ ਹੈ। ਜੇਕਰ ਇਹ ਖੇਤਰ HTML ਫਾਰਮ ਅਤੇ ਤੁਹਾਡੇ ਸਰਵਰ ਵਿੱਚ ਇੱਕੋ ਜਿਹੇ ਨਹੀਂ ਹਨ, ਤਾਂ ਤੁਹਾਡਾ ਅਪਲੋਡ ਅਸਫਲ ਹੋ ਜਾਵੇਗਾ:

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

### ਫਾਈਲ ਜਾਣਕਾਰੀ

ਹਰੇਕ ਫਾਈਲ ਵਿੱਚ ਹੇਠ ਲਿਖੀ ਜਾਣਕਾਰੀ ਹੁੰਦੀ ਹੈ:

ਕੁੰਜੀ | ਵਰਣਨ | ਨੋਟ ਕਰੋ
--- | --- | ---
`ਖੇਤਰ ਦਾ ਨਾਮ` | ਫਾਰਮ ਵਿੱਚ ਦਿੱਤਾ ਗਿਆ ਖੇਤਰ ਦਾ ਨਾਮ |
`ਅਸਲ ਨਾਮ` | ਉਪਭੋਗਤਾ ਦੇ ਕੰਪਿਊਟਰ 'ਤੇ ਫਾਈਲ ਦਾ ਨਾਮ |
`ਇੰਕੋਡਿੰਗ` | ਫਾਈਲ ਦੀ ਏਨਕੋਡਿੰਗ ਕਿਸਮ |
`ਮਾਈਮਟਾਈਪ` | ਫਾਈਲ ਦੀ ਮਾਈਮ ਕਿਸਮ |
`ਆਕਾਰ` | ਬਾਈਟ ਵਿੱਚ ਫਾਈਲ ਦਾ ਆਕਾਰ |
`ਮੰਜ਼ਿਲ` | ਉਹ ਫੋਲਡਰ ਜਿਸ ਵਿੱਚ ਫਾਈਲ ਸੇਵ ਕੀਤੀ ਗਈ ਹੈ | 'ਡਿਸਕ ਸਟੋਰੇਜ'
`ਫ਼ਾਈਲ ਨਾਮ` | 'ਮੰਜ਼ਿਲ' | ਦੇ ਅੰਦਰ ਫਾਈਲ ਦਾ ਨਾਮ 'ਡਿਸਕ ਸਟੋਰੇਜ'
`ਮਾਰਗ` | ਅੱਪਲੋਡ ਕੀਤੀ ਫਾਈਲ ਦਾ ਪੂਰਾ ਮਾਰਗ | 'ਡਿਸਕ ਸਟੋਰੇਜ'
`ਬਫਰ` | ਪੂਰੀ ਫਾਈਲ ਦਾ ਇੱਕ `ਬਫਰ` | 'ਮੈਮੋਰੀ ਸਟੋਰੇਜ'

### `multer(opts)`

ਮਲਟਰ ਇੱਕ ਵਿਕਲਪ ਵਸਤੂ ਨੂੰ ਸਵੀਕਾਰ ਕਰਦਾ ਹੈ, ਜਿਸ ਵਿੱਚੋਂ ਸਭ ਤੋਂ ਬੁਨਿਆਦੀ 'ਡੈਸਟ' ਹੈ
ਜਾਇਦਾਦ, ਜੋ ਕਿ ਮਲਟਰ ਨੂੰ ਦੱਸਦੀ ਹੈ ਕਿ ਫਾਈਲਾਂ ਕਿੱਥੇ ਅਪਲੋਡ ਕਰਨੀਆਂ ਹਨ। ਜੇਕਰ ਤੁਸੀਂ ਇਸ ਨੂੰ ਛੱਡ ਦਿੰਦੇ ਹੋ
ਵਿਕਲਪ ਆਬਜੈਕਟ, ਫਾਈਲਾਂ ਨੂੰ ਮੈਮੋਰੀ ਵਿੱਚ ਰੱਖਿਆ ਜਾਵੇਗਾ ਅਤੇ ਕਦੇ ਵੀ ਡਿਸਕ ਤੇ ਨਹੀਂ ਲਿਖਿਆ ਜਾਵੇਗਾ.

ਮੂਲ ਰੂਪ ਵਿੱਚ, ਮਲਟਰ ਫਾਈਲਾਂ ਦਾ ਨਾਮ ਬਦਲ ਦੇਵੇਗਾ ਤਾਂ ਜੋ ਨਾਮਕਰਨ ਵਿਵਾਦਾਂ ਤੋਂ ਬਚਿਆ ਜਾ ਸਕੇ। ਦ
ਰੀਨਾਮਿੰਗ ਫੰਕਸ਼ਨ ਤੁਹਾਡੀਆਂ ਜ਼ਰੂਰਤਾਂ ਦੇ ਅਨੁਸਾਰ ਅਨੁਕੂਲਿਤ ਕੀਤਾ ਜਾ ਸਕਦਾ ਹੈ.

ਹੇਠਾਂ ਦਿੱਤੇ ਵਿਕਲਪ ਹਨ ਜੋ ਮਲਟਰ ਨੂੰ ਪਾਸ ਕੀਤੇ ਜਾ ਸਕਦੇ ਹਨ।

ਕੁੰਜੀ | ਵਰਣਨ
--- | ---
`dest` ਜਾਂ `storage` | ਫਾਈਲਾਂ ਨੂੰ ਕਿੱਥੇ ਸਟੋਰ ਕਰਨਾ ਹੈ
'ਫਾਇਲਫਿਲਟਰ' | ਕਿਹੜੀਆਂ ਫਾਈਲਾਂ ਸਵੀਕਾਰ ਕੀਤੀਆਂ ਜਾਂਦੀਆਂ ਹਨ ਨੂੰ ਨਿਯੰਤਰਿਤ ਕਰਨ ਲਈ ਫੰਕਸ਼ਨ
`ਸੀਮਾ` | ਅੱਪਲੋਡ ਕੀਤੇ ਡੇਟਾ ਦੀਆਂ ਸੀਮਾਵਾਂ
`preservePath` | ਸਿਰਫ਼ ਅਧਾਰ ਨਾਮ ਦੀ ਬਜਾਏ ਫਾਈਲਾਂ ਦਾ ਪੂਰਾ ਮਾਰਗ ਰੱਖੋ

ਔਸਤ ਵੈੱਬ ਐਪ ਵਿੱਚ, ਸਿਰਫ਼ 'ਡੈਸਟ' ਦੀ ਲੋੜ ਹੋ ਸਕਦੀ ਹੈ, ਅਤੇ ਇਸ ਵਿੱਚ ਦਿਖਾਏ ਅਨੁਸਾਰ ਕੌਂਫਿਗਰ ਕੀਤਾ ਜਾ ਸਕਦਾ ਹੈ
ਹੇਠ ਦਿੱਤੀ ਉਦਾਹਰਨ.

```javascript
const upload = multer({ dest: 'uploads/' })
```

ਜੇਕਰ ਤੁਸੀਂ ਆਪਣੇ ਅੱਪਲੋਡਾਂ 'ਤੇ ਵਧੇਰੇ ਕੰਟਰੋਲ ਚਾਹੁੰਦੇ ਹੋ, ਤਾਂ ਤੁਸੀਂ 'ਸਟੋਰੇਜ' ਦੀ ਵਰਤੋਂ ਕਰਨਾ ਚਾਹੋਗੇ
'dest' ਦੀ ਬਜਾਏ ਵਿਕਲਪ। ਸਟੋਰੇਜ ਇੰਜਣ `ਡਿਸਕ ਸਟੋਰੇਜ` ਵਾਲੇ ਮਲਟਰ ਜਹਾਜ਼
ਅਤੇ 'ਮੈਮੋਰੀ ਸਟੋਰੇਜ'; ਤੀਜੀ ਧਿਰਾਂ ਤੋਂ ਹੋਰ ਇੰਜਣ ਉਪਲਬਧ ਹਨ।

#### `.ਸਿੰਗਲ(ਫੀਲਡਨੇਮ)`

'ਫੀਲਡਨੇਮ' ਨਾਮ ਨਾਲ ਇੱਕ ਸਿੰਗਲ ਫਾਈਲ ਸਵੀਕਾਰ ਕਰੋ। ਸਿੰਗਲ ਫਾਈਲ ਸਟੋਰ ਕੀਤੀ ਜਾਵੇਗੀ
`req.file` ਵਿੱਚ।

#### `.ਐਰੇ(ਫੀਲਡ ਦਾ ਨਾਮ[, maxCount])`

ਫਾਈਲਾਂ ਦੀ ਇੱਕ ਐਰੇ ਨੂੰ ਸਵੀਕਾਰ ਕਰੋ, ਸਾਰੀਆਂ 'ਫੀਲਡਨੇਮ' ਨਾਮ ਨਾਲ। ਵਿਕਲਪਿਕ ਤੌਰ 'ਤੇ ਗਲਤੀ ਹੋ ਜਾਂਦੀ ਹੈ ਜੇ
`maxCount` ਤੋਂ ਵੱਧ ਫ਼ਾਈਲਾਂ ਅੱਪਲੋਡ ਕੀਤੀਆਂ ਗਈਆਂ ਹਨ। ਵਿੱਚ ਫਾਈਲਾਂ ਦੀ ਐਰੇ ਸਟੋਰ ਕੀਤੀ ਜਾਵੇਗੀ
`req.files`।

#### `.ਫੀਲਡਸ(ਫੀਲਡ)`

ਫਾਈਲਾਂ ਦੇ ਮਿਸ਼ਰਣ ਨੂੰ ਸਵੀਕਾਰ ਕਰੋ, 'ਖੇਤਰਾਂ' ਦੁਆਰਾ ਨਿਰਦਿਸ਼ਟ। ਫਾਈਲਾਂ ਦੀਆਂ ਐਰੇ ਨਾਲ ਇੱਕ ਵਸਤੂ
'req.files' ਵਿੱਚ ਸਟੋਰ ਕੀਤਾ ਜਾਵੇਗਾ।

`ਫੀਲਡ` `ਨਾਮ` ਅਤੇ ਵਿਕਲਪਿਕ ਤੌਰ `ਤੇ `maxCount` ਨਾਲ ਵਸਤੂਆਂ ਦੀ ਇੱਕ ਲੜੀ ਹੋਣੀ ਚਾਹੀਦੀ ਹੈ।
ਉਦਾਹਰਨ:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.none()`

ਸਿਰਫ਼ ਟੈਕਸਟ ਖੇਤਰਾਂ ਨੂੰ ਸਵੀਕਾਰ ਕਰੋ। ਜੇਕਰ ਕੋਈ ਫਾਈਲ ਅਪਲੋਡ ਕੀਤੀ ਜਾਂਦੀ ਹੈ, ਤਾਂ ਕੋਡ ਨਾਲ ਗਲਤੀ
"LIMIT\_UNEXPECTED\_FILE" ਜਾਰੀ ਕੀਤੀ ਜਾਵੇਗੀ।

#### `.any()`

ਤਾਰ ਦੇ ਉੱਪਰ ਆਉਣ ਵਾਲੀਆਂ ਸਾਰੀਆਂ ਫਾਈਲਾਂ ਨੂੰ ਸਵੀਕਾਰ ਕਰਦਾ ਹੈ। ਵਿੱਚ ਫਾਈਲਾਂ ਦੀ ਇੱਕ ਐਰੇ ਸਟੋਰ ਕੀਤੀ ਜਾਵੇਗੀ
`req.files`।

**ਚੇਤਾਵਨੀ:** ਯਕੀਨੀ ਬਣਾਓ ਕਿ ਤੁਸੀਂ ਹਮੇਸ਼ਾ ਉਹਨਾਂ ਫਾਈਲਾਂ ਨੂੰ ਸੰਭਾਲਦੇ ਹੋ ਜੋ ਇੱਕ ਉਪਭੋਗਤਾ ਦੁਆਰਾ ਅੱਪਲੋਡ ਕਰਦਾ ਹੈ।
ਮਲਟਰ ਨੂੰ ਕਦੇ ਵੀ ਗਲੋਬਲ ਮਿਡਲਵੇਅਰ ਦੇ ਤੌਰ 'ਤੇ ਸ਼ਾਮਲ ਨਾ ਕਰੋ ਕਿਉਂਕਿ ਇੱਕ ਖਤਰਨਾਕ ਉਪਭੋਗਤਾ ਅਪਲੋਡ ਕਰ ਸਕਦਾ ਹੈ
ਇੱਕ ਰੂਟ ਲਈ ਫਾਈਲਾਂ ਜਿਸਦੀ ਤੁਸੀਂ ਉਮੀਦ ਨਹੀਂ ਕੀਤੀ ਸੀ। ਇਸ ਫੰਕਸ਼ਨ ਦੀ ਵਰਤੋਂ ਸਿਰਫ਼ ਰੂਟਾਂ 'ਤੇ ਕਰੋ
ਜਿੱਥੇ ਤੁਸੀਂ ਅਪਲੋਡ ਕੀਤੀਆਂ ਫਾਈਲਾਂ ਨੂੰ ਸੰਭਾਲ ਰਹੇ ਹੋ।

### `ਸਟੋਰੇਜ`

#### `ਡਿਸਕ ਸਟੋਰੇਜ`

ਡਿਸਕ ਸਟੋਰੇਜ ਇੰਜਣ ਤੁਹਾਨੂੰ ਫਾਈਲਾਂ ਨੂੰ ਡਿਸਕ ਤੇ ਸਟੋਰ ਕਰਨ 'ਤੇ ਪੂਰਾ ਨਿਯੰਤਰਣ ਦਿੰਦਾ ਹੈ।

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

ਇੱਥੇ ਦੋ ਵਿਕਲਪ ਉਪਲਬਧ ਹਨ, 'ਮੰਜ਼ਿਲ' ਅਤੇ 'ਫਾਈਲ ਨਾਮ'। ਉਹ ਦੋਵੇਂ ਹਨ
ਫੰਕਸ਼ਨ ਜੋ ਨਿਰਧਾਰਤ ਕਰਦੇ ਹਨ ਕਿ ਫਾਈਲ ਕਿੱਥੇ ਸਟੋਰ ਕੀਤੀ ਜਾਣੀ ਚਾਹੀਦੀ ਹੈ।

`ਮੰਜ਼ਿਲ` ਦੀ ਵਰਤੋਂ ਇਹ ਨਿਰਧਾਰਤ ਕਰਨ ਲਈ ਕੀਤੀ ਜਾਂਦੀ ਹੈ ਕਿ ਅੱਪਲੋਡ ਕੀਤੀਆਂ ਫ਼ਾਈਲਾਂ ਕਿਸ ਫੋਲਡਰ ਵਿੱਚ ਹੋਣੀਆਂ ਚਾਹੀਦੀਆਂ ਹਨ
ਸਟੋਰ ਕੀਤਾ ਜਾਵੇ। ਇਹ ਇੱਕ `ਸਟਰਿੰਗ` (ਉਦਾਹਰਨ ਲਈ `'/tmp/uploads'`) ਵਜੋਂ ਵੀ ਦਿੱਤਾ ਜਾ ਸਕਦਾ ਹੈ। ਜੇਕਰ ਨਹੀਂ
`ਮੰਜ਼ਿਲ` ਦਿੱਤਾ ਗਿਆ ਹੈ, ਅਸਥਾਈ ਲਈ ਓਪਰੇਟਿੰਗ ਸਿਸਟਮ ਦੀ ਡਿਫੌਲਟ ਡਾਇਰੈਕਟਰੀ
ਫਾਈਲਾਂ ਦੀ ਵਰਤੋਂ ਕੀਤੀ ਜਾਂਦੀ ਹੈ।

**ਨੋਟ:** ਪ੍ਰਦਾਨ ਕਰਦੇ ਸਮੇਂ ਤੁਸੀਂ ਡਾਇਰੈਕਟਰੀ ਬਣਾਉਣ ਲਈ ਜ਼ਿੰਮੇਵਾਰ ਹੋ
ਇੱਕ ਫੰਕਸ਼ਨ ਦੇ ਰੂਪ ਵਿੱਚ `ਮੰਜ਼ਿਲ`। ਇੱਕ ਸਤਰ ਨੂੰ ਪਾਸ ਕਰਦੇ ਸਮੇਂ, ਮਲਟਰ ਇਹ ਯਕੀਨੀ ਬਣਾਵੇਗਾ ਕਿ
ਡਾਇਰੈਕਟਰੀ ਤੁਹਾਡੇ ਲਈ ਬਣਾਈ ਗਈ ਹੈ।

'ਫਾਇਲਨਾਮ' ਦੀ ਵਰਤੋਂ ਇਹ ਨਿਰਧਾਰਤ ਕਰਨ ਲਈ ਕੀਤੀ ਜਾਂਦੀ ਹੈ ਕਿ ਫੋਲਡਰ ਦੇ ਅੰਦਰ ਫਾਈਲ ਦਾ ਨਾਮ ਕੀ ਹੋਣਾ ਚਾਹੀਦਾ ਹੈ।
ਜੇਕਰ ਕੋਈ 'ਫਾਈਲ ਨਾਮ' ਨਹੀਂ ਦਿੱਤਾ ਗਿਆ ਹੈ, ਤਾਂ ਹਰੇਕ ਫਾਈਲ ਨੂੰ ਇੱਕ ਬੇਤਰਤੀਬ ਨਾਮ ਦਿੱਤਾ ਜਾਵੇਗਾ ਜੋ ਨਹੀਂ ਹੈ
ਕਿਸੇ ਵੀ ਫਾਈਲ ਐਕਸਟੈਂਸ਼ਨ ਨੂੰ ਸ਼ਾਮਲ ਕਰੋ।

**ਨੋਟ:** ਮਲਟਰ ਤੁਹਾਡੇ ਲਈ, ਤੁਹਾਡੇ ਫੰਕਸ਼ਨ ਲਈ ਕੋਈ ਫਾਈਲ ਐਕਸਟੈਂਸ਼ਨ ਨਹੀਂ ਜੋੜੇਗਾ
ਇੱਕ ਫਾਈਲ ਐਕਸਟੈਂਸ਼ਨ ਨਾਲ ਪੂਰਾ ਇੱਕ ਫਾਈਲ ਨਾਮ ਵਾਪਸ ਕਰਨਾ ਚਾਹੀਦਾ ਹੈ.

ਹਰੇਕ ਫੰਕਸ਼ਨ ਨੂੰ ਬੇਨਤੀ (`req`) ਅਤੇ ਇਸ ਬਾਰੇ ਕੁਝ ਜਾਣਕਾਰੀ ਦੋਵਾਂ ਪਾਸ ਕੀਤੀ ਜਾਂਦੀ ਹੈ
ਫੈਸਲੇ ਵਿੱਚ ਸਹਾਇਤਾ ਲਈ ਫਾਈਲ (`ਫਾਇਲ`)।

ਨੋਟ ਕਰੋ ਕਿ `req.body` ਅਜੇ ਪੂਰੀ ਤਰ੍ਹਾਂ ਨਾਲ ਭਰਿਆ ਨਹੀਂ ਹੋ ਸਕਦਾ ਹੈ। ਇਹ 'ਤੇ ਨਿਰਭਰ ਕਰਦਾ ਹੈ
ਆਰਡਰ ਕਰੋ ਕਿ ਕਲਾਇੰਟ ਫੀਲਡਾਂ ਅਤੇ ਫਾਈਲਾਂ ਨੂੰ ਸਰਵਰ ਤੇ ਪ੍ਰਸਾਰਿਤ ਕਰੇ।

ਕਾਲਬੈਕ ਵਿੱਚ ਵਰਤੇ ਗਏ ਕਾਲਿੰਗ ਕਨਵੈਨਸ਼ਨ ਨੂੰ ਸਮਝਣ ਲਈ (ਪਾਸ ਕਰਨ ਦੀ ਲੋੜ ਹੈ
ਪਹਿਲੇ ਪਰਮ ਵਜੋਂ null), ਵੇਖੋ
[Node.js ਗਲਤੀ ਹੈਂਡਲਿੰਗ](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### `ਮੈਮੋਰੀ ਸਟੋਰੇਜ`

ਮੈਮੋਰੀ ਸਟੋਰੇਜ ਇੰਜਣ ਮੈਮੋਰੀ ਵਿੱਚ ਫਾਈਲਾਂ ਨੂੰ 'ਬਫਰ' ਆਬਜੈਕਟ ਵਜੋਂ ਸਟੋਰ ਕਰਦਾ ਹੈ। ਇਹ
ਕੋਲ ਕੋਈ ਵਿਕਲਪ ਨਹੀਂ ਹੈ।

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

ਮੈਮੋਰੀ ਸਟੋਰੇਜ ਦੀ ਵਰਤੋਂ ਕਰਦੇ ਸਮੇਂ, ਫਾਈਲ ਜਾਣਕਾਰੀ ਵਿੱਚ ਇੱਕ ਖੇਤਰ ਸ਼ਾਮਲ ਹੋਵੇਗਾ ਜਿਸ ਨੂੰ ਕਿਹਾ ਜਾਂਦਾ ਹੈ
`ਬਫ਼ਰ` ਜਿਸ ਵਿੱਚ ਪੂਰੀ ਫ਼ਾਈਲ ਸ਼ਾਮਲ ਹੁੰਦੀ ਹੈ।

**ਚੇਤਾਵਨੀ**: ਬਹੁਤ ਵੱਡੀਆਂ ਫਾਈਲਾਂ, ਜਾਂ ਮੁਕਾਬਲਤਨ ਛੋਟੀਆਂ ਫਾਈਲਾਂ ਨੂੰ ਵੱਡੀਆਂ ਵਿੱਚ ਅਪਲੋਡ ਕਰਨਾ
ਨੰਬਰ ਬਹੁਤ ਤੇਜ਼ੀ ਨਾਲ, ਤੁਹਾਡੀ ਐਪਲੀਕੇਸ਼ਨ ਦੀ ਮੈਮੋਰੀ ਖਤਮ ਹੋਣ ਦਾ ਕਾਰਨ ਬਣ ਸਕਦੀ ਹੈ
ਮੈਮੋਰੀ ਸਟੋਰੇਜ ਵਰਤੀ ਜਾਂਦੀ ਹੈ।

### `ਸੀਮਾ`

ਹੇਠ ਲਿਖੀਆਂ ਵਿਕਲਪਿਕ ਵਿਸ਼ੇਸ਼ਤਾਵਾਂ ਦੀਆਂ ਆਕਾਰ ਸੀਮਾਵਾਂ ਨੂੰ ਨਿਸ਼ਚਿਤ ਕਰਨ ਵਾਲੀ ਵਸਤੂ। ਮਲਟਰ ਇਸ ਵਸਤੂ ਨੂੰ ਸਿੱਧੇ ਬੱਸਬੁਆਏ ਵਿੱਚ ਭੇਜਦਾ ਹੈ, ਅਤੇ ਵਿਸ਼ੇਸ਼ਤਾਵਾਂ ਦੇ ਵੇਰਵੇ [busboy's page](https://github.com/mscdex/busboy#busboy-methods) 'ਤੇ ਲੱਭੇ ਜਾ ਸਕਦੇ ਹਨ।

ਹੇਠਾਂ ਦਿੱਤੇ ਪੂਰਨ ਅੰਕ ਮੁੱਲ ਉਪਲਬਧ ਹਨ:

ਕੁੰਜੀ | ਵਰਣਨ | ਡਿਫਾਲਟ
--- | --- | ---
`fieldNameSize` | ਅਧਿਕਤਮ ਖੇਤਰ ਦੇ ਨਾਮ ਦਾ ਆਕਾਰ | 100 ਬਾਈਟ
`ਫੀਲਡ ਆਕਾਰ` | ਅਧਿਕਤਮ ਖੇਤਰ ਮੁੱਲ ਦਾ ਆਕਾਰ (ਬਾਈਟ ਵਿੱਚ) | 1MB
`ਖੇਤਰ` | ਗੈਰ-ਫਾਈਲ ਖੇਤਰਾਂ ਦੀ ਅਧਿਕਤਮ ਸੰਖਿਆ | ਅਨੰਤਤਾ
`fileSize` | ਮਲਟੀਪਾਰਟ ਫਾਰਮਾਂ ਲਈ, ਅਧਿਕਤਮ ਫ਼ਾਈਲ ਆਕਾਰ (ਬਾਈਟ ਵਿੱਚ) | ਅਨੰਤਤਾ
'ਫਾਇਲਾਂ' | ਮਲਟੀਪਾਰਟ ਫਾਰਮਾਂ ਲਈ, ਫਾਈਲ ਖੇਤਰਾਂ ਦੀ ਅਧਿਕਤਮ ਸੰਖਿਆ | ਅਨੰਤਤਾ
`ਪੁਰਜੇ` | ਮਲਟੀਪਾਰਟ ਫਾਰਮਾਂ ਲਈ, ਭਾਗਾਂ ਦੀ ਅਧਿਕਤਮ ਸੰਖਿਆ (ਫੀਲਡ + ਫਾਈਲਾਂ) | ਅਨੰਤਤਾ
`ਹੈਡਰਪੇਅਰਸ` | ਮਲਟੀਪਾਰਟ ਫਾਰਮਾਂ ਲਈ, ਹੈਡਰ ਕੁੰਜੀ ਦੀ ਅਧਿਕਤਮ ਸੰਖਿਆ => ਪਾਰਸ ਕਰਨ ਲਈ ਮੁੱਲ ਜੋੜੇ | 2000

ਸੀਮਾਵਾਂ ਨੂੰ ਨਿਸ਼ਚਿਤ ਕਰਨ ਨਾਲ ਤੁਹਾਡੀ ਸਾਈਟ ਨੂੰ ਸੇਵਾ ਤੋਂ ਇਨਕਾਰ (DoS) ਹਮਲਿਆਂ ਤੋਂ ਬਚਾਉਣ ਵਿੱਚ ਮਦਦ ਮਿਲ ਸਕਦੀ ਹੈ।

### `ਫਾਇਲਫਿਲਟਰ`

ਇਸਨੂੰ ਨਿਯੰਤਰਿਤ ਕਰਨ ਲਈ ਇੱਕ ਫੰਕਸ਼ਨ ਵਿੱਚ ਸੈੱਟ ਕਰੋ ਕਿ ਕਿਹੜੀਆਂ ਫਾਈਲਾਂ ਅੱਪਲੋਡ ਕੀਤੀਆਂ ਜਾਣੀਆਂ ਚਾਹੀਦੀਆਂ ਹਨ ਅਤੇ ਕਿਹੜੀਆਂ
ਛੱਡ ਦਿੱਤਾ ਜਾਣਾ ਚਾਹੀਦਾ ਹੈ। ਫੰਕਸ਼ਨ ਇਸ ਤਰ੍ਹਾਂ ਦਿਖਾਈ ਦੇਣਾ ਚਾਹੀਦਾ ਹੈ:

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

## ਅਸ਼ੁੱਧੀ ਨੂੰ ਸੰਭਾਲਣਾ

ਜਦੋਂ ਕੋਈ ਗਲਤੀ ਆਉਂਦੀ ਹੈ, ਤਾਂ ਮਲਟਰ ਗਲਤੀ ਨੂੰ ਐਕਸਪ੍ਰੈਸ ਨੂੰ ਸੌਂਪ ਦੇਵੇਗਾ। ਤੁਸੀਂ ਕਰ ਸਕਦੇ ਹੋ
[ਸਟੈਂਡਰਡ ਐਕਸਪ੍ਰੈਸ ਵੇ] (http://expressjs.com/guide/error-handling.html) ਦੀ ਵਰਤੋਂ ਕਰਦੇ ਹੋਏ ਇੱਕ ਵਧੀਆ ਗਲਤੀ ਪੰਨਾ ਪ੍ਰਦਰਸ਼ਿਤ ਕਰੋ।

ਜੇ ਤੁਸੀਂ ਖਾਸ ਤੌਰ 'ਤੇ ਮਲਟਰ ਤੋਂ ਗਲਤੀਆਂ ਨੂੰ ਫੜਨਾ ਚਾਹੁੰਦੇ ਹੋ, ਤਾਂ ਤੁਸੀਂ ਕਾਲ ਕਰ ਸਕਦੇ ਹੋ
ਮਿਡਲਵੇਅਰ ਫੰਕਸ਼ਨ ਆਪਣੇ ਆਪ। ਨਾਲ ਹੀ, ਜੇਕਰ ਤੁਸੀਂ ਸਿਰਫ਼ [ਮੁਲਟਰ ਤਰੁਟੀਆਂ](https://github.com/expressjs/multer/blob/main/lib/multer-error.js) ਨੂੰ ਫੜਨਾ ਚਾਹੁੰਦੇ ਹੋ, ਤਾਂ ਤੁਸੀਂ `MulterError` ਕਲਾਸ ਦੀ ਵਰਤੋਂ ਕਰ ਸਕਦੇ ਹੋ ਜੋ ਖੁਦ `multer` ਵਸਤੂ ਨਾਲ ਜੁੜੀ ਹੋਈ ਹੈ (ਉਦਾਹਰਨ ਲਈ `multer ਦੀ ਗਲਤੀ ਜਾਂ Multer`)।

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

## ਕਸਟਮ ਸਟੋਰੇਜ ਇੰਜਣ

ਆਪਣਾ ਸਟੋਰੇਜ ਇੰਜਣ ਕਿਵੇਂ ਬਣਾਉਣਾ ਹੈ ਇਸ ਬਾਰੇ ਜਾਣਕਾਰੀ ਲਈ, [ਮਲਟਰ ਸਟੋਰੇਜ ਇੰਜਣ](https://github.com/expressjs/multer/blob/main/StorageEngine.md) ਦੇਖੋ।

## ਲਾਇਸੰਸ

[ਮੇਰਾ](ਲਾਈਸੈਂਸ)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
