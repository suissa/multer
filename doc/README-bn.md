# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

মাল্টার হল `মাল্টিপার্ট/ফর্ম-ডেটা` পরিচালনার জন্য একটি node.js মিডলওয়্যার, যা প্রাথমিকভাবে ফাইল আপলোড করার জন্য ব্যবহৃত হয়। লেখা আছে
সর্বাধিক দক্ষতার জন্য [busboy](https://github.com/mscdex/busboy) এর উপরে।

**দ্রষ্টব্য**: মাল্টার এমন কোনো ফর্ম প্রক্রিয়া করবে না যা মাল্টিপার্ট নয় (`মাল্টিপার্ট/ফর্ম-ডেটা`)।

## অনুবাদ

এই README অন্যান্য ভাষায়ও উপলব্ধ:

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

## ইনস্টলেশন

```sh
$ npm install multer
```

## ব্যবহার

মাল্টার একটি `বডি` অবজেক্ট এবং `ফাইল` বা `ফাইল` অবজেক্টকে `রিকোয়েস্ট` অবজেক্টে যোগ করে। `বডি` অবজেক্টে ফর্মের টেক্সট ফিল্ডের মান থাকে, `ফাইল` বা `ফাইল` অবজেক্টে ফর্মের মাধ্যমে আপলোড করা ফাইল থাকে।

মৌলিক ব্যবহারের উদাহরণ:

আপনার ফর্মে `enctype="multipart/form-data"` ভুলে যাবেন না।

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

যদি আপনাকে একটি টেক্সট-অনলি মাল্টিপার্ট ফর্ম পরিচালনা করতে হয়, তাহলে আপনাকে `.none()` পদ্ধতি ব্যবহার করতে হবে:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

এইচটিএমএল আকারে মাল্টার কীভাবে ব্যবহার করা হয় তার একটি উদাহরণ এখানে। `enctype="multipart/form-data"` এবং `name="uploaded_file"` ক্ষেত্রগুলির বিশেষ নোট নিন:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

তারপর আপনার জাভাস্ক্রিপ্ট ফাইলে আপনি ফাইল এবং বডি উভয় অ্যাক্সেস করতে এই লাইনগুলি যুক্ত করবেন। এটি গুরুত্বপূর্ণ যে আপনি আপনার আপলোড ফাংশনে ফর্ম থেকে `নাম` ফিল্ডের মান ব্যবহার করুন৷ এটি মাল্টারকে বলে যে অনুরোধের কোন ক্ষেত্রটিতে ফাইলগুলি সন্ধান করা উচিত৷ যদি এই ক্ষেত্রগুলি HTML ফর্মে এবং আপনার সার্ভারে একই না হয় তবে আপনার আপলোড ব্যর্থ হবে:

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

### ফাইল তথ্য

প্রতিটি ফাইলে নিম্নলিখিত তথ্য রয়েছে:

কী | বর্ণনা | দ্রষ্টব্য
--- | --- | ---
`ক্ষেত্রের নাম` | ফর্মে নির্দিষ্ট ক্ষেত্রের নাম |
`মূল নাম` | ব্যবহারকারীর কম্পিউটারে ফাইলের নাম |
`এনকোডিং` | ফাইলের এনকোডিং প্রকার |
`মাইমেটাইপ` | ফাইলের মাইম প্রকার |
`আকার` | ফাইলের আকার বাইটে |
`গন্তব্য` | যে ফোল্ডারে ফাইলটি সেভ করা হয়েছে | 'ডিস্ক স্টোরেজ'
`ফাইলের নাম` | `গন্তব্য` এর মধ্যে থাকা ফাইলের নাম 'ডিস্ক স্টোরেজ'
`পথ` | আপলোড করা ফাইলের সম্পূর্ণ পথ | 'ডিস্ক স্টোরেজ'
`বাফার` | সম্পূর্ণ ফাইলের একটি `বাফার` | 'মেমোরি স্টোরেজ'

### `multer(opts)`

মাল্টার একটি অপশন অবজেক্ট গ্রহণ করে, যার মধ্যে সবচেয়ে মৌলিক হল 'ডেস্ট'
সম্পত্তি, যা মাল্টারকে বলে যে ফাইলগুলি কোথায় আপলোড করতে হবে। যদি আপনি বাদ দেন
অপশন অবজেক্ট, ফাইলগুলি মেমরিতে রাখা হবে এবং কখনই ডিস্কে লেখা হবে না।

ডিফল্টরূপে, মাল্টার ফাইলগুলির নাম পরিবর্তন করবে যাতে নামকরণের দ্বন্দ্ব এড়ানো যায়। দ
পুনঃনামকরণ ফাংশন আপনার প্রয়োজন অনুযায়ী কাস্টমাইজ করা যেতে পারে।

নিম্নলিখিত বিকল্পগুলি যেগুলি মাল্টারে প্রেরণ করা যেতে পারে।

কী | বর্ণনা
--- | ---
`dest` বা `storage` | যেখানে ফাইল সংরক্ষণ করতে হবে
`ফাইলফিল্টার` | কোন ফাইল গ্রহণ করা হয় তা নিয়ন্ত্রণ করার ফাংশন
`সীমা` | আপলোড করা ডেটার সীমা
`সংরক্ষণপথ` | শুধুমাত্র বেস নামের পরিবর্তে ফাইলগুলির সম্পূর্ণ পাথ রাখুন

একটি গড় ওয়েব অ্যাপ্লিকেশানে, শুধুমাত্র `dest` প্রয়োজন হতে পারে এবং এতে দেখানো মত কনফিগার করা হতে পারে
নিম্নলিখিত উদাহরণ।

```javascript
const upload = multer({ dest: 'uploads/' })
```

আপনি যদি আপনার আপলোডগুলির উপর আরও নিয়ন্ত্রণ চান তবে আপনি `স্টোরেজ` ব্যবহার করতে চাইবেন
`dest` এর পরিবর্তে বিকল্প। স্টোরেজ ইঞ্জিন 'ডিস্ক স্টোরেজ' সহ মাল্টার জাহাজ
এবং 'মেমোরি স্টোরেজ'; তৃতীয় পক্ষ থেকে আরও ইঞ্জিন পাওয়া যায়।

#### `.একক (ক্ষেত্রের নাম)`

`ক্ষেত্রের নাম` নামের একটি একক ফাইল গ্রহণ করুন। একক ফাইল সংরক্ষণ করা হবে
`req.file`-এ।

#### `.array(ক্ষেত্রের নাম[, maxCount])`

ফাইলের একটি অ্যারে গ্রহণ করুন, সবগুলোই `ক্ষেত্রের নাম` নামের সাথে। ঐচ্ছিকভাবে ত্রুটি আউট যদি
`maxCount` এর বেশি ফাইল আপলোড করা হয়েছে। ফাইলের অ্যারে সংরক্ষণ করা হবে
`req.files`।

#### `.ক্ষেত্র(ক্ষেত্র)`

ফাইলের মিশ্রণ গ্রহণ করুন, 'ক্ষেত্র' দ্বারা নির্দিষ্ট করা। ফাইলের অ্যারে সহ একটি বস্তু
`req.files` এ সংরক্ষণ করা হবে।

`ক্ষেত্র` `নাম` এবং ঐচ্ছিকভাবে `maxCount` সহ অবজেক্টের একটি বিন্যাস হওয়া উচিত।
উদাহরণ:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.none()`

শুধুমাত্র টেক্সট ক্ষেত্র গ্রহণ করুন. কোনো ফাইল আপলোড করা হলে, কোড সহ ত্রুটি
"LIMIT\_UNEXPECTED\_FILE" জারি করা হবে৷

#### `.any()`

তারের উপরে আসা সমস্ত ফাইল গ্রহণ করে। ফাইলের একটি অ্যারে সংরক্ষণ করা হবে
`req.files`।

**সতর্কতা:** নিশ্চিত করুন যে আপনি সর্বদা একটি ব্যবহারকারী আপলোড করা ফাইলগুলি পরিচালনা করেন৷
বিশ্বব্যাপী মিডলওয়্যার হিসাবে মাল্টারকে কখনই যুক্ত করবেন না যেহেতু একজন দূষিত ব্যবহারকারী আপলোড করতে পারে
এমন একটি রুটে ফাইল যা আপনি প্রত্যাশা করেননি। শুধুমাত্র রুটে এই ফাংশন ব্যবহার করুন
যেখানে আপনি আপলোড করা ফাইলগুলি পরিচালনা করছেন।

### `সঞ্চয়স্থান`

#### `ডিস্ক স্টোরেজ`

ডিস্ক স্টোরেজ ইঞ্জিন আপনাকে ডিস্কে ফাইল সংরক্ষণের সম্পূর্ণ নিয়ন্ত্রণ দেয়।

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

দুটি বিকল্প উপলব্ধ আছে, `গন্তব্য` এবং `ফাইলের নাম`। তারা উভয়
ফাংশন যা নির্ধারণ করে যে ফাইলটি কোথায় সংরক্ষণ করা উচিত।

আপলোড করা ফাইলগুলি কোন ফোল্ডারের মধ্যে থাকবে তা নির্ধারণ করতে `গন্তব্য` ব্যবহার করা হয়
সংরক্ষণ করা এটি একটি `স্ট্রিং` হিসাবেও দেওয়া যেতে পারে (যেমন `'/tmp/uploads'`)। যদি না
`গন্তব্য` দেওয়া হয়, অপারেটিং সিস্টেমের অস্থায়ী ডিফল্ট ডিরেক্টরি
ফাইল ব্যবহার করা হয়।

**দ্রষ্টব্য:** প্রদান করার সময় আপনি ডিরেক্টরি তৈরি করার জন্য দায়ী
একটি ফাংশন হিসাবে `গন্তব্য`। একটি স্ট্রিং পাস করার সময়, মাল্টার এটি নিশ্চিত করবে
ডিরেক্টরিটি আপনার জন্য তৈরি করা হয়েছে।

ফোল্ডারের ভিতরে ফাইলটির নাম কী রাখা উচিত তা নির্ধারণ করতে `ফাইলের নাম` ব্যবহার করা হয়।
যদি কোনো ফাইলের নাম না দেওয়া হয়, তাহলে প্রতিটি ফাইলকে একটি এলোমেলো নাম দেওয়া হবে যা নেই
যেকোনো ফাইল এক্সটেনশন অন্তর্ভুক্ত করুন।

**দ্রষ্টব্য:** মাল্টার আপনার, আপনার ফাংশনের জন্য কোনো ফাইল এক্সটেনশন যুক্ত করবে না
একটি ফাইল এক্সটেনশন সহ সম্পূর্ণ একটি ফাইলের নাম ফেরত দেওয়া উচিত।

প্রতিটি ফাংশন অনুরোধ (`req`) এবং কিছু তথ্য উভয়ই পাস করে
ফাইল (`ফাইল`) সিদ্ধান্তে সহায়তা করতে।

মনে রাখবেন যে `req.body` এখনও সম্পূর্ণভাবে জনবহুল নাও হতে পারে। এটা নির্ভর করে
অর্ডার করুন যে ক্লায়েন্ট সার্ভারে ক্ষেত্র এবং ফাইল প্রেরণ করে।

কলব্যাকে ব্যবহৃত কলিং কনভেনশন বোঝার জন্য (পাস করতে হবে
প্রথম প্যারাম হিসাবে null), উল্লেখ করুন
[Node.js এরর হ্যান্ডলিং](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### `মেমোরি স্টোরেজ`

মেমরি স্টোরেজ ইঞ্জিন মেমরিতে ফাইলগুলিকে `বাফার` অবজেক্ট হিসেবে সংরক্ষণ করে। এটা
কোন বিকল্প নেই।

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

মেমরি স্টোরেজ ব্যবহার করার সময়, ফাইল ইনফো নামক একটি ক্ষেত্র থাকবে
`বাফার` যা সম্পূর্ণ ফাইল ধারণ করে।

**সতর্কতা**: খুব বড় ফাইল আপলোড করা, বা বড় আকারে অপেক্ষাকৃত ছোট ফাইল
সংখ্যাগুলি খুব দ্রুত, আপনার অ্যাপ্লিকেশনের মেমরি ফুরিয়ে যেতে পারে
মেমরি স্টোরেজ ব্যবহার করা হয়।

### `সীমা`

নিম্নলিখিত ঐচ্ছিক বৈশিষ্ট্যের আকার সীমা নির্দিষ্ট করে একটি বস্তু। মাল্টার এই বস্তুটিকে সরাসরি বাসবয়-এ পাঠায় এবং বৈশিষ্ট্যের বিশদ বিবরণ [busboy-এর পৃষ্ঠা](https://github.com/mscdex/busboy#busboy-methods) এ পাওয়া যাবে।

নিম্নলিখিত পূর্ণসংখ্যা মান উপলব্ধ:

কী | বর্ণনা | ডিফল্ট
--- | --- | ---
`ক্ষেত্রের নাম আকার` | সর্বোচ্চ ক্ষেত্রের নামের আকার | 100 বাইট
`ক্ষেত্রের আকার` | সর্বোচ্চ ক্ষেত্রের মান আকার (বাইটে) | 1 এমবি
`ক্ষেত্র` | নন-ফাইল ক্ষেত্রের সর্বাধিক সংখ্যা | অনন্ত
`ফাইল সাইজ` | মাল্টিপার্ট ফর্মের জন্য, সর্বোচ্চ ফাইলের আকার (বাইটে) | অনন্ত
`ফাইল` | মাল্টিপার্ট ফর্মের জন্য, ফাইল ফিল্ডের সর্বোচ্চ সংখ্যা | অনন্ত
`অংশ` | মাল্টিপার্ট ফর্মের জন্য, অংশের সর্বাধিক সংখ্যা (ক্ষেত্র + ফাইল) | অনন্ত
`হেডার জোড়া` | মাল্টিপার্ট ফর্মের জন্য, হেডার কী=>পার্স করার জন্য মান জোড়ার সর্বাধিক সংখ্যা | 2000

সীমা নির্দিষ্ট করা আপনার সাইটকে পরিষেবা অস্বীকার (DoS) আক্রমণ থেকে রক্ষা করতে সাহায্য করতে পারে।

### `ফাইল ফিল্টার`

কোন ফাইল আপলোড করা উচিত এবং কোনটি নিয়ন্ত্রণ করতে এটিকে একটি ফাংশনে সেট করুন৷
বাদ দেওয়া উচিত। ফাংশন এই মত হওয়া উচিত:

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

## ত্রুটি হ্যান্ডলিং

একটি ত্রুটির সম্মুখীন হলে, Multer ত্রুটিটি এক্সপ্রেসকে অর্পণ করবে। আপনি পারেন
[স্ট্যান্ডার্ড এক্সপ্রেস উপায়](http://expressjs.com/guide/error-handling.html) ব্যবহার করে একটি সুন্দর ত্রুটি পৃষ্ঠা প্রদর্শন করুন।

আপনি যদি মাল্টার থেকে বিশেষভাবে ত্রুটিগুলি ধরতে চান তবে আপনি কল করতে পারেন
মিডলওয়্যার ফাংশন নিজের দ্বারা। এছাড়াও, আপনি যদি শুধুমাত্র [Multer errors](https://github.com/expressjs/multer/blob/main/lib/multer-error.js) ধরতে চান, তাহলে আপনি `multer` অবজেক্টের সাথে সংযুক্ত `MulterError` ক্লাস ব্যবহার করতে পারেন (যেমন `multer এর err instance of multer)।

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

## কাস্টম স্টোরেজ ইঞ্জিন

আপনার নিজের স্টোরেজ ইঞ্জিন কীভাবে তৈরি করবেন সে সম্পর্কে তথ্যের জন্য, [Multer Storage Engine](https://github.com/expressjs/multer/blob/main/StorageEngine.md) দেখুন।

## লাইসেন্স

[আমার](লাইসেন্স)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
