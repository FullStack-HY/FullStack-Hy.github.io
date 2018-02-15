---
layout: page
title: osa 3
inheader: yes
permalink: /osa3/
---

## Osan 3 oppimistavoitteet

- Web-sovellusten toiminnan perusteet
  - RESTful-periaatteiden alkeet
    - HTTP-pyyntötyypit
    - statuskoodit
    - resurssiperustaiset URL:it
    - HTTP-pyyntöjen- safety ja idempotence-ominaisuudet
- node.js/express
  - npm
    - sovelluksen luominen
    - riippuvuuksien hallinta
    - npm-skriptien alkeet
    - versiointi
    - nodemon
  - HTTP-pyyntöjen käsittely
    - reittien määrittely
    - pyyntöihin vastaaminen eri statuskoodein
  - middlewaret
  - node.js-sovellusten debuggaaminen
  - sovelluksen vieminen tuotantoon
  - sovelluskehitys- ja tuotantoversion tietokantojen erottaminen
- mongo
  - dokumenttitietokantojen perusteet
  - mlab:in (tai vastaavan palvelun) mongon peruskäyttö
  - mongoose
    - skeema ja model
    - olioiden hakeminen, tallettaminen, editointi ja poisto
- konfiguraatiot
  - ESlint

## Muutama huomio

### setState on asynkrooninen

Muutamat ovat jo törmänneet siihen, että **React kutsuu funktiota setState asynkroonisesti**, eli jos meillä on seuraava koodi

```js
console.log(this.state.counter)
this.setState({counter: 55})
console.log(this.state.counter)
```

tulostavat molemmat rivit saman arvon, sillä Reactin tila **ei saa uutta arvoa** heti komennon _this.setState_ jälkeen, vaan vasta sitten kun suorituksen alla oleva metodi on suoritettu loppuun ja _setState_ on saanut mahdollisuuden suoritukselle.

### console.log

_Mikä erottaa kokeneen ja kokemattoman Javascript-ohjelmoijan? Kokeneet käyttävät 10-100 kertaa enemmän console.logia_.

Paradoksaalista kyllä tämä näyttää olevan tilanne, vaikka kokematon ohjelmoija oikeastaan tarvitsisi console.logia (tai jotain muita debuggaustapoja) huomattavissa määrin kokenutta enemmän.

Eli kun joku ei toimi, älä arvaile vaan logaa tai käytä jotain muita debuggauskeinoja.

**HUOM** kun käytät komentoa _console.log_ debuggaukseen, älä yhdistele asioita "javamaisesti" plussalla, eli sen sijaan että kirjoittaisit

```js
console.log('propsin arvo on' + props)
```

erottele tulostettavat asiat pilkulla:

```js
console.log('propsin arvo on', props)
```

Jos yhdistät merkkijonoon olion, tuloksena on suhteellisen hyödytön tulostustmuoto

```bash
propsin arvo on [Object object]
```

kun taas pilkulla tulostettavat asiat erotellessa saat developer-konsoliin olion, jonka sisältöä on mahdollista tarkastella.

### Visual Studio Coden snippetit

