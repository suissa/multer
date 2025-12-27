# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

Multer to oprogramowanie pośrednie node.js do obsługi danych wieloczęściowych/formularzy, które jest używane głównie do przesyłania plików. Jest napisane
na [busboy](https://github.com/mscdex/busboy), aby uzyskać maksymalną wydajność.

**UWAGA**: Multer nie przetworzy żadnego formularza, który nie jest wieloczęściowy („wieloczęściowy/dane formularza”).

## Tłumaczenia

Ten plik README jest również dostępny w innych językach:

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

## Instalacja

```sh
$ npm install multer
```

## Użycie

Multer dodaje obiekt `body` oraz obiekt `file` lub `files` do obiektu `request`. Obiekt `body` zawiera wartości pól tekstowych formularza, obiekt `file` lub `files` zawiera pliki przesłane za pośrednictwem formularza.

Podstawowy przykład użycia:

Nie zapomnij o `enctype="multipart/form-data"` w swoim formularzu.

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

W przypadku konieczności obsługi wieloczęściowego formularza zawierającego wyłącznie tekst, należy użyć metody `.none()`:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

Oto przykład użycia multera w formularzu HTML. Zwróć szczególną uwagę na pola `enctype="multipart/form-data"` i `name="uploaded_file"`:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

Następnie w pliku JavaScript dodałbyś te linie, aby uzyskać dostęp zarówno do pliku, jak i do treści. Ważne jest, aby w funkcji przesyłania użyć wartości pola „nazwa” z formularza. To informuje multer, w którym polu żądania powinien szukać plików. Jeśli te pola nie są takie same w formularzu HTML i na Twoim serwerze, przesyłanie nie powiedzie się:

```javascript
const multer  = require('multer')
const upload = multer({ dest: './public/data/uploads/' })
app.post('/stats', upload.single('uploaded_file'), function (req, res) {
  // req.file is the name of your file in the form above, here 'uploaded_file'
  // req.body will hold the text fields, if there were any
  console.log(req.file, req.body)
});
```



##API

### Informacje o pliku

Każdy plik zawiera następujące informacje:

Klucz | Opis | Uwaga
--- | --- | ---
`nazwa pola` | Nazwa pola podana w formularzu |
`oryginalna nazwa` | Nazwa pliku na komputerze użytkownika |
`kodowanie` | Typ kodowania pliku |
`typ MIME` | Typ MIME pliku |
`rozmiar` | Rozmiar pliku w bajtach |
„miejsce docelowe” | Folder, w którym zapisano plik | „Przechowywanie dysku”.
`nazwa pliku` | Nazwa pliku w `destination` | „Przechowywanie dysku”.
`ścieżka` | Pełna ścieżka do przesłanego pliku | „Przechowywanie dysku”.
`bufor` | `Bufor` całego pliku | „Przechowywanie pamięci”.

### `multer(opcje)`

Multer akceptuje obiekt opcji, z których najbardziej podstawowym jest „dest”.
właściwość, która informuje Multera, gdzie przesłać pliki. W przypadku pominięcia
Options, pliki będą przechowywane w pamięci i nigdy nie będą zapisywane na dysku.

Domyślnie Multer zmieni nazwy plików, aby uniknąć konfliktów nazw. The
funkcję zmiany nazwy można dostosować do własnych potrzeb.

Poniżej znajdują się opcje, które można przekazać do Multera.

Klucz | Opis
--- | ---
„dest” lub „storage” | Gdzie przechowywać pliki
`Filtr plików` | Funkcja kontrolująca, które pliki są akceptowane
„granice” | Limity przesyłanych danych
`zachowaj ścieżkę` | Zachowaj pełną ścieżkę plików zamiast tylko nazwy podstawowej

W przeciętnej aplikacji internetowej może być wymagane tylko polecenie „dest” i skonfigurowane w sposób pokazany w
następujący przykład.

```javascript
const upload = multer({ dest: 'uploads/' })
```

Jeśli chcesz mieć większą kontrolę nad przesyłanymi plikami, skorzystaj z opcji „przechowywanie”.
opcję zamiast `dest`. Statki Multera z silnikami pamięci masowej `DiskStorage`
i „Pamięć pamięci”; Więcej silników jest dostępnych od stron trzecich.

#### `.single(nazwa pola)`

Zaakceptuj pojedynczy plik o nazwie `nazwa pola`. Pojedynczy plik zostanie zapisany
w `pliku wymagań`.

#### `.array(nazwapola[, maxCount])`

Zaakceptuj tablicę plików, wszystkie o nazwie „nazwa pola”. Opcjonalnie błąd if
przesłano więcej niż `maxCount` plików. Tablica plików zostanie zapisana w
`pliki wymagań`.

#### `.pola(pola)`

Zaakceptuj mieszankę plików określoną przez `pola`. Obiekt z tablicami plików
będą przechowywane w `req.files`.

`fields` powinno być tablicą obiektów zawierającą `name` i opcjonalnie `maxCount`.
Przykład:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.brak()`

Akceptuj tylko pola tekstowe. Jeśli zostanie przesłane jakikolwiek plik, wystąpi błąd w kodzie
Zostanie wydany „LIMIT\_UNEXPECTED\_FILE”.

#### `.dowolna()`

Akceptuje wszystkie pliki przesyłane drogą kablową. Tablica plików zostanie zapisana w
`pliki wymagań`.

**OSTRZEŻENIE:** Pamiętaj, aby zawsze obsługiwać pliki przesyłane przez użytkownika.
Nigdy nie dodawaj multera jako globalnego oprogramowania pośredniczącego, ponieważ złośliwy użytkownik może przesłać plik
pliki na trasę, której nie przewidywałeś. Używaj tej funkcji wyłącznie na trasach
gdzie obsługujesz przesłane pliki.

### „przechowywanie”.

#### `Przechowywanie dysku`

Mechanizm przechowywania dyskowego zapewnia pełną kontrolę nad przechowywaniem plików na dysku.

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

Dostępne są dwie opcje: „miejsce docelowe” i „nazwa pliku”. Oboje są
funkcje określające, gdzie plik powinien być przechowywany.

„Miejsce docelowe” służy do określenia, w którym folderze powinny znajdować się przesłane pliki
być przechowywane. Można to również podać jako „ciąg znaków” (np. „/tmp/uploads”). Jeśli nie
Podano „destination”, domyślny katalog systemu operacyjnego dla tymczasowego
używane są pliki.

**Uwaga:** Podczas udostępniania jesteś odpowiedzialny za utworzenie katalogu
„miejsce docelowe” jako funkcja. Podczas przekazywania ciągu znaków multer upewni się, że to nastąpi
katalog jest tworzony dla Ciebie.

`nazwa pliku` służy do określenia, jaką nazwę powinien mieć plik w folderze.
Jeśli nie zostanie podana nazwa pliku, każdemu plikowi zostanie nadana losowa nazwa, która nie jest podana
Dołącz dowolne rozszerzenie pliku.

**Uwaga:** Multer nie doda żadnego rozszerzenia pliku dla Ciebie, Twojej funkcji
powinien zwrócić pełną nazwę pliku z rozszerzeniem.

Do każdej funkcji przekazywane jest zarówno żądanie („req”), jak i pewne informacje na temat
plik („plik”), który pomoże w podjęciu decyzji.

Należy pamiętać, że pole „req.body” mogło nie zostać jeszcze w pełni wypełnione. To zależy od
aby klient przesyłał pola i pliki na serwer.

Aby zrozumieć konwencję wywoływania stosowaną w wywołaniu zwrotnym (konieczność przekazania
null jako pierwszy parametr), patrz
[Obsługa błędów Node.js](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### `Pamięć pamięci`

Mechanizm przechowywania pamięci przechowuje pliki w pamięci jako obiekty „Bufor”. To
nie ma żadnych opcji.

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

Podczas korzystania z pamięci masowej informacje o pliku będą zawierać pole o nazwie
`bufor` zawierający cały plik.

**OSTRZEŻENIE**: Przesyłanie bardzo dużych plików lub stosunkowo małych plików w dużych rozmiarach
numery bardzo szybko, może spowodować, że w aplikacji zabraknie pamięci, gdy
używana jest pamięć.

### „limity”.

Obiekt określający limity rozmiaru następujących opcjonalnych właściwości. Multer przekazuje ten obiekt bezpośrednio do busboya, a szczegóły właściwości można znaleźć na [stronie busboya](https://github.com/mscdex/busboy#busboy-methods).

Dostępne są następujące wartości całkowite:

Klucz | Opis | Domyślne
--- | --- | ---
`rozmiar nazwy pola` | Maksymalny rozmiar nazwy pola | 100 bajtów
`Rozmiar pola` | Maksymalny rozmiar wartości pola (w bajtach) | 1MB
`pola` | Maksymalna liczba pól niebędących plikami | Nieskończoność
`Rozmiar pliku` | W przypadku formularzy wieloczęściowych maksymalny rozmiar pliku (w bajtach) | Nieskończoność
`pliki` | W przypadku formularzy wieloczęściowych maksymalna liczba pól pliku | Nieskończoność
`części` | Dla formularzy wieloczęściowych maksymalna liczba części (pola + pliki) | Nieskończoność
`Pary nagłówków` | W przypadku formularzy wieloczęściowych maksymalna liczba par klucz nagłówka=>wartość do analizy | 2000

Określenie limitów może pomóc chronić witrynę przed atakami typu „odmowa usługi” (DoS).

### `Filtr plików`

Ustaw tę opcję na funkcję kontrolującą, które pliki mają zostać przesłane, a które
należy pominąć. Funkcja powinna wyglądać następująco:

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

## Obsługa błędów

W przypadku napotkania błędu Multer przekaże błąd firmie Express. Możesz
wyświetl ładną stronę błędu, używając [standardowego sposobu ekspresowego] (http://expressjs.com/guide/error-handling.html).

Jeśli chcesz wyłapać błędy konkretnie z Multera, możesz wywołać metodę
funkcję oprogramowania pośredniego samodzielnie. Ponadto, jeśli chcesz wyłapać tylko [błędy Multera](https://github.com/expressjs/multer/blob/main/lib/multer-error.js), możesz użyć klasy `MulterError`, która jest dołączona do samego obiektu `multer` (np. `err instancjaof multer.MulterError`).

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

## Niestandardowy silnik przechowywania

Informacje o tym, jak zbudować własny silnik pamięci masowej, znajdziesz w [Multer Storage Engine](https://github.com/expressjs/multer/blob/main/StorageEngine.md).

## Licencja

[MOJE](LICENCJA)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
