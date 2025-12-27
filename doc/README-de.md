# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

Multer ist eine node.js-Middleware zur Verarbeitung von „Multipart/Form-Data“, die hauptsächlich zum Hochladen von Dateien verwendet wird. Es steht geschrieben
zusätzlich zu [busboy](https://github.com/mscdex/busboy) für maximale Effizienz.

**HINWEIS**: Multer verarbeitet kein Formular, das nicht mehrteilig ist („multipart/form-data“).

## Übersetzungen

Diese README-Datei ist auch in anderen Sprachen verfügbar:

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

## Installation

```sh
$ npm install multer
```

## Nutzung

Multer fügt dem „request“-Objekt ein „body“-Objekt und ein „file“- oder „files“-Objekt hinzu. Das „body“-Objekt enthält die Werte der Textfelder des Formulars, das „file“- oder „files“-Objekt enthält die über das Formular hochgeladenen Dateien.

Grundlegendes Anwendungsbeispiel:

Vergessen Sie nicht den `enctype="multipart/form-data"` in Ihrem Formular.

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

Falls Sie ein mehrteiliges Nur-Text-Formular verarbeiten müssen, sollten Sie die Methode „.none()“ verwenden:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

Hier ist ein Beispiel dafür, wie Multer in einem HTML-Formular verwendet wird. Beachten Sie besonders die Felder „enctype="multipart/form-data"` und `name="uploaded_file"`:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

Dann würden Sie in Ihrer Javascript-Datei diese Zeilen hinzufügen, um sowohl auf die Datei als auch auf den Text zuzugreifen. Es ist wichtig, dass Sie in Ihrer Upload-Funktion den Feldwert „Name“ aus dem Formular verwenden. Dadurch wird Multer mitgeteilt, in welchem ​​Feld der Anfrage nach den Dateien gesucht werden soll. Wenn diese Felder im HTML-Formular und auf Ihrem Server nicht identisch sind, schlägt Ihr Upload fehl:

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

### Dateiinformationen

Jede Datei enthält die folgenden Informationen:

Schlüssel | Beschreibung | Hinweis
--- | --- | ---
`Feldname` | In der Form | angegebener Feldname
`ursprünglicher Name` | Name der Datei auf dem Computer des Benutzers |
„Kodierung“ | Kodierungstyp der Datei |
`mimetype` | Mime-Typ der Datei |
„Größe“ | Größe der Datei in Bytes |
„Ziel“ | Der Ordner, in dem die Datei gespeichert wurde | „Festplattenspeicher“.
`Dateiname` | Der Name der Datei im „Ziel“ | „Festplattenspeicher“.
`Pfad` | Der vollständige Pfad zur hochgeladenen Datei | „Festplattenspeicher“.
`Puffer` | Ein „Puffer“ der gesamten Datei | „Speicherspeicher“.

### `multer(opts)`

Multer akzeptiert ein Optionsobjekt, dessen grundlegendstes „Ziel“ ist
Eigenschaft, die Multer mitteilt, wohin die Dateien hochgeladen werden sollen. Falls Sie das weglassen
Wenn Sie ein Optionsobjekt verwenden, bleiben die Dateien im Speicher und werden nie auf die Festplatte geschrieben.

Standardmäßig benennt Multer die Dateien um, um Namenskonflikte zu vermeiden. Die
Die Umbenennungsfunktion kann an Ihre Bedürfnisse angepasst werden.

Im Folgenden sind die Optionen aufgeführt, die an Multer übergeben werden können.

Schlüssel | Beschreibung
--- | ---
„dest“ oder „storage“ | Wo sollen die Dateien gespeichert werden?
`fileFilter` | Funktion zur Steuerung, welche Dateien akzeptiert werden
„Grenzen“ | Grenzen der hochgeladenen Daten
`preservePath` | Behalten Sie den vollständigen Pfad der Dateien bei, anstatt nur den Basisnamen

In einer durchschnittlichen Web-App ist möglicherweise nur „dest“ erforderlich und wie in gezeigt konfiguriert
das folgende Beispiel.

```javascript
const upload = multer({ dest: 'uploads/' })
```

Wenn Sie mehr Kontrolle über Ihre Uploads wünschen, sollten Sie den „Speicher“ verwenden
Option anstelle von „dest“. Multer wird mit der Speicher-Engine „DiskStorage“ ausgeliefert
und „MemoryStorage“; Weitere Motoren sind von Drittanbietern erhältlich.

#### `.single(Feldname)`

Akzeptieren Sie eine einzelne Datei mit dem Namen „Feldname“. Die einzelne Datei wird gespeichert
in „req.file“.

#### `.array(fieldname[, maxCount])`

Akzeptieren Sie ein Array von Dateien, alle mit dem Namen „Feldname“. Optional kann ein Fehler ausgegeben werden, wenn
Es werden mehr als „maxCount“ Dateien hochgeladen. Das Array der Dateien wird in gespeichert
`req.files`.

#### `.fields(fields)`

Akzeptieren Sie eine Mischung von Dateien, die durch „Felder“ angegeben werden. Ein Objekt mit Arrays von Dateien
wird in „req.files“ gespeichert.

„fields“ sollte ein Array von Objekten mit „name“ und optional einem „maxCount“ sein.
Beispiel:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.none()`

Akzeptieren Sie nur Textfelder. Wenn eine Datei hochgeladen wird, liegt ein Fehler im Code vor
Es wird „LIMIT\_UNEXPECTED\_FILE“ ausgegeben.

#### `.any()`

Akzeptiert alle Dateien, die über das Kabel eingehen. Es wird eine Reihe von Dateien gespeichert
`req.files`.

**WARNUNG:** Stellen Sie sicher, dass Sie immer mit den Dateien umgehen, die ein Benutzer hochlädt.
Fügen Sie Multer niemals als globale Middleware hinzu, da ein böswilliger Benutzer einen Upload durchführen könnte
Dateien auf eine Route, die Sie nicht erwartet haben. Benutzen Sie diese Funktion nur auf Routen
wo Sie mit den hochgeladenen Dateien umgehen.

### `Speicher`

#### `DiskStorage`

Mit der Festplattenspeicher-Engine haben Sie die volle Kontrolle über das Speichern von Dateien auf der Festplatte.

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

Es stehen zwei Optionen zur Verfügung: „Ziel“ und „Dateiname“. Sie sind beides
Funktionen, die bestimmen, wo die Datei gespeichert werden soll.

„Ziel“ wird verwendet, um zu bestimmen, in welchem Ordner die hochgeladenen Dateien gespeichert werden sollen
gespeichert werden. Dies kann auch als „String“ angegeben werden (z. B. „/tmp/uploads“). Wenn nein
Als „Ziel“ wird das Standardverzeichnis des Betriebssystems für temporäre Zwecke angegeben
Dateien verwendet wird.

**Hinweis:** Sie sind für die Erstellung des Verzeichnisses bei der Bereitstellung verantwortlich
„Ziel“ als Funktion. Beim Übergeben einer Zeichenfolge stellt Multer sicher, dass dies der Fall ist
Das Verzeichnis wird für Sie erstellt.

„Dateiname“ wird verwendet, um zu bestimmen, wie die Datei im Ordner benannt werden soll.
Wenn kein „Dateiname“ angegeben wird, erhält jede Datei einen zufälligen Namen, der keinen Namen hat
Fügen Sie eine beliebige Dateierweiterung ein.

**Hinweis:** Multer fügt für Sie, Ihre Funktion, keine Dateierweiterung an
sollte einen Dateinamen mit einer Dateierweiterung zurückgeben.

Jeder Funktion werden sowohl die Anfrage („req“) als auch einige Informationen darüber übergeben
die Datei („file“) als Entscheidungshilfe.

Beachten Sie, dass „req.body“ möglicherweise noch nicht vollständig ausgefüllt ist. Es kommt darauf an
Damit der Client Felder und Dateien an den Server übermittelt.

Zum Verständnis der im Rückruf verwendeten Aufrufkonvention (die bestanden werden muss
null als erster Parameter), siehe
[Node.js-Fehlerbehandlung](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/produktion/design/errors)

#### `MemoryStorage`

Die Speicher-Engine speichert die Dateien im Speicher als „Puffer“-Objekte. Es
hat keine Optionen.

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

Bei Verwendung von Speichermedien enthalten die Dateiinformationen ein Feld namens
„Puffer“, der die gesamte Datei enthält.

**WARNUNG**: Hochladen sehr großer Dateien oder relativ kleiner Dateien in großen Mengen
Zahlen sehr schnell verarbeiten, kann dazu führen, dass Ihrer Anwendung nicht mehr genügend Arbeitsspeicher zur Verfügung steht
Speicher wird verwendet.

### `Grenzen`

Ein Objekt, das die Größenbeschränkungen der folgenden optionalen Eigenschaften angibt. Multer übergibt dieses Objekt direkt an Busboy. Die Details der Eigenschaften finden Sie auf [Busboys Seite](https://github.com/mscdex/busboy#busboy-methods).

Die folgenden ganzzahligen Werte sind verfügbar:

Schlüssel | Beschreibung | Standard
--- | --- | ---
`fieldNameSize` | Maximale Feldnamengröße | 100 Byte
`fieldSize` | Maximale Feldwertgröße (in Bytes) | 1 MB
`Felder` | Maximale Anzahl von Nicht-Dateifeldern | Unendlichkeit
`fileSize` | Bei mehrteiligen Formularen beträgt die maximale Dateigröße (in Bytes) | Unendlichkeit
`Dateien` | Bei mehrteiligen Formularen beträgt die maximale Anzahl an Dateifeldern | Unendlichkeit
„Teile“ | Bei mehrteiligen Formularen beträgt die maximale Anzahl der Teile (Felder + Dateien) | Unendlichkeit
`headerPairs` | Bei mehrteiligen Formularen die maximale Anzahl der zu analysierenden Header-Schlüssel=>Wert-Paare | 2000

Durch die Angabe der Grenzwerte können Sie Ihre Website vor Denial-of-Service-Angriffen (DoS) schützen.

### `fileFilter`

Stellen Sie dies auf eine Funktion ein, um zu steuern, welche Dateien hochgeladen werden sollen und welche
sollte übersprungen werden. Die Funktion sollte so aussehen:

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

## Fehlerbehandlung

Wenn ein Fehler auftritt, delegiert Multer den Fehler an Express. Du kannst
Zeigen Sie eine schöne Fehlerseite mit [der Standard-Express-Methode] an (http://expressjs.com/guide/error-handling.html).

Wenn Sie Fehler speziell von Multer abfangen möchten, können Sie die aufrufen
Middleware-Funktion selbst. Wenn Sie außerdem nur [die Multer-Fehler] (https://github.com/expressjs/multer/blob/main/lib/multer-error.js) abfangen möchten, können Sie die Klasse „MulterError“ verwenden, die an das „Multer“-Objekt selbst angehängt ist (z. B. „err Instanz von Multer.MulterError“).

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

## Benutzerdefinierte Speicher-Engine

Informationen zum Erstellen Ihrer eigenen Speicher-Engine finden Sie unter [Multer Storage Engine](https://github.com/expressjs/multer/blob/main/StorageEngine.md).

## Lizenz

[MEINE](LIZENZ)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
