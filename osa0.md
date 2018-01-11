---
layout: page
title: osa 0
inheader: yes
permalink: /osa0/
---

## Yleistä

Kurssilla tutustutaan Javascriptilla tapahtuvaan moderniin websovelluskehitykseen. Pääpaino on React-kirjaston avulla toteutettavissa single page -sovelluksissa, ja niitä tukevissa Node.js:llä toteutetuissa REST-rajapinnoissa.

Kurssilla käsitellään myös sovellusten testaamista, konfigurointia ja suoritusympäristöjen hallintaa sekä NoSQL-tietokantoja.

## Oletetut esitiedot

Osallistujilta edellytetään vahvaa ohjelmointiruutiinia, web-ohjelmoinnin ja tietokantojen perustuntemusta (esim. opintojakson Tietokantasovellus-suoritusta) sekä valmiutta omatoimiseen tiedonhakuun.

Kurssille osallistuminen ei edellytä käsiteltyjen tekniikoiden tai Javascript-kielen hallintaa.

## Kurssimateriaali

Kurssimateriaali on tarkoitettu luettavaksi osa kerrallaan "alusta loppuun". Materiaalin seassa on tehtäviä, jotka on sijoiteltu siten, että kunkin tehtävän tekemiseen pitäsi olla riittävät tekniset valmiudet sitä edeltävässä materiaalissa. Voit siis tehdä tehtävä sitä mukaa kun niitä tulee materiaalissa vastaan. Voi myös olla, että koko osan tehtävät kannattaa tehdä vasta luettuaan ensin osan alusta loppuun kertaalleen. Useissa osissa tehtävät ovat samaa ohjelmaa laajentavia pienistä osista koostuvia kokonaisuuksia. Muutamia tehtävien ohjelmia kehitetään eteenpäin useamman osan aikana. 

Toki tehtävät voi tehdä materiaalia lukemattakin, jos esitiedot ovat riittävät.

Materiaali perustuu muutamien osasta osaan vaihtuvien esimerkkiohjelmien asteittaiseen laajentamiseen. Materiaali toiminee parhaiten, jos kirjoitat samalla koodin myös itse ja teet koodiin myös pieniä modifikaatioita. Materiaalin käyttämien ohjelmien koodin eri vaiheiden tilanteita on tallennettu Githubiin.

## Suoritustapa

Kurssi koostuu kahdeksasta osata, joista ensimmäinen on historiallisista syistä numero nolla. Osat voi tulkita löyhästi ajatellen viikoiksi. Osia kuitenkin ilmestyy nopeampaa tahtia, ja suoritusnopeuskin on melko vapaa.