VS Codeen on helppo määritellä "snippettejä", eli Netbeansin "sout":in tapaisia oikoteitä yleisesti käytettyjen koodinpätkien generointiin. Ohje snippetien luomiseen [täällä](https://github.com/FullStack-HY/FullStack-Hy.github.io/blob/master/snippet_ohje.md)

VS Code -plugineina löytyy myös hyödyllisiä valmiiksi määriteltyjä snippetejä, esim.
[tämä](https://marketplace.visualstudio.com/items?itemName=xabikos.ReactSnippets)

### Pakolliset tehtävät, tehtävien vaikutus arvosanaan

Joissain yhteyksissä on ollut pientä epäselvyyttä mitä tiettyjen tehtävien pakollisuus tarkoittaa, ja mikä eipakollisten tehtävien rooli on. Tarkennusta asiaan tehtävien sivun [alussa](/tehtävät).

### linkkivinkit

Kurssisivun alaisuudessa on nyt [osio](https://fullstack-hy.github.io/linkit/), jonne kaikkien toivotaan lisäilevän hyödyllisiksi kokemiaan linkkejä. Lisääminen onnistuu tekemällä pull request [tänne](https://github.com/FullStack-HY/FullStack-Hy.github.io/blob/master/linkit.md)

Kun lisäät linkin, laita linkin yhteyteen pieni kuvaus mitä linkin takaa löytyy.

## Node.js

Siirrämme tässä osassa fokuksen backendiin, eli palvelimella olevaan toiminnallisuuteen.

Backendin toteutusympäristönä käytämme [Node.js](https://nodejs.org/en/):ää, joka on melkein missä vaan, erityisesti palvelimilla ja omalla koneellasikin toimiva, Googlen [chrome V8](https://developers.google.com/v8/) -Javascriptmoottoriin perustuva Javascriptin suoritusympäristö.

Kurssimateriaalia tehtäessä on ollut käytössä Node.js:n versio _v8.6.0_. Huolehdi että omasi on vähintään yhtä tuore (ks. komentoriviltä _node -v_).

Kuten [osassa 1](/osa1#javascriptiä) todettiin, selaimet eivät vielä osaa uusimpia Javascriptin ominaisuuksia ja siksi selainpuolen koodi täytyy kääntää eli _transpiloida_ esim [babel](https://babeljs.io/):illa. Backendissa tilanne on kuitenkin toinen, uusin Node hallitsee riittävissä määrin myös Javascriptin uusia versioita (muutamia vielä standardoimattomia ominaisuuksia lukuunottamatta), joten suoritamme Nodella suoraan kirjoittamaamme koodia ilman transpilointivaihetta.

Tavoitteenamme on tehdä [osan 2](/osa2) muistiinpanosovellukseen sopiva backend. Aloitetaan kuitenkin ensin perusteiden läpikäyminen toteuttamalla perinteinen "hello world"-sovellus.

Osassa 2 oli jo puhe [npm](/osa2#npm):stä, eli Javascript-projektien hallintaan liittyvästä, alunperin Node-ekosysteemistä kotoisin olevasta työkalusta. Mennään sopivaan hakemistoon ja luodaan projektimme runko komennolla _npm init_. Vastaillaan kysymyksiin sopivasti ja tuloksena on hakemiston juureen sijoitettu projektin tietoja kuvaava tiedosto _package.json_

```json
{
  "name": "notebackend",
  "version": "0.0.1",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Matti Luukkainen",
  "license": "MIT"
}
```

Tiedosto määrittelee mm. että ohjelmamme käynnistyspiste on tiedosto _index.js_.

Tehdään kenttään _scripts_ pieni lisäys:

```bash
{
  // ...
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  // ...
}
```

Luodaan sitten sovelluksen ensimmäinen versio, eli projektin juureen sijoitettava tiedosto _index.js_ ja sille seuraava sisältö:

```js
console.log('hello world')
```

Voimme suorittaa ohjelman joko "suoraan" nodella, komentorivillä

```bash
node index.js
```

tai [npm scriptinä](https://docs.npmjs.com/misc/scripts)

```bash
npm start
```

npm-skripti _start_ toimii koska määrittelimme sen tiedostoon _package.json_

```bash
{
  // ...
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  // ...
}
```

Vaikka esim. projektin suorittaminen onnistuukin suoraan käyttämällä komentoa _node index.js_, on npm-projekteille suoritettavat operaatiot yleensä tapana määritellä nimenomaan npm-skripteinä.

Oletusarvoinen _package.json_ määrittelee valmiiksi myös toisen yleisesti käytetyn npm-scriptin eli _npm test_. Koska projektissamme ei ole vielä testikirjastoa, ei _npm test_ kuitenkaan tee vielä muuta kuin suorittaa komennon

```bash
echo "Error: no test specified" && exit 1
```

### Yksinkertainen web-palvelin

Muutetaan sovellus web-palvelimeksi:

```js
const http = require('http')

const app = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' })
  res.end('Hello World')
})

const port = 3001
app.listen(port)
console.log(`Server running on port ${port}`)
```

Konsoliin tulostuu

```bash
Server running on port 3001
```

Voimme avata selaimella osoitteessa <http://localhost:3001> olevan vaatimattoman sovelluksemme:

![]({{ "/assets/3/1.png" | absolute_url }})

Palvelin toimii itseasiassa täsmälleen samalla tavalla riippumatta urlin loppuosasta, eli myös sivun <http://localhost:3001/foo/bar> sisältö on sama.

**HUOM** jos koneesi portti 3001 on jo jonkun sovelluksen käytössä, aiheuttaa käynnistäminen virheen:

```bash
> notebackend@0.0.1 start /Users/mluukkai/opetus/_2018fullstack_koodi/materiaali/osa3/notebackend
> node index.js

Server running on port 3001
events.js:182
      throw er; // Unhandled 'error' event
      ^

Error: listen EADDRINUSE :::3001
    at Object._errnoException (util.js:1019:11)
```

Sammuta portissa 3001 oleva sovellus (edellisessä osassa json-server käynnistettiin porttiin 3001) tai määrittele sovellukselle jokin toinen portti.

Tarkastellaan koodia hiukan. Ensimmäinen rivi

```js
const http = require('http')
```

ottaa käyttöön Noden sisäänrakennetun [web-palvelimen](https://nodejs.org/docs/latest-v8.x/api/http.html) määrittelevän moduulin. Kyse on käytännössä samasta asiasta, mihin olemme selainpuolen koodissa tottuneet hieman syntaksiltaan erilaisessa muodossa:

```js
import http from 'http'
```

Selaimen puolella käytetään (nykyään) ES6:n moduuleita, eli moduulit määritellään [exportilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) ja otetaan käyttöön [importilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import).

Node.js kuitenkin käyttää ns. [CommonJS](https://en.wikipedia.org/wiki/CommonJS)-moduuleja. Syy tälle on siinä, että Node-ekosysteemillä oli tarve moduuleihin jo kauan ennen kuin Javascript tuki kielen tasolla moduuleja. Node ei toistaiseksi tue ES-moduuleja, mutta tuki on todennäköisesti jossain vaiheessa [tulossa](https://nodejs.org/api/esm.html).

CommonJS-moduulit toimivat kohtuullisessa määrin samaan tapaan kuin ES6-moduulit, ainakin tämän kurssin tarpeiden puitteissa.

Koodi jatkuu seuraavasti:

```js
const app = http.createServer((request, response) => {
  response.writeHead(200, { 'Content-Type': 'text/plain' })
  response.end('Hello World')
})
```

koodi luo [http](https://nodejs.org/docs/latest-v8.x/api/http.html)-palvelimen metodilla _createServer_ web-palvelimen, jolle se rekisteröi _tapahtumankäsittelijän_, joka suoritetaan _jokaisen_ osoitteen <http:/localhost:3001/> alle tulevan HTTP-pyynnön yhteydessä.

Pyyntöön vastataan statuskoodilla 200, asettamalla _Content-Type_-headerille arvo _text/plain_ ja asettamalla palautettavan sivun sisällöksi merkkijono _Hello World_.

Viimeiset rivit sitovat muuttujaan _app_ sijoitetun http-palvelimen kuuntelemaan porttiin 3001 tulevia HTTP-pyyntöjä:

```js
const PORT = 3001
app.listen(PORT)
console.log(`Server running on port ${PORT}`)
```

Koska tällä kurssilla palvelimen rooli on pääasiassa tarjota frontille JSON-muotoista "raakadataa", muutetaan heti palvelinta siten, että se palauttaa kovakoodatun listallisen JSON-muotoisia muistiinpanoja:

```js
let notes = [
  {
    id: 1,
    content: 'HTML on helppoa',
    date: '2017-12-10T17:30:31.098Z',
    important: true
  },
  {
    id: 2,
    content: 'Selain pystyy suorittamaan vain javascriptiä',
    date: '2017-12-10T18:39:34.091Z',
    important: false
  },
  {
    id: 3,
    content: 'HTTP-protokollan tärkeimmät metodit ovat GET ja POST',
    date: '2017-12-10T19:20:14.298Z',
    important: true
  }
]

const app = http.createServer((request, response) => {
  response.writeHead(200, { 'Content-Type': 'application/json' })
  response.end(JSON.stringify(notes))
})
```
Käynnistetään palvelin uudelleen (palvelin sammutetaan painamalla _ctrl_ ja _c_ yhtä aikaa konsolissa) ja refreshataan selain.

Headerin _Content-Type_ arvolla _application/json_ kerrotaan, että kyse on JSON-muotoisesta datasta. Muuttujassa _notes_ oleva taulukko muutetaan jsoniksi metodilla <code>JSON.stringify(notes)</code>.


Kun avaamme selaimen, on tulostusasu sama kuin [osassa 2](/osa2#datan-haku-palvelimelta) käytetyn [json-serverin](https://github.com/typicode/json-server) tarjoamalla muistiinpanojen listalla:

![]({{ "/assets/3/2.png" | absolute_url }})

Voimme jo melkein ruveta käyttämään uutta backendiämme osan 2 muistiinpano-frontendin kanssa. Mutta vain _melkein_, sillä kun käynnistämme frontendin, tulee konsoliin virheilmoitus

![]({{ "/assets/3/3.png" | absolute_url }})

Syy virheelle selviää pian, parantelemme kuitenkin ensin koodia muilta osin.

## Express

Palvelimen koodin tekeminen suoraan Noden sisäänrakennetun web-palvelimen [http](https://nodejs.org/docs/latest-v8.x/api/http.html):n päälle on mahdollista, mutta työlästä, erityisesti jos sovellus kasvaa hieman isommaksi.

Nodella tapahtuvaa web-sovellusten ohjelmointia helpottamaan onkin kehitelty useita _http_:tä miellyttävämmän ohjelmoitirajapinnan tarjoamia kirjastoja. Näistä ylivoimaisesti suosituin on [express](http://expressjs.com).

Otetaan express käyttöön määrittelemällä se projektimme riippuvuudeksi komennolla

```bash
npm install express --save
```

Riippuvuus tulee nyt määritellyksi tiedostoon _package.json_:

```bash
{
  // ...
  "dependencies": {
    "express": "^4.16.2"
  }
}
```

Riippuvuuden koodi asentuu kaikkien projektin riippuvuuksien tapaan projektin juuressa olevaan hakemistoon _node_modules_. Hakemistosta löytyy expressin lisäksi suuri määrä muutakin tavaraa

<img src="/assets/3/4.png" height="200">

Kyseessä ovat expressin riippuvuudet ja niiden riippuvuudet ym... eli projektimme [transitiiviset riippuvuudet](https://lexi-lambda.github.io/blog/2016/08/24/understanding-the-npm-dependency-model/).

Projektiin asentui expressin versio 4.16.2. Mitä tarkoittaa _package.json:issa_ versiomerkinnän edessä oleva väkänen, eli miksi muoto on

```json
"express": "^4.16.2"
```

npm:n yhteydessä käytetään ns. [semanttista versiointia](https://docs.npmjs.com/getting-started/semantic-versioning).

Merkintä _^4.16.2_ tarkoittaa, että jos/kun projektin riippuvuudet päivitetään, asennetaan expressistä versio, joka on vähintään _4.16.2_, mutta asennetuksi voi tulla versio, jonka _patch_ eli viimeinen numero tai _minor_ eli keskimmäinen numero voi olla suurempi. Pääversio eli _major_ täytyy kuitenkin olla edelleen sama.


Voimme päivittää projektin riippuvuudet komennolla

```bash
npm update
```

Vastaavasti jos aloitamme projektin koodaamisen toisella koneella, saamme haettua ajantasaiset, _package.json_:in määrittelyn kanssa yhteensopivat riippuvuudet komennolla

```bash
npm install
```

Jos riippuvuuden _major_-versionumero ei muutu, uudempien versioiden pitäisi olla [taaksepäin yhteensopivia](https://en.wikipedia.org/wiki/Backward_compatibility), eli jos ohjelmamme käyttäisi tulevaisuudessa esim. expressin versiota 4.99.175, tässä osassa tehtävän koodin pitäisi edelleen toimia ilman muutoksia. Sen sijaan tulevaisuudessa joskus julkaistava express 5.0.0. voi sisältää sellaisia muutoksia, että koodimme ei enää toimisi.

### Web ja express

Palataan taas sovelluksen ääreen ja muutetaan se muotoon:

```js
const express = require('express')
const app = express()

let notes = [
  ...
]

app.get('/', (req, res) => {
  res.send('<h1>Hello World!</h1>')
})

app.get('/notes', (req, res) => {
  res.json(notes)
})

const PORT = 3001
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```

Jotta sovelluksen uusi versio saadaan käyttöön, on sovellus uudelleenkäynnistettävä.

Sovellus ei muutu paljoa. Heti alussa otetaan käyttöön _express_, joka on tällä kertaa _funktio_, jota kutsumalla luodaan muuttujaan _app_ sijoitettava express-sovellusta vastaava olio:

```js
const express = require('express')
const app = express()
```

Seuraavaksi määritellään sovellukselle kaksi _routea_. Näistä ensimmäinen määrittelee tapahtumankäsittelijän, joka hoitaa sovelluksen juureen eli polkuun _/_ tulevia HTTP GET -pyyntöjä:

```js
app.get('/', (request, response) => {
  response.send('<h1>Hello World!</h1>')
})
```

Tapahtumankäsittelijäfunktiolla on kaksi parametria. Näistä ensimmäinen eli [request](http://expressjs.com/en/4x/api.html#req) sisältää kaikki HTTP-pyynnön tiedot ja toisen parametrin [response](http://expressjs.com/en/4x/api.html#res):n avulla määritellään, miten pyyntöön vastataan.

Koodissa pyyntöön vastataan käyttäen _response_-olion metodia [send](http://expressjs.com/en/4x/api.html#res.send), jonka kutsumisen seurauksena palvelin vastaa HTTP-pyyntöön lähettämällä vastaukseksi _send_:in parametrina olevan merkkijonon _<h1>Hello World!</h1>_. Koska parametri on merkkijono, tulee vastauksessa _content-type_-headerin arvoksi _text/html_, statuskoodiksi tulee oletusarvoisesti 200.

Routeista toinen määrittelee tapahtumankäsittelijän, joka hoitaa sovelluksen polkuun _notes_ tulevia HTTP GET -pyyntöjä:

```js
app.get('/notes', (request, response) => {
  response.json(notes)
})
```

Pyyntöön vastataan _response_-olion metodilla [json](http://expressjs.com/en/4x/api.html#res.json), joka lähettää HTTP-pyynnön vastaukseksi parametrina olevaa Javascript-olioa (eli taulukkoa _notes_) vastaavan JSON-muotoisen merkkijonon. Express asettaa headerin _Content-type_ arvoksi _application/json_.

Pieni huomio JSON-muodossa palautettavasta datasta.

Aiemmassa, pelkkää Nodea käyttämässä versiossa, jouduimme muuttamaan palautettavan datan json-muotoon metodilla _JSON.stringify_:

```js
response.end(JSON.stringify(notes))
```

Expressiä käytettäessä tämä ei ole tarpeen, sillä muunnos tapahtuu automaattisesti.

Kannattaa huomata, että [JSON](https://en.wikipedia.org/wiki/JSON) on merkkijono, eikä Javascript-olio kuten muuttuja _notes_.

Seuraava interaktiivisessa [node-repl](https://nodejs.org/docs/latest-v8.x/api/repl.html):issä suoritettu kokeilu havainnollistaa asiaa:

<img src="/assets/3/5.png" height="200">

Saat käynnistettyä interaktiivisen node-repl:in kirjoittamalla komentoriville _node_. Esim. joidenkin komentojen toimivuuttaa on koodatessa kätevä tarkastaa konsolissa, suosittelen!

## nodemon

Jos muutamme sovelluksen koodia, joudumme uudelleenkäynnistämään sovelluksen (eli ensin sammuttamaan konsolista _ctrl_ ja _c_ ja sitten käynnistämään uudelleen), jotta muutokset tulisivat voimaan. Verrattuna Reactin mukavaan workflowhun, missä selain päivittyi automaattisesti koodin muuttuessa tuntuu uudelleenkäynnistely kömpelöltä.

Ongelmaan ratkaisu on [nodemon](https://github.com/remy/nodemon):

> nodemon will watch the files in the directory in which nodemon was started, and if any files change, nodemon will automatically restart your node application.

Asennetaan nodemon määrittelemällä se _kehitysaikaiseksi riippuvuudeksi_ (development dependency) komennolla:

```bash
npm install --save-dev nodemon
```

Tiedoston _package.json_ sisältö muuttuu seuraavasti:

```bash
{
  //...
  "dependencies": {
    "express": "^4.16.2"
  },
  "devDependencies": {
    "nodemon": "^1.13.3"
  }
}
```

Jos nodemon-riippuvuus kuitenkin meni normaaliin "dependencies"-ryhmään, päivitä package.json manuaalisesti vastaamaan yllä näkyvää (versiot kuitenkin säilyttäen).

Kehitysaikaisilla riippuvuuksilla tarkoitetaan työkaluja, joita tarvitaan ainoastaan sovellusta kehitettäessä, esim. testaukseen tai sovelluksen automaattiseen uudelleenkäynnistykseen kuten _nodemon_.

Kun sovellusta suoritetaan tuotantomoodissa, eli samoin kun sitä tullaan suorittamaan tuotantopalvelimella (esim. Herokussa, mihin tulemme kohta siirtämään sovelluksemme), ei kehitysaikaisia riippuvuuksia tarvita.

Voimme käynnistää ohjelman _nodemon_:illa seuraavasti:

```bash
node_modules/.bin/nodemon index.js
```

Sovelluksen koodin muutokset aiheuttavat nyt automaattisen palvelimen uudelleenkäynnistymisen. Kannattaa huomata, että vaikka palvelin uudelleenkäynnistyy automaattisesti, selain täytyy kuitenkin refreshata, sillä toisin kuin Reactin yhteydessä, meillä ei nyt ole eikä tässä skenaariossa (missä palautamme JSON-muotoista dataa) edes voisikaan olla selainta päivittävää [hot reload](https://gaearon.github.io/react-hot-loader/getstarted/) -toiminnallisuutta.

Komento on ikävä, joten määritellään sitä varten _npm-skripti_ tiedostoon _package.json_:

```bash
{
  // ..
  "scripts": {
    "start": "node index.js",
    "watch": "nodemon index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  // ..
}
```

Skriptissä ei ole tarvetta käyttää nodemonin polusta sen täydellistä muotoa _node_modules/.bin/nodemon_ sillä _npm_ osaa etsiä automaattisesti suoritettavaa tiedostoa kyseisestä hakemistosta.

Voimme nyt käynnistää palvelimen sovelluskehitysmoodissa komennolla

```bash
npm run watch
```

Toisin kuin skriptejä _start_ tai _test_ suoritettaessa, joudumme sanomaan myös _run_.

## Lisää routeja

Laajennetaan sovellusta siten, että se toteuttaa samanlaisen RESTful-periaatteeseen nojaavan HTTP-rajapinnan kun [json-server](https://github.com/typicode/json-server#routes).

### REST

Representational State Transfer eli REST on Roy Fieldingin vuonna 2000 ilmestyneessä [väitöskirjassa](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm) määritelty skaalautuvien web-sovellusten rakentamiseksi tarkoitettu arkkitehtuurityyli.

Emme nyt rupea määrittelemään REST:iä Fieldingiläisittäin tai rupea väittämään mitä REST on tai mitä se ei ole vaan otamme hieman [kapeamman näkökulman](https://en.wikipedia.org/wiki/Representational_state_transfer#Applied_to_Web_services) miten REST tai RESTful API:t yleensä tulkitaan Web-sovelluksissa. Alkuperäinen REST-periaate ei edes sinänsä rajoitu Web-sovelluksiin.

Mainitsimme jo [edellisestä osassa](/osa2#rest-api:n-käyttö), että yksittäisiä asioita, meidän tapauksessamme muistiinpanoja kutsutaan RESTful-ajattelussa _resursseiksi_. Jokaisella resurssilla on URL eli sen yksilöivä osoite.

Erittäin yleinen konventio on muodostaa resurssien yksilöivät URLit liittäen resurssityypin nimi ja resurssin yksilöivä tunniste.

Oletetaan että palvelumme juuriosoite on _www.example.com/api_

Jos nimitämme muistiinpanoja _note_-resursseiksi, yksilöidään yksittäinen muistiinpano, jonka tunniste on 10 URLilla _www.example.com/api/notes/10_.

Kaikkia muistiinpanoja edustavan kokoelmaresurssin URL taas on _www.example.com/api/notes_

Resursseille voi suorittaa erilaisia operaatiota. Suoritettavan operaation määrittelee HTTP-operaation tyyppi, jota kutsutaan usein myös _verbiksi_:

| URL | verbi           |  toiminnallisuus |
|------- | --- | --- |
| notes/10 &nbsp;&nbsp;  | GET | hakee yksittäisen resurssin |
| notes    | GET         | hakee kokoelman kaikki resurssit |
| notes    | POST        | luo uuden resurssin pyynnön mukana olevasta datasta |
| notes/10 | DELETE &nbsp;&nbsp;    | poistaa yksilöidyn resurssin |
| notes/10 | PUT         | korvaa yksilöidyn resurssin pyynnön mukana olevalla datalla |
| notes/10 | PATCH       | korvaa yksilöidyn resurssin osan pyynnön mukana olevalla datalla |
|          |  |  |

Näin määrittyy suurin piirtein asia, mitä REST kutsuu nimellä [uniform interface](https://en.wikipedia.org/wiki/Representational_state_transfer#Architectural_constraints), eli jossain määrin yhtenäinen tapa määritellä rajapintoja, jotka mahdollistavat (tietyin tarkennuksin) järjestelmien yhteiskäytön.


Tämänkaltaista tapaa tulkita REST:iä on nimitetty kolmiportaisella asteikolla [kypsyystason 2](https://martinfowler.com/articles/richardsonMaturityModel.html) REST:iksi. REST:in kehittäjän Roy Fieldingin mukaan tällöin kyseessä ei vielä ole ollenkaan asia, jota tulisi kutsua [REST-apiksi](http://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven). Maailman "REST"-apeista valtaosa ei täytäkään puhdasverisen Fieldingiläisen REST-apin määritelmää.

Joissain yhteyksissä (ks. esim [Richardsom, Ruby: RESTful Web Services](http://shop.oreilly.com/product/9780596529260.do)) edellä esitellyn kaltaista suoraviivaisehkoa resurssien [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete)-tyylisen manipuloinnin mahdollistavaa API:a nimitetään REST:in sijaan [resurssipohjaiseksi](https://en.wikipedia.org/wiki/Resource-oriented_architecture) arkkitehtuurityyliksi. Emme nyt kuitenkaan takerru liian tarkasti määritelmällisiin asioihin vaan jatkamme sovelluksen parissa.

### Yksittäisen resurssin haku

Laajennetaan nyt sovellusta siten, että se tarjoaa muistiinpanojen operointiin REST-rajapinnan. Tehdään ensin [route](http://expressjs.com/en/guide/routing.html) yksittäisen resurssin katsomista varten.

Yksittäisen muistiinpanon identifioi URL, joka on muotoa _notes/10_, missä lopussa oleva numero vastaa resurssin muistiinpanon id:tä.

Voimme määritellä expressin routejen poluille [parametreja](http://expressjs.com/en/guide/routing.html) käyttämällä kaksoispistesyntaksia:

```js
app.get('/notes/:id', (request, response) => {
  const id = request.params.id
  const note = notes.find(note => note.id === id )
  response.json(note)
})
```

Nyt <code>app.get('/notes/:id', ...)</code> käsittelee kaikki HTTP GET -pyynnöt, jotka ovat muotoa _note/JOTAIN_, missä _JOTAIN_ on mielivaltainen merkkijono.


Polun parametrin _id_ arvoon päästään käsiksi pyynnön tiedot kertovan olion [request](http://expressjs.com/en/api.html#req) kautta:

```js
const id = request.params.id
```

Jo tutuksi tulleella taulukon _find_-metodilla haetaan taulukosta parametria vastaava muistiinpano ja palautetaan se pyynnön tekijälle.

Kun sovellusta testataan menemällä selaimella osoitteeseen <http://localhost:3001/notes/1>, havaitaan että se ei toimi, selain näyttää tyhjältä. Tämä on tietenkin softadevaajan arkipäivää, ja on ruvettava debuggaamaan.

Vanha hyvä keino on alkaa lisäillä koodiin _console.log_-komentoja:

```js
app.get('/notes/:id', (request, response) => {
  const id = request.params.id
  console.log(id)
  const note = notes.find(note => note.id === id)
  console.log(note)
  response.json(note)
})
```

Konsoliin tulostuu

<pre>
1
undefined
</pre>

eli halutun muistiinpanon id välittyy sovellukseen aivan oikein, mutta _find_ komento ei löydä mitään.

Päätetään tulostella konsoliin myös _find_-komennon sisällä olevasta vertailijafunktiosta, joka onnistuu helposti kun tiiviissä muodossa oleva funktio <code>note => note.id === id</code> kirjoitetaan eksplisiittisen returnin sisältävässä muodossa:

```js
app.get('/notes/:id', (request, response) => {
  const id = request.params.id
  const note = notes.find(note => {
    console.log(note.id, typeof note.id, id, typeof id, note.id === id)
    return note.id === id
  })
  console.log(note)
  response.json(note)
})
```

Jokaisesta vertailufunktion kutsusta tulostetaan nyt monta asiaa. Konsolin tulostus on seuraava:

<pre>
1 'number' '1' 'string' false
2 'number' '1' 'string' false
3 'number' '1' 'string' false
</pre>

ongelman syy selviää: muuttujassa _id_ on tallennettuna merkkijono '1' kun taas muistiinpanojen id:t ovat numeroita. Javascriptissä === vertailu katsoo kaikki eri tyyppiset arvot oletusarvoisesti erisuuriksi, joten 1 ei ole '1'.

Korjataan ongelma, muuttamalla parametrina oleva merkkijonomuotoinen id [numeroksi](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number):

```js
app.get('/notes/:id', (request, response) => {
  const id = Number(request.params.id)
  const note = notes.find(note => note.id === id)
  response.json(note)
})
```

ja nyt yksittäisen resurssin hakeminen toimii

<img src="/assets/3/6.png" height="200">

toiminnallisuuteen jää kuitenkin pieni ongelma.

Jos haemme muistiinpanoa sellaisella indeksillä, mitä vastaavaa muistiinpanoa ei ole olemassa, vastaa palvelin seuraavasti

![]({{ "/assets/3/7.png" | absolute_url }})

HTTP-statuskoodi on onnistumisesta kertova 200. Vastaukseen ei liity dataa, sillä headerin _content-length_ arvo on 0, ja samaa todistaa selain: mitään ei näy.

Syynä tälle käyttäytymiselle on se, että muuttujan _note_ arvoksi tulee _undefined_ jos muistiinpanoa ei löydy. Tilanne tulisi käsitellä palvelimella järkevämmin, eli statuskoodin 200 sijaan tulee vastata statuskoodilla [404 not found](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.5).

Tehdään koodiin muutos

```js
app.get('/notes/:id', (request, response) => {
  const id = Number(request.params.id)
  const note = notes.find(note => note.id === id)

  if ( note ) {
    response.json(note)
  } else {
    response.status(404).end()
  }
})
```

Koska vastaukseen ei nyt liity mitään dataa käytetään statuskoodin asettavan metodin [status](http://expressjs.com/en/4x/api.html#res.status) lisäksi metodia [end](http://expressjs.com/en/4x/api.html#res.end) ilmoittamaan siitä, että pyyntöön tulee vastata ilman dataa.

Koodin haarautumisessa hyväksikäytetään sitä, että mikä tahansa Javascript-olio on [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy), eli katsotaan todeksi vertailuoperaatiossa. undefined taas on [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) eli epätosi.

Nyt sovellus toimii, eli palauttaa oikean virhekoodin. Sovellus ei kuitenkaan palauta mitään käyttäjälle näytettävää kuten web-sovellukset yleensä tekevät jos mennään osoitteeseen jota ei ole olemassa. Emme kuitenkaan tarvitse nyt mitään näytettävää, sillä REST API:t ovat ohjelmalliseen käyttöön tarkoitettuja rajapintoja ja pyyntöön liitetty virheestä kertova statuskoodi on riittävä.

### Resurssin poisto

Toteutetaan seuraavaksi resurssin poistava route. Poisto tapahtuu tekemällä HTTP DELETE -pyyntö resurssin urliin:


```js
app.delete('/notes/:id', (request, response) => {
  const id = Number(request.params.id)
  notes = notes.filter(note => note.id !== id)

  response.status(204).end()
})
```

Jos poisto onnistuu, eli poistettava muistiinpano on olemassa, vastataan statuskoodilla [204 no content](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.2.5) sillä mukaan ei lähetetä mitään dataa.

Ei ole täyttä yksimielisyyttä siitä mikä statuskoodi DELETE-pyynnöstä pitäisi palauttaa jos poistettavaa resurssia ei ole olemassa. Vaihtoehtoja ovat lähinnä 204 ja 404. Yksinkertaisuuden vuoksi sovellus palauttaa nyt molemmissa tilanteissa statuskoodin 204.

### Postman

Herää kysymys miten voimme testata poisto-operaatiota? HTTP GET -pyyntöjä on helppo testata selaimessa. Voisimme toki kirjoittaa Javascript-koodin, joka testaa deletointia, mutta jokaiseen mahdolliseen tilanteeseen testikoodinkaan tekeminen ei ole aina paras ratkaisu.

On olemassa useita backendin testaamista helpottavia työkaluja, eräs näistä on edellisessä osassa nopeasti mainittu komentorivityökalu [curl](https://curl.haxx.se).

Käytetään nyt kuitenkin [postman](https://www.getpostman.com/)-nimistä sovellusta.


Asennetaan postman ja kokeillaan

![]({{ "/assets/3/8.png" | absolute_url }})

Postmanin käyttö on tässä tilanteessa suhteellisen yksinkertaista, riittää määritellä url ja valita oikea pyyntötyyppi.

Palvelin näyttää vastaavan oikein. Tekemällä HTTP GET osoitteeseen _http://localhost:3001/notes_ selviää että poisto-operaatio oli onnistunut, muistiinpanoa, jonka id on 2 ei ole enää listalla.

Koska muistiinpanot on talletettu palvelimen muistiin, uudelleenkäynnistys palauttaa tilanteen ennalleen.

### Visual Studio Coden REST client

Jos käytät Visual Studio Codea, voit postmanin sijaan käyttää VS Coden
[REST client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) -pluginia.

Kun plugin on asennettu, on sen käyttö erittäin helppoa. Tehdään projektin juureen hakemisto _requests_, jonka sisään talletetaan REST Client -pyynnöt _.rest_-päätteisinä tiedostoina.

Luodaan kaikki muistiinpanot hakevan pyynnön määrittelevä tiedosto _get_all_notes.rest_

![]({{ "/images/3/8a.png" | absolute_url }})

Klikkaamalla _Send request_ -tekstiä, REST client suorittaa määritellyn HTTP-pyynnön ja palvelimen vastaus avautuu editoriin:

![]({{ "/images/3/8b.png" | absolute_url }})

### Datan vastaanottaminen

Toteutetaan seuraavana uusien muistiinpanojen lisäys, joka siis tapahtuu tekemällä HTTP POST -pyyntö osoitteeseen _http://localhost:3001/notes_ ja liittämällä pyynnön mukaan eli [bodyyn](https://www.w3.org/Protocols/rfc2616/rfc2616-sec7.html#sec7) luotavan muistiinpanon tiedot JSON-muodossa.

Jotta pääsisimme pyynnön mukana lähetettyyn dataan helposti käsiksi, tarvitsemme [body-parser](https://github.com/expressjs/body-parser)-kirjaston apua.

Otetaan body-parser käyttöön ja luodaan alustava määrittely HTTP POST -pyynnön käsittelyyn

```js
const express = require('express')
const app = express()
const bodyParser = require('body-parser')

app.use(bodyParser.json())

//...

app.post('/notes', (request, response) => {
  const note = request.body
  console.log(note)

  response.json(note)
})
```

Tapahtumankäsittelijäfunktio pääsee dataan käsiksi viittaamalla _request.body_.

Ilman body-parser-käyttöönottoa pyynnön kentän _body_ arvo olisi ollut määrittelemätön. body-parserin toimintaperiaatteena on, että se ottaa pyynnön mukana olevan JSON-muotoisen datan, muuttaa sen Javascript-olioksi ja sijoittaa _request_-olion kenttään _body_ ennen kuin routen käsittelijää kutsutaan.

Toistaiseksi sovellus ei vielä tee vastaanotetulle datalle mitään muuta kuin tulostaa sen konsoliin ja palauttaa sen pyynnön vastauksessa.

Ennen toimintalogiikan viimeistelyä varmistetaan ensin postmanilla, että lähetetty tieto menee varmasti perille. Pyyntötyypin ja urlin lisäksi on määriteltävä myös pyynnön mukana menevä data eli _body_:

![]({{ "/assets/3/9.png" | absolute_url }})

Näyttää kuitenkin siltä, että mitään ei mene perille, palvelin vastaanottaa ainoastaan tyhjän olion. Missä on vika? Olemme unohtaneet määritellä headerille _Content-Type_ oikean arvon:

<img src="/assets/3/10.png" height="200">

Nyt kaikki toimii! Ilman oikeaa headerin arvoa palvelin ei osaa parsia dataa oikeaan muotoon. Se ei edes yritä arvailla missä muodossa data on, sillä potentiaalisia datan siirtomuotoja eli _Content-Typejä_ on olemassa [suuri määrä](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types).

Jos käytät VS Codea niin edellisessä luvussa esitelty REST client kannattaa asentaa viimeistään _nyt_. POST-pyyntö tehdään REST clientillä seuraavasti:

![]({{ "/images/3/8c.png" | absolute_url }})

Eli pyyntöä varten on luotu oma tiedosto _new_note.rest_. Pyyntö on muotoiltu [dokumentaation ohjetta](https://github.com/Huachao/vscode-restclient/blob/master/README.md#usage) noudatellen.

REST clientin eräs suuri etu postmaniin verratatuna on se, että pyynnöt saa kätevästi talletettua projektin repositorioon ja tällöin ne ovat helposti koko kehitystiimin käytössä. Postmanillakin on mahdollista tallettaa pyyntöjä, mutta tilanne menee helposti kaaoottiseksi etenkin jos työn alla on useita toisistaan riippumattomia projekteja.

> **Tärkeä sivuhuomio**
>
> Välillä debugatessa tulee vastaan tilanteita, joissa backendissä on tarve selvittää mitä headereja HTTP-pyynnöille on asetettu. Eräs menetelmä tähän on _request_-olion melko kehnosti nimetty metodi [get](http://expressjs.com/en/4x/api.html#req.get), jonka avulla voi selvittää yksittäisen headerin arvon. _request_-oliolla on myös kenttä _headers_, jonka arvona ovat kaikki pyyntöön liittyvät headerit.
>
> Ongelmia voi esim syntyä jos jätät vahingossa VS REST clientillä ylimmän rivin ja headerit määrittelevien rivien väliin tyhjän rivin. Tällöin REST client tulkitsee, että millekään headerille ei aseteta arvoa ja näin backend ei osaa tulkita pyynnön mukana olevaa dataa JSON:iksi.
>
> Puuttuvan _content-type_-headerin ongelma selviää kun backendissa tulostaa pyynnön headerit esim. komennolla console.log(request.headers)

Palataan taas sovelluksen pariin. Kun tiedämme, että sovellus vastaanottaa tiedon oikein, voimme viimeistellä sovelluslogiikan:

```js
app.post('/notes', (request, response) => {
  const maxId = notes.length > 0 ? notes.map(n => n.id).sort().reverse()[0] : 0
  const note = request.body
  note.id = maxId + 1

  notes = notes.concat(note)

  response.json(note)
})
```

Uudelle muistiinpanolle tarvitaan uniikki id. Ensin selvitetään olemassaolevista id:istä suurin muuttujaan _maxId_. Uuden muistiinpanon id:ksi asetetaan sitten _maxId+1_. Tämä tapa ei ole itseasiassa kovin hyvä, mutta emme nyt välitä siitä sillä tulemme pian korvaamaan tavan miten muistiinpanot talletetaan.

Tämän hetkisessä versiossa on vielä se ongelma, että voimme HTTP POST -pyynnöllä lisätä mitä tahansa kenttiä sisältäviä olioita. Parannellaan sovellusta siten, että kenttä _content_ vaaditaan. Kentille _important_ ja _date_ asetetaan oletusarvot. Kaikki muut kentät hylätään:

```js
const generateId = () => {
  const maxId = notes.length > 0 ? notes.map(n => n.id).sort().reverse()[0] : 1
  return maxId + 1
}

app.post('/notes', (request, response) => {
  const body = request.body

  if (body.content === undefined) {
    return response.status(400).json({error: 'content missing'})
  }

  const note = {
    content: body.content,
    important: body.important|| false,
    date: new Date(),
    id: generateId()
  }

  notes = notes.concat(note)

  response.json(note)
})
```

Tunnisteena toimivan id-kentän arvon generointilogiikka on eriytetty funktioon _generateId_.

Jos kenttä _content_ puuttuu, vastataan statuskoodilla [400 bad request](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.1):

```js
if (body.content === undefined) {
  return response.status(400).json({error: 'content missing'})
}
```

Huomaa, että returnin kutsuminen on tärkeää, jos sitä ei tapahdu, jatkaa koodi suoritusta metodin loppuun asti ja virheellinen muistiinpano tallettuu tietokantaan!

Jos content-kentällä on arvo, luodaan muistiinpano syötteen perusteella. Kuten edellisessä osassa mainitsimme, aikaleimoja ei kannata luoda selaimen koodissa, sillä käyttäjän koneen kellon aikaan ei voi luottaa. Aikaleiman eli kentän _date_ arvon generointi tapahtuukin nyt palvelimen toimesta.

Jos kenttä _important_ puuttuu, asetetaan sille oletusarvo _false_. Oletusarvo generoidaan nyt hieman erikoisella tavalla:

```js
important: body.important || false,
```

jos sovelluksen vastaanottamassa muuttujaan _body_ talletetussa datassa on kenttä _important_, tulee lausekkeelle sen arvo. Jos kenttää ei ole olemassa, tulee lausekkeen arvoksi oikeanpuoleinen osa eli _important_.

> Jos ollaan tarkkoja, niin kentän _body.important_ arvon ollessa _false_, tulee lausekkeen <code>body.important || false</code> arvoksi oikean puoleinen _false_...

Sovelluksen tämän hetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part3-notes-backend/tree/part3-1)

Huomaa, että repositorion master-haarassa on myöhemmän vaiheen koodi, tämän hetken koodi on tagissa [part3-1](https://github.com/FullStack-HY/part3-notes-backend/tree/part3-1):

![]({{ "/master/3/1b.png" | absolute_url }})

Jos kloonaat projektin itsellesi, suorita komento _npm install_ ennen käynnistämistä eli komentoa _npm start_.

## Tehtäviä

Tee nyt tehtävät [3.1-3.6](/tehtävät#expressin-alkeet)

## Huomioita HTTP pyyntötyyppien käytöstä

[HTTP-standardi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) puhuu pyyntötyyppien yhteydessä kahdesta ominaisuudesta, **safe** ja **idempotent**.

HTTP-pyynnöistä GET:in tulisi olla _safe_:

> In particular, the convention has been established that the GET and HEAD methods SHOULD NOT have the significance of taking an action other than retrieval. These methods ought to be considered "safe".

Safety siis tarkoittaa, että pyynnön suorittaminen ei saa aiheutta palvelimelle _sivuvaikutuksia_ eli esim. muuttaa palvelimen tietokannan tilaa, pyynnön tulee ainoastaan palauttaa palvelimella olevaa dataa.

Mikään ei automaattisesti takaa, että GET-pyynnöt olisivat luonteeltaan _safe_, kyseessä onkin HTTP-standardin suositus palvelimien toteuttajille. RESTful-periaatetta noudattaessa GET-pyyntöjä käytetäänkin aina siten, että ne ovat safe.

HTTP-standardi määrittelee myös pyyntötyypin [HEAD](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.4), jonka tulee olla safe. Käytännössä HEAD:in tulee toimia kuten GET, mutta se ei palauta vastauksenaan muuta kuin statuskoodin ja headerit, viestin bodyä HEAD ei palauta ollenkaan.

HTTP-pyynnöistä muiden paitsi POST:in tulisi olla _idempotentteja_:

> Methods can also have the property of "idempotence" in that (aside from error or expiration issues) the side-effects of N > 0 identical requests is the same as for a single request. The methods GET, HEAD, PUT and DELETE share this property

Eli jos pyynnöllä on sivuvaikutuksia, lopputulos on sama suoritetaanko pyyntö yhden tai useamman kerran.

Esim. jos tehdään HTTP PUT pyyntö osoitteeseen _/notes/10_ ja pyynnön mukana on <code>{ content: "ei sivuvaikutuksia", important: true }</code>, on lopputulos sama riippumatta siitä kuinka monta kertaa pyyntö suoritetaan.

Kuten metodin GET _safety_ myös _idempotence_ on HTTP-standardin suositus palvelimien toteuttajille. RESTful-periaatetta noudattaessa GET, HEAD, PUT ja DELETE-pyyntöjä käytetäänkin aina siten, että ne ovat idempotentteja.

HTTP pyyntötyypeistä POST on ainoa joka ei ole _safe_ eikä _idempotent_. Jos tehdään 5 kertaa HTTP POST -pyyntö osoitteeseen _/notes_ siten että pyynnön mukana on <code>{ content: "monta samaa", important: true }</code>, tulee palvelimelle 5 saman sisältöistä muistiinpanoa.

## Middlewaret

Äsken käyttöönottamamme [body-parser](https://github.com/expressjs/body-parser) on terminologiassa niin sanottu [middleware](http://expressjs.com/en/guide/using-middleware.html).

Middlewaret ovat funktioita, joiden avulla voidaan käsitellä _request_- ja _response_-olioita.

Esim. body-parser ottaa pyynnön mukana tulevan raakadatan _request_-oliosta, parsii sen Javascript-olioksi ja sijoittaa olion _request_:in kenttään _body_

Middlewareja voi olla käytössä useita jolloin ne suoritetaan peräkkäin siinä järjestyksessä kun ne on otettu koodissa käyttöön.

Toteutetaan itse yksinkertainen middleware, joka tulostaa konsoliin palvelimelle tulevien pyyntöjen perustietoja.

Middleware on funktio, joka saa kolme parametria:

```js
const logger = (request, response, next) => {
  console.log('Method:',request.method)
  console.log('Path:  ', request.path)
  console.log('Body:  ', request.body)
  console.log('---')
  next()
}
```

Middleware kutsuu lopussa parametrina olevaa funktiota _next_, jolla se siirtää kontrollin seuraavalle middlewarelle.

Middleware otetaan käyttöön seuraavasti:

```js
app.use(logger)
```

Middlewaret suoritetaan siinä järjestyksessä, jossa ne on otettu käyttään sovellusolion metodilla _use_. Middlewaret tulee myös määritellä ennen routeja jos ne halutaan suorittaa ennen niitä. On myös eräitä tapauksia, joissa middleware tulee määritellä vasta routejen jälkeen, käytännössä tällöin on kyse middlewareista, joita suoritetaan vain, jos mikään route ei käsittele HTTP-pyyntöä.

Lisätään routejen jälkeen seuraava middleware, jonka ansiosta saadaan routejen käsittelemättömistä virhetilanteista JSON-muotoinen virheilmoitus:

```js
const error = (request, response) => {
  response.status(404).send({error: 'unknown endpoint'})
}

app.use(error)
```

## Tehtäviä

Tee nyt tehtävät [3.7 ja 3.8](/tehtävät#lisää-middlewareja)

## Yhteys frontendiin

Palataan yritykseemme käyttää nyt tehtyä backendiä [osassa 2](/osa2) tehdyllä React-frontendillä. Aiempi yritys lopahti seuraavaan virheilmoitukseen

![]({{ "/assets/3/3.png" | absolute_url }})

Frontendin tekemä GET-pyyntö osoitteeseen <http://localhost:3001/notes> ei jostain syystä toimi. Mistä on kyse? Backend toimii kuitenkin selaimesta ja postmanista käytettäessä ilman ongelmaa.

### Same origin policy ja CORS

Kyse on asiasta nimeltään CORS eli Cross-origin resource sharing. [Wikipedian](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) sanoin

> Cross-origin resource sharing (CORS) is a mechanism that allows restricted resources (e.g. fonts) on a web page to be requested from another domain outside the domain from which the first resource was served. A web page may freely embed cross-origin images, stylesheets, scripts, iframes, and videos. Certain "cross-domain" requests, notably Ajax requests, are forbidden by default by the same-origin security policy.

Lyhyesti sanottuna meidän kontekstissa kyse on seuraavasta: websovelluksen selaimessa suoritettava Javascript-koodi saa oletusarvoisesti kommunikoida vain samassa [originissa](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) olevan palvelimen kanssa. Koska palvelin on localhostin portissa 3001 ja frontend localhostin portissa 3000, niiden origin ei ole sama.


Korostetaan vielä, että [same origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) ja CORS eivät ole mitenkään React- tai Node-spesifisiä asioita, vaan yleismaailmallisia periaatteita Web-sovellusten toiminnasta.

Voimme sallia muista _origineista_ tulevat pyynnöt käyttämällä Noden [cors](https://github.com/expressjs/cors)-middlewarea.

Asennetaan _cors_ komennolla

```bash
npm install cors --save
```

Otetaan middleware käyttöön ja sallitaan kaikki origineista tulevat pyynnöt:

```js
const cors = require('cors')

app.use(cors())
```

Nyt frontend toimii! Tosin muistiinpanojen tärkeäksi muuttavaa toiminnallisuutta backendissa ei vielä ole.

CORS:ista voi lukea tarkemmin esim. [Mozillan sivuilta](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

## Sovellus internettiin

Kun koko "stäkki" on saatu vihdoin kuntoon, siirretään sovellus internettiin. Viime aikoina on tullut uusia mielenkiintoisa sovellusten hostausmahdollisuuksia, esim. [Zeit](https://zeit.co). Käytetään seuraavassa vanhaa kunnon [Herokua](https://www.heroku.com).

Lisätään projektin juureen tiedosto _Procfile_, joka kertoo herokulle, miten sovellus käynnistetään

```bash
web: node index.js
```

Muutetaan tiedoston _index.js_ lopussa olevaa sovelluksen käyttämän portin määrittelyä seuraavasti:

```js
const PORT = process.env.PORT || 3001
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```

Nyt käyttöön tulee [ympäristömuuttujassa](https://en.wikipedia.org/wiki/Environment_variable) _PORT_ määritelty portti tai 3001 jos ympäristömuuttuja _PORT_ ei ole määritelty. Heroku konfiguroi sovelluksen portin ympäristömuuttujan avulla.

Tehdään projektihakemistosta git-repositorio, lisätään _.gitignore_ jolla seuraava sisältö

```bash
node_modules
```

Luodaan heroku-sovellus komennolla _heroku create_ ja deployataan sovellus komennoilla _git add -A_, _git commit -m \"Initiate app.\"_ ja _git push heroku master_.

Jos kaikki meni hyvin, sovellus toimii:

![]({{ "/images/3/11b.png" | absolute_url }})

Jos ei, vikaa voi selvittää herokun lokeja lukemalla, eli komennolla _heroku logs_.

Esim. tätä materiaalia tehdessä törmättiin ongelmaan joka aiheutti seuraavan tulostuksen lokeihin

![]({{ "/assets/3/11.png" | absolute_url }})

Syynä ongelmalle oli se, että middlewarea _cors_ asennettaessa oli unohtunut antaa optio __--save__, joka tallentaa tiedon riippuvuudesta tiedostoon _package.json_. Koska näin kävi, ei Heroku ollut asentanut corsia sovelluksen käyttöön.

> **HUOM** ainakin alussa on järkevää tarkkailla herokussa olevan sovelluksen lokeja koko ajan. Parhaiten tämä onnistuu antamalla komento _heroku logs -t_, jolloin logit tulevat konsoliin sitä mukaan kun palvelimella tapahtuu jotain.

Myös frontend toimii herokussa olevan backendin avulla. Voit varmistaa asian muuttamalla frontendiin määritellyn backendin osoitteen viittaamaan _localhost:3001_:n sijaan herokussa olevaan backendiin.

Seuraavaksi herää kysymys miten saamme myös frontendin internettiin? Vaihtoehtoja on useita.

### Frontendin tuotantoversio

Olemme toistaiseksi suorittaneet React-koodia _sovelluskehitysmoodissa_, missä sovellus on konfiguroitu antamaan havainnollisia virheilmoituksia, päivittämään koodiin tehdyt muutokset automaattisesti selaimeen ym.

Kun sovellus viedään tuotantoon, täytyy siitä tehdä [production build](https://reactjs.org/docs/optimizing-performance.html#use-the-production-build)
eli tuotantoa varten optimoitu versio.

create-react-app:in avulla tehdyistä sovelluksista saadaan muodostettua tuotantoversio komennolla [npm run build](https://github.com/facebookincubator/create-react-app#npm-run-build-or-yarn-build).

Suoritetaan nyt komento frontendin projektin juuressa.

Komennon seurauksena syntyy hakemiston _build_ (joka sisältää jo sovelluksen ainoan html-tiedoston _index.html_) sisään hakemisto _static_, minkä alle generoituu sovelluksen Javascript-koodin [minifioitu](https://en.wikipedia.org/wiki/Minification_(programming)) versio. Vaikka sovelluksen koodi on kirjoitettu useaan tiedostoon, generoituu kaikki Javascript yhteen tiedostoon, samaan tiedostoon tulee itseasiassa myös kaikkien sovelluksen koodin tarvitsemien riippuvuuksien koodi.

Minifioitu koodi ei ole miellyttävää luettavaa. Koodin alku näyttää seuraavalta:

```js
!function(e){function t(r){if(n[r])return n[r].exports;var o=n[r]={i:r,l:!1,exports:{}};return e[r].call(o.exports,o,o.exports,t),o.l=!0,o.exports}var n={};t.m=e,t.c=n,t.d=function(e,n,r){t.o(e,n)||Object.defineProperty(e,n,{configurable:!1,enumerable:!0,get:r})},t.n=function(e){var n=e&&e.__esModule?function(){return e.default}:function(){return e};return t.d(n,"a",n),n},t.o=function(e,t){return Object.prototype.hasOwnProperty.call(e,t)},t.p="/",t(t.s=12)}([function(e,t,n){"use strict";function r(e){return"[object Array]"===E.call(e)}function o(e){return"[object ArrayBuffer]"===E.call(e)}function a(e){return"undefined"!==typeof FormData&&e instanceof FormData}function i(e){return"undefined"!==typeof ArrayBuffer&&ArrayBuffer.isView?ArrayBuffer.isView(e):e&&e.buffer&&e.buffer instanceof ArrayBuffer}function u(e){return"string"===typeof e}function l(e){return"number"===typeof e}function s(e){return"undefined"===typeof e}function c(e){return null!==e&&"object"===typeof
```

### Staattisten tiedostojen tarjoaminen backendistä

Eräs mahdollisuus frontendin tuotantoon viemiseen on kopioida tuotantokoodi, eli hakemisto _build_ backendin repositorion juureen ja määritellä backend näyttämään pääsivunaan frontendin _pääsivu_, eli tiedosto _build/index.html_.

Aloitetaan kopioimalla frontendin tuotantokoodi backendin alle, projektin juureen. Omalla koneellani kopiointi tapahtuu frontendin hakemistosta käsin komennolla

```bash
cp -r build ../../osa3/notebackend
```

Backendin sisältävän hakemiston tulee nyt näyttää seuraavalta:

![]({{ "/images/3/11x.png" | absolute_url }})


Jotta saamme expressin näyttämään _staattista sisältöä_ eli sivun _index.html_ ja sen lataaman Javascriptin ym. tarvitsemme expressiin sisäänrakennettua middlewarea [static](http://expressjs.com/en/starter/static-files.html).

Kun lisäämme muiden middlewarejen määrittelyn yhteyteen seuraavan

```js
app.use(express.static('build'))
```

tarkastaa express GET-tyyppisten HTTP-pyyntöjen yhteydessä ensin löytyykö pyynnön polkua vastaavan nimistä tiedostoa hakemistosta _build_. Jos löytyy, palauttaa express tiedoston.

Nyt HTTP GET -pyyntö osoitteeseen _www.palvelimenosoite.com/index.html_ tai _www.palvelimenosoite.com_ näyttää Reactilla tehdyn frontendin. GET-pyynnön esim. osoitteeseen _www.palvelimenosoite.com/notes_ hoitaa backendin koodi.

Koska tässä tapauksessa sekä frontend että backend toimivat samassa osoitteessa, voidaan React-sovelluksessa tapahtuva backendin _baseUrl_ määritellä [suhteellisena](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2) URL:ina, eli ilman palvelinta yksilöivää osaa:

```js
import axios from 'axios'
const baseUrl = '/notes'

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

// ...
```

Muutoksen jälkeen on luotava uusi production build ja kopioitava se backendin repositorion juureen.

Sovellusta voidaan käyttää nyt _backendin_ osoitteesta <http://localhost:3001>:

![]({{ "/images/3/11f.png" | absolute_url }})

Sovelluksemme toiminta vastaa nyt täysin osan 0 luvussa [Single page app](/osa0#Single-page-app) läpikäydyn esimerkkisovelluksen toimintaa.

Kun mennään selaimella osoitteeseen <http://localhost:3001> palauttaa palvelin hakemistossa _build_ olevan tiedoston _index.html_, jonka sisältö hieman tiivistettynä on seuraava:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>React App</title>
    <link href="/static/css/main.d2f2b65b.css" rel="stylesheet">
  </head>
  <body>
    <div id="root"></div>
    <script type="text/javascript" src="/static/js/main.c18b620c.js"></script>
  </body>
</html>
```

Sivu sisältää ohjeen ladata sovelluksen tyylit määrittelevän CSS-tiedoston, sekä _script_-tagin, jonka ansiosta selain lataa sovelluksen Javascript-koodin, eli varsinaisen React-sovelluksen.

React-koodi hakee palvelimelta muistiinpanot osoitteesta <http://localhost:3001/api/notes> ja renderöi ne ruudulle. Selaimen ja palvelimen kommunikaatio selviää tuttuun tapaan konsolin välilehdeltä _Network_:

![]({{ "/images/3/11g.png" | absolute_url }})

Kun sovelluksen "internettiin vietävä" versio todetaan toimivan paikalliseksi, commitoidaan frontendin tuotantoversio backendin repositorioon ja pushataan koodi uudelleen herokuun.

[Sovellus](https://fullstack-notes.herokuapp.com) toimii moitteettomasti lukuunottamatta vielä backendiin toteuttamatonta muistiinpanon tärkeyden muuttamista:

![]({{ "/images/3/11c.png" | absolute_url }})

> **HUOM** ennen pushaamista on tietysti muistettava lisätä hakemisto _build_ repositorioon!

Sovelluksemme tallettama tieto ei ole ikuisesti pysyvää, sillä sovellus tallettaa muistiinpanot muuttujaan. Jos sovellus kaatuu tai se uudelleenkäynnistetään, kaikki tiedot katoavat.

Tarvitsemme sovelluksellemme tietokannan. Ennen tietokannan käyttöönottoa katsotaan kuitenkin vielä muutamaa asiaa.

## Frontendin deployauksen suoraviivaistus

Jotta uuden frontendin version generointi onnistuisi jatkossa ilman turhia manuaalisia askelia, tehdään frontendin repositorion juureen yksinkertainen shell-scripti, joka suorittaa uuden tuotantoversion buildaamisen eli komennon _npm run build_ ja sen siirron backendin alle. Annetaan skriptille nimeksi _deploy.sh_. Sisältö on seuraava

```bash
#!/bin/sh
npm run build
cp -r build ../../osa3/notebackend/
```

Skripille pitää antaa vielä suoritusoikeudet:

```bash
chmod u+x deploy.sh
```

Skripti voidaan suorittaa frontendin juuresta komennolla _./deploy.sh_


### Backendin urlit

Backendin tarjoama muistiinpanojen käsittelyn rajapinta on nyt suoraan sovelluksen URL:in <https://fullstack-notes.herokuapp.com> alla. Eli <https://fullstack-notes.herokuapp.com/notes> on kaikkien mustiinpanojen lista ym. Koska backendin roolina on tarjota frontendille koneluettava rajapinta, eli API, olisi ehkä parempi erottaa API:n tarjoama osoitteisto selkeämmin, esim. aloittamalla kaikki sanalla _api_.

Tehdään muutos ensin muuttamalla käsin **kaikki backendin routet**:

```js
//...
app.get('/api/notes', (request, response) => {
  response.json(notes)
})
//...
```

Frontendin koodiin riittää seuraava muutos

```js
import axios from 'axios'
const baseUrl = '/api/notes'

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

// ...
```

Muutosten jälkeen esim. kaikki muistiinpanot tarjoavan API-endpointin osoite on  <https://fullstack-notes.herokuapp.com/api/notes>

![]({{ "/images/3/11d.png" | absolute_url }})

Frontend on edelleen sovelluksen juuressa eli osoitteessa <https://fullstack-notes.herokuapp.com/>.

> Sivuhuomautus: API:en versiointi
>
>Joskus API:n urleissa ilmaistaan myös API:n versio. Eri versioita saatetaan tarvita, jos aikojen kuluessa API:in tehdään laajennuksia, jotka ilman versiointia hajoittaisivat olemassaolevia osia ohjelmista. Versioinnin avulla voidaan tuoda vanhojen rinnalle uusia, hieman eri tavalla toimivia versioita API:sta.
>
>API:n version ilmaiseminen URL:issa ei kuitenkaan ole välttämättä, ainakaan kaikkien mielestä järkevää vaikka tapaa paljon käytetäänkin. Oikeasta tavasta API:n versiointiin [kiistellään ympäri internettiä](https://stackoverflow.com/questions/389169/best-practices-for-api-versioning).

### Proxy

Frontendiin tehtyjen muutosten seurauksena on nyt se, että kun suoritamme frontendiä sovelluskehitysmoodissa, eli käynnistämällä sen komennolla _npm start_, yhteys backendiin ei toimi:

![]({{ "/images/3/11e.png" | absolute_url }})

Syynä tälle on se, että backendin osoite muutettiin suhteellisesti määritellyksi:

```js
const baseUrl = '/api/notes'
```

Koska frontend toimii osoitteessa _localhost:3000_, menevät backendiin tehtävät pyynnöt väärään osoitteeseen _localhost:3000/api/notes_. Backend toimii kuitenkin osoitteessa _localhost:3001_

create-react-app:illa luoduissa projekteissa ongelma on helppo ratkaista. Riittää, että frontendin repositorion tiedostoon _package.json_ lisätään seuraava määritelmä:

```bash
{
  "dependencies": {
    // ...
  },
  "scripts": {
    // ...
  },
  "proxy": "http://localhost:3001"
}
```

Uudelleenkäynnistyksen jälkeen Reactin sovelluskehitysympäristö toimii [proxynä](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#proxying-api-requests-in-development) ja jos React-koodi tekee HTTP-pyynnön palvelimen _http://localhost:3000_ johonkin osoitteeseen joka ei ole React-sovelluksen vastuulla (eli kyse ei ole esim. sovelluksen Javascript-koodin tai CSS:n lataamisesta), lähetetään pyyntö edelleen osoitteessa _http://localhost:3001_ olevalle palvelimelle.

Nyt myös frontend on kunnossa, se toimii sekä sovelluskehitysmoodissa että tuotannossa yhdessä palvelimen kanssa.

Eräs negatiivinen puoli käyttämässämme lähestymistavassa on se, että sovelluksen uuden version tuotantoon vieminen edellyttää frontendin koodin tuotantoversion generoinnista ja sen backendin repositorioin kopioimisesta huolehtivan skriptin _delpoy.sh_ suorittamisen. Tämä taas hankaloittaa automatisoidun [deployment pipelinen](https://martinfowler.com/bliki/DeploymentPipeline.html) toteuttamista. Deployment pipelinellä tarkoitetaan automatisoitua ja hallittua tapaa viedä koodi sovelluskehittäjän koneelta erilaisten testien ja laadunhallinnallisten vaiheiden kautta tuotantoympäristöön.

Tähänkin on useita erilaisia ratkaisuja (esim. sekä frontendin että backendin [sijoittaminen samaan repositorioon](https://github.com/mars/heroku-cra-node)), emme kuitenkaan nyt mene niihin.

Myös frontendin koodin deployaaminen omana sovelluksenaan voi joissain tilanteissa olla järkevää. create-react-app:in avulla luotujen sovellusten osalta se on [suoraviivaista](https://github.com/mars/create-react-app-buildpack).

Sovelluksen tämänhetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part3-notes-backend/tree/part3-2), tagissa _part3-2_.

## Tehtäviä

Tee nyt tehtävät [3.9-3.11](/tehtävät#yhteys-frontendiin-ja-vienti-tuotantoon)

## Node-sovellusten debuggaaminen

Node-sovellusten debuggaaminen on jossain määrin hankalampaa kuin selaimessa toimivan Javascriptin.

Vanha hyvä keino on tietysti konsoliin tulostelu. Se kannattaa aina. On mielipiteitä, joiden mukaan konsoliin tulostelun sijaan olisi syytä suosia jotain kehityneempää menetelmää, mutta en ole ollenkaan samaa mieltä. Jopa maailman aivan eliittiin kuuluvat open source -kehittäjät [käyttävät](https://tenderlovemaking.com/2016/02/05/i-am-a-puts-debuggerer.html) tätä [mentelmää](https://swizec.com/blog/javascript-debugging-slightly-beyond-console-log/swizec/6633).

### Visual Studio Code

Visual Studio Coden debuggeri voi olla hyödyksi joissain tapauksissa. Seuraavassa screenshot, missä koodi on pysäytetty kesken uuden muistiinpanon lisäyksen

![]({{ "/images/3/17a.png" | absolute_url }})

Koodi on pysähtynyt nuolen osoittaman _breakpointin_ kohdalle ja konsoliin on evaluoitu muuttujan _request.params_ arvo. Vasemmalla olevassa ikkunassa on nähtävillä myös muuta ohjelman tilaan liittyvää.

Ylhäällä olevista nuolista yms. voidaan kontrolloida debuggauksen etenemistä.

Itse en juurikaan käytä Visual Studio Code debuggeria.

### Chromen dev tools

Debuggaus onnisuu myös Chromen developer-konsolilla, käynnistämällä sovellus komennolla:

```bash
node --inspect index.js
```

Debuggeriin pääsee käsiksi kirjoittamalla chromen osoiteriville

```bash
chrome://inspect
```

Avautuvasta näkymästä valitaan debugattava sovellus:

![]({{ "/assets/3/18.png" | absolute_url }})

Debuggausnäkymä toimii kuten React-koodia debugattaessa, _source_-välilehdelle voidaan esim. asettaa breakpointeja, eli kohtia joihin suoritus pysähtyy:

![]({{ "/images/3/19a.png" | absolute_url }})


Kaikki sovelluksen console.log-tulostukset tulevat debuggerin _Console_-välilehdelle. Voit myös tutkia siellä muuttujien arvoja ja suorittaa mielivaltaista Javascript-koodia:

![]({{ "/images/3/20a.png" | absolute_url }})

### Epäile kaikkea

Full Stack -sovellusten debuggaaminen vaikuttaa alussa erittäin hankalalta. Kun kohta kuvaan tulee myös tietokanta ja frontend on yhdistetty backendiin, on potentiaalisia virhelähteitä todella paljon.

Kun sovellus "ei toimi", onkin selvitettävä missä vika on. On erittäin yleistä, että vika on sellaisessa paikassa, mitä ei osaa ollenkaan epäillä, ja menee minuutti-, tunti- tai jopa päiväkausia ennen kuin oikea ongelmien lähde löytyy.

Avainasemassa onkin systemaattisuus. Koska virhe voi olla melkein missä vaan, kaikkea pitää epäillä, ja tulee pyrkiä poissulkemaan ne osat tarkastelusta, missä virhe ei ainakaan ole. Konsoliin kirjoitus, Postman, debuggeri ja kokemus auttavat.

Virheiden ilmaantuessa ylivoimaisesti huonoin strategia on jatkaa koodin kirjoittamista. Se on tae siitä, että koodissa on pian kymmenen ongelmaa lisää ja niiden syyn selvittäminen on entistäkin vaikeampaa. Toyota Production Systemin periaate [Stop and fix](http://gettingtolean.com/toyota-principle-5-build-culture-stopping-fix/#.Wjv9axP1WCQ) toimii tässäkin yhteydessä paremmin kuin hyvin.

## Mongo

Jotta saisimme talletettua muistiinpanot pysyvästi, tarvitsemme tietokannan. Useimmilla laitoksen kursseilla on käytetty relaatiotietokantoja. Tällä kurssilla käytämme [MongoDB](https://www.mongodb.com/):tä, joka on ns. [dokumenttitietokanta](https://en.wikipedia.org/wiki/Document-oriented_database).

Dokumenttitietokannat poikkeavat jossain määrin relaatiotietokannoista niin datan organisointitapansa kuin kyselykielensäkin suhteen. Dokumenttitietokantojen ajatellaan kuuluvan sateenvarjotermin [NoSQL](https://en.wikipedia.org/wiki/NoSQL) alle. Lisää dokumenttitietokannoista ja NoSQL:stä Tietokantojen perusteiden [viikon 7 materiaalista](https://materiaalit.github.io/tikape-s17/part7/).

**Lue nyt Tietokantojen perusteiden dokumenttitietokantoja kuvaava osuus.** Jatkossa oletetaan, että hallitset käsitteet _dokumentti_ ja _kokoelma_ (collection).

MongoDB:n voi luonnollisesti asentaa omalle koneelle. Internetistä löytyy kuitenkin myös palveluna toimivia Mongoja (esim [mlab](https://mlab.com/) ja [MongoDbCloud](https://www.mongodb.com/cloud/atlas)) ja seuraava ohje olettaa, että käytössä on jo vuosien kokemuksella luotettavaksi havaittu [mlab](https://mlab.com/).

Mlab-kanta on helppo ottaa käyttöön suoraan [Herokun kautta](https://elements.heroku.com/addons/mongolab), vaikka tämä on maksutonta, edellyttää se luottokorttitietojen antamista Herokulle.

Määrittelemmekin seuraavassa kannan suoraan [mlab](https://mlab.com/):iin, jolloin luottokorttitietoja ei tarvita.

Aloita luomalla mlabiin käyttäjätili, saatuasi mlabilta verifiointimailin ja kirjauduttuasi mailin linkin kautta sisään, voit luoda tietokannan:

![]({{ "/images/3/12a.png" | absolute_url }})

Määrittele esim. _Amazon web services_, tyypiksi ilmainen _sandbox_ ja sijoituspaikaksi Irlanti ja anna kannalle sopiva nimi.

Kun kanta on hetken kuluttua valmis, mene tietokannan hallintanäkymään

![]({{ "/images/3/12b.png" | absolute_url }})

Näkymä kertoo _MongoDB URI:n_ eli osoitteen, jonka avulla sovelluksemme käyttämä MongoDB-kirjasto saa yhteyden kantaan.

Osoite näyttää seuraavalta:

```
mongodb://<dbuser>:<dbpassword>@ds211088.mlab.com:11088/fullstack-notes
```

Tarvitsemme kannan käyttöä varten _dbuserin_ eli käyttäjätunnuksen. Käyttäjätunnuksen luominen tapahtuu tietokannan hallintanäkymästä

![]({{ "/images/3/12c.png" | absolute_url }})

Jos luotiin käyttäjätunnus _fullstack_ jonka salasana on _sekred_, on tietokannan osoite seuraava:

```
mongodb://fullstack:sekred@ds211088.mlab.com:11088/fullstack-notes
```

Olemme nyt valmiina kannan käyttöön.

Voisimme käyttää kantaa Javascript-koodista suoraan Mongon virallisen
[MongoDB Node.js driver](https://mongodb.github.io/node-mongodb-native/) -kirjaston avulla, mutta se on ikävän työlästä. Käytämmekin hieman korkeammalla tasolla toimivaa [mongoose](http://mongoosejs.com/index.html)-kirjastoa.

Mongoosesta voisi käyttää luonnehdintaa _object document mapper_ (ODM), ja sen avulla Javascript-olioiden tallettaminen mongon dokumenteiksi on suoraviivaista.

Asennetaan mongoose:

```bash
npm install mongoose --save
```

Ei lisätä mongoa käsittelevää koodia heti backendin koodin sekaan, vaan tehdään erillinen kokeilusovellus tiedostoon _mongo.js_:

```js
const mongoose = require('mongoose')

// korvaa url oman tietokantasi urlilla. ethän laita salasanaa Gothubiin!
const url = 'mongodb://fullstack:sekred@ds211088.mlab.com:11088/fullstack-notes'

mongoose.connect(url)

const Note = mongoose.model('Note', {
  content: String,
  date: Date,
  important: Boolean
})

const note = new Note({
  content: 'HTML on helppoa',
  date: new Date(),
  important: true
})

note
  .save()
  .then(response => {
    console.log('note saved!')
    mongoose.connection.close()
  })
```

Kun koodi suoritetaan komennolla _node mongo.js_ lisää mongoose tietokantaaan uuden dokumentin.

Mlab:in hallintanäkymä näyttää lisäämämme datan:

![]({{ "/images/3/13a.png" | absolute_url }})

Kuten näkymä kertoo, on muistiinpanoa vastaava _dokumentti_ lisätty kokoelmaan (collection) nimeltään _notes_.

Koodi sisältää muutamia mielenkiintoisia asioita. Aluksi avataan yhteys ja määritellään, että mongoose käyttää _promiseja_, eikä oldschool-takaisinkutsufunktioita:

```js
const mongoose = require('mongoose')

const url = 'mongodb://fullstack:fullstack@ds211088.mlab.com:11088/fullstack-notes'

mongoose.connect(url)
```

Valitettavasti mongoosen dokumentaatiossa käytetään joka paikassa takaisinkutsufunktioita, joten sieltä ei kannata suoraan copypasteta koodia, sillä promisejen ja vanhanaikaisten callbackien sotkeminen samaan koodiin ei ole kovin järkevää.

<div class='important'>
Kannattaa muistaa, että tietokannan salasanaa ei kannata laittaa missään vaiheessa Githubiin!
</div>

### Skeema

Yhteyden avaamisen jälkeen määritellään mustiinpanoa vastaava [model](http://mongoosejs.com/docs/models.html):

```js
const Note = mongoose.model('Note', {
  content: String,
  date: Date,
  important: Boolean
})
```

Modelin parametrina määritellään _muistiinpanon_ [skeema](http://mongoosejs.com/docs/guide.html), joka kertoo mongooselle, miten muistiinpano-oliot tulee tallettaa tietokantaan.

Ensimmäisenä parametrina oleva _Note_ määrittelee, että mongoose tallettaa muistiinpanoa vastaavat oliot kokoelmaan nimeltään _notes_ sillä  [mongoosen konventiona](http://mongoosejs.com/docs/models.html) on määritellä kokoelmien nimet monikossa (esim. _notes_), kun niihin viitataan modelin määrittelyssä yksikkömuodossa (esim. _Note_).

Mongoosen dokumentaatiossa skeema ja sitä vastaava model määritellään kumpikin erikseen:

```js
const noteSchema = new mongoose.Schema({
  content: String,
  date: Date,
  important: Boolean
})

const Note = mongoose.model('Note', noteSchema);
```

Koska meillä ei ole skeema-oliolle muuta käyttöä kuin modelin parametrina, käytämme hyväksemme sitä, että skeema voidaan määritellä modeleille suoraan antamalla toisena parametrina skeeman määrittelevä olio.

Dokumenttikannat, kuten Mongo ovat _skeemattomia_, eli tietokanta itsessään ei välitä mitään sinne talletettavan tiedon muodosta. Samaan kokoelmaankin on mahdollista tallettaa olioita joilla on täysin eri kentät.

Mongoosea käytettäessä periaatteena on kuitenkin se, että tietokantaan talletettavalle tiedolle määritellään _sovelluksen koodin tasolla skeema_, joka määrittelee minkä muotoisia olioita kannan eri kokoelmiin talletetaan.

### Olioiden luominen ja tallettaminen

Seuraavaksi sovellus luo muistiinpanoa vastaavan [model](http://mongoosejs.com/docs/models.html):in avulla muistiinpano-olion:

```js
const note = new Note({
  content: 'Selain pystyy suorittamaan vain javascriptiä',
  date: new Date(),
  important: false
})
```

Modelit ovat ns. _konstruktorifunktioita_, jotka luovat parametrien perusteella Javascript-olioita. Koska oliot on luotu modelien konstruktorifunktiolla, niillä on kaikki modelien ominaisuudet, eli joukko metodeja, joiden avulla olioita voidaan mm. tallettaa tietokantaan.

Tallettaminen tapahtuu metodilla _save_. Metodi palauttaa _promisen_, jolle voidaan rekisteröidä _then_-metodin avulla tapahtumankäsittelijä:

```js
note
  .save()
  .then(result => {
    console.log('note saved!')
    mongoose.connection.close()
  })
```

Kun olio on tallennettu kantaan, kutsutaan _then_:in parametrina olevaa tapahtumankäsittelijää, joka sulkee tietokantayhteyden komennolla <code>mongoose.connection.close()</code>. Ilman yhteyden sulkemista ohjelman suoritus ei pääty.

Tallennusoperaation tulos on takaisinkutsun parametrissa _result_. Yhtä olioa tallentaessamme tulos ei ole kovin mielenkiintoinen, olion sisällön voi esim. tulostaa konsoliin jos haluaa tutkia sitä tarkemmin sovelluslogiikassa tai esim. debugatessa.

Talletetaan kantaan myös pari muuta muistiinpanoa muokkaamalla dataa koodista ja suorittamalla ohjelma uudelleen.

### Olioiden hakeminen tietokannasta

Kommentoidaan koodista uusia muistiinpanoja generoiva osa, ja korvataan se seuraavalla:

```js
Note
  .find({})
  .then(result => {
    result.forEach(note => {
      console.log(note)
    })
    mongoose.connection.close()
  })
```

Kun koodi suoritetaan, kantaan talletetut muistiinpanot tulostuvat.

Oliot haetaan kannasta _Note_-modelin metodilla [find](http://mongoosejs.com/docs/api.html#find_find). Metodin parametrina on hakuehto. Koska hakuehtona on tyhjä olio <code>{}</code>, saimme kannasta kaikki _notes_-kokoelmaan talletetut oliot.

Hakuehdot noudattavat mongon [syntaksia](https://docs.mongodb.com/manual/reference/operator/).

Voisimme hakea esim. ainoastaan tärkeät muistiinpanot seuraavasti:

```js
Note
  .find({ important: true })
  .then(result => {
    // ...
  })
```

### Tehtäviä

Tee nyt tehtävä [3.12](/tehtävät#mongoosen-alkeet)

## Tietokantaa käyttävä backend

Nyt meillä on periaatteessa hallussamme riittävä tietämys ottaa mongo käyttöön sovelluksessamme.

Aloitetaan nopean kaavan mukaan, copypastetaan tiedostoon _index.js_ mongoosen määrittelyt, eli

```js
const mongoose = require('mongoose')

const url = 'mongodb://fullstack:sekred@ds211088.mlab.com:11088/fullstack-notes'

mongoose.connect(url)

const Note = mongoose.model('Note', {
  content: String,
  date: Date,
  important: Boolean
})
```

ja muutetaan kaikkien muistiinpanojen hakemisesta vastaava käsittelijä seuraavaan muotoon

```js
app.get('/api/notes', (request, response) => {
  Note
    .find({})
    .then(notes => {
      response.json(notes)
    })
})
```

Voimme todeta selaimella, että backend toimii kaikkien dokumenttien näyttämisen osalta:

![]({{ "/assets/3/14.png" | absolute_url }})

Toiminnallisuus on muuten kunnossa, mutta frontend olettaa, että olioiden yksikäsitteinen tunniste on kentässä _id_. Emme myöskään halua näyttää frontendille mongon versiointiin käyttämää kenttää <em>\_\_v</em>. Tehdään pieni apufunktio, jonka avulla yksittäinen muistiinpano saadaan muutettua mongon sisäisestä esitysmuodosta haluamaamme muotoon:

```js
const formatNote = (note) => {
  return {
    content: note.content,
    date: note.date,
    important: note.important,
    id: note._id
  }
}
```

ja palautetaan HTTP-pyynnön vastauksena funktion avulla muotoiltuja oliota:

```js
app.get('/api/notes', (request, response) => {
  Note
    .find({})
    .then(notes => {
      response.json(notes.map(formatNote))
    })
})
```

Nyt siis muuttujassa _notes_ on taulukollinen mongon palauttamia olioita. Kun suoritamme operaation <code>notes.map(formatNote)</code> seurauksena on uusi taulukko, missä on jokaista alkuperäisen taulukon alkiota vastaava funktion _formatNote_ avulla muodostettu alkio.

Jos kannasta haettavilla olioilla olisi suuri määrä kenttiä, apufunktio _formatNote_ kannattaisi muotoilla hieman geneerisemmässä muodossa, esim:

```js
const formatNote = (note) => {
  const formattedNote = { ...note._doc, id: note._id }
  delete formattedNote._id
  delete formattedNote.__v

  return formattedNote
}
```

Ensimmäinen rivi luo uuden olion, mihin kopioituu kaikki vanhan olion kentät. Uuteen olioon lisätään myös kenttä _id_:

```js
const formattedNote = { ...note._doc, id: note._id }
```

Ennen olion palauttamista turhat kentät poistetaan.

Jos ohjelma käyttäisi muunkin tyyppisiä olioita kuin _muistiinpanoja_ sopisi sama funktio niidenkin muotoiluun. Jatkon kannalta on kuitenkin parempi, että pidämme alkuperäisen version funktiosta.

On myös mahdollista estää mongoosea palauttamasta tiettyjen kenttien arvoa, tai pyytää sitä palauttamaan vain tietyt kentät. Saamme estettyä parametrin <em>\_\_v</em>:n lisäämällä _find_-metodiin toiseksi parametriksi _{\_\_v: 0}_ seuraavasti:

```js
app.get('/api/notes', (request, response) => {
  Note
    .find({}, {__v: 0})
    .then(notes => {
      response.json(notes.map(formatNote))
    })
})
```

Kyselyjen palauttamien kenttien määrittely tapahtuu Mongon [syntaksin mukaan](https://docs.mongodb.com/manual/tutorial/project-fields-from-query-results/).

### Tietokantamäärittelyjen eriyttäminen omaksi moduuliksi

Ennen kuin täydennämme backendin muutkin osat käyttämään tietokantaa, eriytetään mongoose-spesifinen koodi omaan moduuliin.

Tehdään moduulia varten hakemisto _models_ ja sinne tiedosto _note.js_:

```js
const mongoose = require('mongoose')

const url = 'mongodb://fullstack:sekred@ds211088.mlab.com:11088/fullstack-notes'

mongoose.connect(url)

const Note = mongoose.model('Note', {
  content: String,
  date: Date,
  important: Boolean
})

module.exports = Note
```

Noden [moduulien](https://nodejs.org/docs/latest-v8.x/api/modules.html) määrittely poikkeaa hiukan osassa 2 määrittelemistämme frontendin käyttämistä [ES6-moduuleista](/osa2/#refaktorointia---moduulit).

Moduulin ulos näkyvä osa määritellään asettamalla arvo muuttujalle _module.exports_. Asetamme arvoksi määritellyn modelin _Note_. Muut moduulin sisällä määritellyt asiat, esim. muuttujat _mongoose_ ja _url_ eivät näy moduulin käyttäjälle.

Moduulin käyttöönotto tapahtuu lisäämällä tiedostoon _index.js_ seuraava rivi

```js
const Note = require('./models/note')
```

Näin muuttuja _Note_ saa arvokseen saman olion, jonka moduuli määrittelee.

### Muut operaatiot

Muutetaan nyt kaikki operaatiot tietokantaa käyttävään muotoon.

Uuden muistiinpanon luominen tapahtuu seuraavasti:

```js
app.post('/api/notes', (request, response) => {
  const body = request.body

  if (body.content === undefined) {
    return response.status(400).json({error: 'content missing'})
  }

  const note = new Note({
    content: body.content,
    important: body.important || false,
    date: new Date()
  })

  note
    .save()
    .then(savedNote => {
      response.json(formatNote(savedNote))
    })
})
```

Muistiinpano-oliot siis luodaan _Note_-konstruktorifunktiolla. Pyyntöön vastataan _save_-operaation takaisinkutsufunktion sisällä. Näin varmistutaan, että operaatio vastaus tapahtuu vain jos operaatio on onnistunut. Palaamme virheiden käsittelyyn myöhemmin.

Takaisinkutsufunktion parametrina oleva _savedNote_ on talletettu muistiinpano. HTTP-pyyntöön palautetaan kuitenkin siitä funktiolla _formatNote_ formatoitu muoto:

```js
response.json(formatNote(savedNote))
```
Yksittäisen muistiinpanon tarkastelu muuttuu muotoon

```js
app.get('/api/notes/:id', (request, response) => {
  Note
    .findById(request.params.id)
    .then(note => {
      response.json(formatNote(note))
    })
})
```

### Frontendin ja backendin yhteistominnallisuuden varmistaminen

Kun backendia laajennetaan, kannattaa sitä testailla aluksi **ehdottomasti selaimella,  postmanilla tai VS Coden REST clientillä:**. Seuraavassa kokeillaan uuden muistiinpanon luomista tietokannan käyttöönoton jälkeen:

![]({{ "/images/3/14b.png" | absolute_url }})

Vasta kun kaikki on todettu toimivaksi, kannattaa siirtyä testailemaan että muutosten jälkeinen backend toimii yhdessä myös frontendin kanssa. Kaikkien kokeilujen tekeminen ainoastaan frontendin kautta on todennäköisesti varsin tehotonta.

Todennäköisesti voi olla kannattavaa edetä frontin ja backin integroinnissa toiminnallisuus kerrallaan, eli ensin voidaan toteuttaa esim. kaikkien muistiinpanojen näyttäminen backendiin ja testata että toiminnallisuus toimii selaimella. Tämän jälkeen varmistetaan, että frontend toimii yhteen muutetun backendin kanssa. Kun kaikki on todettu olevan kunnossa, siirrytään seuraavan ominaisuuden toteuttamiseen.

Kun kuvioissa on mukana tietokanta, on tietokannan tilan tarkastelu mlabin hallintanäkymästä varsin hyödyllistä, usein myös suoraan tietokantaa käyttävät Node-apuohjelmat, kuten tiedostoon _mongo.js_ kirjoittamamme koodi auttavat sovellukehityksen edetessä.

Sovelluksen tämän hetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part3-notes-backend/tree/part3-3), tagissa _part3-3_.

### Tehtäviä

Tee nyt tehtävät [3.13-3.15](/tehtävät#backend-ja-tietokanta)

### Virheiden käsittely

Jos yritämme mennä selaimella sellaisen yksittäisen muistiinpanon sivulle, jota ei ole olemassa, eli esim. urliin <http://localhost:3001/api/notes/5a3b80015b6ec6f1bdf68d> missä _5a3b80015b6ec6f1bdf68d_ ei ole minkään tietokannassa olevan muistiinpanon tunniste, jää selain "jumiin" sillä palvelin ei vastaa pyyntöön koskaan.

Palvelimen konsolissa näkyykin virheilmoitus:

![]({{ "/assets/3/15.png" | absolute_url }})

Kysely on epäonnistunut ja kyselyä vastaava promise mennyt tilaan _rejected_. Koska emme käsittele promisen epäonnistumista, ei pyyntöön vastata koskaan. Osassa 2 tutustuimme jo [promisejen virhetilanteiden käsittelyyn](/osa2#promise-ja-virheet).

Lisätään tilanteeseen yksinkertainen virheidenkäsittelijä:

```js
app.get('/api/notes/:id', (request, response) => {
  Note
    .findById(request.params.id)
    .then(note => {
      response.json(formatNote(note))
    })
    .catch(error => {
      console.log(error)
      response.status(404).end()
    })
})
```

Kaikissa virheeseen päättyvissä tilanteissa HTTP-pyyntöön vastataan statuskoodilla 404 not found. Konsoliin tulostetaan tarkempi tieto virheestä.

Tapauksessamme on itseasiassa olemassa kaksi erityyppistä virhetilannetta. Toinen vastaa sitä, että yritetään hakea muistiinpanoa virheellisen muotoisella _id_:llä, eli sellasiella mikä ei vastaa mongon id:iden muotoa.

Jos teemme näin tulostuu konsoliin:

<pre>
Method: GET
Path:   /api/notes/5a3b7c3c31d61cb9f8a0343
Body:   {}
---
{ CastError: Cast to ObjectId failed for value "5a3b7c3c31d61cb9f8a0343" at path "_id"
    at CastError (/Users/mluukkai/opetus/_fullstack/osa3-muisiinpanot/node_modules/mongoose/lib/error/cast.js:27:11)
    at ObjectId.cast (/Users/mluukkai/opetus/_fullstack/osa3-muisiinpanot/node_modules/mongoose/lib/schema/objectid.js:158:13)
    ...
</pre>

Toinen virhetilanne taas vastaa tilannetta, missä haettavan muistiinpanon id on periaatteessa oikeassa formaatissa, mutta tietokannasta ei löydy indeksillä mitään:

<pre>
Method: GET
Path:   /api/notes/5a3b7c3c31d61cbd9f8a0343
Body:   {}
---
TypeError: Cannot read property '_doc' of null
    at formatNote (/Users/mluukkai/opetus/_fullstack/osa3-muisiinpanot/index.js:46:33)
    at Note.findById.then.note (/Users/mluukkai/opetus/_fullstack/osa3-muisiinpanot/index.js:65:21)
</pre>

Nämä tilanteen on syytä erottaa toisistaan, ja itseasiassa jälkimmäinen poikkeus on oman koodimme <code>/Users/mluukkai/opetus/\_fullstack/osa3-muisiinpanot/index.js:46</code> aiheuttama.

Muutetaan koodia seuraavasti:

```js
app.get('/api/notes/:id', (request, response) => {
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
      console.log(error)
      response.status(400).send({ error: 'malformatted id' })
    })
})
```

Jos kannasta ei löydy haettua olioa, muuttujan _note_ arvo on _undefined_ ja koodi ajautuu _else_-haaraan. Siellä vastataan kyselyyn _404 not found_.

Jos id ei ole hyväksyttävässä muodossa, ajaudutaan _catch_:in avulla määriteltyyn virheidenkäsittelijään. Sopiva statuskoodi on [400 bad request](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.1) koska kyse on juuri siitä:

> The request could not be understood by the server due to malformed syntax. The client SHOULD NOT repeat the request without modifications.

Vastaukseen on lisätty myös hieman dataa kertomaan virheen syystä.

Promisejen yhteydessä kannattaa melkeinpä aina lisätä koodiin myös virhetilainteiden käsittely, muuten seurauksena on usein hämmentäviä vikoja.

Ei ole koskaan huono idea tulostaa poikkeuksen aiheuttanutta olioa konsoliin virheenkäsittelijässä:

```js
.catch(error => {
  console.log(error)
  response.status(400).send({ error: 'malformatted id' })
})
```

Virheenkäsittelijään joutumisen syy voi olla joku ihan muu kuin mitä on tullut alunperin ajatelleeksi. Jos virheen tulostaa konsoliin, voi säästyä pitkiltä ja turhauttavilta väärää asiaa debuggaavilta sessioita.

Aina kun ohjelmoit ja projektissa on mukana backend **tulee ehdottomasti koko ajan pitää silmällä backendin konsolin tulostuksia**. Jos työskentelet pienellä näytöllä, riittää että konsolista on näkyvissä edes pieni kaistale:

![]({{ "/images/3/15b.png" | absolute_url }})

### loput operaatiot

Toteutetaan vielä jäljellä olevat operaatiot, eli yksittäisen muistiinpanon poisto ja muokkaus.

Poisto onnistuu helpoiten metodilla [findByIdAndRemove](http://mongoosejs.com/docs/api.html#findbyidandremove_findByIdAndRemove):

```js
app.delete('/api/notes/:id', (request, response) => {
  Note
    .findByIdAndRemove(request.params.id)
    .then(result => {
      response.status(204).end()
    })
    .catch(error => {
      response.status(400).send({ error: 'malformatted id' })
    })
})
```

Vastauksena on statauskoodi _204 no content_ molemmissa "onnistuneissa" tapauksissa, eli jos olio poistettiin tai olioa ei ollut mutta _id_ oli periaatteessa oikea. Takaisinkutsun parametrin _result_ perusteella olisi mahdollisuus haarautua ja palauttaa tilanteissa eri statuskoodi jos sille on tarvetta.

Muistiinpanon tärkeyden muuttamisen mahdollistava olemassaolevan muistiinpanon päivitys onnistuu helposti metodilla [findOneAndUpdate](http://mongoosejs.com/docs/api.html#findoneandupdate_findOneAndUpdate). Tässä ja myöhemmin sivulla on _findOneAndUpdate_, mutta koodissa alla [findByIdAndUpdate](http://mongoosejs.com/docs/api.html#findbyidandupdate_findByIdAndUpdate), joka vastaa `findOneAndUpdate({ _id: id }, ...)` kutsua.

```js
app.put('/api/notes/:id', (request, response) => {
  const body = request.body

  const note = {
    content: body.content,
    important: body.important
  }

  Note
    .findByIdAndUpdate(request.params.id, note, { new: true } )
    .then(updatedNote => {
      response.json(formatNote(updatedNote))
    })
    .catch(error => {
      console.log(error)
      response.status(400).send({ error: 'malformatted id' })
    })
})
```

Operaatio mahdollistaa myös muistiinpanon sisällön editoinnin. Päivämäärän muuttaminen ei ole mahdollista.

Huomaa, että metodin _findOneAndUpdate_ parametrina tulee antaa normaali Javascript-olio, eikä uuden olion luomisessa käytettävä _Note_-konstruktorifunktiolla luotu olio.

Pieni, mutta tärkeä detalji liittyen operaatioon _findOneAndUpdate_. Oletusarvoisesti tapahtumankäsittelijä saa parametrikseen _updatedNote_ päivitetyn olion [ennen muutosta](http://mongoosejs.com/docs/api.html#findoneandupdate_findOneAndUpdate) olleen tilan. Lisäsimme operaatioon parametrin <code>{ new: true }</code> jotta saamme muuttuneen olion palautetuksi kutsujalle.

Backend vaikuttaa toimivan postmanista VS Code REST clientistä tehtyjen kokeilujen perusteella ja myös frontend toimii moitteettomasti tietokantaa käyttävän backendin kanssa.

### Tehtäviä

Tee nyt tehtävät [3.16-3.18](/tehtävät#lisää-operaatioita)

## Refaktorointia - promisejen ketjutus

Useat routejen tapahtumankäsittelijöistä muuttivat palautettavan datan oikeaan formaattiin kutsumalla metodia _formatNote_:

```js
const formatNote = (note) => {
  return {
    id: note._id,
    content: note.content,
    date: note.date,
    important: note.important
  }
}
```

esim uuden muistiinpanon luomisessa metodia kutsutaan _then_:in parametrina palauttama olio parametrina:

```js
app.post('/api/notes', (request, response) => {
  // ...

  note
    .save()
    .then(savedNote => {
      response.json(formatNote(savedNote))
    })

})
```

Voisimme tehdä saman myös hieman tyylikkäämmin [promiseja ketjuttamalla](https://javascript.info/promise-chaining):

```js
app.post('/api/notes', (request, response) => {
  // ...

  note
    .save()
    .then(savedNote => {
      return formatNote(savedNote)
    })
    .then(savedAndFormattedNote => {
      response.json(savedAndFormattedNote)
    })

})
```

Eli ensimmäisen _then_:in takaisinkutsussa otamme mongoosen palauttaman olion _savedNote_ ja formatoimme sen. Operaation tulos palautetaan returnilla. Kuten osassa 2 [todettiin](/osa2/#palvelimen-kanssa-tapahtuvan-kommunikoinnin-eristäminen-omaan-moduuliin), promisen then-metodi palauttaa myös promisen. Eli kun palautamme _formatNote(savedNote)_:n takaisinkutsufunktiosta, syntyy promise, jonka arvona on formatoitu muistiinpano. Pääsemme käsiksi arvoon rekisteröimällä _then_-kutsulla uuden tapahtumankäsittelijän.

Itseasiassa selviämme vieläkin tiiviimmällä koodilla:

```js
app.post('/api/notes', (request, response) => {
  // ...

  note
    .save()
    .then(formatNote)
    .then(savedAndFormattedNote => {
      response.json(savedAndFormattedNote)
    })

})
```

koska _formatNote_ on viite funktioon, on oleellisesti ottaen kyse samasta kuin kirjoittaisimme:

```js
app.post('/api/notes', (request, response) => {
  // ...

  note
    .save()
    .then(savedNote => {
      return {
        id: savedNote._id,
        content: savedNote.content,
        date: savedNote.date,
        important: savedNote.important
      }
    })
    .then(savedAndFormattedNote => {
      response.json(savedAndFormattedNote)
    })

})
```

## Sovelluksen vieminen tuotantoon

Sovelluksen pitäisi toimia tuotannossa, eli herokussa sellaisenaan. Frontendin muutosten takia on tehtävä siitä uusi tuotantoversio ja kopioitava se backendiin.

Sovellusta voi käyttää sekä frontendin kautta <https://fullstack-notes.herokuapp.com>, ja myös API:n <https://fullstack-notes.herokuapp.com/api/notes> suora käyttö selaimella ja postmanilla onnistuu.

Sovelluksessamme on tällä hetkellä eräs ikävä piirre. Tietokannan osoite on kovakoodattu backendiin ja samaa tietokantaa käytetään sekä tuotannossa, että sovellusta kehitettäessä.

Tarvitsemme oman kannan sovelluskehitystä varten. Luodaan mlabiin toinen tietokanta ja sille käyttäjä.

Tietokannan osoitetta ei kannata kirjoittaa koodiin. Eräs hyvä tapa tietokannan osoitteen määrittelemiseen on [ympäristömuuttujien](https://en.wikipedia.org/wiki/Environment_variable) käyttö.

Talletetaan kannan osoite ympäristömuuttujaan _MONGODB_URI_.

Ympäristömuuttujiin pääsee Node-sovelluksesta käsiksi seuraavasti:

```js
const mongoose = require('mongoose')

const url = process.env.MONGODB_URI

// ...

module.exports = Note
```

Tee muutos koodiin ja deployaa uusi versio herokuun. Sovelluksen pitäisi toimia kun asetat ympäristömuuttujan arvo herokuun komennolla _heroku config:set_

```bash
heroku config:set MONGODB_URI=mongodb://fullstack:sekred@ds211088.mlab.com:11088/fullstack-notes
```

Sovelluksen pitäisi toimia muutosten jälkeen. Aina kaikki ei kuitenkaan mene suunnitelmien mukaan. Jos ongelmia ilmenee, _heroku logs_ auttaa. Oma sovellukseni ei toiminut muutoksen jälkeen. Loki kertoi seuraavaa

![]({{ "/images/3/21.png" | absolute_url }})

eli tietokannan osoite olikin jostain syystä määrittelemätön. Komento _heroku config_ paljasti että olin vahingossa määritellyt ympäristömuuttujan _MONGO_URL_ kun koodi oletti sen olevan nimeltään _MONGODB_URI_.

Muutoksen jälkeen sovellus ei toimi paikallisesti, koska ympäristömuuttujalla _MONGODB_URI_ ei ole mitään arvoa. Tapoja määritellä ympäristömuuttujalle arvo on monia, käytetään nyt [dotenv](https://www.npmjs.com/package/dotenv)-kirjastoa.

Asennetaan kirjasto komennolla

```bash
npm install dotenv --save
```

Sovelluksen juurihakemistoon tehdään sitten tiedosto nimeltään _.env_, minne tarvittavien ympäristömuuttujien arvot asetetaan. Määritellään tiedostoon sovelluskehitystä varten luodun tietokannan osoite:

```bash
MONGODB_URI=mongodb://fullstack:sekred@ds111078.mlab.com:11078/fullstact-notes-dev
```

Tiedosto .env **tulee heti gitignorata** sillä emme halua julkaista .env -tiedoston sisältöä verkkoon.

dotenvissä määritellyt ympäristömuuttujat otetaan koodissa käyttöön komennolla

```js
require('dotenv').config()
```

ja niihin viitataan Nodessa kuten "normaaleihin" ympäristömuuttujiin syntaksilla _process.env.MONGODB_URI_

Otetaan dotenv käyttöön seuraavasti:

```js
const mongoose = require('mongoose')

if ( process.env.NODE_ENV !== 'production' ) {
  require('dotenv').config()
}

const url = process.env.MONGODB_URI

// ...

module.exports = Note
```

Nyt dotenvissä olevat ympäristömuuttujat otetaan käyttöön ainoastaan silloin kun sovellus ei ole _production_- eli tuotantomoodssa (kuten esim. Herokussa).

Uudelleenkäynnistyksen jälkeen sovellus toimii taas paikallisesti.

Node-sovellusten konfigurointiin on olemassa ympäristömuuttujien ja dotenvin lisäksi lukuisia vaihtoehtoja, mm. [node-conf](https://github.com/lorenwest/node-config). Ympäristömuuttujien käyttö riittää meille nyt, joten emme rupea overengineeraamaan. Palaamme aiheeseen kenties myöhemmin.

Sovelluksen tämän hetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part3-notes-backend/tree/part3-4), tagissa _part3-4_.

### Tehtäviä

Tee nyt tehtävät [3.19 - 3.21](/tehtävät#loppuhuipennus)

## Lint

Ennen osan lopetusta katsomme vielä nopeasti paitsioon jäänyttä tärkeää työkalua [lintiä](https://en.wikipedia.org/wiki/Lint_(software)). Wikipedian sanoin:

> Generically, lint or a linter is any tool that detects and flags errors in programming languages, including stylistic errors. The term lint-like behavior is sometimes applied to the process of flagging suspicious language usage. Lint-like tools generally perform static analysis of source code.

Staattisesti tyypitetyissä, käännettävissä kielissä esim. Javassa ohjelmointiympäristöt, kuten NetBeans osaavat huomautella monista koodiin liittyvistä asioista, sellaisistakin, jotka eivät ole välttämättä käännösvirheitä. Erilaisten [staattisen analyysin](https://en.wikipedia.org/wiki/Static_program_analysis) lisätyökalujen, kuten [checkstylen](http://checkstyle.sourceforge.net/) avulla voidaan vielä laajentaa Javassa huomautettavien asioiden määrää koskemaan koodin tyylillisiä seikkoja, esim. sisentämistä.

Javascript-maailmassa tämän hetken johtava työkalu staattiseen analyysiin, eli "linttaukseen" on [ESlint](https://eslint.org/).

Asennetaan ESlint backendiin kehitysaikaiseksi riippuvuudeksi komennolla

```bash
npm install eslint --save-dev
```

Tämän jälkeen voidaan muodostaa alustava ESlint-konfiguraatio komennolla

```bash
node_modules/.bin/eslint --init
```

Vastaillaan kysymyksiin:

![]({{ "/images/3/24.png" | absolute_url }})

Konfiguraatiot tallentuvat tiedostoon _.eslintrc.js_:

```js
module.exports = {
    "env": {
        "node": true
        "es6": true
    },
    "extends": "eslint:recommended",
    "rules": {
        "indent": [
            "error",
            4
        ],
        "linebreak-style": [
            "error",
            "unix"
        ],
        "quotes": [
            "error",
            "single"
        ],
        "semi": [
            "error",
            "never"
        ]
    }
};
```

Muutetaan heti konfiguraatioista sisennystä määrittelevä sääntö, siten että sisennystaso on 2 välilyöntiä

```
"indent": [
    "error",
    2
],
```

Esim tiedoston _index.js_ tarkastus tapahtuu komennolla

```bash
node_modules/.bin/eslint index.js
```

Kannattaa ehkä tehdä linttaustakin varten _npm-skripti_:

```bash
{
  // ...
  "scripts": {
    "start": "node index.js",
    "watch": "nodemon index.js",
    "lint": "eslint ."
  },
  // ...
}
```

Nyt komennot _npm run lint_ suorittaa tarkastukset koko projektille.

Myös hakemistossa _build_ oleva frontendin tuotantoversio tulee näin tarkastettua. Sitä emme kuitenkaan halua, eli tehdään projektin juureen tiedosto [.eslintignore](https://eslint.org/docs/user-guide/configuring#ignoring-files-and-directories) ja sille seuraava sisältö

```bash
build
```

Näin koko hakemiston _build_ sisältö jätetään huomioimatta linttauksessa.

Lintillä on jonkin verran huomautettavaa koodistamme:

![]({{ "/images/3/22.png" | absolute_url }})

Ei kuitenkaan korjata ongelmia vielä.

Parempi vaihtoehto kuin linttauksen suorittaminen komentoriviltä on konfiguroida editorille _lint-plugin_, joka suorittaa linttausta koko ajan. Näin pääset korjaamaan pienet virheet välittömästi. Tietoja esim. Visual Studion ESlint-pluginsta [täällä](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint).

VS Coden ESlint-plugin alleviivaa tyylisääntöjä rikkovat kohdat punaisella:

![]({{ "/images/3/23.png" | absolute_url }})

Näin ongelmat on helppo korjata koodiin heti.

ESlintille on määritelty suuri määrä [sääntöjä](https://eslint.org/docs/rules/), joita on helppo ottaa käyttöön muokkaamalla tiedostoa _.eslintrc.js_.

Otetaan käyttöön sääntö [eqeqeq](https://eslint.org/docs/rules/eqeqeq) joka varoittaa, jos koodissa yhtäsuuruutta verrataan muuten kuin käyttämällä kolmea = -merkkiä. Sääntö lisätään konfiguraatiotiedostoon kentän _rules_ alle.

```bash
"rules": {
  // ...
  "eqeqeq": "error"
},
```

Tehdään samalla muutama muukin muutos tarkastettaviin sääntöihin.

Estetään rivien lopussa olevat [turhat välilyönnit](https://eslint.org/docs/rules/no-trailing-spaces), vaaditaan että [aaltosulkeiden edessä/jälkeen on aina välilyönti](https://eslint.org/docs/rules/object-curly-spacing) ja vaaditaan myös konsistenttia välilyöntien käyttöä [nuolifunktioiden parametrien suhteen](https://eslint.org/docs/rules/arrow-spacing):

```bash
"rules": {
  // ...
  "eqeqeq": "error",
  "no-trailing-spaces": "error",
  "object-curly-spacing": [
      "error", "always"
  ],
  "arrow-spacing": [
      "error", { "before": true, "after": true }
  ]
},
```

Oletusarvoinen konfiguraatiomme ottaa käyttöön joukon valmiiksi määriteltyjä sääntöjä _eslint:recommended_

```bash
"extends": "eslint:recommended",
```

Mukana on myös _console.log_-komennoista varoittava sääntö-
Yksittäisen sääntö on helppo kytkeä [pois päältä](https://eslint.org/docs/user-guide/configuring#configuring-rules) määrittelemällä sen "arvoksi" konfiguraatiossa 0. Tehdään toistaiseksi näin säännölle _no-console_.

```bash
"rules": {
  // ...
  "eqeqeq": "error",
  "no-trailing-spaces": "error",
  "object-curly-spacing": [
      "error", "always"
  ],
  "arrow-spacing": [
      "error", { "before": true, "after": true }
  ],
  "no-console": 0
},
```

**HUOM** kun teet muutoksia tiedostoon _.eslintrc.js_, kannattaa muutosten jälkeen suorittaa linttaus komentoriviltä ja varmistaa että konfiguraatio ei ole viallinen:

![]({{ "/images/3/25.png" | absolute_url }})

Jos konfiguraatiossa on jotain vikaa, voi editorin lint-plugin näyttää mitä sattuu.

Monissa yrityksissä on tapana määritellä yrityksen laajuiset koodausstandardit ja näiden käyttöä valvova ESlint-konfiguraatio. Pyörää ei kannata välttämättä keksiä uudelleen ja voi olla hyvä idea ottaa omaan projektiin joku käyttöön jossain muualla hyväksi havaittu konfiguraatio. Viime aikoina monissa projekteissa on omaksuttu AirBnB:n [Javascript](https://github.com/airbnb/javascript)-tyyliohjeet ottamalla käyttöön firman määrittelemä [ESLint](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb)-konfiguraatio.

Sovelluksen tämän hetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part3-notes-backend/tree/part3-5), tagissa _part3-5_.

### Tehtäviä

Tee nyt viimeinen tehtävä [3.22](/tehtävät#lint)
