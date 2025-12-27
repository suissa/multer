# Multer [![NPM Version][npm-version-image]][npm-url] [![NPM Downloads][npm-downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Test Coverage][test-image]][test-url] [![OpenSSF Scorecard Badge][ossf-scorecard-badge]][ossf-scorecard-visualizer]

Multer è un middleware node.js per la gestione di "multipart/form-data", utilizzato principalmente per caricare file. E' scritto
sopra [busboy](https://github.com/mscdex/busboy) per la massima efficienza.

**NOTA**: Multer non elaborerà alcun modulo che non sia multipart (`multipart/form-data`).

## Traduzioni

Questo README è disponibile anche in altre lingue:

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

## Installazione

```sh
$ npm install multer
```

## Utilizzo

Multer aggiunge un oggetto "body" e un oggetto "file" o "files" all'oggetto "request". L'oggetto `body` contiene i valori dei campi di testo del modulo, l'oggetto `file` o `files` contiene i file caricati tramite il modulo.

Esempio di utilizzo di base:

Non dimenticare `enctype="multipart/form-data"` nel tuo modulo.

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

Nel caso in cui sia necessario gestire un modulo multiparte di solo testo, è necessario utilizzare il metodo `.none()`:

```javascript
const express = require('express')
const app = express()
const multer  = require('multer')
const upload = multer()

app.post('/profile', upload.none(), function (req, res, next) {
  // req.body contains the text fields
})
```

Ecco un esempio di come viene utilizzato multer in un modulo HTML. Presta particolare attenzione ai campi `enctype="multipart/form-data"` e `name="uploaded_file"`:

```html
<form action="/stats" enctype="multipart/form-data" method="post">
  <div class="form-group">
    <input type="file" class="form-control-file" name="uploaded_file">
    <input type="text" class="form-control" placeholder="Number of speakers" name="nspeakers">
    <input type="submit" value="Get me the stats!" class="btn btn-default">
  </div>
</form>
```

Quindi nel tuo file javascript aggiungeresti queste righe per accedere sia al file che al corpo. È importante utilizzare il valore del campo "nome" dal modulo nella funzione di caricamento. Questo indica a multer in quale campo della richiesta deve cercare i file. Se questi campi non sono gli stessi nel modulo HTML e sul tuo server, il caricamento fallirà:

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

### Informazioni sul file

Ogni file contiene le seguenti informazioni:

Chiave | Descrizione | Nota
--- | --- | ---
`nomecampo` | Nome del campo specificato nel modulo |
`nomeoriginale` | Nome del file sul computer dell'utente |
`codifica` | Tipo di codifica del file |
`mimetipo` | Tipo MIME del file |
`dimensione` | Dimensione del file in byte |
`destinazione` | La cartella in cui è stato salvato il file | "Archiviazione su disco".
`nome file` | Il nome del file all'interno della `destinazione` | "Archiviazione su disco".
`percorso` | Il percorso completo del file caricato | "Archiviazione su disco".
`buffer` | Un `Buffer` dell'intero file | "Memoria".

### `multer(opta)`

Multer accetta un oggetto opzioni, il più elementare dei quali è "dest".
property, che dice a Multer dove caricare i file. Nel caso in cui si ometta il
oggetto opzioni, i file verranno mantenuti in memoria e mai scritti su disco.

Per impostazione predefinita, Multer rinominerà i file in modo da evitare conflitti di denominazione. Il
la funzione di ridenominazione può essere personalizzata in base alle proprie esigenze.

Di seguito sono riportate le opzioni che possono essere passate a Multer.

Chiave | Descrizione
--- | ---
`dest` o `storage` | Dove archiviare i file
`filtrofile` | Funzione per controllare quali file sono accettati
`limiti` | Limiti dei dati caricati
`preservePath` | Conserva il percorso completo dei file anziché solo il nome di base

In un'app Web media, potrebbe essere richiesto solo "dest" e configurato come mostrato in
il seguente esempio.

```javascript
const upload = multer({ dest: 'uploads/' })
```

Se desideri un maggiore controllo sui tuoi caricamenti, ti consigliamo di utilizzare lo `storage`
opzione invece di "dest". Multer viene fornito con motori di archiviazione `DiskStorage`
e "Memoria"; Altri motori sono disponibili da terze parti.

#### `.single(nomecampo)`

Accetta un singolo file con il nome "fieldname". Il singolo file verrà archiviato
in "req.file".

#### `.array(nomecampo[, conteggiomax])`

Accetta una serie di file, tutti con il nome "fieldname". Facoltativamente errore se
vengono caricati più di file "maxCount". L'array di file verrà archiviato in
"req.files".

#### `.fields(campi)`

Accetta un mix di file, specificati da "fields". Un oggetto con matrici di file
verrà archiviato in "req.files".

"fields" dovrebbe essere un array di oggetti con "name" e facoltativamente un "maxCount".
Esempio:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.none()`

Accetta solo campi di testo. Se viene effettuato il caricamento di file, errore con il codice
Verrà emesso "LIMIT\_UNEXPECTED\_FILE".

#### `.any()`

Accetta tutti i file che arrivano via cavo. Verrà archiviata una serie di file
"req.files".

**ATTENZIONE:** Assicurati di gestire sempre i file caricati da un utente.
Non aggiungere mai multer come middleware globale poiché un utente malintenzionato potrebbe caricare
file su un percorso che non avevi previsto. Utilizzare questa funzione solo sui percorsi
dove gestisci i file caricati.

### `archiviazione`

#### `Archiviazione su disco`

Il motore di archiviazione su disco ti offre il controllo completo sull'archiviazione dei file su disco.

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

Sono disponibili due opzioni: "destinazione" e "nome file". Sono entrambi
funzioni che determinano dove deve essere archiviato il file.

"destinazione" viene utilizzato per determinare in quale cartella devono essere caricati i file
essere memorizzato. Può anche essere fornito come `stringa` (ad esempio `'/tmp/uploads'`). Se no
Viene fornita la "destinazione", la directory predefinita del sistema operativo per i file temporanei
vengono utilizzati i file.

**Nota:** sei responsabile della creazione della directory al momento della fornitura
"destinazione" come funzione. Quando si passa una stringa, multer si assicurerà che
la directory viene creata per te.

"filename" viene utilizzato per determinare il nome del file all'interno della cartella.
Se non viene fornito alcun `nome file`, a ciascun file verrà assegnato un nome casuale che non lo specifica
includere qualsiasi estensione di file.

**Nota:** Multer non aggiungerà alcuna estensione di file per te, la tua funzione
dovrebbe restituire un nome file completo di estensione file.

Ad ogni funzione vengono passati sia la richiesta (`req`) che alcune informazioni su
il file ("file") per facilitare la decisione.

Tieni presente che "req.body" potrebbe non essere stato ancora completamente popolato. Dipende da
ordine che il client trasmetta campi e file al server.

Per comprendere la convenzione di chiamata utilizzata nel callback (è necessario passare
null come primo parametro), fare riferimento a
[Gestione degli errori di Node.js](https://web.archive.org/web/20220417042018/https://www.joyent.com/node-js/production/design/errors)

#### `Memoria`

Il motore di archiviazione della memoria archivia i file in memoria come oggetti "Buffer". Esso
non ha alcuna opzione.

```javascript
const storage = multer.memoryStorage()
const upload = multer({ storage: storage })
```

Quando si utilizza la memoria, le informazioni sul file conterranno un campo chiamato
"buffer" che contiene l'intero file.

**ATTENZIONE**: caricamento di file molto grandi o di file relativamente piccoli in grandi dimensioni
numeri molto rapidamente, può causare l'esaurimento della memoria dell'applicazione quando
viene utilizzata la memoria.

### `limiti`

Un oggetto che specifica i limiti di dimensione delle seguenti proprietà facoltative. Multer passa questo oggetto direttamente a busboy e i dettagli delle proprietà possono essere trovati sulla [pagina di busboy](https://github.com/mscdex/busboy#busboy-methods).

Sono disponibili i seguenti valori interi:

Chiave | Descrizione | Predefinito
--- | --- | ---
`dimensionenomecampo` | Dimensione massima del nome del campo | 100 byte
`dimensionecampo` | Dimensione massima del valore del campo (in byte) | 1 MB
`campi` | Numero massimo di campi non file | Infinito
`dimensionefile` | Per i moduli multistrato, la dimensione massima del file (in byte) | Infinito
"file" | Per i moduli multistrato, il numero massimo di campi file | Infinito
"parti" | Per i moduli multistrato, il numero massimo di parti (campi + file) | Infinito
`headerPairs` | Per i moduli multiparte, il numero massimo di coppie chiave=>valore dell'intestazione da analizzare | 2000

Specificare i limiti può aiutare a proteggere il tuo sito dagli attacchi Denial of Service (DoS).

### `Filtrofile`

Impostalo su una funzione per controllare quali file devono essere caricati e quali
dovrebbe essere saltato. La funzione dovrebbe assomigliare a questa:

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

## Gestione degli errori

Quando si verifica un errore, Multer delegherà l'errore a Express. Puoi
visualizza una bella pagina di errore utilizzando [il metodo rapido standard](http://expressjs.com/guide/error-handling.html).

Se vuoi rilevare errori specificatamente da Multer, puoi chiamare il file
funzione middleware da solo. Inoltre, se vuoi catturare solo [gli errori Multer](https://github.com/expressjs/multer/blob/main/lib/multer-error.js), puoi utilizzare la classe `MulterError` che è allegata all'oggetto `multer` stesso (ad esempio `err istanza di multer.MulterError`).

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

## Motore di archiviazione personalizzato

Per informazioni su come creare il proprio motore di archiviazione, vedere [Multer Storage Engine](https://github.com/expressjs/multer/blob/main/StorageEngine.md).

## Licenza

[MIA](LICENZA)

[ci-image]: https://github.com/expressjs/multer/actions/workflows/ci.yml/badge.svg
[ci-url]: https://github.com/expressjs/multer/actions/workflows/ci.yml
[test-url]: https://coveralls.io/r/expressjs/multer?branch=main
[test-image]: https://badgen.net/coveralls/c/github/expressjs/multer/main
[npm-downloads-image]: https://badgen.net/npm/dm/multer
[npm-url]: https://npmjs.org/package/multer
[npm-version-image]: https://badgen.net/npm/v/multer
[ossf-scorecard-badge]: https://api.scorecard.dev/projects/github.com/expressjs/multer/badge
[ossf-scorecard-visualizer]: https://ossf.github.io/scorecard-visualizer/#/projects/github.com/expressjs/multer