Materiaalissa osasta _n_ osaan _n+1_ eteneminen ei ole mielekästä ennen kuin riittävä osaaminen osan _n_ asioista on saavutettu. Kurssilla sovelletaankin pedagogisin termein _tavoiteoppimista_,[engl. mastery learning](https://en.wikipedia.org/wiki/Mastery_learning) ja on tarkoitus, että etenet seuraavaan osaan vasta, kun riittävä määrä (noin 80%) edellisen osan tehtävistä on tehty. Jokaisen osan _vapaaehtoiset_ (mutta arvosteluun vaikuttavat) tehtävät on merkattu tähdellä. 

Estääksemme sen, että aloitat kurssin tekemisen vasta viimeisellä viikolla, on jokaisella osalla  myös hard deadline. Etenemiselle on siis jonkun verran joustoa, jotta ehdit tekemään kustakin osasta tarvittavan määrän tehtäviä. 

Osien _hard deadlinet_ ja arvioidut keskimääräiset (kaikkien tehtävien tekemisen vaatimat) työmäärät seuraavassa:

| osa            | deadline&nbsp; &nbsp;    | työmäärä  |
| -------------- |:-----------------:       |:-----:|
| osa 0 ja osa 1 | 28.1.                    | 10 |
| osa 2          | 4.2.                     | 10 |
| osa 3          | 11.2.                    | 10 |
| osa 4          | 25.2.                    | 15 |
| osa 5          | 4.3.                     | 15 |
| osa 6          | 11.3.                    | 15 |
| osa 7          | 11.3.                    | - |


Keskimääräisten työmäärien arvio perustuu kurssin [betaversiota](https://mluukkai.github.io) joululoman aikana testanneilta 13:lta opiskelijalta kerättyyn dataan. Kannattaa huomata, että työmäärissä on suuri varianssi, esim. osan 0 ja 1 yhteenlaskettu ajankäyttö vaihteli 5 ja 20 tunnin väliltä.

Tämän kurssin eri osiin jo tehtyjen palautusten ajankäyttöstatistiikan näet [tehtävien palautussivulta](http://studies.cs.helsinki.fi/fs2018-stats) .

Suuri jousto suoritusajoissa tuo opiskelijalle entistä suuremman vastuun omasta ajankäytöstä. Nyt ilmoteutista deadlineista **ei jousteta ollenkaan**. Kannattaa muistaa se, että mitä lähempänä deadlinea tehtävien tekemisen aloittaa, sitä suurempi vaara on joutua eriasteisiin konfiguraatiohelvetteihin. Apuakin on rajallisemmin tarjolla kun kello lähenee deadlinepäivän keskiyötä.

## Arvosteluperiaatteet

Arvosana määräytyy tehtyjen tehtävien perusteella. Noin 50% deadlineen mennessä tehdyistä tehtävistä tuo arvosanan 1 ja 80% arvosanan 5. Kurssin lopussa on koe, joka on suoritettava hyväksytysti. Koe ei kuitenkaan vaikuta arvosanaan.

Kurssilta on mahdollisuus ansaita lisäopintopisteitä. Jos teet 87.5% kurssin tehtävistä, saat yhden lisäopintopisteen. Tekemällä 95% tehtävistä saat 2 lisäopintopistettä. Opetushallinnollisista syistä lisäopintopisteet rekisteröidään omana kurssinaan. Kurssin nimi määritellään myöhemmin.

## Alkutoimet

Tällä kurssilla suositellaan Chrome-selaimen käyttöä sillä se tarjoaa parhaan välineistön web-sovelluskehitystä ajatellen.

Kurssin tehtävät palautetaan GitHubiin, joten Git tulee olla asennettuna ja sitä on syytä osata käyttää.

Asenna myös joku järkevä webkoodausta tukeva tekstieditori, enemmän kuin suositeltava valinta on [Visual studio code](https://code.visualstudio.com/). Myös [Atom](https://atom.io/) on tarkoitukseen toimiva. 

Älä koodaa nanolla, notepadilla tai geditillä. Netbeanskaan ei ole omimmillaan Web-koodauksessa ja se on myös turhan raskas verrattuna esim. Visual Studio Codeen.

Asenna koneeseesi heti myös [Node.js](https://nodejs.org/en/). Materiaali on tehty versiolla 8.6, älä asenna mitään sitä vanhempaa versiota.

Asennusohjeita on koottu [tänne](/asennusohjeita). 

Noden myötä koneelle asentuu myös Node package manager [npm](https://www.npmjs.com/get-npm) jota tulemme tarvitsemaan kurssin aikana aktiivisesti. Tuoreen noden kera asentuu myös [npx](https://www.npmjs.com/package/npx), jota tarvitaan myös muutaman kerran.  

## Typoja materiaalissa

Kurssilla on paljon materiaalia ja se on olosuhteiden pakosta kirjoitettu todella nopeasti. Materiaalissa on betatestauksesta ja oikoluvuista huolimatta kirjoitusvirheitä. Jos löydät kirjoitusvirheen tai joku asia on sanottu epäselvästi tai kielioppisääntöjen vastaisesti, tee _pull request_ repositoriossa <https://github.com/FullStack-HY/FullStack-Hy.github.io> olevaan kurssimateriaaliin. 

Pull requestin tekeminen on helppoa. Esim. jos tällä sivulla on typo, mene sivun "lähdekoodiin" https://github.com/FullStack-HY/FullStack-Hy.github.io/blob/master/osa0.md klikkaa kynän kuvaa oikealta, tee muutokset ja klikkaa muutama kerta "vihreää" [Ohtu-kurssin ohjeen mukaan](https://github.com/mluukkai/ohjelmistotuotanto2017/blob/master/laskarit/2.md#typokorjauksia--vinkkejä).



<div class='important'>
<h1>TÄSTÄ ENTEENPÄIN DOKUMENTTIA EI OLE PÄVITETTY</h1>

lukeminen omalla vastuulla
</div>

## Osan 0 oppimistavoitteet

- Web-sovellusten toiminnan perusteet
  - HTML:n perusteet
  - HTTP-protokolla: metodit GET ja POST, statuskoodit, headerit
  - palvelimella suoritettavan koodin rooli
  - selaimessa suoritettavan javascript:in rooli
  - JSON-muotoinen data
  - DOM
  - sivujen ulkoasun muotoilun periaate CSS:llä
  - single page app -periaate
- Chrome developer konsolin peruskäyttö

## Web-sovelluksen toimintaperiaatteita ##

Käymme aluksi läpi web-sovellusten toimintaperiaatteita tarkastelemalla osoitteessa <https://fullstack-exampleapp.herokuapp.com/> olevaa esimerkkisovellusta. Huom.
sovelluksen toinen versio on osoitteessa <https://exampleapp-ghqykidlgq.now.sh/>, voit käyttää kumpaa vaan.

Sovelluksen olemassaolon tarkoitus on ainoastaan havainnollistaa kurssin peruskäsitteistöä, sovellus ei ole missään tapauksessa esimerkki siitä _miten_ web-sovelluksia kannattaisi kehittää, päinvastoin, sovellus käyttää eräitä vanhentuneita tekniikoita sekä huonoja käytänteitä.

Kurssin suosittelemaa tyyliä noudattavan koodin kirjoittaminen alkaa luvusta [React](#react).

Käytä nyt ja _koko ajan_ tämän kurssin aikana Chrome-selainta.

Avataan selaimella [esimerkkisovellus](https://fullstack-exampleapp.herokuapp.com/).

<div class="important">
  <h3>Web-sovelluskehityksen sääntö numero yksi</h3>
  Pidä selaimen developer-konsoli koko ajan auki
</div>

Konsoli avautuu macilla painamalla yhtä aikaa _alt_ _cmd_ ja _i_.

Ennen kun jatkat eteenpäin, selvitä miten saat koneellasi konsolin auki (googlaa tarvittaessa) ja muista pitää se auki *aina* kun teet web-sovelluksia.

Konsoli näyttää seuraavalta:
![]({{ "/assets/1/1.png" | absolute_url }})

Varmista, että välilehti _Network_ on avattuna ja aktivoi valinta _Disable cache_ kuten kuvassa on tehty.

**HUOM:** konsolin tärkein välilehti on _Console_. Käytämme nyt johdanto-osassa kuitenkin ensin melko paljon välilehteä _Network_.

### HTTP GET

Selain ja web-palvelin kommunikoivat keskenään [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)-protokollaa käyttäen. Avoinna oleva konsolin Network-tabi kertoo miten selain ja palvelin kommunikoivat.

Kun nyt reloadaat selaimen, kertoo konsoli, että tapahtuu kaksi asiaa
- selain hakee web-palvelimelta sivun _fullstack-exampleapp.herokuapp.com/_ sisällön
- ja lataa kuvan _kuva.png_

![]({{ "/assets/1/2.png" | absolute_url }})

Jos ruutusi on pieni, saatat joutua isontamaan konsoli-ikkunaa, jotta saat selaimen tekemät haut näkyviin.

Klikkaamalla näistä ensimmäistä, paljastuu tarkempaa tietoa siitä mistä on kyse:

![]({{ "/assets/1/3.png" | absolute_url }})

Ylimmästä osasta _General_ selviää, että selain teki [GET](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET)-metodilla pyynnön osoitteeseen _https://fullstack-exampleapp.herokuapp.com/_ ja että pyyntö oli onnistunut, sillä pyyntöön saatiin vastaus, jonka [Status code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) on 200.

Pyyntöön ja palvelimen lähettämään vastaukseen liittyy erinäinen määrä otsakkeita eli [headereita](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields):

![]({{ "/assets/1/4.png" | absolute_url }})

Ylempänä oleva _Response headers_ kertoo mm. vastauksen koon tavuina ja vastaushetken. Tärkeä headeri _Content-Type_ kertoo, että vastaus on [utf-8](https://en.wikipedia.org/wiki/UTF-8) muodossa oleva teksti-tiedosto, jonka sisältö on muotoiltu HTML:llä. Näin selain tietää, että kyseessä on normaali [HTML](https://en.wikipedia.org/wiki/HTML)-sivu, joka tulee renderöidä käyttäjän selaimeen.

Välilehti _Preview_ näyttää miltä pyyntöön vastauksena lähetetty data näyttää. Kyseessä on siis normaali HTML-sivu, jonka _body_-osassa määritellään selaimessa näytettävän sivun rakenne:

![]({{ "/assets/1/5.png" | absolute_url }})

Sivu sisältää _div_-elementin, jonka sisällä on otsikko sekä tieto luotujen muistiinpanojen määrästä, linkki sivulle _muistiinpanot_ ja kuvaa vastaava _img_-tagi.

img-tagin ansiosta selain tekee HTTP-pyynnön, jonka avulla se hakee kuvan _kuva.png_ palvelimelta. Pyynnön tiedot näyttävät seuraavalta:

![]({{ "/assets/1/6.png" | absolute_url }})

eli pyyntö on tehty osoitteeseen _https://fullstack-exampleapp.herokuapp.com/kuva.png_ ja se on tyypiltään HTTP GET. Vastaukseen liittyvät headerit kertovat että vastauksen koko on 89350 tavua ja vastauksen _Content-type_ on _image/png_, eli kyseessä on png-tyyppinen kuva. Tämän tiedon ansiosta selain tietää miten kuva on sijoitettava HTML-sivulle.

### Perinteinen web-sovellus

Esimerkkisovelluksen pääsivu toimii perinteisen web-sovelluksen tapaan. Mentäessä sivulle, selain hakee palvelimelta sivun strukturoinnin ja tekstuaalisen sisällön määrittelevän HTML-dokumentin.

Palvelin on muodostanut dokumentin jollain tavalla. Dokumentti voi olla _staattista sisältöä_ eli palvelimen hakemistossa oleva tekstitiedosto. Dokumentti voi myös olla _dynaaminen_, eli palvelin voi muodostaa HTML-dokumentit esim. tietokannassa olevien tietojen perusteella. Esimerkkisovelluksessa sivun HTML-koodi on muodostettu dynaamisesti sillä se sisältää tiedon luotujen muistiinpanojen lukumäärästä.

Etusivun muodostama koodi näyttää seuraavalta:

```js
const getFrontPageHtml = (noteCount) => {
  return (`
    <!DOCTYPE html>
    <html>
      <head>
      </head>
      <body>
        <div class="container">
          <h1>Full stack -esimerkkisovellus</h1>
          <p>muistiinpanoja luotu ${noteCount} kappaletta</p>
          <a href="/notes">muistiinpanot</a>
          <img src="kuva.png" width="200" />
        </div>
      </body>
    </html>
  `)
}

app.get('/', (req, res) => {
  const page = getFrontPageHtml(notes.length)
  res.send(page)
})
```

Koodia ei tarvitse vielä ymmärtää, mutta käytännössä HTML-sivun sisältö on talletettu ns. template stringinä. Etusivun dynaamisesti muuttuva osa, eli muistiinpanojen lukumäärä (koodissa _noteCount_) korvataan template stringissä sen hetkisellä konkreettisella lukuarvolla (koodissa _notes.length_).

Perinteisissä websovelluksissa selain on "tyhmä", se ainoastaan pyytää palvelimelta HTML-muodossa olevia sisältöjä, kaikki sovelluslogiikka on palvelimessa. Palvelin voi olla tehty esim. kurssin [Web-palvelinohjelmointi, Java](https://courses.helsinki.fi/fi/tkt21007/119558639) tapaan Springillä tai, [Tietokantasovelluksessa](http://tsoha.github.io/#/johdanto#top) PHP:llä tai [Ruby on Railsilla](http://rubyonrails.org/). Esimerkissä on käytetty Node.js:n [Express](https://expressjs.com/)-sovelluskehystä. Tulemme käyttämään kurssilla Node.js:ää ja Expressiä web-palvelimen toteuttamiseen.

### Selaimessa suoritettava sovelluslogiikka

Pidä konsoli edelleen auki. Tyhjennä konsolin näkymä painamalla vasemmalla olevaa &empty;-symbolia.

Kun menet nyt muistiinpanojen sivulle, selain tekee 4 HTTP-pyyntöä:

![]({{ "/assets/1/7.png" | absolute_url }})

Kaikki pyynnöt ovat _eri tyyppisiä_. Ensimmäinen pyyntö on tyypiltään _document_
kyseessä on sivun HTML-koodi, joka näyttää seuraavalta:

![]({{ "/assets/1/8.png" | absolute_url }})

Kun vertaamme, selaimen näyttämää sivua ja pyynnön palauttamaa HTML-koodia, huomaamme, että koodi ei sisällä ollenkaan kolmea muistiinpanoa sisältävää listaa.

HTML-koodin _head_ osio sisältää _script_-tagin, jonka ansiosta selain lataa _main.js_-nimisen javascript-tiedoston palvelimelta.

Ladattu javascript-koodi näyttää seuraavalta

```js
var xhttp = new XMLHttpRequest()

xhttp.onreadystatechange = function () {
  if (this.readyState == 4 && this.status == 200) {
    const data = JSON.parse(this.responseText)
    console.log(data)

    var ul = document.createElement('ul')
    ul.setAttribute('class', 'notes')

    data.forEach(function(note) {
      var li = document.createElement('li')

      ul.appendChild(li);
      li.appendChild(document.createTextNode(note.content))
    })

    document.getElementById('notes').appendChild(ul)
  }
}

xhttp.open('GET', '/data.json', true)
xhttp.send()
```

Koodin yksityiskohdat eivät ole tässä vaiheessa vielä oleellisia.

Heti ladattuaan _script_-tagin sisältämän javascriptin selain suorittaa koodin.

Kaksi viimeistä riviä määrittelevät, että selain tekee GET-tyyppisen HTTP-pyynnön osoitteeseen palvelimen osoitteeseen _/data.json_:

```js
xhttp.open('GET', '/data.json', true)
xhttp.send()
```

Kyseessä on alin Network tabin näyttämistä selaimen tekemistä pyynnöistä.

Voimme kokeilla mennä osoitteeseen <https://fullstack-exampleapp.herokuapp.com/data.json> suoraan selaimella:

![]({{ "/assets/1/9.png" | absolute_url }})

Osoitteesta löytyvät muistiinpanot [JSON](https://en.wikipedia.org/wiki/JSON)-muotoisena "raakadatana". Oletusarvoisesti selain ei osaa näyttää JSON-dataa kovin hyvin, mutta on olemassa lukuisia plugineja jotka hoitavat muotoilun. Asenna nyt chromeen esim. [JSONView](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc) ja lataa sivu uudelleen. Data on nyt miellyttävämmin muotoiltua:

![]({{ "/assets/1/10.png" | absolute_url }})

Ylläoleva javascript-koodi siis lataa muistiinpanot sisältävän JSON-muotoisen datan ja muodostaa datan avulla selaimeen "bulletlistan" muistiinpanojen sisällöstä:

Tämän saa aikaan seuraava koodi:

```js
const data = JSON.parse(this.responseText)
console.log(data)

var ul = document.createElement('ul')
ul.setAttribute('class', 'notes')

data.forEach(function(note) {
  var li = document.createElement('li')

  ul.appendChild(li);
  li.appendChild(document.createTextNode(note.content))
})

document.getElementById('notes').appendChild(ul)
```

Koodi muodostaa ensin järjestämätöntä listaa edustavan [ul](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ul)-tagin:

```js
var ul = document.createElement('ul')
ul.setAttribute('class', 'notes')
```

ja lisää ul:n sisään yhden [li](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/li)-elementin kutakin muistiinpanoa kohti. Ainoastaan muistiinpanon _content_-kenttä tulee li-elementin sisällöksi, raakadatassa olevia aikaleimoja ei käytetä mihinkään.


```js
data.forEach(function(note) {
  var li = document.createElement('li')

  ul.appendChild(li);
  li.appendChild(document.createTextNode(note.content))
})
```

Avaa nyt konsolin _Console_-välilehti:

![]({{ "/assets/1/11.png" | absolute_url }})

Painamalla rivin alussa olevaa kolmiota saat laajennettua konsolissa olevan rivin:

![]({{ "/assets/1/12.png" | absolute_url }})

Konsoliin ilmestynyt tulostus johtuu siitä, että koodiin oli lisätty komento _console.log_:

```js
const data = JSON.parse(this.responseText)
console.log(data)
```

eli vastaanotettuaan datan palvelimelta, koodi tulostaa datan konsoliin.

Tulet tarvitsemaan komentoa _console.log_ kurssilla todella monta kertaa...

### Tapahtumankäsittelijä ja takaisinkutsu ###

Koodin rakenne on hieman erikoinen:

```js
var xhttp = new XMLHttpRequest()

xhttp.onreadystatechange = function () {
  // koodi, joka käsittelee palvelimen vastauksen
}

xhttp.open('GET', '/data.json', true)
xhttp.send()
```

eli palvelimelle tehtävä pyyntö suoritetaan vasta viimeisellä rivillä. Palvelimen vastauksen käsittelyn määrittelevä koodi on kirjoitettu jo aiemmin. Mistä on kyse?

Rivillä
```js
xhttp.onreadystatechange = function () {
```

kyselyn tekevään <code>xhttp</code>-olioon määritellään _tapahtumankäsittelijä_ (event handler) tilanteelle _onreadystatechange_. Kun kyselyn tekevän olion tila muuttuu, kutsuu selain tapahtumankäsittelijänä olevaa funktiota. Funktion koodi tarkastaa, että [readyState](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/readyState):n arvo on 4 (joka kuvaa tilannetta _The operation is complete_) ja, että vastauksen HTTP-statuskoodi on onnistumisesta kertova 200.

```js
xhttp.onreadystatechange = function () {
  if (this.readyState == 4 && this.status == 200) {
    // koodi, joka käsittelee palvelimen vastauksen
  }
}
```

Tapahtumankäsittelijöihin liittyvä mekanismi koodin suorittamiseen on javascriptissä erittäin yleistä. Tapahtumankäsittelijöinä olevia javascript-funktioita kutsutaan [callback](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)- eli takaisinkutsufunktioiksi sillä sovelluksen koodi ei kutsu niitä itse vaan suoritusympäristö, eli web-selain suorittaa funktion kutsumisen sopivana ajankohtana, eli kyseisen _tapahtuman_ tapahduttua.

### Document Object Model eli DOM ###

Voimme ajatella, että HTML-sivut muodostavat implisiittisen puurakenteen

<pre>
html
  head
    link
    script
  body
    div
      h1
      div
        ul
          li
          li
          li
      form
        input
        input
</pre>

Sama puumaisuus on nähtävissä konsolin välilehdellä _Elements_

![]({{ "/assets/1/13.png" | absolute_url }})

Selainten toiminta perustuu ideaan esittää HTML-elementit puurakenteena.

Document Object Model eli [DOM](https://en.wikipedia.org/wiki/Document_Object_Model) on ohjelmointirajapinta eli _API_, jonka mahdollistaa selaimessa esitettävien web-sivujen muokkaamisen ohjelmallisesti.

Edellisessä luvussa esittelemämme javascript-koodi käytti nimenomaan DOM-apia lisätäkseen sivulle muistiinpanojen listan.

HTML-dokumentin

![]({{ "/assets/1/13b.png" | absolute_url }})

DOM:a havainnollistava kuva Wikipedian sivulta:

![](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/DOM-model.svg/428px-DOM-model.svg.png)

### document-olio ja sivun manipulointi konsolista ###

HTML-dokumenttia esittävän DOM-puun ylimpänä solmuna on olio nimeltään _document_. Olioon pääsee käsiksi Console-välilehdeltä:

![]({{ "/assets/1/14.png" | absolute_url }})

Voimme suorittaa konsolista käsin DOM-apin avulla erilaisia operaatioita selaimessa näytettävälle web-sivulle hyödyntämällä _document_-olioa.

Lisätään nyt sivulle uusi muistiinpano suoraan konsolista.

Haetaan ensin sivulta muistiinpanojen lista, eli sivun ul-elementeistä ensimmäinen:
```js
lista = document.getElementsByTagName('ul')[0]
```

luodaan uusi li-elementti ja lisätään sille sopiva tekstisisältö:

```js
uusi = document.createElement('li')
uusi.textContent = 'Sivun manipulointi konsolista on helppoa'
```

liitetään li-elementti listalle:

```js
lista.appendChild(uusi)
```

![]({{ "/assets/1/15.png" | absolute_url }})

Vaikka selaimen näyttämä sivu päivittyy, ei muutos ole lopullinen. Jos sivu uudelleenladataan, katoaa uusi muistiinpano, sillä muutos ei mennyt palvelimelle asti. Selaimen lataama javascript luo muistiinpanojen listan aina palvelimelta osoitteesta <https://fullstack-exampleapp.herokuapp.com/data.json> haettavan JSON-muotoisen raakadatan perusteella.

### CSS ###

Muistiinpanojen sivun HTML-koodin _head_-osio sisältää _link_-tagin, joka määrittelee, että selaimen tulee ladata palvelimelta osoitteesta [main.css](https://fullstack-exampleapp.herokuapp.com/main.css) sivulla käytettävä [css](https://developer.mozilla.org/en-US/docs/Web/CSS)-tyylitiedosto

Cascading Style Sheets eli CSS on kieli, jonka avulla web-sovellusten ulkoasu määritellään.

Ladattu css-tiedosto näyttää seuraavalta:

```css
.container {
  padding: 10px;
  border: 1px solid;
}

.notes {
  color: blue;
}
```

Tiedosto määrittelee kaksi [luokkaselektoria](https://developer.mozilla.org/en-US/docs/Web/CSS/Class_selectors), joiden avulla valitaan tietty sivun alue ja määritellään alueelle sovellettavat tyylisäännöt.

Luokkaselektori alkaa aina pisteellä ja sisältää luokan nimen.

Luokat ovat [attribuuteja](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/class) joita voidaan liittää HTML-elementeille.

Konsolin _Elements_-välilehti mahdollistaa class-attribuuttien tarkastelun:

![]({{ "/assets/1/16.png" | absolute_url }})

sovelluksen uloimmalle _div_-elementille on siis liitetty luokka _container_. Muistiinpanojen listan sisältävä _ul_-elementin sisällä oleva lista sisältää luokan _notes_.

CSS-säännön avulla on määritelty, että _container_-luokan sisältävä elementti ympäröidään yhden pikselin paksuisella [border](https://developer.mozilla.org/en-US/docs/Web/CSS/border):illa. Elementille asetetaan myös 10 pikselin [padding](https://developer.mozilla.org/en-US/docs/Web/CSS/padding), jonka ansiosta elementin sisältön ja elementin ulkorajan väliin jätetään hieman tilaa.

Toinen määritelty CSS-sääntö asettaa muistiinpanojen kirjainten värin siniseksi.

HTML-elementeillä on muitakin attribuutteja kuin luokkia. Muistiinpanot sisältävä _div_-elementti sisältää [id](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/id)-attribuutin. Javascript-koodi hyödyntää attribuuttia elementin etsimiseen.

Konsolin _Elements_-välilehdellä on mahdollista manipuloida elementtien tyylejä:
![]({{ "/assets/1/17.png" | absolute_url }})

Tehdyt muutokset eivät luonnollisestikaan jää voimaan kun selaimen sivu uudelleenladataan, eli jos muutokset halutaan pysyviksi, tulee ne konsolissa tehtävien kokeilujen jälkeen tallettaa palvelimella olevaan tyylitiedostoon.

### Lomake ja HTTP POST ###

Tutkitaan seuraavaksi sitä, miten uusien muistiinpanojen luominen tapahtuu. Tätä varten muistiinpanojen sivu sisältää lomakkeen eli [formin](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Your_first_HTML_form).

![]({{ "/assets/1/18.png" | absolute_url }})

Kun lomakkeen painiketta painetaan, lähettää selain lomakkeelle syötetyn datan palvelimelle. Avataan _Network_-tabi ja katsotaan miltä lomakkeen lähettäminen näyttää:

![]({{ "/assets/1/19.png" | absolute_url }})

Lomakkeen lähettäminen aiheuttaa yllättäen yhteensä _viisi_ HTTP-pyyntöä. Näistä ensimmäinen vastaa lomakkeen lähetystapahtumaa. Tarkennetaan siihen:

![]({{ "/assets/1/20.png" | absolute_url }})

Kyseessä on siis HTTP POST -pyyntö ja se on tehty palvelimen osoitteeseen <em>new\_note</em>. Palvelin vastaa pyyntöön HTTP-statuskoodilla 302. Kyseessä on ns. [uudelleenohjauspyyntö](https://en.wikipedia.org/wiki/URL_redirection) eli redirectaus, minkä avulla palvelin kehoittaa selainta tekemään automaattisesti uuden HTTP GET -pyynnön headerin _Location_ kertomaan paikkaan, eli osoitteeseen _notes_.

Selain siis lataa uudelleen muistiinpanojen sivun. Sivunlataus saa aikaan myös kolme muuta HTTP-pyyntöä: tyylitiedoston (main.css), javascript-koodin (main.js) ja muistiinpanojen raakadatan (data.json) lataamisen.

Network-välilehti näyttää myös lomakkeen mukana lähetetyn datan:

![]({{ "/assets/1/21.png" | absolute_url }})

Jos käytät normaalia Chrome-selainta, ei konsoli ehkä näytä lähetettävää dataa. Kyseessä on eräissä Chromen versioissa oleva [bugi](https://bugs.chromium.org/p/chromium/issues/detail?id=766715). Bugi on korjattu Chromen uusimpaan versioon.

Lomakkeen lähettäminen tapahtuu HTTP POST -pyyntönä ja osoitteeseen _new_note_ form-tagiin määriteltyjen attribuuttien _action_ ja _method_ ansiosta:

<img src="/assets/1/22.png" height="150">

POST-pyynnöstä huolehtiva palvelimen koodi on yksinkertainen (huom: tämä koodi on siis palvelimella eikä näy selaimessasi):

```js
app.post('/new_note', (req, res) => {
  notes.push({
    content: req.body.note,
    date: new Date()
  })

  return res.redirect('/notes')
})
```

POST-pyyntöihin liitettävä data lähetetään pyynnön mukana "runkona" eli [bodynä](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST). Palvelin saa POST-pyynnön datan pyytämällä sitä pyyntöä vastaavan olion _req_ kentästä _req.body_.

Tekstikenttään kirjoitettu data on avaimen _note_ alla, eli palvelin viittaa siihen _req.body.note_.

Palvelin luo uutta muistiinpanoa vastaavan olion ja laittaa sen muistiinpanot sisältävään taulukkoon nimeltään _notes_:

```js
notes.push({
  content: req.body.note,
  date: new Date()
})
```

Muistiinpano-olioilla on siis kaksi kenttää, varsinaisen sisällön kuvaava _content_ ja luomishetken kertova _date_.

Palvelin ei talleta muistiinpanoja tietokantaan, joten uudet muistiinpanot katoavat aina Herokun uudelleenkäynnistäessä palvelun.

## Single page app ##

Esimerkkisovelluksemme pääsivu toimii perinteisten web-sivujen tapaan, kaikki sovelluslogiikka on palvelimella, selain ainoastaan renderöi palvelimen lähettämää HTML-koodia.

Muistiinpanoista huolehtivassa sivussa osa sovelluslogiikasta, eli olemassaolevien muistiinpanojen HTML-koodin generointi on siirretty selaimen vastuulle. Selain hoitaa tehtävän suorittamalla palvelimelta lataamansa Javascript-koodin. Selaimella suoritettava koodi hakee ensin muistiinpanot palvelimelta JSON-muotoisena raakadatana ja lisää sivulle muistiinpanoja edustavat HTML-elementit [DOM-apia](/osa1#document-object-model-eli-dom) hyödyntäen.

Viime aikoina on noussut esiin tyyli tehdä web-sovelukset käyttäen [Single-page application](https://en.wikipedia.org/wiki/Single-page_application) (SPA) -tyyliä, missä sovelluksille ei enää tehdä esimerkkisovelluksemme tapaan erillisiä, palvelimen sille lähettämiä sivuja, vaan sovellus koostuu ainoastaan yhdestä palvelimen lähettämästä HTML-sivusta, jonka sisältöä manipuloidaan selaimessa suoritettavalla Javascriptillä.

Sovelluksemme muistiinpanosivu muistuttaa jo hiukan SPA-tyylistä sovellusta, sitä se ei kuitenkaan vielä ole, sillä vaikka muistiinpanojen renderöintilogiikka on toteutettu selaimessa, käyttää sivu vielä perinteistä mekanisimia uusien muistiinpanojen luomiseen. Eli se lähettää uuden muistiinpanon tiedot lomakkeen avulla ja palvelin pyytää _uudelleenohjauksen_ avulla selainta lataamaan muistiinpanojen sivun uudelleen.

Osoitteesta <https://fullstack-exampleapp.herokuapp.com/spa> löytyy sovelluksen single page app -versio.

Sovellus näyttää ensivilkaisulta täsmälleen samalta kuin edellinen versio.

HTML-koodi on lähes samanlainen, erona on ladattava javascript-tiedosto (_spa.js_) ja pieni muutos form-tagin määrittelyssä:

![]({{ "/assets/1/23.png" | absolute_url }})

Avaa nyt _Network_-tabi ja tyhjennä se &empty;-symbolilla. Kun luot uuden muistiinpanon, huomaat, että selain lähettää ainoastaan yhden pyynnön palvelimelle:

![]({{ "/assets/1/24.png" | absolute_url }})

Pyyntö kohdistuu osoitteeseen _new_note_spa_, on tyypiltään POST ja se sisältää JSON-muodossa olevan uuden muistiinpanon, johon kuuluu sekä sisältö (_content_), että aikaleima (_date_):

```js
{
  content: "single page app ei tee turhia sivun latauksia",
  date: "2017-12-11T10:51:29.025Z"
}
```

Pyyntöön liitetty headeri _Content-Type_ kertoo palvelimelle, että pyynnön mukana tuleva data on JSON-muotoista:

![]({{ "/assets/1/25.png" | absolute_url }})

Ilman headeria, palvelin ei osaisi parsia pyynnön mukana tulevaa dataa oiken.

Palvelin vastaa kyselyyn statuskoodilla [201 created](https://httpstatuses.com/201). Tällä kertaa palvelin ei pyydä uudelleenohjausta kuten aiemmassa versiossamme. Selain pysyy samalla sivulla ja muita HTTP-pyyntöjä ei suoriteta.

Ohjelman single page app -versiossa lomakkeen tietoja ei lähetetä selaimen "normaalin" lomakkeiden lähetysmekanismin avulla, lähettämisen hoitaa selaimen lataamassa Javascript-tiedostossa määritelty koodi. Katsotaan hieman koodia vaikka yksityiskohdista ei tarvitse nytkään välittää liikaa.

```js
var form = document.getElementById('notes_form')
form.onsubmit = function (e) {
 e.preventDefault()

  var note = {
    content: e.target.elements[0].value,
    date: new Date()
  }

  notes.push(note)
  e.target.elements[0].value = ''
  redrawNotes()
  sendToServer(note)
}
```

Komennolla <code>document.getElementById('notes_form')</code> koodi hakee sivulta lomake-elementin ja rekisteröi sille tapahtumankäsittelijän hoitamaan tilanteen, missä lomake "submitoidaan", eli lähetetään. Tapahtumankäsittelijä kutsuu heti metodia <code>e.preventDefault()</code> jolla se estää lomakkeen lähetyksen oletusarvoisen toiminnan. Oletusarvoinen toiminta aiheuttaisi lomakkeen lähettämisen ja sivun uuden lataamisen, sitä emme single page -sovelluksissa halua tapahtuvan.

Tämän jälkeen se luo muistiinpanon, lisää sen muistiinpanojen listalle komennolla <code>notes.push(note)</code>, piirtää ruudun sisällön eli muistiinpanojen listan uudelleen ja lähettää uuden muistiinpanon palvelimelle.

Palvelimelle muistiinpanon lähettävä koodi seuraavassa:
```js
var sendToServer = function (note) {
  var xhttpForPost = new XMLHttpRequest()
  // ...

  xhttpForPost.open('POST', '/new_note_spa', true)
  xhttpForPost.setRequestHeader('Content-type', 'application/json')
  xhttpForPost.send(JSON.stringify(note));
}
```

Koodissa siis määritellään, että kyse on HTTP POST -pyynnöstä, määritellään headerin _Content-type_ avulla lähetettävän datan tyypiksi JSON, ja lähetetään data JSON-merkkijonona.

Sovelluksen koodi on nähtävissä osoitteessa <https://github.com/mluukkai/example_app>. Kannattaa huomata, että sovellus on tarkoitettu ainoastaan kurssin käsitteistöä demonstroivaksi esimerkiksi, koodi on osin tyyliltään huonoa ja siitä ei tulee ottaa mallia omia sovelluksia tehdessä.

## Kirjastot

Kurssin esimerkkisovellus on tehty ns. [vanilla Javascriptillä](https://medium.freecodecamp.org/is-vanilla-javascript-worth-learning-absolutely-c2c67140ac34) eli käyttäen pelkkää DOM-apia ja Javascript-kieltä sivujen rakenteen manipulointiin.

Pelkän Javascriptin ja DOM-apin käytön sijaan Web-ohjelmoinnissa hyödynnetään yleensä kirjastoja, jotka sisältävät DOM-apia helpommin käytettäviä työkaluja sivujen muokkaukseen. Eräs tälläinen kirjasto on edelleenkin hyvin suosittu [JQuery](https://jquery.com/).

JQuery on kehitetty aikana, jolloin web-sivut olivat vielä suurimmaksi osaksi perinteisiä, eli palvelin muodosti HTML-sivuja joiden toiminnallisuutta rikastettiin selaimessa JQueryllä kirjoitetun Javascript-koodin avulla.

Single page app -tyylin noustua suosioon on ilmestynyt useita JQueryä "modernimpia" tapoja sovellusten kehittämiseen. Googlen kehittämä [AngularJS](https://angularjs.org/) oli vielä muutama vuosi sitten erittäin suosittu. Angularin suosio kuitenkin romahti siinä vaiheessa kun Angular-tiimi [ilmoitti](https://jaxenter.com/angular-2-0-announcement-backfires-112127.html) lokakuussa 2014, että version 1 tuki lopetetaan ja Angular 2 ei tule olemaan taaksepäin yhteensopiva ykkösversion kanssa. Angular 2 ja uudemmat versiot eivät ole saaneet kovin innostunutta vastaanottoa.

Nykyisin suosituin tapa toteuttaa web-sovellusten selainpuolen logiikka on Facebookin kehittämä [React](https://reactjs.org/)-kirjasto. Tulemme tutustumaan kurssin aikana Reactiin ja sen kanssa yleisesti käytettyyn [Redux](https://github.com/reactjs/redux)-kirjastoon.

Reactin asema näyttää tällä hetkellä vahvalta, mutta Javascript-maailma ei lepää koskaan. Viime aikoina huomioita on alkanut kiinnittää mm. uudempi tulokas [VueJS](https://vuejs.org/).

## Full stack -websovelluskehitys

Mitä tarkoitetaan kurssin nimellä _full stack -websovelluskehitys_? Full stack on hypen omainen termi; kaikki puhuvat siitä, mutta kukaan ei oikein tiedä mitä se tarkoittaa tai ainakaan mitään yhteneväistä määritelmää termille ei ole.

Käytännössä kaikki websovellukset sisältävät (ainakin) kaksi "kerrosta", ylempänä, eli lähempänä loppukäyttäjää olevan selaimen ja alla olevan palvelimen. Palvelimen alapuolella on usein vielä tietokanta. Näin websovelluksen arkkitehtuuri on pino eli _stack_.

Websovelluskehityksen yhteydessä puhutaan usein myös "frontista" ([frontend](https://en.wikipedia.org/wiki/Front_and_back_ends)) ja "backistä" ([backend](https://en.wikipedia.org/wiki/Front_and_back_ends)). Selain on frontend ja selaimessa suoritettava javascript on frontend-koodia. Palvelimella taas pyörii backend-koodi.

Tämän kurssin kontekstissa full stack -sovelluskehitys tarkoittaa sitä, että fokus on kaikissa sovelluksen osissa, niin frontendissä kuin backendissäkin.

Ohjelmoimme myös palvelinpuolta, eli backendia Javascriptilla, käyttäen [Node.js](https://nodejs.org/en/)-suoritusympäristöä. Näin full stack -sovelluskehitys saa vielä uuden ulottuvuuden, käytämme samaa kieltä pinon kaikissa osissa. Full stack -sovelluskehitys ei välttämättä edellytä sitä,että kaikissa sovelluksen kerroksissa on käytössä sama kieli (javascript). Termi on kuitenkin (todennäköisesti) lanseerattu vasta sen jälkeen kun Node.js mahdollisti Javascriptin käyttämisen kaikkialla.

Aiemmin on ollut yleisempää, että sovelluskehittäjät ovat erikoistuneet tiettyyn sovelluksen osaan, esim. backendiin. Tekniikat backendissa ja frontendissa ovat saattaneet olla hyvin erilaisia. Full stack -trendin myötä on tullut tavanomaiseksi että sovelluskehittäjä hallitsee riittävästi kaikkia sovelluksen tasoja ja tietokantaa. Usein full stack -kehittäjän on myös omattava riittävä määrä konfiguraatio- ja ylläpito-osaamista, jotta kehittäjä pystyy operoimaan sovellustaan esim. pilvipalveluissa.

## Javascript fatigue

Full stack -sovelluskehitys on monella tapaa haastavaa. Asioita tapahtuu monessa paikassa ja mm. debuggaaminen on oleellisesti normaalia työpöytäsovellusta hankalampaa. Javascript ei toimi aina niinkuin sen olettaisi toimivan ja sen suoritusympäristöjen asynkroninen toimintamalli aiheuttaa monenlaisia haasteita. Verkon yli tapahtuva kommunikointi edellyttää HTTP-protokollan tuntemusta. On tunnettava myös tietokantoja ja hallittava palvelinten konfigurointia ja ylläpitoa. Hyvä olisi myös hallita riittävästi CSS:ää, jotta sovellukset saataisiin edes siedettävän näköisiksi.

Oman haasteensa tuo vielä se, että Javascript-maailma etenee koko ajan kovaa vauhtia eteenpäin. Kirjastot, työkalut ja itse kielikin ovat jatkuvan kehityksen alla. Osa alkaa kyllästyä nopeaan kehitykseen ja sitä kuvaamaan on lanseerattu termi [Javascript](https://medium.com/@ericclemmons/javascript-fatigue-48d4011b6fc4) [fatigue](https://auth0.com/blog/how-to-manage-javascript-fatigue/) eli [Javascript](https://hackernoon.com/how-it-feels-to-learn-javascript-in-2016-d3a717dd577f)-väsymys.

Javascript-väsymys tulee varmasti iskemään myös tällä kurssilla. Onneksi nykyään on olemassa muutamia tapoja loiventaa oppimiskäyrää, ja voimme aloittaa keskittymällä konfiguraation sijaan koodaamiseen. Konfiguraatioita ei voi välttää, mutta seuraavat pari viikkoa voimme edetä iloisin mielin vailla pahimpia konfiguraatiohelvettejä.

### Tehtäviä web-sovelluksen perusteista

Ennen Reactiin siirtymistä tee [tehtävät 1-6](../tehtavat#web-sovellusten-perusteet)
