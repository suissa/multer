# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

Multer — це проміжне програмне забезпечення node.js для обробки `multipart/form-data`, яке в основному використовується для завантаження файлів. Написано
поверх [busboy](https://github.com/mscdex/busboy) для максимальної ефективності.

**ПРИМІТКА**: Multer не оброблятиме жодну форму, яка не є складовою (`multipart/form-data`).

## Переклади

Цей README також доступний іншими мовами:

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

## Встановлення

```sh
$ npm install multer
```

## Використання

Multer додає об’єкт `body` і об’єкт `file` або `files` до об’єкта `request`. Об’єкт `body` містить значення текстових полів форми, об’єкт `file` або `files` містить файли, завантажені через форму.

Основний приклад використання:

Не забудьте `enctype="multipart/form-data"` у вашій формі.

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

Якщо вам потрібно обробити лише текстову багатокомпонентну форму, вам слід скористатися методом `.none()`:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

Ось приклад використання multer у формі HTML. Зверніть особливу увагу на поля `enctype="multipart/form-data"` і `name="uploaded_file"`:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

Потім у ваш файл javascript ви повинні додати ці рядки для доступу як до файлу, так і до тіла. Важливо використовувати значення поля `name` з форми у функції завантаження. Це вказує multer, у якому полі запиту слід шукати файли. Якщо ці поля не збігаються у формі HTML і на вашому сервері, ваше завантаження не вдасться:

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

### Інформація про файл

Кожен файл містить таку інформацію:

Ключ | Опис | Примітка
--- | --- | ---
`назва поля` | Назва поля, зазначена у формі |
`оригінальна назва` | Ім'я файлу на комп'ютері користувача |
`кодування` | Тип кодування файлу |
`mimetype` | Тип файлу Mime |
`розмір` | Розмір файлу в байтах |
`пункт призначення` | Папка, до якої збережено файл | `DiskStorage`
`назва файлу` | Ім'я файлу в полі `destination` | `DiskStorage`
`шлях` | Повний шлях до завантаженого файлу | `DiskStorage`
`буфер` | `Буфер` всього файлу | `MemoryStorage`

### `multer(opts)`

Multer приймає об’єкт параметрів, основним з яких є `dest`
властивість, яка повідомляє Multer, куди завантажувати файли. Якщо ви опустите
об’єкт параметрів, файли зберігатимуться в пам’яті й ніколи не записуватимуться на диск.

За замовчуванням Multer перейменує файли, щоб уникнути конфліктів імен. The
Функцію перейменування можна налаштувати відповідно до ваших потреб.

Нижче наведено параметри, які можна передати Multer.

Ключ | опис
--- | ---
`dest` або `storage` | Де зберігати файли
`fileFilter` | Функція контролю того, які файли приймаються
`ліміти` | Обмеження завантажуваних даних
`preservePath` | Зберігайте повний шлях до файлів, а не лише основну назву

У звичайній веб-програмі може знадобитися лише `dest` і налаштувати його, як показано в
наступний приклад.

```javascript
const upload = multer({ dest: 'uploads/' })
```

Якщо ви хочете більше контролювати свої завантаження, ви захочете використовувати `сховище`
параметр замість `dest`. Multer поставляється з механізмами зберігання даних `DiskStorage`
і `MemoryStorage`; Інші двигуни доступні від сторонніх розробників.

#### `.single(назва поля)`

Прийняти один файл із назвою `fieldname`. Буде збережено один файл
у `req.file`.

#### `.array(fieldname[, maxCount])`

Прийняти масив файлів, усі з іменем `fieldname`. Необов'язково помилка if
завантажено більше ніж `maxCount` файлів. Масив файлів зберігатиметься в
`req.files`.

#### `.fields(поля)`

Приймати суміш файлів, визначених `полями`. Об'єкт з масивами файлів
буде збережено в `req.files`.

`fields` має бути масивом об’єктів з `name` і, можливо, `maxCount`.
приклад:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.none()`

Приймати лише текстові поля. Якщо завантажується будь-який файл, виникає помилка з кодом
Буде видано "LIMIT\_UNEXPECTED\_FILE".

#### `.any()`

Приймає всі файли, які надходять по дроту. Буде збережено масив файлів
`req.files`.

**УВАГА:** Переконайтеся, що ви завжди обробляєте файли, які завантажує користувач.
Ніколи не додавайте multer як глобальне проміжне програмне забезпечення, оскільки зловмисник може завантажити
файли за маршрутом, який ви не передбачали. Використовуйте цю функцію лише на маршрутах
де ви обробляєте завантажені файли.

### `сховище`

#### `DiskStorage`

Механізм зберігання дисків дає вам повний контроль над збереженням файлів на диску.

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

Доступні два параметри: `destination` і `filename`. Вони обидва
функції, які визначають місце зберігання файлу.

`destination` використовується для визначення теки, у якій мають бути завантажені файли
зберігатися. Це також можна вказати як «рядок» (наприклад, «/tmp/uploads»). Якщо ні
`destination` вказано, типовий каталог операційної системи для тимчасового використання
використовуються файли.

**Примітка.** Ви несете відповідальність за створення каталогу під час надання
`destination` як функція. Під час передачі рядка multer переконається в цьому
каталог створений для вас.

`filename` використовується для визначення імені файлу в папці.
Якщо `назви файлу` не вказано, кожному файлу буде присвоєно випадкову назву, яка цього не робить
включити будь-яке розширення файлу.

**Примітка:** Multer не додаватиме розширення файлу для вас, вашої функції
має повертати назву файлу разом із розширенням файлу.

Кожній функції передається як запит (`req`), так і деяка інформація про
файл (`file`), щоб допомогти прийняти рішення.

Зауважте, що `req.body` може бути ще не повністю заповненим. Це залежить від
щоб клієнт передавав поля та файли на сервер.

Щоб зрозуміти угоду про виклики, яка використовується у зворотному виклику (необхідно передати
null як перший параметр), див
[Обробка помилок Node.js](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### `MemoryStorage`

Механізм зберігання пам’яті зберігає файли в пам’яті як об’єкти «Буфер». Це
не має жодних варіантів.

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

У разі використання пам’яті інформація про файл міститиме поле під назвою
`buffer`, який містить весь файл.

**ПОПЕРЕДЖЕННЯ**: завантаження дуже великих файлів або відносно невеликих файлів у великих розмірах
чисел дуже швидко, може спричинити брак пам’яті програми, коли
використовується пам'ять.

### `ліміти`

Об’єкт, що визначає обмеження розміру наступних додаткових властивостей. Multer передає цей об’єкт безпосередньо в busboy, а деталі властивостей можна знайти на [сторінці busboy](https://github.com/mscdex/busboy#busboy-methods).

Доступні такі цілі значення:

Ключ | Опис | За замовчуванням
--- | --- | ---
`fieldNameSize` | Максимальний розмір імені поля | 100 байт
`розмір поля` | Максимальний розмір значення поля (у байтах) | 1 МБ
`поля` | Максимальна кількість нефайлових полів | Нескінченність
`розмір файлу` | Для багатокомпонентних форм максимальний розмір файлу (у байтах) | Нескінченність
`файли` | Для багатокомпонентних форм максимальна кількість полів файлу | Нескінченність
`частини` | Для багатокомпонентних форм максимальна кількість частин (поля + файли) | Нескінченність
`headerPairs` | Для багатокомпонентних форм максимальна кількість пар ключ=>значення заголовка для аналізу | 2000 рік

Зазначення обмежень може допомогти захистити ваш сайт від атак типу «відмова в обслуговуванні» (DoS).

### `fileFilter`

Встановіть для цього функцію, щоб контролювати, які файли потрібно завантажити та які
слід пропустити. Функція має виглядати так:

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

## Обробка помилок

У разі виявлення помилки Multer делегує помилку Express. Ви можете
відобразити гарну сторінку помилки за допомогою [стандартного швидкого способу](http://expressjs.com/guide/error-handling.html).

Якщо ви хочете виловлювати помилки саме від Multer, ви можете зателефонувати
самостійно функціонувати проміжне програмне забезпечення. Крім того, якщо ви хочете виловлювати лише [помилки Multer](https://github.com/expressjs/multer/blob/main/lib/multer-error.js), ви можете використовувати клас `MulterError`, який приєднується до самого об’єкта `multer` (наприклад, `err instanceof multer.MulterError`).

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

## Спеціальний механізм зберігання

Щоб отримати інформацію про те, як створити власний механізм зберігання, перегляньте [Multer Storage Engine](https://github.com/expressjs/multer/blob/main/StorageEngine.md).

## Ліцензія

[МОЯ](ЛІЦЕНЗІЯ)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
