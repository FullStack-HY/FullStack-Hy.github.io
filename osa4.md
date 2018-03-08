---
layout: page
title: osa 4
inheader: yes
permalink: /osa4/
---
## Osan 4 oppimistavoitteet

- Node.js / Express
  - Router
  - sovelluksen jakaminen osiin
- Node.js -sovellusten testaus
  - jest/supertest
- JS
  - async/await
- Mongoose
  - Monimutkaisemmat skeemat
  - Viittaukset kokoelmien välillä
  - populointi
- Web
  - Token-autentikaatio
  - JWT

## Sovelluksen rakenteen parantelu

Jatketaan [osassa 3](/osa3) tehdyn muistiinpanosovelluksen backendin kehittämistä.

Muutetaan sovelluksen rakennetta siten, että projektin juuressa oleva _index.js_ ainoastaan konfiguroi sovelluksen tietokannan ja käytettävät middlewaret. Routejen määrittely siirretään omaan tiedostoonsa, eli siitä tehdään [moduuli](/osa3/#tietokantamäärittelyjen-eriyttäminen-omaksi-moduuliksi).

Routejen tapahtumankäsittelijöitä kutsutaan usein _kontrollereiksi_. Luodaankin hakemisto _controllers_ ja sinne tiedosto _notes.js_ johon tulemme siirtämään kaikki muistiinpanoihin liittyvien reittien määrittelyt.

Tiedoston sisältö on seuraava:

```js
const notesRouter = require('express').Router()
const Note = require('../models/note')

const formatNote = (note) => {
  return {
    id: note._id,
    content: note.content,
    date: note.date,
    important: note.important
  }
}

notesRouter.get('/', (request, response) => {
  Note
    .find({})
    .then(notes => {
      response.json(notes.map(formatNote))
    })
})

notesRouter.get('/:id', (request, response) => {
  Note
    .findById(request.params.id)
    .then(note => {
      if (note) {
        response.json(formatNote(note))
      } else {
        response.status(404).end()
      }
    })
    .catch(error => {
      response.status(400).send({ error: 'malformatted id' })
    })
})

notesRouter.delete('/:id', (request, response) => {
  Note
    .findByIdAndRemove(request.params.id)
    .then(result => {
      console.log(result)
      response.status(204).end()
    })
    .catch(error => {
      response.status(400).send({ error: 'malformatted id' })
    })
})

notesRouter.post('/', (request, response) => {
  const body = request.body

  if (body.content === undefined) {
    response.status(400).json({ error: 'content missing' })
  }

  const note = new Note({
    content: body.content,
    important: body.important === undefined ? false : body.important,
    date: new Date()
  })

  note
    .save()
    .then(note => {
      return formatNote(note)
    })
    .then(formattedNote => {
      response.json(formattedNote)
    })

})

notesRouter.put('/:id', (request, response) => {
  const body = request.body

  const note = {
    content: body.content,
    important: body.important
  }

  Note
    .findByIdAndUpdate(request.params.id, note, { new: true })
    .then(updatedNote => {
      response.json(formatNote(updatedNote))
    })
    .catch(error => {
      console.log(error)
      response.status(400).send({ error: 'malformatted id' })
    })
})

module.exports = notesRouter
```

Kyseessä on käytännössä melkein suora copypaste tiedostosta _index.js_.

Muutoksia on muutama. Tiedoston alussa luodaan [router](http://expressjs.com/en/api.html#router)-olio:

```js
const notesRouter = require('express').Router()

//...

module.exports = notesRouter
```

Tiedosto eksporttaa moduulin käyttäjille määritellyn routerin.

Kaikki määriteltävät routet liitetään router-olioon, samaan tapaan kuin aiemmassa versiossa routet liitettiin sovellusta edustavaan olioon.

Huomioinarvoinen seikka routejen määrittelyssä on se, että polut ovat typistyneet, aiemmin määrittelimme esim.

```js
app.delete('/api/notes/:id', (request, response) => {
```

nyt riittää määritellä

```js
notesRouter.delete('/:id', (request, response) => {
```

Mistä routereissa oikeastaan on kyse? Expressin manuaalin sanoin

> A router object is an isolated instance of middleware and routes. You can think of it as a “mini-application,” capable only of performing middleware and routing functions. Every Express application has a built-in app router.

Router on siis _middleware_, jonka avulla on mahdollista määritellä joukko "toisiinsa liittyviä" routeja yhdessä paikassa, yleensä omassa moduulissaan.

Ohjelman käynnistystiedosto, eli määrittelyt tekevä _index.js_ ottaa määrittelemämme routerin käyttöön seuraavasti:

```js
const notesRouter = require('./controllers/notes')
app.use('/api/notes', notesRouter)
```

Näin määrittelemäämme routeria käytetään _jos_ polun alkuosa on _/api/notes_. notesRouter-olion sisällä täytyy tämän takia käyttää ainoastaan polun loppuosia, eli tyhjää polkua _/_ tai pelkkää parametria _/:id_.

### sovelluksen muut osat

Sovelluksen käynnistyspisteenä toimiva _index.js_ näyttää muutosten jälkeen seuraavalta:

```js
const express = require('express')
const app = express()
const bodyParser = require('body-parser')
const cors = require('cors')
const mongoose = require('mongoose')
const middleware = require('./utils/middleware')
const notesRouter = require('./controllers/notes')

if (process.env.NODE_ENV !== 'production') {
  require('dotenv').config()
}

mongoose
  .connect(process.env.MONGODB_URI)
  .then( () => {
    console.log('connected to database', process.env.MONGODB_URI)
  })
  .catch( err => {
    console.log(err)
  })

app.use(cors())
app.use(bodyParser.json())
app.use(express.static('build'))
app.use(middleware.logger)

app.use('/api/notes', notesRouter)

app.use(middleware.error)

const PORT = process.env.PORT || 3001
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```

Tiedostossa siis otetaan käyttöön joukko middlewareja, näistä yksi on polkuun _/api/notes_ kiinnitettävä _notesRouter_ (tai notes-kontrolleri niin kuin jotkut sitä kutsuisivat).

Tietokannan yhteydenmuodostuksen suorittavaan funktioon on myös lisätty tapahtumankäsittelijä, joka ilmoittaa onko yhteyden muodostus onnistunut vai ei.

Middlewareista kaksi _middleware.logger_ ja _middleware.error_ on määritelty hakemiston _utils_ tiedostossa _middleware.js_:

```js
const logger = (request, response, next) => {
  console.log('Method:', request.method)
  console.log('Path:  ', request.path)
  console.log('Body:  ', request.body)
  console.log('---')
  next()
}

const error = (request, response) => {
  response.status(404).send({ error: 'unknown endpoint' })
}

module.exports = {
  logger,
  error
}
```

Tietokantayhteyden muodostaminen on nyt siirretty konfiguraatiot tekevän _index.js_:n vastuulle. Hakemistossa _models_ oleva tiedosto _note.js_ sisältää nyt ainoastaan muistiinpanojen skeeman määrittelyn.

```js
const mongoose = require('mongoose')

const Note = mongoose.model('Note', {
  content: String,
  date: Date,
  important: Boolean
})

module.exports = Note
```

Sovelluksen tämänhetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part3-notes-backend/tree/part4-1), tagissa _part4-1_:

![]({{ "/master/4/1b.png" | absolute_url }})

Jos kloonaat projektin itsellesi, suorita komento _npm install_ ennen käynnistämistä eli komentoa _npm start_.

Express-sovelluksien rakenteelle, eli hakemistojen ja tiedostojen nimennälle ei ole olemassa mitään yleismaailmallista standardia samaan tapaan kuin esim. Ruby on Railsissa. Tässä käyttämämme malli noudattaa eräitä internetissä vastaan tulevia hyviä käytäntöjä.

## Tehtäviä

Tee nyt tehtävät [4.1 ja 4.2](/tehtävät#sovelluksen-alustus-ja-rakenne)

## node-sovellusten testaaminen

Olemme laiminlyöneet ikävästi yhtä oleellista ohjelmistokehityksen osa-aluetta, automatisoitua testaamista.

Aloitamme yksikkötestauksesta. Sovelluksemme logiikka on sen verran yksinkertaista, että siinä ei ole juurikaan mielekästä yksikkötestattavaa. Luodaan tiedosto _utils/for_testing.js_ ja määritellään sinne pari yksinkertaista funktiota testattavaksi:

```js
const palindrom = (string) => {
  return string.split('').reverse().join('')
}

const average = (array) => {
  const reducer = (sum, item) => {
    return sum + item
  }

  return array.reduce(reducer, 0) / array.length
}

module.exports = {
  palindrom,
  average
}
```

> Metodi _average_ käyttää taulukoiden metodia [reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce). Jos metodi ei ole vieläkään tuttu, on korkea aika katsoa youtubesta [Functional Javascript](https://www.youtube.com/watch?v=BMUiFMZr7vk&list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84) -sarjasta ainakin kolme ensimmäistä videoa.

Javascriptiin on tarjolla runsaasti erilaisia testikirjastoja eli _test runnereita_. Käytämme tällä kurssilla Facebookin kehittämää ja sisäisesti käyttämää [jest](https://facebook.github.io/jest/):iä, joka on toiminnaltaan ja syntakstiltaankin hyvin samankaltainen kuin tämän hetken eniten käytetty testikirjasto [Mocha](https://mochajs.org/). Muitakin mahdollisuuksia olisi, esim. eräissä piireissä suosiota nopeasti saavuttanut [ava](https://github.com/avajs/ava).

Jest on tälle kurssille luonteva valinta, sillä se sopii hyvin backendien testaamiseen, mutta suorastaan loistaa Reactilla tehtyjen frontendien testauksessa.

> *Huomio Windows-käyttäjille:* jest ei välttämättä toimi, jos projektin hakemistopolulla on hakemisto, jonka nimessä on välilyöntejä.

Koska testejä on tarkoitus suorittaa ainoastaan sovellusta kehitettäessä, asennetaan _jest_ kehitysaikaiseksi riippuvuudeksi komennolla

```bash
npm install --save-dev jest
```

määritellään _npm_ skripti _test_ suorittamaan testaus jestillä ja raportoimaan testien suorituksesta _verbose_-tyylillä:

```bash
{
  //...
  "scripts": {
    "start": "node index.js",
    "watch": "nodemon index.js",
    "lint": "eslint .",
    "test": "jest --verbose"
  },
  //...
}
```

Tehdään testejä varten hakemisto _tests_ ja sinne tiedosto _palindrom.test.js_, jonka sisältö on seuraava

```js
const palindrom = require('../utils/for_testing').palindrom

test('palindrom of a', () => {
  const result = palindrom('a')

  expect(result).toBe('a')
})

test('palindrom of react', () => {
  const result = palindrom('react')

  expect(result).toBe('tcaer')
})

test('palindrom of saippuakauppias', () => {
  const result = palindrom('saippuakauppias')

  expect(result).toBe('saippuakauppias')
})
```

Edellisessä osassa käyttöön ottamamme ESlint valittaa testien käyttämistä komennoista _test_ ja _expect_ sillä käyttämämme konfiguraatio kieltää _globaalina_ määriteltyjen asioiden käytön. Poistetaan valitus lisäämällä tiedostoon _.eslintrc.js_ kenttä _globals_:

```js
module.exports = {
    "env": {
        "es6": true,
        "node": true
    },
    "extends": "eslint:recommended",
    "rules": {
      //...
    },
    "globals": {
        "test": true,
        "expect": true,
        "describe": true
    }
}
```

Testi ottaa ensimmäisellä rivillä käyttöön testattavan funktion sijoittaen sen muuttujaan _palindrom_:

```js
const palindrom = require('../utils/for_testing').palindrom
```

Yksittäiset testitapaukset määritellään funktion _test_ avulla. Ensimmäisenä parametrina on merkkijonomuotoinen testin kuvaus. Toisena parametrina on _funktio_, joka määrittelee testitapauksen toiminnallisuuden. Esim. toisen testitapauksen toiminnallisuus näyttää seuraavalta:

```js
() => {
  const result = palindrom('react')

  expect(result).toBe('tcaer')
}
```

Ensin suoritetaan testattava koodi, eli generoidaan merkkijonon _react_ palindromi. Seuraavaksi varmistetaan tulos metodin [expect](https://facebook.github.io/jest/docs/en/expect.html#content) avulla. Expect käärii tuloksena olevan arvon olioon, joka tarjoaa joukon _matcher_-funktioita, joiden avulla tuloksen oikeellisuutta voidaan tarkastella. Koska kyse on kahden merkkijonon samuuden vertailusta, sopii tilanteeseen matcheri [toBe](https://facebook.github.io/jest/docs/en/expect.html#tobevalue).

Kuten odotettua, testit menevät läpi:

![]({{ "/assets/4/1.png" | absolute_url }})

Jest olettaa oletusarvoisesti, että testitiedoston nimessä on merkkijono _.test_. Käytetään kurssilla konventiota, millä testitiedostojen nimen loppu on _.test.js_

Jestin antamat virheilmoitukset ovat hyviä, rikotaan testi

```js
test('palindrom of react', () => {
  const result = palindrom('react')

  expect(result).toBe('tkaer')
})
```

seurauksena on seuraava virheilmotus

![]({{ "/assets/4/2.png" | absolute_url }})

Lisätään muutama testi metodille _average_, tiedostoon _tests/average.test.js_.

```js
const average = require('../utils/for_testing').average

describe('average', () => {

  test('of one value is the value itself', () => {
    expect(average([1])).toBe(1)
  })

  test('of many is caclulated right', () => {
    expect(average([1, 2, 3, 4, 5, 6])).toBe(3.5)
  })

  test('of empty array is zero', () => {
    expect(average([])).toBe(0)
  })

})
```

Testi paljastaa, että metodi toimii väärin tyhjällä taulukolla (sillä nollallajaon tulos on Javascriptissä _NaN_):

![]({{ "/assets/4/3.png" | absolute_url }})

Metodi on helppo korjata

```js
const average = (array) => {
  const reducer = (sum, item) => {
    return sum + item
  }
  return array.length === 0 ? 0 : array.reduce(reducer, 0) / array.length
}
```

Eli jos taulukon pituus on 0, palautetaan 0 ja muussa tapauksessa palautetaan metodin _reduce_ avulla laskettu keskiarvo.

Pari huomiota keskiarvon testeistä. Määrittelimme testien ympärille nimellä _average_ varustetun _describe_-lohkon.

```js
describe('average', () => {
  // testit
})
```

Describejen avulla yksittäisessä tiedostossa olevat testit voidaan jaotella loogisiin kokonaisuuksiin. Testituloste hyödyntää myös describe-lohkon nimeä:

![]({{ "/assets/4/4.png" | absolute_url }})

Kuten myöhemmin tulemme näkemään, _describe_-lohkot ovat tarpeellisia siinä vaiheessa, jos haluamme osalle yksittäisen testitiedoston testitapauksista jotain yhteisiä alustus- tai lopetustoimenpiteitä.

Toisena huomiona se, että kirjoitimme testit aavistuksen tiiviimmässä muodossa, ottamatta testattavan metodin tulosta erikseen apumuuttujaan:

```js
  test('of empty array is zero', () => {
    expect(average([])).toBe(0)
  })
```

## Tehtäviä

Tee nyt tehtävät [4.3-4.7](/tehtävät#yksikkötestaus)

## API:n testaaminen

Joissain tilanteissa voisi olla mielekästä suorittaa ainakin osa backendin testauksesta siten, että oikea tietokanta eristettäisiin testeistä ja korvattaisiin "valekomponentilla" eli mockilla. Eräs tähän sopiva ratkaisu olisi [mongo-mock](https://github.com/williamkapke/mongo-mock).

Koska sovelluksemme backend on koodiltaan kuitenkin suhteellisen yksinkertainen, päätämme testata sitä kokonaisuudessaan, siten että myös testeissä käytetään tietokantaa. Tämän kaltaisia, useita sovelluksen komponentteja yhtäaikaa käyttäviä testejä voi luonnehtia [integraatiotesteiksi](https://en.wikipedia.org/wiki/Integration_testing).

### test-ympäristö

Edellisen osan luvussa [Sovelluksen vieminen tuotantoon](/osa3#sovelluksen-vieminen tuotantoon) mainitsimme, että kun sovellusta suoritetaan Herokussa, on se _production_-moodissa.

Noden konventiona on määritellä projektin suoritusmoodi ympäristömuuttujan _NODE_ENV_ avulla. Lataammekin sovelluksen nykyisessä versiossa tiedostossa _.env_ määritellyt ympäristömuuttujat ainoastaan jos sovellus _ei ole_ production moodissa:

```js
if (process.env.NODE_ENV !== 'production') {
  require('dotenv').config()
}
```

Yleinen käytäntö on määritellä sovelluksille omat moodinsa myös sovelluskehitykseen ja testaukseen.

Määrtellään nyt tiedostossa _package.json_, että testejä suorittaessa sovelluksen _NODE_ENV_ saa arvokseen _test_:

```bash
{
  // ...
  "scripts": {
    "start": "NODE_ENV=production node index.js",
    "watch": "NODE_ENV=development nodemon index.js",
    "test": "NODE_ENV=test jest --verbose",
    "lint": "eslint ."
  },
  // ...
}
```

Samalla määriteltiin, että suoritettaessa sovellusta komennolla _npm run watch_ eli nodemonin avulla, on sovelluksen moodi _development_. Jos sovellusta suoritetaan normaalisti Nodella, on moodiksi määritelty _production_.

Määrittelyssämme on kuitenkin pieni ongelma, se ei toimi windowsilla. Tilanne korjautuu asentamalla kirjasto [cross-env](https://www.npmjs.com/package/cross-env) komennolla

```bash
npm install --save-dev cross-env
```

ja muuttamalla _package.js_ kaikilla käyttöjärjestelmillä toimivaan muotoon

```bash
{
  // ...
  "scripts": {
    "start": "cross-env NODE_ENV=production node index.js",
    "watch": "cross-env NODE_ENV=development nodemon index.js",
    "test": "cross-env NODE_ENV=test jest --verbose",
    "lint": "eslint ."
  },
  // ...
}
```

Nyt sovelluksen toimintaa on mahdollista muokata sen suoritusmoodiin perustuen. Eli voimme määritellä, esim. että testejä suoritettaessa ohjelma käyttää erillistä, testejä varten luotua tietokantaa.

Sovelluksen testikanta voidaan luoda tuotantokäytön ja sovelluskehityksen tapaan [mlabiin](https://mlab.com/). Ratkaisu ei ole optimaalinen erityisesti, jos sovellusta on tekemässä yhtä aikaa useita henkilöitä. Testien suoritus nimittäin yleensä edellyttää, että samaa tietokantainstanssia ei ole yhtä aikaa käyttämässä useampia testiajoja.

Testaukseen kannattaakin käyttää verkossa olevaa jaettua tietokantaa mielummin esim. sovelluskehittäjän paikallisen koneen tietokantaa. Optimiratkaisu olisi tietysti se, että jokaista testiajoa varten olisi käytettävissä oma tietokanta, sekin periaatteessa onnistuu "suhteellisen helposti" mm. [keskusmuistissa toimivan Mongon](https://docs.mongodb.com/manual/core/inmemory/) ja [docker](https://www.docker.com)-kontainereiden avulla. Etenemme kuitenkin nyt lyhyemmän kaavan mukaan ja käytetään testikantana normaalia Mongoa.

Voisimme kirjoittaa ympäristökohtaiset konfiguraatiot, esim. oikean tietokannan valinnan suoraan tiedostoon _index.js_, se kuitenkin tekisi tiedoston koodista sekavan. Eristetään sovelluksen ympäristökohtainen konfigurointi omaan tiedostoon _utils/config.js_ sijoitettavaan moduuliin.

Ideana on, että _index.js_ voi käyttää konfiguraatioita seuraavasti:

```js
const config = require('./utils/config')

// ...

mongoose
  .connect(config.mongoUrl)
  .then( () => {
    console.log('connected to database', config.mongoUrl)
  })
  .catch( err => {
    console.log(err)
  })


// ...

const PORT = config.port
```

Konfiguraation suorittavan moduulin koodi on seuraavassa:

```js
if (process.env.NODE_ENV !== 'production') {
  require('dotenv').config()
}

let port = process.env.PORT
let mongoUrl = process.env.MONGODB_URI

if (process.env.NODE_ENV === 'test') {
  port = process.env.TEST_PORT
  mongoUrl = process.env.TEST_MONGODB_URI
}

module.exports = {
  mongoUrl,
  port
}
```

Koodi lataa ympäristömuuttujat tiedostosta _.env_ jos se _ei ole_ tuotantomoodissa. Tuotantomoodissa käytetään Herokuun asetettuja ympäristömuuttujia.

Tiedostossa _.env_ on nyt määritelty _erikseen_ sekä sovelluskehitysympäristön ja testausympäristön tietokannan osoite (esimerkissä molemmat ovat sovelluskehityskoneen lokaaleja mongo-kantoja) ja portti:

```bash
MONGODB_URI=mongodb://fullstack:sekred@ds111078.mlab.com:11078/fullstact-notes-dev
PORT=3001

TEST_PORT=3002
TEST_MONGODB_URI=mongodb://fullstack:sekred@ds113098.mlab.com:13098/fullstack-notes-test
```

Eri porttien käyttö mahdollistaa sen, että sovellus voi olla käynnissä testien suorituksen aikana.

Omatekemämme eri ympäristöjen konfiguroinnista huolehtivaa _config_-moduuli toimii hieman samassa hengessä kuin [node-config](https://github.com/lorenwest/node-config)-kirjasto. Omatekemä konfigurointiympäristö sopii tarkoitukseemme, sillä sovellus on yksinkertainen ja oman konfiguraatio-moduulin tekeminen on myös jossain määrin opettavaista. Isommissa sovelluksissa kannattaa harkita valmiiden kirjastojen, kuten [node-config](https://github.com/lorenwest/node-config):in käyttöä.

Tiedosto _index.js_ muutetaan nyt muotoon:

```js
const http = require('http')
const express = require('express')
const app = express()
const bodyParser = require('body-parser')
const cors = require('cors')
const mongoose = require('mongoose')
const middleware = require('./utils/middleware')
const notesRouter = require('./controllers/notes')
const config = require('./utils/config')

mongoose
  .connect(config.mongoUrl)
  .then( () => {
    console.log('connected to database', config.mongoUrl)
  })
  .catch( err => {
    console.log(err)
  })

app.use(cors())
app.use(bodyParser.json())
app.use(express.static('build'))
app.use(middleware.logger)

app.use('/api/notes', notesRouter)

app.use(middleware.error)

const server = http.createServer(app)

server.listen(config.port, () => {
  console.log(`Server running on port ${config.port}`)
})

server.on('close', () => {
  mongoose.connection.close()
})

module.exports = {
  app, server
}
```

> **HUOM**: koska käytämme useimpia kirjastoja koodissa vain kerran, olisi mahdollista tiivistää koodia hiukan kirjoittamalla esim. <code>app.use(cors())</code> sijaan <code>app.use(require('cors')())</code> ja jättää apumuuttuja _cors_ kokonaan määrittelemättä. On kuitenkin epäselvää kannattaako tälläiseen koodirivien säästelyyn lähteä. Ei ainakaan silloin jos koodin ymmärrettävyys kärsisi.

Tiedoston lopussa on muutama tärkeä muutos.

Sovelluksen käynnistäminen tapahtuu nyt _server_-muuttujassa olevan olion kautta. Serverille määritellään tapahtumankäsitteljäfunktio tapahtumalle _close_ eli tilanteeseen, missä sovellus sammutetaan. Tapahtumankäsittelijä sulkee tietokantayhteyden.

Sekä sovellus _app_ että sitä suorittava _server_-olio määritellään eksportattavaksi tiedostosta. Tämä mahdollistaa sen, että testit voivat käynnistää ja sammuttaa backendin.

Sovelluksen tämänhetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part3-notes-backend/tree/part4-2), tagissa _part4-2_.

### supertest

Käytetään API:n testaamiseen Jestin apuna [supertest](https://github.com/visionmedia/supertest)-kirjastoa.

Kirjasto asennetaan kehitysaikaiseksi riippuvuudeksi komennolla

```bash
npm install --save-dev supertest
```

Luodaan heti ensimmäinen testi tiedostoon _tests/note_api.test.js:_

```js
const supertest = require('supertest')
const { app, server } = require('../index')
const api = supertest(app)

test('notes are returned as json', async () => {
  await api
    .get('/api/notes')
    .expect(200)
    .expect('Content-Type', /application\/json/)
})

afterAll(() => {
  server.close()
})
```

Toisella rivillä testi käynnistää backendin ja käärii sen kolmannella rivillä funktion _supertest_ avulla ns. [superagent](https://github.com/visionmedia/superagent)-olioksi. Tämä olio sijoitetaan muuttujaan _api_ ja sen kautta testit voivat tehdä HTTP-pyyntöjä backendiin.

Testimetodi tekee HTTP GET -pyynnön osoitteeseen _api/notes_ ja varmistaa, että pyyntöön vastataan statuskoodilla 200 ja että data palautetaan oikeassa muodossa, eli että _Content-Type_:n arvo on _application/json_.

Testissä on muutama detalji joihin tutustumme vasta [hieman myöhemmin](#async-await) tässä osassa. Testikoodin määrittelevä nuolifunktio alkaa sanalla _async_ ja _api_-oliolle tehtyä metodikutsua edeltää sama _await_. Teemme ensin muutamia testejä ja tutustumme sen jälkeen async/await-magiaan. Tällä hetkellä niistä ei tarvitse välittää, kaikki toimii kun kirjoitat testimetodit esimerkin mukaan. Async/await-syntaksin käyttö liittyy siihen, että palvelimelle tehtävät pyynnöt ovat _asynkronisia_ operaatioita. [Async/await-kikalla](https://facebook.github.io/jest/docs/en/asynchronous.html) saamme pyynnön näyttämään koodin tasolla synkroonisesti toimivalta.

Huom! Jos eslint herjaa async -syntaksista, niin saat ongelman korjattua lisäämällä seuraavan `.eslintrc` tiedostoon
```
module.exports = {
  //...
  "parserOptions": {
    "ecmaVersion": 2017
  }
}
```

Kaikkien testien (joita siis tällä kertaa on vain yksi) päätteeksi on vielä lopputoimenpiteenä pyydettävä backendia suorittava _server_-olio sammuttamaan itsensä. Tämä onnistuu helposti metodissa [afterAll](https://facebook.github.io/jest/docs/en/api.html#afterallfn-timeout):

```js
afterAll(() => {
  server.close()
})
```

HTTP-pyyntöjen tiedot loggaava middleware _logger_ häiritsee hiukan testien tulostusta. Jos haluat hiljentää sen testien suorituksen ajaksi, muuta funktiota esim. seuraavasti:

```js
const logger = (request, response, next) => {
  if ( process.env.NODE_ENV === 'test' ) {
    return next()
  }
  console.log('Method:', request.method)
  console.log('Path:  ', request.path)
  console.log('Body:  ', request.body)
  console.log('---')
  next()
}
```


Tehdään pari testiä lisää:

```js
test('there are five notes', async () => {
  const response = await api
    .get('/api/notes')

  expect(response.body.length).toBe(5)
})

test('the first note is about HTTP methods', async () => {
  const response = await api
    .get('/api/notes')

  expect(response.body[0].content).toBe('HTML on helppoa')
})
```

Molemmat testit sijoittavat pyynnön vastauksen muuttujaan _response_ ja toisin kuin edellinen testi, joka käytti _supertestin_ mekanismeja statuskoodin ja vastauksen headereiden oikeellisuuden varmistamiseen, tällä kertaa tutkitaan vastauksessa olevan datan, eli _response.body_:n oikeellisuutta Jestin [expect](https://facebook.github.io/jest/docs/en/expect.html#content):in avulla.


Async/await-kikan hyödyt tulevat nyt selkeästi esiin. Normaalisti tarvitsisimme asynkronisten pyyntöjen vastauksiin käsille pääsemiseen promiseja ja takaisinkutsuja, mutta nyt kaikki menee mukavasti:

```js
const res = await api
  .get('/api/notes')

// tänne tullaan vasta kun edellinen komento eli HTTP-pyyntö on suoritettu
// muuttujassa res on nyt HTTP-pyynnön tulos
expect(res.body.length).toBe(5)
```

Testit menevät läpi. Testit ovat kuitenkin huonoja, niiden läpimeno riippuu tietokannan tilasta (joka sattuu omassa testikannassani olemaan sopiva). Jotta saisimme robustimmat testit, tulee tietokannan tila nollata testien alussa ja sen jälkeen laittaa kantaan hallitusti testien tarvitsema data.

### Error: listen EADDRINUSE :::3002

Jos jotain patologista tapahtuu voi käydä niin, että testien suorittama palvelin jää päälle. Tällöin uusi testiajo aiheuttaa ongelmia, ja seurauksena on virheilmoitus

<pre>
Error: listen EADDRINUSE :::3002
</pre>

Ratkaisu tilanteeseen on tappaa palvelinta suorittava prosessi. Portin 3002 varaava prosessi löytyy OSX:lla ja Linuxilla esim. komennolla <code>lsof -i :3002</code>.

```bash
COMMAND  PID     USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
node    8318 mluukkai   14u  IPv6 0x5428af4833b85e8b      0t0  TCP *:redwood-broker (LISTEN)
```

Windowsissa portin varaavan prosessin näkee resmon.exe:n Verkko-välilehdeltä.

Komennon avulla selviää ikävyyksiä aiheuttavan prosesin PID eli prosessi-id. Prosessin saa tapettua komennolla <code>KILL 8318</code> olettaen että PID on 8318 niin kuin kuvassa. Joskus prosessi on sitkeä eikä kuole ennen kuin se tapetaan komennolla <code>KILL -9 8318</code>.

Windowsissa vastaava komento on <code>taskkill /f /pid 8318</code>.

## Tietokannan alustaminen ennen testejä

Testimme käyttää jo jestin metodia [afterAll](https://facebook.github.io/jest/docs/en/api.html#afterallfn-timeout) sulkemaan backendin testien suoritusten jälkeen. Jest tarjoaa joukon muitakin [funktioita](https://facebook.github.io/jest/docs/en/setup-teardown.html#content), joiden avulla voidaan suorittaa operaatioita ennen yhdenkään testin suorittamista tai ennen jokaisen testin suoritusta.

Päätetään alustaa tietokanta ennen kaikkien testin suoritusta, eli funktiossa [beforeAll](https://facebook.github.io/jest/docs/en/api.html#beforeallfn-timeout):

```js
const supertest = require('supertest')
const {app, server} = require('../index')
const api = supertest(app)
const Note = require('../models/note')

const initialNotes = [
  {
    content: 'HTML on helppoa',
    important: false
  },
  {
    content: 'HTTP-protokollan tärkeimmät metodit ovat GET ja POST',
    important: true
  }
]

beforeAll(async () => {
  await Note.remove({})

  let noteObject = new Note(initialNotes[0])
  await noteObject.save()

  noteObject = new Note(initialNotes[1])
  await noteObject.save()
})
```

Tietokanta siis tyhjennetään aluksi ja sen jälkeen kantaan lisätään kaksi taulukkoon _initialNotes_ talletettua muistiinpanoa. Näin testien suoritus aloitetaan aina hallitusti samasta tilasta.

Muutetaan kahta jälkimmäistä testiä vielä seuraavasti:

```js
test('all notes are returned', async () => {
  const response = await api
    .get('/api/notes')

  expect(response.body.length).toBe(initialNotes.length)
})

test('a specific note is within the returned notes', async () => {
  const response = await api
    .get('/api/notes')

  const contents = response.body.map(r => r.content)

  expect(contents).toContain('HTTP-protokollan tärkeimmät metodit ovat GET ja POST')
})
```

Huomaa jälkimmäisen testin ekspektaatio. Komennolla <code>response.body.map(r => r.content)</code> muodostetaan taulukko API:n palauttamien muistiinpanojen sisällöistä. Jestin [toContain](https://facebook.github.io/jest/docs/en/expect.html#tocontainitem)-ekspektaatiometodilla tarkistetaan että parametrina oleva muistiinpano on kaikkien API:n palauttamien muistiinpanojen joukossa.

Ennen kun teemme lisää testejä, tarkastellaan tarkemmin mitä _async_ ja _await_ tarkoittavat.

## async-await

Async- ja await ovat ES7:n mukanaan tuoma uusi syntaksi, joka mahdollistaa _promisen palauttavien asynkronisten funktioiden_ kutsumisen siten, että kirjoitettava koodi näyttää synkroniselta.

Esim. muistiinpanojen hakeminen tietokannasta hoidetaan promisejen avulla seuraavasti:

```js
Note
  .find({})
  .then(notes => {
    console.log('operaatio palautti seuraavat muistiinpanot', notes)
  })
```

Metodikutsu _Note.find()_ palauttaa promisen, ja saamme itse operaation tuloksen rekisteröimällä promiselle tapahtumankäsittelijän metodilla _then_.

Kaikki operaation suorituksen jälkeinen koodi kirjoitetaan tapahtumankäsittelijään. Jos haluaisimme tehdä peräkkäin useita asynkronisia funktiokutsuja, menisi tilanne ikävämmäksi. Joutuisimme tekemään kutsut tapahtumankäsittelijästä. Näin syntyisi potentiaalisesti monimutkaista koodia, pahimmassa tapauksessa jopa niin sanottu [callback-helvetti](http://callbackhell.com/).

[Ketjuttamalla promiseja](https://javascript.info/promise-chaining) tilanne pysyy jollain tavalla hallinnassa, callback-helvetin eli monien sisäkkäisten callbackien sijaan saadaan aikaan siistihkö _then_-kutsujen ketju. Olemmekin nähneet jo kurssin aikana muutaman sellaisen. Seuraavassa vielä erittäin keinotekoinen esimerkki, joka hakee ensin kaikki muistiinpanot ja sitten tuhoaa niistä ensimmäisen:

```js
Note
  .find({})
  .then(notes => {
    return notes[0].remove()
  })
  .then(response => {
    console.log('the first note is removed')
    // more code here
  })
```

Then-ketju on ok, mutta parempaankin pystytään. Jo ES6:ssa esitellyt [generaattorifunktiot](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator) mahdollistivat [ovelan tavan](https://github.com/getify/You-Dont-Know-JS/blob/master/async%20%26%20performance/ch4.md#iterating-generators-asynchronously) määritellä asynkronista koodia siten että se "näyttää synkroniselta". Syntaksi ei kuitenkaan ole täysin luonteva ja sitä ei käytetä kovin yleisesti.

ES7:ssa _async_ ja _await_ tuovat generaattoreiden tarjoaman toiminnallisuuden ymmärrettävästi ja syntaksin puolesta selkeällä tavalla koko Javascript-kansan ulottuville.

Voisimme hakea tietokannasta kaikki muistiinpanot [await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)-operaattoria hyödyntäen seuraavasti:

```js
const notes = await Note.find({})

console.log('operaatio palautti seuraavat muistiinpanot ', notes)
```

Koodi siis näyttää täsmälleen synkroniselta koodilta. Suoritettavan koodinpätkän suhteen tilanne on se, että suoritus pysähtyy komentoon <code>const notes = await Note.find({})</code> ja jatkuu kyselyä vastaavan promisen _fulfillmentin_ eli onnistuneen suorituksen jälkeen seuraavalta riviltä. Kun suoritus jatkuu, promisea vastaavan operaation tulos on muuttujassa _notes_.

Ylempänä oleva monimutkaisempi esimerkki suoritettaisiin awaitin avulla seuraavasti:

```js
const notes = await Note.find({})
const response = await notes[0].remove()

console.log('the first note is removed')
```

Koodi siis yksinkertaistuu huomattavasti verrattuna promiseja käyttävään then-ketjuun.

Awaitin käyttöön liittyy parikin tärkeää seikkaa. Jotta asynkronisia operaatioita voi kutsua awaitin avulla, niiden täytyy olla promiseja. Tämä ei sinänsä ole ongelma, sillä myös "normaaleja" callbackeja käyttävä asynkroninen koodi on helppo kääriä promiseksi.

Mistä tahansa kohtaa Javascript-koodia ei awaitia kuitenkaan pysty käyttämään. Awaitin käyttö onnistuu ainoastaan jos ollaan [async](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_functio)-funktiossa.

Eli jotta edelliset esimerkit toimisivat, on ne suoritettava async-funktioiden sisällä, huomaa funktion määrittelevä rivi:

```js
const main = async () => {
  const notes = await Note.find({})
  console.log('operaatio palautti seuraavat muistiinpanot', notes)

  const notes = await Note.find({})
  const response = await notes[0].remove()

  console.log('the first note is removed')
}

main()
```

Koodi määrittelee ensin asynkronisen funktion, joka sijoitetaan muuttujaan _main_. Määrittelyn jälkeen koodi kutsuu metodia komennolla <code>main()</code>

### testin beforeAll-metodin optimointi

Palataan takaisin testien pariin, ja tarkastellaan määrittelemäämme testit alustavaa funktiota _beforeAll_:

```js
const initialNotes = [
  {
    content: 'HTML on helppoa',
    important: false
  },
  {
    content: 'HTTP-protokollan tärkeimmät metodit ovat GET ja POST',
    important: true
  }
]

beforeAll(async () => {
  await Note.remove({})

  let noteObject = new Note(initialNotes[0])
  await noteObject.save()

  noteObject = new Note(initialNotes[1])
  await noteObject.save()
})
```

Funktio tallettaa tietokantaan taulukon _initialNotes_ nollannen ja ensimmäisen alkion, kummankin erikseen taulukon alkioita indeksöiden. Ratkaisu on ok, mutta jos haluaisimme tallettaa alustuksen yhteydessä kantaan useampia alkioita, olisi toisto parempi ratkaisu:

```js
beforeAll(async () => {
  await Note.remove({})
  console.log('cleared')

  initialNotes.forEach(async (note) => {
    let noteObject = new Note(note)
    await noteObject.save()
    console.log('saved')
  })
  console.log('done')
})

test('notes are returned as json', async () => {
  console.log('entered test')
  // ...
}
```

Talletamme siis taulukossa _initialNotes_ määritellyt muistiinpanot tietokantaan _forEach_-loopissa. Testeissä kuitenkin ilmenee jotain häikkää, ja sitä varten koodin sisään on lisätty aputulosteita.

Konsoliin tulostuu

<pre>
cleared
done
entered test
saved
saved
</pre>

Yllättäen ratkaisu ei async/awaitista huolimatta toimi niin kuin oletamme, testin suoritus aloitetaan ennen kun tietokannan tila on saatu alustettua!

Ongelma on siinä, että jokainen forEach-loopin läpikäynti generoi oman asynkronisen operaation ja _beforeAll_ ei odota näiden suoritusta. Eli forEach:in sisällä olevat _await_-komennot eivät ole funktiossa _beforeAll_ vaan erillisissä funktioissa joiden päättymistä _beforeAll_ ei odota.

Koska testien suoritus alkaa heti _beforeAll_ metodin suorituksen jälkeen, testien suoritus ehditään jo aloittaa ennen kuin tietokanta on alustettu toivottuun alkutilaan.

Toimiva ratkaisu tilanteessa on odottaa asynkronisten talletusoperaatioiden valmistumista _beforeAll_-funktiossa, esim. metodin [Promise.all](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) avulla:

```js
beforeAll(async () => {
  await Note.remove({})

  const noteObjects = initialNotes.map(note => new Note(note))
  const promiseArray = noteObjects.map(note => note.save())
  await Promise.all(promiseArray)
})

```

Ratkaisu on varmasti aloittelijalle tiiviydestään huolimatta hieman haastava. Taulukkoon _noteObjects_ talletetaan taulukkoon _initialNotes_ talletettuja Javascript-oliota vastaavat _Note_-konstruktorifunktiolla generoidut Mongoose-oliot. Seuraavalla rivillä luodaan uusi taulukko, joka _muodostuu promiseista_, jotka saadaan kun jokaiselle _noteObjects_ taulukon alkiolle kutsutaan metodia _save_, eli ne talletetaan kantaan.

Metodin [Promise.all](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) avulla saadaan koostettua taulukollinen promiseja yhdeksi promiseksi, joka valmistuu, eli menee tilaan _fulfilled_ kun kaikki sen parametrina olevan taulukon promiset ovat valmistuneet.
Siispä viimeinen rivi, <code>await Promise.all(promiseArray)</code> odottaa, että kaikki tietokantaan talletetusta vastaavat promiset ovat valmiina, eli alkiot on talletettu tietokantaan.

> Promise.all-metodia käyttäessä päästään tarvittaessa käsiksi sen parametrina olevien yksittäisten promisejen arvoihin, eli promiseja vastaavien operaatioiden tuloksiin. Jos odotetaan promisejen valmistumista _await_-syntaksilla <code>const results = await Promise.all(promiseArray)</code> palauttaa operaatio taulukon, jonka alkioina on _promiseArray_:n promiseja vastaavat arvot samassa järjestyksessä kuin promiset ovat taulukossa.

Promise.all suorittaa kaikkia syötteenä saamiaan promiseja rinnakkain. Jos operaatioiden suoritusjärjestyksellä on merkitystä, voi tämä aiheuttaa ongelmia. Tällöin asynkroniset operaatiot on mahdollista määrittää [for...of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) lohkon sisällä, jonka suoritusjärjestys on taattu.

```js
beforeAll(async () => {
  await Note.remove({})

  for (let note of initialNotes) {
    let noteObject = new Note(note)
    await noteObject.save()
  }
})
```

Javascriptin asynkroninen suoritusmalli aiheuttaakin siis helposti yllätyksiä ja myös async/await-syntaksin kanssa pitää olla koko ajan tarkkana. Vaikka async/await peittää monia promisejen käsittelyyn liittyviä seikkoja, promisejen toiminta on syytä tuntea mahdollisimman hyvin!

Sovelluksen tämänhetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part3-notes-backend/tree/part4-3), tagissa _part4-3_.


### async/await backendissä

Muutetaan nyt backend käyttämään asyncia ja awaitia. Koska kaikki asynkroniset operaatiot tehdään joka tapauksessa funktioiden sisällä, awaitin käyttämiseen riittää, että muutamme routejen käsittelijät async-funktioiksi.

Kaikkien muistiinpanojen hakemisesta vastaava route muuttuu seuraavasti:

```js
notesRouter.get('/', async (request, response) => {
  const notes = await Note.find({})
  response.json(notes.map(formatNote))
})
```

Voimme varmistaa refaktoroinnin onnistumisen selaimella, sekä suorittamalla juuri määrittelemämme testit.

### ESlint ja async/await nuolifunktioissa

Ennen testejä tehdään pieni täsmennyt ESlint-konfiguraatioon. Tällä hetkellä ESlint valittaa _async_-määreellä varustetuista nuolifunktioista:

![]({{ "/images/4/4b.png" | absolute_url }})

kyse on siitä, että ESlint ei vielä osaa tulkita uutta syntaksia kunnolla. Pääsemme valituksesta eroon asentamalla _babel-eslint_-pluginin:

```bash
npm install babel-eslint --save-dev
```

Pluginin käyttöönotto tulee määritellä tiedostossa _.eslintrc.js_ :

```bash
module.exports = {
  "env": {
    "node": true,
    "es6": true
  },
  "parser": "babel-eslint",
  // ...
}
```

Aiheeton valitus poistuu.

### Testejä ja backendin refaktorointia

Koodia refaktoroidessa vaanii aina [regression](https://en.wikipedia.org/wiki/Regression_testing) vaara, eli on olemassa riski, että jo toimineet ominaisuudet hajoavat. Tehdäänkin muiden operaatioiden refaktorointi siten, että ennen koodin muutosta tehdään jokaiselle API:n routelle sen toiminnallisuuden varmistavat testit.

Aloitetaan lisäysoperaatiosta. Tehdään testi, joka lisää uuden muistiinpanon ja tarkistaa, että API:n palauttamien muistiinpanojen määrä kasvaa, ja että lisätty muistiinpano on palautettujen joukossa:

```js
test('a valid note can be added ', async () => {
  const newNote = {
    content: 'async/await yksinkertaistaa asynkronisten funktioiden kutsua',
    important: true
  }

  await api
    .post('/api/notes')
    .send(newNote)
    .expect(200)
    .expect('Content-Type', /application\/json/)

  const response = await api
    .get('/api/notes')

  const contents = response.body.map(r => r.content)

  expect(response.body.length).toBe(initialNotes.length + 1)
  expect(contents).toContain('async/await yksinkertaistaa asynkronisten funktioiden kutsua')
})
```

Kuten odotimme ja toivoimme, menee testi läpi.

Tehdään myös testi, joka varmistaa, että muistiinpanoa, jolle ei ole asetettu sisältöä, ei talleteta

```js
test('note without content is not added ', async () => {
  const newNote = {
    important: true
  }

  const intialNotes = await api
    .get('/api/notes')

  await api
    .post('/api/notes')
    .send(newNote)
    .expect(400)

  const response = await api
    .get('/api/notes')

  expect(response.body.length).toBe(intialNotes.body.length)
})
```

Testi ei mene läpi.

Käy ilmi, että myös operaation suoritus postman tai Visual Studio Coden REST clientillä johtaa virhetilanteeseen. Koodissa on siis bugi.

> **Huom:** testejä tehdessä täytyy aina varmistua siitä, että testi testaa oikeaa asiaa, ja usein ensimmäistä kertaa testiä tehdessä se että testi ei mene läpi tarkoittaa sitä, että testi on tehty väärin. Myös päinvastaista tapahtuu, eli testi menee läpi mutta koodissa onkin virhe, eli testi ei testaa sitä mitä sen piti testata. Tämän takia testit kannattaa aina "testata" rikkomalla koodi ja varmistamalla, että testi huomaa koodiin tehdyt virheet.

Kun suoritamme operaation postmanilla konsoli paljastaa, että kyseessä on _Unhandled promise rejection_, eli koodi ei käsittele promisen virhetilannetta:

<pre>
Server running on port 3001
Method: POST
Path:   /api/notes/
Body:   { important: true }
---
(node:28657) UnhandledPromiseRejectionWarning: Unhandled promise rejection (rejection id: 1): Error: Can't set headers after they are sent.
(node:28657) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.
</pre>

Kuten jo edellisessä osassa mainittiin, tämä ei ole hyvä idea. Kannattaakin aloittaa lisäämällä promise-ketjuun metodilla _catch_ virheenkäisttelijä, joka tulostaa konsoliin virheen syyn:

```js
notesRouter.post('/', (request, response) => {
  // ...

  note
    .save()
    .then(note => {
      return formatNote(note)
    })
    .then(formattedNote => {
      response.json(formattedNote)
    })
    .catch(error => {
      console.log(error)
      response.status(500).json({ error: 'something went wrong...' })
    })
```

Konsoliin tulostuu seuraava virheilmoitus

<pre>
Error: Can't set headers after they are sent.
    at validateHeader (_http_outgoing.js:489:11)
    at ServerResponse.setHeader (_http_outgoing.js:496:3)
</pre>

Aloittelijalle virheilmoitus ei välttämättä kerro paljoa, mutta googlaamalla virheilmoituksella, pieni etsiminen tuottaisi jo tuloksen.

Kyse on siitä, että koodi kutsuu _response_-olion metodia _send_ kaksi kertaa, tai oikeastaan koodi kutsuu metodia _json_, joka kutsuu edelleen metodia _send_.

Kaksi kertaa tapahtuva _send_-kutsu johtuu siitä, että koodin alun _if_-lauseessa on ongelma:

```js
notesRouter.post('/', (request, response) => {
  const body = request.body

  if (body.content === undefined) {
    response.status(400).json({ error: 'content missing' })
    // suoritus jatkuu!
  }

  //...
}
```

kun koodi kutsuu <code>response.status(400).json(...)</code> suoritus jatkaa koodin alla olevaa osaan ja se taas aiheuttaa uuden <code>response.json()</code>-kutsun.

Korjataan ongelma lisäämällä _if_-lauseeseen _return_:

```js
notesRouter.post('/', (request, response) => {
  const body = request.body

  if (body.content === undefined) {
    return response.status(400).json({ error: 'content missing' })
  }

  //...
}
```

Edellisen osan lopussa koodi oli vielä oikein, mutta siirtäessämme osan alussa koodia tiedostosta _index.js_ uuteen paikkaan, on _return_ kadonnut matkalta.

Promiseja käyttävä koodi toimii nyt ja testitkin menevät läpi. Olemme valmiit muuttamaan koodin käyttämään async/await-syntaksia.

Koodi muuttuu seuraavasti (huomaa, että käsittelijän alkuun on laitettava määre _async_):

```js
notesRouter.post('/', async (request, response) => {
  const body = request.body

  if (body.content === undefined) {
    return response.status(400).json({ error: 'content missing' })
  }

  const note = new Note({
    content: body.content,
    important: body.important === undefined ? false : body.important,
    date: new Date()
  })

  const savedNote = await note.save()
  response.json(formatNote(savedNote))
})
```

Koodiin jää kuitenkin pieni ongelma: virhetilanteita ei nyt käsitellä ollenkaan. Miten niiden suhteen tulisi toimia?

### virheiden käsittely ja async/await

Jos sovellus POST-pyyntöä käsitellessään aiheuttaa jonkinlaisen ajonaikaisen virheen, syntyy jälleen tuttu tilanne:

<pre>
(node:30644) UnhandledPromiseRejectionWarning: Unhandled promise rejection (rejection id: 1): TypeError: formattedNote.nonexistingMethod is not a function
</pre>

eli käsittelemätön promisen rejektoituminen. Pyyntöön ei vastata tilanteessa mitenkään.

Async/awaitia käyttäessä kannattaa käyttää vanhaa kunnon _try/catch_-mekanismia virheiden käsittelyyn:

```js
notesRouter.post('/', async (request, response) => {
  try {
    const body = request.body

    if (body.content === undefined) {
      return response.status(400).json({ error: 'content missing' })
    }

    const note = new Note({
      content: body.content,
      important: body.important === undefined ? false : body.important,
      date: new Date()
    })

    const savedNote = await note.save()
    response.json(formatNote(note))
  } catch (exception) {
    console.log(exception)
    response.status(500).json({ error: 'something went wrong...' })
  }
})
```

Iso try/catch tuo koodiin hieman ikävän vivahteen, mutta mikään ei ole ilmaista.

Tehdään sitten testit yksittäisen muistiinpanon tietojen katsomiselle ja muistiinpanon poistolle:

```js
test('a specific note can be viewed', async () => {
  const resultAll = await api
    .get('/api/notes')
    .expect(200)
    .expect('Content-Type', /application\/json/)

  const aNoteFromAll = resultAll.body[0]

  const resultNote = await api
    .get(`/api/notes/${aNoteFromAll.id}`)

  const noteObject = resultNote.body

  expect(noteObject).toEqual(aNoteFromAll)
})

test('a note can be deleted', async () => {
  const newNote = {
    content: 'HTTP DELETE poistaa resurssin',
    important: true
  }

  const addedNote = await api
    .post('/api/notes')
    .send(newNote)

  const notesAtBeginningOfOperation = await api
    .get('/api/notes')

  await api
    .delete(`/api/notes/${addedNote.body.id}`)
    .expect(204)

  const notesAfterDelete = await api
    .get('/api/notes')

  const contents = notesAfterDelete.body.map(r => r.content)

  expect(contents).not.toContain('HTTP DELETE poistaa resurssin')
  expect(notesAfterDelete.body.length).toBe(notesAtBeginningOfOperation.body.length - 1)
})
```

Testit eivät tässä vaiheessa ole optimaaliset, parannetaan niitä kohta. Ensin kuitenkin refaktoroidaan backend käyttämään async/awaitia.

```js
notesRouter.get('/:id', async (request, response) => {
  try {
    const note = await Note.findById(request.params.id)

    if (note) {
      response.json(formatNote(note))
    } else {
      response.status(404).end()
    }

  } catch (exception) {
    console.log(exception)
    response.status(400).send({ error: 'malformatted id' })
  }
})

notesRouter.delete('/:id', async (request, response) => {
  try {
    await Note.findByIdAndRemove(request.params.id)

    response.status(204).end()
  } catch (exception) {
    console.log(exception)
    response.status(400).send({ error: 'malformatted id' })
  }
})
```

Async/await ehkä selkeyttää koodia jossain määrin, mutta saavutettava hyöty ei ole sovelluksessamme vielä niin iso mitä se tulee olemaan jos asynkronisia kutsuja on tehtävä useampia.

Kaikki eivät kuitenkaan ole vakuuttuneita siitä, että async/await on hyvä lisä Javascriptiin, lue esim. [ES7 async functions - a step in the wrong direction](https://spion.github.io/posts/es7-async-await-step-in-the-wrong-direction.html)

Sovelluksen tämänhetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part3-notes-backend/tree/part4-4), tagissa _part4-4_. Samassa on "vahingossa" mukana testeistä seuraavan luvun jälkeinen paranneltu versio.

### Varoitus

Jos huomaat kirjoittavasi sekaisin async/awaitia ja _then_-kutsuja, on 99% varmaa, että teet jotain väärin. Käytä siis jompaa kumpaa tapaa, älä missään tapauksessa "varalta" molempia.

## Tehtäviä

Tee nyt tehtävät [4.8-4.11](/tehtävät#api:n-testaaminen)

## Testien refaktorointi

Testimme sisältävät tällä hetkellä jossain määrin toisteisuutta ja niiden rakenne ei ole optimaalinen. Testit ovat myös osittain epätäydelliset, esim. reittejä GET /api/notes/:id ja DELETE /api/notes/:id ei tällä hetkellä testata epävalidien id:iden osalta.

Testeissä on myös eräs hieman ikävä ja jopa riskialtis piirre. Testit luottavat siihen, että ne suoritetaan siinä järjestyksessä, missä ne on kirjoitettu testitiedostoon. Tämä pitää kyllä paikkansa, vaikkakin se ei ole kovin selkeästi määritelty ominaisuus eli siihen ei ole hyvä luottaa. Testit tuleekin kirjoittaa siten, että yksittäiset testit ovat riippumattoimia toistensa suorituksesta.

Parannellaan testejä hiukan.

Tehdään testejä varten muutama apufunktio moduuliin _tests/test_helper.js_

```js
const Note = require('../models/note')

const initialNotes = [
  {
    content: 'HTML on helppoa',
    important: false
  },
  {
    content: 'HTTP-protokollan tärkeimmät metodit ovat GET ja POST',
    important: true
  }
]

const format = (note) => {
  return {
    content: note.content,
    important: note.important,
    id: note._id
  }
}

const nonExistingId = async () => {
  const note = new Note()
  await note.save()
  await note.remove()

  return note._id.toString()
}

const notesInDb = async () => {
  const notes = await Note.find({})
  return notes.map(format)
}

module.exports = {
  initialNotes, format, nonExistingId, notesInDb
}
```

Tärkein apufunktioista on _notesInDb_ joka palauttaa kaikki tietokannassa kutsuhetkellä olevat oliot.

Jossain määrin parannellut testit seuraavassa:

```js
const supertest = require('supertest')
const { app, server } = require('../index')
const api = supertest(app)
const Note = require('../models/note')
const { format, initialNotes, nonExistingId, notesInDb } = require('./test_helper')

describe('when there is initially some notes saved', async () => {
  beforeAll(async () => {
    await Note.remove({})

    const noteObjects = initialNotes.map(n => new Note(n))
    await Promise.all(noteObjects.map(n => n.save()))
  })

  test('all notes are returned as json by GET /api/notes', async () => {
    const notesInDatabase = await notesInDb()

    const response = await api
      .get('/api/notes')
      .expect(200)
      .expect('Content-Type', /application\/json/)

    expect(response.body.length).toBe(notesInDatabase.length)

    const returnedContents = response.body.map(n => n.content)
    notesInDatabase.forEach(note => {
      expect(returnedContents).toContain(note.content)
    })
  })

  test('individual notes are returned as json by GET /api/notes/:id', async () => {
    const notesInDatabase = await notesInDb()
    const aNote = notesInDatabase[0]

    const response = await api
      .get(`/api/notes/${aNote.id}`)
      .expect(200)
      .expect('Content-Type', /application\/json/)

    expect(response.body.content).toBe(aNote.content)
  })

  test('404 returned by GET /api/notes/:id with nonexisting valid id', async () => {
    const validNonexistingId = await nonExistingId()

    const response = await api
      .get(`/api/notes/${validNonexistingId}`)
      .expect(404)
  })

  test('400 is returned by GET /api/notes/:id with invalid id', async () => {
    const invalidId = "5a3d5da59070081a82a3445"

    const response = await api
      .get(`/api/notes/${invalidId}`)
      .expect(400)
  })

  describe('addition of a new note', async () => {

    test('POST /api/notes succeeds with valid data', async () => {
      const notesAtStart = await notesInDb()

      const newNote = {
        content: 'async/await yksinkertaistaa asynkronisten funktioiden kutsua',
        important: true
      }

      await api
        .post('/api/notes')
        .send(newNote)
        .expect(200)
        .expect('Content-Type', /application\/json/)

      const notesAfterOperation = await notesInDb()

      expect(notesAfterOperation.length).toBe(notesAtStart.length + 1)

      const contents = notesAfterOperation.map(r => r.content)
      expect(contents).toContain('async/await yksinkertaistaa asynkronisten funktioiden kutsua')
    })

    test('POST /api/notes fails with proper statuscode if content is missing', async () => {
      const newNote = {
        important: true
      }

      const notesAtStart = await notesInDb()

      await api
        .post('/api/notes')
        .send(newNote)
        .expect(400)

      const notesAfterOperation = await notesInDb()

      const contents = notesAfterOperation.map(r => r.content)

      expect(notesAfterOperation.length).toBe(notesAtStart.length)
    })
  })

  describe('deletion of a note', async () => {
    let addedNote

    beforeAll(async () => {
      addedNote = new Note({
        content: 'poisto pyynnöllä HTTP DELETE',
        important: false
      })
      await addedNote.save()
    })

    test('DELETE /api/notes/:id succeeds with proper statuscode', async () => {
      const notesAtStart = await notesInDb()

      await api
        .delete(`/api/notes/${addedNote._id}`)
        .expect(204)

      const notesAfterOperation = await notesInDb()

      const contents = notesAfterOperation.map(r => r.content)

      expect(contents).not.toContain(addedNote.content)
      expect(notesAfterOperation.length).toBe(notesAtStart.length - 1)
    })
  })

  afterAll(() => {
    server.close()
  })

})
```

Muutama huomio testeistä. Olemme jaotelleet testejä [desribe](http://facebook.github.io/jest/docs/en/api.html#describename-fn)-lohkojen avulla ja muutamissa lohkoissa on oma [beforeAll](http://facebook.github.io/jest/docs/en/api.html#beforeallfn-timeout)-funktiolla suoritettava alustuskoodi.

Joissain tapauksissa tämä olisi parempi tehdä operaatioilla [beforeEach](https://facebook.github.io/jest/docs/en/api.html#beforeeachfn-timeout), joka suoritetaan _ennen jokaista testiä_, näin testeistä saisi varmemmin toisistaan riippumattomia. Esimerkissä beforeEachia ei kuitenkaan ole käytetty.

Testien raportointi tapahtuu _describe_-lohkojen ryhmittelyn mukaan:

![]({{ "/assets/4/5.png" | absolute_url }})

Backendin tietokannan tilaa muuttavat testit, esim. uuden muistiinpanon lisäämistä testaava testi _'addition of a new note'_, on tehty siten, että ne ensin aluksi selvittävät tietokannan tilan apufunktiolla _notesInDb()_

```js
const notesAtBeginningOfOperation = await notesInDb()
```

suorittavat testattavan operaation:

```js
const newNote = {
  content: 'async/await yksinkertaistaa asynkronisten funktioiden kutsua',
  important: true
}

await api
  .post('/api/notes')
  .send(newNote)
  .expect(200)
  .expect('Content-Type', /application\/json/)
```

selvittävät tietokannan tilan operaation jälkeen

```js
const notesAfterOperation = await notesInDb()
```

ja varmentavat, että operaation suoritus vaikutti tietokantaan halutulla tavalla

```js
expect(notesAfterOperation.length).toBe(notesAtBeginningOfOperation.length + 1)

const contents = notesAfterOperation.map(r => r.content)
expect(contents).toContain('async/await yksinkertaistaa asynkronisten funktioiden kutsua')
```

Testeihin jää vielä paljon parannettavaa mutta on jo aika siirtyä eteenpäin.

Käytetty tapa API:n testaamiseen, eli HTTP-pyyntöinä tehtävät operaatiot ja tietokannan tilan tarkastelu Mongoosen kautta ei ole suinkaan ainoa tai välttämättä edes paras tapa tehdä API-tason integraatiotestausta. Universaalisti parasta tapaa testien tekoon ei ole, vaan kaikki on aina suhteessa käytettäviin resursseihin ja testattavaan ohjelmistoon.

## Tehtäviä

Tee nyt tehtävät [4.12-4.14](/tehtävät#lisää-toiminnallisuutta-ja-testejä)

## Käyttäjien hallinta ja monimutkaisempi tietokantaskeema

Haluamme toteuttaa sovellukseemme käyttäjien hallinnan. Käyttäjät tulee tallettaa tietokantaan ja jokaisesta muistiinpanosta tulee tietää sen luonut käyttäjä. Muistiinpanojen poisto ja editointi tulee olla sallittua ainoastaan muistiinpanot tehneelle käyttäjälle.

Aloitetaan lisäämällä tietokantaan tieto käyttäjistä. Käyttäjän (User) ja muistiinpanojen (Note) välillä on yhden suhde moneen -yhteys:

![](https://yuml.me/a187045b.png)

Relaatiotietokantoja käytettäessä ratkaisua ei tarvitsisi juuri miettiä. Molemmille olisi oma taulunsa ja muistiinpanoihin liitettäisiin sen luonutta käyttäjää vastaava id vierasavaimeksi (foreign key).

Dokumenttitietokantoja käytettäessä tilanne on kuitenkin toinen, erilaisia tapoja mallintaa tilanne on useita.

Olemassaoleva ratkaisumme tallentaa jokaisen luodun muistiinpanon tietokantaan _notes_-kokoelmaan eli _collectioniin_. Jos emme halua muuttaa tätä, lienee luontevinta tallettaa käyttäjät omaan kokoelmaansa, esim. nimeltään _users_.

Mongossa voidaan kaikkien dokumenttitietokantojen tapaan käyttää olioiden id:itä viittaamaan muissa kokoelmissa talletettaviin dokumentteihin, vastaavasti kuten viiteavaimia käytetään relaatiotietokannoissa.

Dokumenttitietokannat kuten Mongo eivät kuitenkaan tue relaatiotietokantojen _liitoskyselyitä_ vastaavaa toiminnallisuutta, joka mahdollistaisi useaan kokoelmaan kohdistuvan tietokantahaun (tämä ei ole tarkalleen ottaen enää välttämättä pidä paikkaansa, sillä versiosta 3.2. alkaen Mongo on tukenut useampaan kokoelmaan kohdistuvia [lookup-aggregaattikyselyitä](https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/), emme kuitenkaan käsittele niitä kurssilla).

Jos tarvitsemme liitoskyselyitä vastaavaa toiminnallisuutta, tulee se toteuttaa sovelluksen tasolla, eli käytännössä tekemällä tietokantaan useita kyselyitä. Tietyissä tilanteissa mongoose-kirjasto osaa hoitaa liitosten tekemisen, jolloin kysely näyttää mongoosen käyttäjälle toimivan liitoskyselyn tapaan. Mongoose tekee kuitenkin näissä tapauksissa taustalla useamman kyselyn tietokantaan.

### Viitteet kokoelmien välillä

Jos käyttäisimme relaatiotietokantaa, muistiinpano sisältäisi _viiteavaimen_ sen tehneeseen käyttäjään. Dokumenttitietokannassa voidaan toimia samoin.

Oletetaan että kokoelmassa _users_ on kaksi käyttäjää:

```js
[
  {
    username: 'mluukkai',
    _id: 123456
  },
  {
    username: 'hellas',
    _id: 141414
  }
]
```

Kokoelmassa _notes_ on kolme muistiinpanoa, kaikkien kenttä _user_ viittaa _users_-kentässä olevaan käyttäjään:

```js
[
  {
    content: 'HTML on helppoa',
    important: false,
    _id: 221212,
    user: 123456
  },
  {
    content: 'HTTP-protokollan tärkeimmät metodit ovat GET ja POST',
    important: true,
    _id: 221255,
    user: 123456
  },
  {
    content: 'Java on kieli, jota käytetään siihen asti kunnes aurinko sammuu',
    important: false,
    _id: 221244,
    user: 141414
  }
]
```

Mikään ei kuitenkaan määrää dokumenttitietokannoissa, että viitteet on talletettava muistiinpanoihin, ne voivat olla _myös_ (tai ainoastaan) käyttäjien yhteydessä:

```js
[
  {
    username: 'mluukkai',
    _id: 123456,
    notes: [221212, 221255]
  },
  {
    content: 'hellas',
    _id: 141414,
    notes: [141414]
  }
]
```

Koska käyttäjiin liittyy potentiaalisesti useita muistiinpanoja, niiden id:t talletetaan käyttäjän kentässä _notes_ olevaan taulukkoon.


Dokumenttitietokannat tarjoavat myös radikaalisti erilaisen tavan datan organisointiin, joissain tilanteissa saattaisi olla mielekästä tallettaa muistiinpanot kokonaisuudessa käyttäjien sisälle:

```js
[
  {
    username: 'mluukkai',
    _id: 123456,
    notes: [
      {
        content: 'HTML on helppoa',
        important: false
      },
      {
        content: 'HTTP-protokollan tärkeimmät metodit ovat GET ja POST',
        important: true
      }
    ]
  },
  {
    content: 'hellas',
    _id: 141414,
    notes: [
      {
        content: 'Java on kieli, jota käytetään siihen asti kunnes aurinko sammuu',
        important: false
      }
    ]
  }
]
```

Muistiinpanot olisivat tässä skeemaratkaisussa siis yhteen käyttäjään alisteisia kenttiä, niillä ei olisi edes omaa identiteettiä, eli id:tä tietokannan tasolla.

Dokumenttitietokantojen yhteydessä skeeman rakenne ei siis ole ollenkaan samalla tavalla ilmeinen kuin relaatiotietokannoissa, ja valittava ratkaisu kannattaa määritellä siten että se tukee parhaalla tavalla sovelluksen käyttötapauksia. Tämä ei luonnollisestikaan ole helppoa, sillä järjestelmän kaikki käyttötapaukset eivät yleensä ole selvillä kun projektin alkuvaiheissa mietitään datan organisointitapaa.

Hieman paradoksaalisesti tietokannan tasolla skeematon Mongo edellyttääkin projektin alkuvaiheissa jopa radikaalimpien datan organisoimiseen liittyvien ratkaisujen tekemistä kuin tietokannan tasolla skeemalliset relaatiotietokannat, jotka tarjoavat keskimäärin kaikkiin tilanteisiin melko hyvin sopivan tavan organisoida dataa.

### Käyttäjien mongoose-skeema

Päätetään tallettaa käyttäjän yhteyteen myös tieto käyttäjän luomista muistiinpanoista, eli käytännössä muistiinpanojen id:t. Määritellään käyttäjää edustava model tiedostoon _models/user:_

```js
const mongoose = require('mongoose')

const User = mongoose.model('User', {
  username: String,
  name: String,
  passwordHash: String,
  notes: [{ type: mongoose.Schema.Types.ObjectId, ref: 'Note' }]
})

module.exports = User
```

Muistiinpanojen id:t on talletettu käyttäjien sisälle taulukkona mongo-id:itä. Määrittely on seuraava

```js
{ type: mongoose.Schema.Types.ObjectId, ref: 'Note' }
```

kentän tyyppi on _ObjectId_ joka viittaa _Note_-tyyppisiin dokumentteihin. Mongo ei itsessään tiedä mitään siitä, että kyse on kentästä joka viittaa nimenomaan muistiinpanoihin, kyseessä onkin puhtaasti mongoosen syntaksi.

Laajennetaan tiedostossa _model/note.js_ olevaa muistiinpanon skeemaa siten, että myös muistiinpanossa on tieto sen luoneesta käyttäjästä


```js
const Note = mongoose.model('Note', {
  content: String,
  date: Date,
  important: Boolean,
  user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' }
})
```

Relaatiotietokantojen käytänteistä poiketen _viitteet on nyt talletettu molempiin dokumentteihin_, muistiinpano viittaa sen luoneeseen käyttäjään ja käyttäjä sisältää taulukollisen viitteitä sen luomiin muistiinpanoihin.

### Käyttäjien luominen

Toteutetaan seuraavaksi route käyttäjien luomista varten. Käyttäjällä on siis _username_ jonka täytyy olla järjestelmässä yksikäsitteinen, nimi eli _name_ sekä _passwordHash_, eli salasanasta [yksisuuntaisen funktion](https://en.wikipedia.org/wiki/Cryptographic_hash_function) perusteella laskettu tunniste. Salasanojahan ei ole koskaan viisasta tallentaa tietokantaan selväsanaisena!

Asennetaan salasanojen hashaamiseen käyttämämme [bcrypt](https://github.com/kelektiv/node.bcrypt.js)-kirjasto:

```bash
npm install bcrypt --save
```

Käyttäjien luominen tapahtuu osassa 3 läpikäytyjä [RESTful](/osa3#rest)-periaatteita seuraten tekemällä HTTP POST -pyyntö polkuun _users_.

Määritellään käyttäjienhallintaa varten oma _router_ tiedostoon _controllers/users.js_, ja liitetään se _index.js_-tiedostossa huolehtimaan polulle _/api/users/_ tulevista pyynnöistä:

```js
const usersRouter = require('./controllers/users')

// ...

app.use('/api/users', usersRouter)
```

Routerin alustava sisältö on seuraava:

```js
const bcrypt = require('bcrypt')
const usersRouter = require('express').Router()
const User = require('../models/user')

usersRouter.post('/', async (request, response) => {
  try {
    const body = request.body

    const saltRounds = 10
    const passwordHash = await bcrypt.hash(body.password, saltRounds)

    const user = new User({
      username: body.username,
      name: body.name,
      passwordHash
    })

    const savedUser = await user.save()

    response.json(savedUser)
  } catch (exception) {
    console.log(exception)
    response.status(500).json({ error: 'something went wrong...' })
  }
})

module.exports = usersRouter
```

Tietokantaan siis _ei_ talleteta pyynnön mukana tulevaa salasanaa, vaan funktion _bcrypt.hash_ avulla laskettu _hash_.

Materiaalin tilamäärä ei valitettavasti riitä käsittelemään sen tarkemmin salasanojen [tallennuksen perusteita](https://codahale.com/how-to-safely-store-a-password/), esim. mitä maaginen luku 10 muuttujan [saltRounds](https://github.com/kelektiv/node.bcrypt.js/#a-note-on-rounds) arvona tarkoittaa. Lue linkkien takaa lisää.

Koodissa ei tällä hetkellä ole mitään virheidenkäsittelyä eikä validointeja, eli esim. käyttäjätunnuksen ja salasanan halutun muodon tarkastuksia.

Uutta ominaisuutta voidaan ja kannattaakin joskus testailla käsin esim. postmanilla. Käsin tapahtuva testailu muuttuu kuitenkin nopeasti työlääksi, etenkin kun tulemme pian vaatimaan, että samaa käyttäjätunnusta ei saa tallettaa kantaan kahteen kertaan.

Pienellä vaivalla voimme tehdä automaattisesti suoritettavat testit, jotka helpottavat sovelluksen kehittämistä merkittävästi.

Alustava testi näyttää seuraavalta:

```js
const User = require('../models/user')
const { format, initialNotes, nonExistingId, notesInDb, usersInDb } = require('./test_helper')

//...

describe.only('when there is initially one user at db', async () => {
  beforeAll(async () => {
    await User.remove({})
    const user = new User({ username: 'root', password: 'sekret' })
    await user.save()
  })

  test('POST /api/users succeeds with a fresh username', async () => {
    const usersBeforeOperation = await usersInDb()

    const newUser = {
      username: 'mluukkai',
      name: 'Matti Luukkainen',
      password: 'salainen'
    }

    await api
      .post('/api/users')
      .send(newUser)
      .expect(200)
      .expect('Content-Type', /application\/json/)

    const usersAfterOperation = await usersInDb()
    expect(usersAfterOperation.length).toBe(usersBeforeOperation.length+1)
    const usernames = usersAfterOperation.map(u=>u.username)
    expect(usernames).toContain(newUser.username)
  })
})
```

Koska testi on määritelty [describe.only](https://facebook.github.io/jest/docs/en/api.html#describeonlyname-fn)-lohkoksi, suorittaa _Jest_ ainoastaan lohkon sisälle määritellyt testit. Tämä on alkuvaiheessa hyödyllistä, sillä ennen kuin uusia käyttäjiä lisäävä toiminnallisuus on valmis, kannattaa suorittaa testeistä ainoastaan kyseistä toiminnallisuutta tutkivat testitapaukset.

Testit käyttävät myös tiedostossa _tests/test_helper.js_ määriteltyä apufunktiota _usersInDb()_ tarkastamaan lisäysoperaation jälkeisen tietokannan tilan:

```js
const User = require('../models/user')

// ...

const usersInDb = async () => {
  const users = await User.find({})
  return users
}

module.exports = {
  initialNotes, format, nonExistingId, notesInDb, usersInDb
}
```

Lohkon _beforeAll_ lisää kantaan käyttäjän, jonka username on _root_. Voimmekin tehdä uuden testin, jolla varmistetaan, että samalla käyttäjätunnuksella ei voi luoda uutta käyttäjää:

```js
test('POST /api/users fails with proper statuscode and message if username already taken', async () => {
  const usersBeforeOperation = await usersInDb()

  const newUser = {
    username: 'root',
    name: 'Superuser',
    password: 'salainen'
  }

  const result = await api
    .post('/api/users')
    .send(newUser)
    .expect(400)
    .expect('Content-Type', /application\/json/)

  expect(result.body).toEqual({ error: 'username must be unique'})

  const usersAfterOperation = await usersInDb()
  expect(usersAfterOperation.length).toBe(usersBeforeOperation.length)
})
```

Testi ei tietenkään mene läpi tässä vaiheessa. Toimimme nyt oleellisesti [TDD:n eli test driven developmentin](https://en.wikipedia.org/wiki/Test-driven_development) hengessä, uuden ominaisuuden testi on kirjoitettu ennen ominaisuuden ohjelmointia.

Koodi laajenee seuraavasti:

```js
usersRouter.post('/', async (request, response) => {
  try {
    const body = request.body

    const existingUser = await User.find({username: body.username})
    if (existingUser.length>0) {
      return response.status(400).json({ error: 'username must be unique' })
    }

    //...

  }
})
```

Eli haetaan tietokannasta ne user-dokumentit, joiden _username_-kentän arvo on sama kuin pyynnössä oleva. Jos sellainen user-dokumentti löytyy, vastataan pyyntöön statuskoodilla _400 bad request_ ja kerrotaan syy ongelmaan.

Voisimme toteuttaa käyttäjien luomisen yhteyteen myös muita tarkistuksia, esim. onko käyttäjätunnus tarpeeksi pitkä, koostuuko se sallituista merkeistä ja onko salasana tarpeeksi hyvä. Jätämme ne kuitenkin harjoitustehtäväksi.

Ennen kuin menemme eteenpäin, lisätään alustava versio joka palauttaa kaikki käyttäjät palauttavasta käsittelijäfunktiosta:

```js
const formatUser = (user) => {
  return {
    id: user.id,
    username: user.username,
    name: user.name,
    notes: user.notes
  }
}

usersRouter.get('/', async (request, response) => {
  const users = await User.find({})
  response.json(users.map(formatUser))
})
```

Lista näyttää seuraavalta

![]({{ "/images/4/5b.png" | absolute_url }})

### Formatointifunktioiden siirto modelien märittelyn yhteyteen

Kuten muistinpanojenkin tapauksessa, olemme myös nyt määritellet apufunktion _formatUser_, joka muodostaa tietokannan palauttamista _user_-olioista selaimelle lähetettävän muodon, joista on mm. poistettu kenttä _passwordHash_.

Formatointifunktio on nyt sijoitettu routejen määrittelyn yhteyteen. Paikka ei välttämättä ole optimaalinen ja päätetäänkin viedä formatointi _User_-skeeman vastuulle, sen [staattiseksi metodiksi](http://mongoosejs.com/docs/guide.html#statics).

Tehdään seuraava muutos tiedostoon _models/user.js_:

```js
const mongoose = require('mongoose')

const userSchema = new mongoose.Schema({
  username: String,
  name: String,
  passwordHash: String,
  notes: [{ type: mongoose.Schema.Types.ObjectId, ref: 'Note' }]
})

userSchema.statics.format = (user) => {
  return {
    id: user.id,
    username: user.username,
    name: user.name,
    notes: user.notes
  }
}

const User = mongoose.model('User', userSchema)

module.exports = User
```

Näin määriteltyä metodia kutsutaan _User.format(user)_. Voimme muuttaa tiedostosta _controllesr/users.js_ olevat routet seuraavaan muotoon:

```js
usersRouter.get('/', async (request, response) => {
  const users = await User.find({})
  response.json(users.map(User.format))
})

usersRouter.post('/', async (request, response) => {
  try {
    // ...
    const savedUser = await user.save()

    response.json(User.format(savedUser))
  } catch (exception) {
    // ...
  }
})
```

Formatointifunktion määritteleminen skeeman määrittelyn yhteydessä on sikäli luontevaa, että jos skeemaan tulee muutoksia, on formatointifunktio samassa tiedostossa ja todennäköisyys sen päivittämisen unohtamiselle pienenee.

Tehdään sama muutos muistiinpanojen formatointiin, eli muutetaan _models/note.js_ muotoon

```js
const mongoose = require('mongoose')

const noteSchema = new mongoose.Schema({
  content: String,
  date: Date,
  important: Boolean,
  user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' }
})

noteSchema.statics.format = (note) => {
  return {
    id: note._id,
    content: note.content,
    date: note.date,
    important: note.important
  }
}

const Note = mongoose.model('Note', noteSchema)

module.exports = Note
```

ja muutetaan tiedostosta _controllers/notes.js_ metotodikutsut _formatNote(note)_ muotoon _Note.format(note)_ ja kutsu _notes.map(formatNote)_ muotoon _notes.map(Note.format)_

Testien suoritus varmistaa, että sovelluksemme ei hajonnut refaktoroinnin myötä.

Pääsemme nyt eroon myös testien yhteyteen määritellystä muistiinpanoja formatoivasta apumetodista _format_, sillä myös testeissä kannattaa hyödyntää funktiota _Note.format_.

Sovelluksen tämänhetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part3-notes-backend/tree/part4-5), tagissa _part4-5_.

### Muistiinpanon luominen

Muistiinpanot luovaa koodia on nyt mukautettava siten, että uusi muistiinpano tulee liitetyksi sen luoneeseen käyttäjään.

Laajennetaan ensin olemassaolevaa toteutusta siten, että tieto muistiinpanon luovan käyttäjän id:stä lähetetään pyynnön rungossa kentän _userId_ arvona:

```js
const User = require('../models/user')

//...

notesRouter.post('/', async (request, response) => {
  try {
    const body = request.body

    if (body.content === undefined) {
      return response.status(400).json({ error: 'content missing' })
    }

    const user = await User.findById(body.userId)

    const note = new Note({
      content: body.content,
      important: body.important === undefined ? false : body.important,
      date: new Date(),
      user: user._id
    })

    const savedNote = await note.save()

    user.notes = user.notes.concat(savedNote._id)
    await user.save()

    response.json(Note.format(note))
  } catch(exception) {
    console.log(exception)
    response.status(500).json({ error: 'something went wrong...' })
  }
})
```

Huomionarvoista on nyt se, että myös _user_-olio muuttuu. Sen kenttään _notes_ talletetaan luodun muistiinpanon _id_:

```js
const user = User.findById(userId)

user.notes = user.notes.concat(savedNote._id)
await user.save()
```

Kokeillaan nyt lisätä uusi muistiinpano

![]({{ "/assets/4/6.png" | absolute_url }})

Operaatio vaikuttaa toimivan. Lisätään vielä yksi muistiinpano ja mennään kaikkien käyttäjien sivulle:

![]({{ "/assets/4/7.png" | absolute_url }})

Huomaamme siis, että käyttäjällä on kaksi muistiinpanoa.

Jos laajennamme muistiinpanojen JSON:in muotoileman koodin näyttämään muistiinpanoon liittyvän käyttäjän

```js
noteSchema.statics.format = (note) => {
  return {
    id: note._id,
    content: note.content,
    date: note.date,
    important: note.important,
    user: note.user
  }
}
```

tulee muistiinpanon luoneen käyttäjän id näkyviin muistiinpanon yhteyteen.

![]({{ "/assets/4/8.png" | absolute_url }})

### populate

Haluaisimme API:n toimivan siten, että haettaessa esim. käyttäjien tiedot polulle _/api/users_ tehtävällä HTTP GET -pyynnöllä tulisi käyttäjien tekemien muistiinpanojen id:iden lisäksi näyttää niiden sisältö. Relaatiotietokannoilla toiminnallisuus toteutettaisiin _liitoskyselyn_ avulla.

Kuten aiemmin mainittiin, eivät dokumenttitietokannat tue (kunnolla) eri kokoelmien välisiä liitoskyselyitä. Mongoose-kirjasto osaa kuitenkin tehdä liitoksen puolestamme. Mongoose toteuttaa liitoksen tekemällä useampia tietokantakyselyitä, joten siinä mielessä kyseessä on täysin erilainen tapa kuin relaatiotietokantojen liitoskyselyt, jotka ovat _transaktionaalisia_, eli liitoskyselyä tehdessä tietokannan tila ei muutu. Mongoosella tehtävä liitos taas on sellainen, että mikään ei takaa sitä, että liitettävien kokoelmien tila on konsistentti, toisin sanoen jos tehdään users- ja notes-kokoelmat liittävä kysely, kokoelmien tila saattaa muuttua kesken mongoosen liitosoperaation.

Liitoksen tekeminen suoritetaan mongoosen komennolla [populate](http://mongoosejs.com/docs/populate.html). Päivitetään ensin kaikkien käyttäjien tiedot palauttava route:

```js
usersRouter.get('/', async (request, response) => {
  const users = await User
    .find({})
    .populate('notes')

  response.json(users.map(User.format))
})
```

Funktion [populate](http://mongoosejs.com/docs/populate.html) kutsu siis ketjutetaan kyselyä vastaavan metodikutsun (tässä tapauksessa _find_) perään. Populaten parametri määrittelee, että _user_-dokumenttien _notes_-kentässä olevat _note_-olioihin viittaavat _id_:t korvataan niitä vastaavilla dokumenteilla.

Lopputulos on jo melkein haluamamme kaltainen:

![]({{ "/images/4/9a.png" | absolute_url }})

Populaten yhteydessä on myös mahdollista rajata mitä kenttiä sisällytettävistä dokumenteista otetaan mukaan. Rajaus tapahtuu Mongon [syntaksilla](https://docs.mongodb.com/manual/tutorial/project-fields-from-query-results/#return-the-specified-fields-and-the-id-field-only):

```js
usersRouter.get('/', async (request, response) => {
  const users = await User
    .find({})
    .populate('notes', { content: 1, date: 1 } )

  response.json(users.map(User.format))
})
```

Tulos on nyt halutun kaltainen (:

![]({{ "/images/4/10a.png" | absolute_url }})

Lisätään sopiva käyttäjän tietojen populointi, muistiinpanojen yhteyteen:

```js
notesRouter.get('/', async (request, response) => {
  const notes = await Note
    .find({})
    .populate('user', { username: 1, name: 1 } )

  response.json(notes.map(Note.format))
})
```

Nyt käyttäjän tiedot tulevat muistiinpanon kenttään _user_.

![]({{ "/images/4/11a.png" | absolute_url }})

Korostetaan vielä, että tietokannan tasolla ei siis ole mitään määrittelyä siitä, että esim. muistiinpanojen kenttään _user_ talletetut id:t viittaavat käyttäjä-kokoelman dokumentteihin.

Mongoosen _populate_-funktion toiminnallisuus perustuu siihen, että olemme määritelleet viitteiden "tyypit" olioiden mongoose-skeemaan _ref_-kentän avulla:

```js
const Note = mongoose.model('Note', {
  content: String,
  date: Date,
  important: Boolean,
  user: { type: mongoose.Schema.Types.ObjectId, ref: 'User' }
})
```

## Kirjautuminen

Käyttäjien tulee pystyä kirjautumaan sovellukseemme ja muistiinpanot pitää automaattisesti liittää kirjautuneen käyttäjän tekemiksi.

Toteutamme nyt backendiin tuen [token-perustaiselle](https://scotch.io/tutorials/the-ins-and-outs-of-token-based-authentication#toc-how-token-based-works) autentikoinnille.

Token-autentikaation periaatetta kuvaa seuraava sekvenssikaavio:

![]({{ "/images/4/12a.png" | absolute_url }})

- Alussa käyttäjä kirjaantuu Reactilla toteutettua kirjautumislomaketta käyttäen
  - lisäämme kirjautumislomakkeen frontendiin [osassa 5](osa5)
- Tämän seurauksena selaimen React-koodi lähettää käyttäjätunnuksen ja salasanan HTTP POST -pyynnöllä palvelimen osoitteeseen _/api/login_
- Jos käyttäjätunnus ja salasana ovat oikein, generoi palvelin _Tokenin_, jonka yksilöi jollain tavalla kirjautumisen tehneen käyttäjän
  - token on kryptattu, joten sen väärentäminen on (kryptografisesti) mahdotonta
- backend vastaa selaimelle onnistumisesta kertovalla statuskoodilla ja palauttaa Tokenin vastauksen mukana
- Selain tallentaa tokenin esimerkiksi React-sovelluksen tilaan
- Kun käyttäjä luo uuden muistiinpanon (tai tekee jonkin operaation, joka edellyttää tunnistautumista), lähettää React-koodi Tokenin pyynnön mukana palvelimelle
- Palvelin tunnistaa pyynnön tekijän tokenin perusteella

Tehdään ensin kirjautumistoiminto. Asennetaan [jsonwebtoken](https://github.com/auth0/node-jsonwebtoken)-kirjasto, jonka avulla koodimme pystyy generoimaan [JSON web token](https://jwt.io/) -muotoisia tokeneja.

```bash
npm install jsonwebtoken --save
```

Tehdään kirjautumisesta vastaava koodi tiedostoon _controllers/login.js_

```js
const jwt = require('jsonwebtoken')
const bcrypt = require('bcrypt')
const loginRouter = require('express').Router()
const User = require('../models/user')

loginRouter.post('/', async (request, response) => {
  const body = request.body

  const user = await User.findOne({ username: body.username })
  const passwordCorrect = user === null ?
    false :
    await bcrypt.compare(body.password, user.passwordHash)

  if ( !(user && passwordCorrect) ) {
    return response.status(401).send({ error: 'invalid username or password' })
  }

  const userForToken = {
    username: user.username,
    id: user._id
  }

  const token = jwt.sign(userForToken, process.env.SECRET)

  response.status(200).send({ token, username: user.username, name: user.name })
})

module.exports = loginRouter
```

Koodi aloittaa etsimällä pyynnön mukana olevaa _username_:a vastaavan käyttäjän tietokannasta. Seuraavaksi katsotaan onko pyynnön mukana oleva _password_ oikea. Koska tietokantaan ei ole talletettu salasanaa, vaan salasanasta laskettu _hash_, tehdään vertailu metodilla _bcrypt.compare_:

```js
await bcrypt.compare(body.password, user.passwordHash)
```

Jos käyttäjää ei ole olemassa tai salasana on väärä, vastataan kyselyyn statuskoodilla [401 unauthorized](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.2) ja kerrotaan syy vastauksen bodyssä.

Jos salasana on oikein, luodaan metodin _jwt.sign_ avulla token, joka sisältää kryptatussa muodossa käyttäjätunnuksen ja käyttäjän id:

```js
const userForToken = {
  username: user.username,
  id: user._id
}

const token = jwt.sign(userForToken, process.env.SECRET)
```

Token on digitaalisesti allekirjoitettu käyttämällä _salaisuutena_ ympäristömuuttujassa _SECRET_ olevaa merkkijonoa. Digitaalinen allekirjoitus varmistaa sen, että ainoastaan salaisuuden tuntevilla on mahdollisuus generoida validi token. Ympäristömuuttujalle pitää muistaa asettaa arvo tiedostoon .env.

Onnistuneeseen pyyntöön vastataan statuskoodilla _200 ok_ ja generoitu token sekä kirjautuneen käyttäjän käyttäjätunnus ja nimi lähetetään vastauksen bodyssä pyynnön tekijälle.

Kirjautumisesta huolehtiva koodi on vielä liitettävä sovellukseen lisäämällä tiedostoon _index.js_ muiden routejen käyttöönoton yhteyteen

```js
const loginRouter = require('./controllers/login')

//...

app.use('/api/login', loginRouter)
```

Kokeillaan kirjautumista, käytetään VS Coden REST-clientiä:

![]({{ "/images/4/12b.png" | absolute_url }})

Kirjautuminen ei kuitenkaan toimi, konsoli näyttää seuraavalta:

```bash
Method: POST
Path:   /api/login
Body:   { username: 'mluukkai', password: 'salainen' }
---
(node:17486) UnhandledPromiseRejectionWarning: Unhandled promise rejection (rejection id: 2): Error: secretOrPrivateKey must have a value
````

Ongelman aiheuttaa komento _jwt.sign(userForToken, process.env.SECRET)_ sillä ympäristömuuttujalle _SECRET_ on unohtunut määritellä arvo. Kun arvo määritellään tiedostoon _.env_, alkaa kirjautuminen toimia.

Onnistunut kirjautuminen palauttaa kirjautuneen käyttäjän tiedot ja tokenin:

![]({{ "/images/4/12c.png" | absolute_url }})

Virheellisellä käyttäjätunnuksella tai salasanalla kirjautuessa annetaan asianmukaisella statuskoodilla varustettu virheilmoitus

![]({{ "/images/4/12d.png" | absolute_url }})

### Muistiinpanojen luominen vain kirjautuneille

Muutetaan vielä muistiinpanojen luomista, siten että luominen onnistuu ainoastaan jos luomista vastaavan pyynnön mukana on validi token. Muistiinpano talletetaan tokenin identifioiman käyttäjän tekemien muistiinpanojen listaan.

Tapoja tokenin välittämiseen selaimesta backendiin on useita. Käytämme ratkaisussamme [Authorization](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization)-headeria. Tokenin lisäksi headerin avulla kerrotaan mistä [autentikointiskeemasta](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication#Authentication_schemes) on kyse. Tämä voi olla tarpeen, jos palvelin tarjoaa useita eri tapoja autentikointiin. Skeeman ilmaiseminen kertoo näissä tapauksissa palvelimelle, miten mukana olevat kredentiaalit tulee tulkita.
Meidän käyttöömme sopii _Bearer_-skeema.

Käytännössä tämä tarkoittaa, että jos token on esimerkiksi merkkijono _eyJhbGciOiJIUzI1NiIsInR5c2VybmFtZSI6Im1sdXVra2FpIiwiaW_, laitetaan pyynnöissä headerin Authorization arvoksi merkkijono
<pre>
Bearer eyJhbGciOiJIUzI1NiIsInR5c2VybmFtZSI6Im1sdXVra2FpIiwiaW
</pre>

Modifioitu muistiinpanojen luomisesta huolehtiva koodi seuraavassa:

```js
const jwt = require('jsonwebtoken')

// ...

const getTokenFrom = (request) => {
  const authorization = request.get('authorization')
  if (authorization && authorization.toLowerCase().startsWith('bearer ')) {
    return authorization.substring(7)
  }
  return null
}

notesRouter.post('/', async (request, response) => {
  const body = request.body

  try {
    const token = getTokenFrom(request)
    const decodedToken = jwt.verify(token, process.env.SECRET)

    if (!token || !decodedToken.id) {
      return response.status(401).json({ error: 'token missing or invalid' })
    }

    if (body.content === undefined) {
      return response.status(400).json({ error: 'content missing' })
    }

    const user = await User.findById(decodedToken.id)

    const note = new Note({
      content: body.content,
      important: body.important === undefined ? false : body.important,
      date: new Date(),
      user: user._id
    })

    const savedNote = await note.save()

    user.notes = user.notes.concat(savedNote._id)
    await user.save()

    response.json(Note.format(note))
  } catch(exception) {
    if (exception.name === 'JsonWebTokenError' ) {
      response.status(401).json({ error: exception.message })
    } else {
      console.log(exception)
      response.status(500).json({ error: 'something went wrong...' })
    }
  }
})
```

Apufunktio _getTokenFrom_ eristää tokenin headerista _authorization_. Tokenin oikeellisuus varmistetaan metodilla _jwt.verify_. Metodi myös dekoodaa tokenin, eli palauttaa olion, jonka perusteella token on laadittu:

```js
const decodedToken = jwt.verify(token, process.env.SECRET)
```

Tokenista dekoodatun olion sisällä on kentät _username_ ja _id_ eli se kertoo palvelimelle kuka pyynnön on tehnyt.

Jos tokenia ei ole tai tokenista dekoodattu olio ei sisällä käyttäjän identiteettiä (eli _decodedToken.id_ ei ole määritelty), palautetaan virheestä kertova statuskoodi [401 unauthorized](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.2) ja kerrotaan syy vastauksen bodyssä:

```js
if (!token || !decodedToken.id) {
  return response.status(401).json({ error: 'token missing or invalid' })
}
```

Kun pyynnön tekijän identiteetti on selvillä, jatkuu suoritus entiseen tapaan.

Tokenin verifiointi voi myös aiheuttaa poikkeuksen _JsonWebTokenError_. Syynä tälle voi olla viallinen, väärennetty tai eliniältään vanhentunut token. Poikkeusten käsittelyssä haaraudutaan virheen tyypin perusteella ja vastataan 401 jos poikkeus johtuu tokenista, ja muuten vastataan [500 internal server error](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.5.1).

Uuden muistiinpanon luominen onnistuu nyt postmanilla jos _authorization_-headerille asetetaan oikeanlainen arvo, eli merkkijono _bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ_, missä osa on _login_-operaation palauttama token.

Postmanilla luominen näyttää seuraavalta

![]({{ "/assets/4/14.png" | absolute_url }})

ja Visual Studio Coden REST clientillä

![]({{ "/images/4/14a.png" | absolute_url }})

Sovelluksen tämänhetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part3-notes-backend/tree/part4-6), tagissa _part4-6_.

Jos sovelluksessa on useampia rajapintoja jotka vaativat kirjautumisen kannattaa JWT:n validointi eriyttää omaksi middlewarekseen, tai käyttää jotain jo olemassa olevaa kirjastoa kuten [express-jwt](https://www.npmjs.com/package/express-jwt).

### Loppuhuomioita

Koodissa on tapahtunut paljon muutoksia ja matkan varrella on tapahtunut tyypillinen kiivaasti etenevän ohjelmistoprojektin ilmiö: suuri osa testeistä on hajonnut. Koska kurssin tämä osa on jo muutenkin täynnä uutta asiaa, jätämme testien korjailun harjoitustehtäväksi.

Käyttäjätunnuksia, salasanoja ja tokenautentikaatiota hyödyntäviä sovelluksia tulee aina käyttää salatun [HTTPS](https://en.wikipedia.org/wiki/HTTPS)-yhteyden yli. Voimme käyttää sovelluksissamme Noden [HTTP](https://nodejs.org/docs/latest-v8.x/api/http.html)-serverin sijaan [HTTPS](https://nodejs.org/api/https.html)-serveriä. Toisaalta koska sovelluksemme tuotantoversio on Herokussa, sovelluksemme pysyy käyttäjien kannalta suojattuna sen ansiosta, että Heroku reitittää kaiken liikenteen selaimen ja Herokun palvelimien välillä HTTPS:n yli.

Toteutamme kirjautumisen frontendin puolelle kurssin [seuraavassa osassa](/osa5).

## Tehtäviä

Tee nyt tehtävät [4.15-4.21](/tehtavat#blogilistan-käyttäjät)


<!---
note left of kayttaja
  käyttäjä täyttää kirjautumislomakkeelle
  käyttäjätunnuksen ja salasanan
end note
kayttaja -> selain: painetaan login-nappia

selain -> backend: HTTP POST /api/login {username, password}
note left of backend
  backend generoi käyttäjän identifioivan TOKENin
end note
backend -> selain: TOKEN palautetaan vastauksen bodyssä
note left of selain
  selain tallettaa TOKENin
end note
note left of kayttaja
  käyttäjä luo uden muistiinpanon
end note
kayttaja -> selain: painetaan create note -nappia
selain -> backend: HTTP POST /api/notes {content} headereissa TOKEN
note left of backend
  backend tunnistaa TOKENin perusteella kuka käyttää kyseessä
end note

backend -> selain: 201 created

kayttaja -> kayttaja:
-->
