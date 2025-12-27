# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

مولتر یک میان افزار node.js برای مدیریت «multipart/form-data» است که در درجه اول برای آپلود فایل ها استفاده می شود. نوشته شده است
در بالای [busboy] (https://github.com/mscdex/busboy) برای حداکثر کارایی.

**توجه**: مولتر هیچ فرمی را که چند قسمتی نباشد پردازش نمی کند («چند بخشی/فرم-داده»).

## ترجمه ها

این README به زبان های دیگر نیز موجود است:

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

## نصب

```sh
$ npm install multer
```

## استفاده

مولتر یک شی «body» و یک شی «file» یا «files» را به شی «درخواست» اضافه می کند. شی «body» حاوی مقادیر فیلدهای متنی فرم است، شی «فایل» یا «فایل» حاوی فایل‌های آپلود شده از طریق فرم است.

مثال استفاده پایه:

"enctype="multipart/form-data"" را در فرم خود فراموش نکنید.

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

در صورتی که نیاز دارید یک فرم چند قسمتی فقط متنی را مدیریت کنید، باید از روش «.none()» استفاده کنید:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

در اینجا مثالی در مورد نحوه استفاده از مولتر در فرم HTML آورده شده است. به فیلدهای "enctype="multipart/form-data""" و "name="uploaded_file"" توجه ویژه داشته باشید:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

سپس در فایل جاوا اسکریپت خود این خطوط را برای دسترسی به فایل و بدنه اضافه می کنید. مهم است که از مقدار فیلد «نام» از فرم در تابع آپلود خود استفاده کنید. این به multer می گوید که در کدام فیلد در درخواست باید به دنبال فایل ها بگردد. اگر این فیلدها در فرم HTML و سرور شما یکسان نباشند، آپلود شما با شکست مواجه می شود:

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

### اطلاعات فایل

هر فایل حاوی اطلاعات زیر است:

کلید | توضیحات | توجه داشته باشید
--- | --- | ---
"نام فیلد" | نام فیلد مشخص شده در فرم |
"نام اصلی" | نام فایل در کامپیوتر کاربر |
"رمزگذاری" | نوع کدگذاری فایل |
`mimettype` | نوع Mime فایل |
"اندازه" | حجم فایل بر حسب بایت |
"مقصد" | پوشه ای که فایل در آن ذخیره شده است | "DiskStorage".
"نام فایل" | نام فایل در "مقصد" | "DiskStorage".
"مسیر" | مسیر کامل فایل آپلود شده | "DiskStorage".
"بافر" | یک "بافر" از کل فایل | "ذخیره حافظه".

### «multer(opts)».

مولتر یک شی گزینه را می پذیرد که ابتدایی ترین آن "dest" است
ویژگی، که به مولتر می‌گوید کجا فایل‌ها را آپلود کند. در صورت حذف
شی گزینه ها، فایل ها در حافظه نگهداری می شوند و هرگز روی دیسک نوشته نمی شوند.

به‌طور پیش‌فرض، مولتر نام فایل‌ها را تغییر می‌دهد تا از تداخل نام‌گذاری جلوگیری شود. را
تابع تغییر نام را می توان با توجه به نیازهای شما سفارشی کرد.

در زیر گزینه هایی وجود دارد که می توانند به مولتر منتقل شوند.

کلید | توضیحات
--- | ---
"دستگاه" یا "ذخیره" | محل ذخیره فایل ها
"fileFilter" | تابعی برای کنترل فایل هایی که پذیرفته می شوند
"محدودیت" | محدودیت های داده های آپلود شده
`preservePath` | مسیر کامل فایل ها را به جای فقط نام پایه نگه دارید

در یک برنامه وب متوسط، ممکن است فقط «dest» مورد نیاز باشد و همانطور که در نشان داده شده است پیکربندی شود
مثال زیر

```javascript
const upload = multer({ dest: 'uploads/' })
```

اگر می‌خواهید کنترل بیشتری روی آپلودهای خود داشته باشید، باید از «ذخیره‌سازی» استفاده کنید
گزینه به جای "dest". کشتی ها را با موتورهای ذخیره سازی «DiskStorage» مولتی کنید
و "MemoryStorage"؛ موتورهای بیشتری از اشخاص ثالث در دسترس هستند.

#### `.single(name)`

یک فایل با نام "فیلد نام" را بپذیرید. تک فایل ذخیره خواهد شد
در `req.file`.

#### آرایه (نام فیلد[, maxCount])».

آرایه‌ای از فایل‌ها را بپذیرید، همه با نام «فیلدنام». به صورت اختیاری error out if
بیش از فایل های «maxCount» آپلود شده است. آرایه فایل ها در آن ذخیره می شود
"ref.files".

#### «فیلدها(فیلدها)».

ترکیبی از فایل‌ها را که با «فیلدها» مشخص شده‌اند، بپذیرید. یک شی با آرایه ای از فایل ها
در `req.files` ذخیره خواهد شد.

"فیلدها" باید آرایه ای از اشیاء با "name" و در صورت اختیاری "maxCount" باشد.
مثال:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.none()`

فقط فیلدهای متنی را بپذیرید. اگر هر فایلی آپلود شد، خطا در کد
"LIMIT\_UNEXPECTED\_FILE" صادر خواهد شد.

#### «.any()».

تمام فایل هایی که از طریق سیم می آیند را می پذیرد. آرایه ای از فایل ها در آن ذخیره می شود
"ref.files".

**هشدار:** مطمئن شوید که همیشه فایل هایی را که کاربر آپلود می کند مدیریت می کنید.
هرگز multer را به عنوان میان افزار جهانی اضافه نکنید زیرا یک کاربر مخرب می تواند آپلود کند
فایل ها به مسیری که پیش بینی نکرده بودید. از این عملکرد فقط در مسیرها استفاده کنید
جایی که فایل های آپلود شده را مدیریت می کنید.

### "ذخیره سازی".

#### "DiskStorage".

موتور ذخیره سازی دیسک به شما کنترل کامل روی ذخیره فایل ها روی دیسک را می دهد.

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

دو گزینه در دسترس است، "مقصد" و "نام فایل". آنها هر دو هستند
توابعی که تعیین می کنند فایل در کجا باید ذخیره شود.

«مقصد» برای تعیین اینکه فایل‌های آپلود شده باید در کدام پوشه باشد استفاده می‌شود
ذخیره شود. این همچنین می‌تواند به‌عنوان یک «رشته» (به عنوان مثال «»/tmp/uploads»» ارائه شود. اگر نه
"مقصد" داده شده است، فهرست راهنمای پیش فرض سیستم عامل برای موقت
فایل ها استفاده می شود.

**توجه:** شما مسئول ایجاد دایرکتوری هنگام ارائه هستید
"مقصد" به عنوان یک تابع. هنگام عبور یک رشته، multer از آن اطمینان حاصل می کند
دایرکتوری برای شما ایجاد شده است.

"نام فایل" برای تعیین نام فایل در داخل پوشه استفاده می شود.
اگر "نام فایل" داده نشده باشد، به هر فایل یک نام تصادفی داده می شود که این نام را ندارد
شامل هر پسوند فایل

**توجه:** مولتر هیچ پسوند فایلی را برای شما، تابع شما اضافه نمی کند
باید یک نام فایل کامل با پسوند فایل را برگرداند.

هر تابع هم درخواست (`req`) و هم اطلاعاتی در مورد آن ارسال می شود
پرونده ('پرونده') برای کمک به تصمیم گیری.

توجه داشته باشید که «req.body» ممکن است هنوز به طور کامل پر نشده باشد. بستگی به
دستور دهید که مشتری فیلدها و فایل ها را به سرور ارسال کند.

برای درک قرارداد تماس استفاده شده در تماس برگشتی (نیاز به عبور
null به عنوان اولین پارامتر)، مراجعه کنید
[کنترل خطای Node.js](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### "MemoryStorage".

موتور ذخیره‌سازی حافظه فایل‌ها را به‌عنوان اشیاء «بافر» در حافظه ذخیره می‌کند. آن را
هیچ گزینه ای ندارد

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

هنگام استفاده از حافظه، اطلاعات فایل حاوی فیلدی به نام خواهد بود
"بافر" که شامل کل فایل است.

**هشدار**: آپلود فایل های بسیار بزرگ، یا فایل های نسبتا کوچک در حجم بالا
اعداد بسیار سریع، می توانند باعث شوند که حافظه برنامه شما تمام شود
حافظه ذخیره سازی استفاده می شود.

### "محدودیت".

یک شی که محدودیت اندازه ویژگی های اختیاری زیر را مشخص می کند. مولتر این شی را مستقیماً به busboy می‌دهد، و جزئیات ویژگی‌ها را می‌توانید در [صفحه busboy] (https://github.com/mscdex/busboy#busboy-methods) پیدا کنید.

مقادیر صحیح زیر موجود است:

کلید | توضیحات | پیش فرض
--- | --- | ---
"FieldNameSize" | حداکثر اندازه نام فیلد | 100 بایت
"اندازه فیلد" | حداکثر اندازه مقدار فیلد (به بایت) | 1 مگابایت
"فیلدها" | حداکثر تعداد فیلدهای غیر فایل | بی نهایت
"اندازه فایل" | برای فرم های چند بخشی، حداکثر اندازه فایل (بر حسب بایت) | بی نهایت
"فایل" | برای فرم های چند قسمتی، حداکثر تعداد فیلدهای فایل | بی نهایت
"قطعات" | برای فرم های چند قسمتی، حداکثر تعداد قسمت ها (فیلدها + فایل ها) | بی نهایت
`HeaderPairs` | برای فرم‌های چند بخشی، حداکثر تعداد کلید هدر=>جفت ارزش برای تجزیه | 2000

تعیین محدودیت ها می تواند به محافظت از سایت شما در برابر حملات انکار سرویس (DoS) کمک کند.

### "fileFilter".

این را روی تابعی تنظیم کنید تا کنترل کنید کدام فایل ها و کدام فایل ها باید آپلود شوند
باید از آن گذشت. تابع باید به شکل زیر باشد:

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

## رسیدگی به خطا

هنگام مواجه شدن با خطا، مولتر خطا را به Express محول می کند. شما می توانید
یک صفحه خطای خوب را با استفاده از [راه استاندارد سریع] (http://expressjs.com/guide/error-handling.html) نمایش دهید.

اگر می‌خواهید خطاها را به طور خاص از مولتر دریافت کنید، می‌توانید با آن تماس بگیرید
عملکرد میان افزار توسط خودتان همچنین، اگر می‌خواهید فقط [خطاهای Multer] (https://github.com/expressjs/multer/blob/main/lib/multer-error.js) را بگیرید، می‌توانید از کلاس «MulterError» که به خود شی «multer» متصل است (مثلاً «er instanceof multer.Multer») استفاده کنید.

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

## موتور ذخیره سازی سفارشی

برای اطلاعات در مورد نحوه ساخت موتور ذخیره سازی خود، به [Multer Storage Engine] (https://github.com/expressjs/multer/blob/main/StorageEngine.md) مراجعه کنید.

## مجوز

[MY](LICENSE)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
