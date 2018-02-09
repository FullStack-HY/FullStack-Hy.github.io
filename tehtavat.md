---
layout: page
title: tehtävät
inheader: yes
permalink: /tehtävät/
---

# tehtävät

* [osa0](#osa-0) deadline 30.1. klo 23:59
* [osa1](#osa-1) deadline 30.1. klo 23:59
* [osa2](#osa-2) deadline 5.2. klo 23:59
* [osa3](#osa-3) deadline 12.2. klo 23:59
* [osa4](#osa-4) deadline 25.2. klo 23:59
* [osa5](#osa-5) deadline 4.3. klo 23:59
* [osa6](#osa-6) deadline 11.3. klo 23:59
* [osa7](#osa-7) deadline 11.3. klo 23:59

## Pakolliset tehtävät ja tehtävien vaikuttaminen arvosteluun

Osa tehtävistä on "pakollisia" ja osa vapaaehtoisia. Vapaaehtoiset on merkattu tähdellä. Pakolliset eivät ole absoluuttisen pakollisia, mutta niiden tekemättä jättäminen saattaa aiheuttaa haasteita seuraaviin osiin.

Arvosananja opintopistemäärä lasketaan _kaikkien_ tehtävien summan perusteella. Katso tarkemmin osan 0 luvut [suoritustapa](/osa0#suoritustapa) ja [arvosteluperusteet](/osa0#arvosteluperusteet). Vain deadlineja ennen tehdyt ja merkatut tehtävät huomioidaan arvostelussa.

## Arvosanarajat

Kurssilla on yhteensä 149 tehtävää. Tehtävien maksimimääräksi lasketaan kuitenkin 141, sillä osan 7 tehtävistä kaikki eivät vaikuta arvosteluun.

Arvosanarajat:

| tehtävää  &nbsp;       | arvosana&nbsp;    | op  |
| -------------- |:-----------------:       |:-----:|
| 70    | 1/5  | 5   |
| 81    | 2/5  | 5   |
| 92    | 3/5  | 5   |
| 103   | 4/5  | 5   |
| 113   | 5/5  | 5   |
| 124   | hyv  | 6   |
| 134   | hyv  | 7   |


Ylimääräiset opintopisteet tullaan kirjaamaan kaikille arvosanalla "hyväksytty".


## Palauttaminen

Olethan lukenut huolellisesti kurssimateriaalin osan 0 luvun [Suoritustapa](/osa0/#Suoritustapa)?

Tehtävät palautetaan GitHubin kautta ja merkitsemällä tehdyt tehtävät [palautussovellukseen](https://studies.cs.helsinki.fi/fs-stats/).

Jos palautat eri osien tehtäviä samaan repositorioon, käytä järkevää hakemistojen nimentää.

Tehtävät palautetaan yksi osa kerrallaan. Kun olet palauttanut osan tehtävät, et voi enää palauttaa saman osan tekemättä jättämiäsi tehtäviä.

GitHubiin palautettuja tehtäviä tarkastetaan pistokokein. Jos GitHubista löytyy kurssin mallivastausten koodia tai useammalta opiskelijalta löytyy samaa koodia, käsitellään tilanne yliopiston [vilppikäytäntöjen](http://blogs.helsinki.fi/alakopsaa/opettajalle/epailen-opiskelijaa-vilpista-mita-tehda/) mukaan.

Suurin osa tehtävistä on moniosaisia, samaa ohjelmaa pala palalta rakentavia kokonaisuuksia. Tälläisissä tehtäväsarjoissa ohjelman lopullisen version palauttaminen riittää, voit toki halutessasi tehdä commitin jokaisen tehtävän jälkeisestä tilanteesta, mutta se ei ole välttämätöntä.

## Osa 0

Deadline 30.1. klo 23:59

Osassa on 6 tehtävää joista kaikki ovat pakollisia. Voit edetä osaan 1 vasta tehtävät palautettuasi.

### web-sovellusten perusteet ###

#### 0.1 HTML ja CSS ####

Kertaa HTML:n ja CSS:n perusteet lukemalla Mozillan tutoriaalit [HTML:stä](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/HTML_basics) ja [CSS:stä](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/CSS_basics).

#### 0.2 HTML:n lomakkeet

Tutustu HTML:n lomakkeiden perusteisiin lukemalla Mozillan tutoriaali [Your first form](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Your_first_HTML_form).

#### 0.3 muistiinpanojen sivu

Kun käyttäjä menee selaimella osoitteeseen <https://fullstack-exampleapp.herokuapp.com/> voidaan sen seurauksena olevaa tapahtumaketjua kuvata [sekvenssikaaviona](https://en.wikipedia.org/wiki/Sequence_diagram) esim. seuraavasti:

<img src="/assets/teht/1.png" height="400">

Kaavio on luotu [websequencediagrams](https://www.websequencediagrams.com)-palvelussa, seuraavasti:

<pre>
kayttaja->selain:
note left of selain
kayttaja kirjottaa osoiteriville
fullstack-exampleapp.herokuapp.com
end note
selain->palvelin: GET fullstack-exampleapp.herokuapp.com
note left of palvelin
  muodostetaan HTML missä olemassaolevien
  muistiinpanojen lukumäärä päivitettynä
end note
palvelin->selain: status 200, sivun HTML-koodi

selain->palvelin: GET fullstack-exampleapp.herokuapp.com/kuva.png
palvelin->selain: status 200, kuva

note left of selain
 selain näyttää palvelimen palauttaman HTML:n
 johon on upotettu palvelimelta haettu kuva
end note
</pre>

**Tee vastaavanlainen kaavio, joka kuvaa mitä tapahtuu kun käyttäjä navigoi muistiinpanojen sivulle** eli urliin <https://fullstack-exampleapp.herokuapp.com/notes>

Kaavion ei ole pakko olla sekvenssikaavio. Mikä tahansa järkevä kuvaustapa käy.

Kaikki oleellinen tämän ja seuraavien kolmen tehtävän tekemiseen liittyvä informaatio on selitettynä [osan 0](../osa0) tekstissä. Näiden tehtävien ideana on, että luet tekstin vielä kerran ja mietit tarkkaan mitä missäkin tapahtuu. Ohjelman [koodin](https://github.com/mluukkai/example_app) lukemista ei näissä tehtävissä edellytetä, vaikka sekin on toki mahdollista.

#### 0.4 Uusi muistiinpano

Tee kaavio tilanteesta, missä käyttäjä luo uuden muistiinpanon ollessaan sivulla <https://fullstack-exampleapp.herokuapp.com/notes>, eli kirjoittaa tekstikenttään jotain ja painaa nappia _tallenna_

Kirjoita tarvittaessa palvelimella tai selaimessa tapahtuvat operaatiot sopivina kommentteina kaavion sekaan.

#### 0.5 Single page app

Tee kaavio tilanteesta, missä käyttäjä menee selaimella osoitteeseen <https://fullstack-exampleapp.herokuapp.com/spa> eli muistiinpanojen [single page app](../osa0/#single-page-app)-versioon

#### 0.6 Uusi muistiinpano SPA:ssa

Tee kaavio tilanteesta, missä käyttäjä luo uuden muistiinpanon single page -versiossa.

### Tehtävien palautus

Palauta tehtävät [palautussovellukseen](https://studies.cs.helsinki.fi/fs-stats/).

## Osa 1

Deadline 30.1. klo 23:59

Osassa on 14 tehtävää, joista pakollisia on 9. Voit edetä osaan 2 kun olet tehnyt kaikki pakolliset tehtävät. Palautuksen tekemisen jälkeen et voi enää palauttaa osan tehtäviä.

Osa sisältää muutaman tehtäväsarjan, joissa yksittäistä ohjelmaa laajennetaan pala palalta. Ohjelmien lopullisen version palauttaminen riittää, voit toki halutessasi tehdä commitin jokaisen tehtävän jälkeisestä tilanteesta, mutta se ei ole välttämätöntä.

<div class='important'>

<p>Ennen kun teet tehtäviä, on enemmän kuin suositeltavaa, että käyt huolellisesti läpi <a href='https://fullstack-hy.github.io/osa1/'>osan 1 materiaalin</a>. Tehtävien tekeminen ilman materiaalin lukemista tapahtuu täysin omalla vastuulla.</p>

</div>

### reactin alkeet ###

#### 1.1 jako komponenteiksi

Luo create-react-app:illa uusi sovellus. Muuta _index.js_ muotoon

```react
import React from 'react'
import ReactDOM from 'react-dom'

const App = () => {
  const kurssi = 'Half Stack -sovelluskehitys'
  const osa1 = 'Reactin perusteet'
  const tehtavia1 = 10
  const osa2 = 'Tiedonvälitys propseilla'
  const tehtavia2 = 7
  const osa3 = 'Komponenttien tila'
  const tehtavia3 = 14

  return (
    <div>
      <h1>{kurssi}</h1>
      <p>{osa1} {tehtavia1}</p>
      <p>{osa2} {tehtavia2}</p>
      <p>{osa3} {tehtavia3}</p>
      <p>yhteensä {tehtavia1 + tehtavia2 + tehtavia3} tehtävää</p>
    </div>
  )
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
)
```

ja poista ylimääräiset tiedostot.

Koko sovellus on nyt ikävästi yhdessä komponentissa. Refaktoroi sovelluksen koodi siten, että se koostuu kolmesta komponentista _Otsikko_, _Sisalto_ ja _Yhteensa_. Kaikki data pidetään edelleen komponentissa _App_, joka välittää tarpeelliset tiedot kullekin komponentille _props:ien_ avulla. _Otsikko_ huolehtii kurssin nimen renderöimisestä, _Sisalto_ osista ja niiden tehtävämääristä ja _Yhteensa_ tehtävien yhteismäärästä.

Komponentin _App_ runko tulee olemaan suunnilleen seuraavanlainen:

```react
const App = () => {
  // const-määrittelyt

  return (
    <div>
      <Otsikko kurssi={kurssi} />
      <Sisalto ... />
      <Yhteensa ... />
    </div>
  )
}
```

#### 1.2 lisää komponentteja

Refaktoroi vielä komponentti _Sisalto_ siten, että se ei itse renderöi yhdenkään osan nimeä eikä sen tehtävälukumäärää vaan ainoastaan kolme _Osa_-nimistä komponenttia, joista kukin siis renderöi yhden osan nimen ja tehtävämäärän.

```react
const Sisalto = ... {
  return (
    <div>
      <Osa .../>
      <Osa .../>
      <Osa .../>
    </div>
  )
}
```

Sovelluksemme tiedonvälitys on tällä hetkellä todella alkukantaista, sillä se perustuu yksittäisiin muuttujiin. Tilanne paranee pian.

### Javascriptin alkeet ###

#### 1.3 tieto olioissa

Siirrytään käyttämään sovelluksessamme oliota. Muuta _App_:in muuttujamäärittelyt seuraavaan muotoon ja muuta sovelluksen kaikkia osia niin, että se taas toimii:

```react
const App = () => {
  const kurssi = 'Half Stack -sovelluskehitys'
  const osa1 = {
    nimi: 'Reactin perusteet',
    tehtavia: 10
  }
  const osa2 = {
    nimi: 'Tiedonvälitys propseilla',
    tehtavia: 7
  }
  const osa3 = {
    nimi: 'Komponenttien tila',
    tehtavia: 14
  }

  return (
    <div>
      ...
    </div>
  )
}
```

#### 1.4 oliot taulukkoon

Ja laitetaan oliot taulukkoon, eli muuta _App_:in muuttujamäärittelyt seuraavaan muotoon ja muuta sovelluksen kaikki osat vastaavasti:

```react
const App = () => {
  const kurssi = 'Half Stack -sovelluskehitys'
  const osat = [
    {
      nimi: 'Reactin perusteet',
      tehtavia: 10
    },
    {
      nimi: 'Tiedonvälitys propseilla',
      tehtavia: 7
    },
    {
      nimi: 'Komponenttien tila',
      tehtavia: 14
    }
  ]

  return (
    <div>
      ...
    </div>
  )
}
```

**HUOM:** tässä vaiheessa _voit olettaa, että taulukossa on aina kolme alkiota_, eli taulukkoa ei ole pakko käydä läpi looppaamalla. Palataan taulukossa olevien olioiden perusteella tapahtuvaan komponenttien renderöintiin asiaan tarkemmin kurssin [seuraavassa osassa](../osa2).

Älä kuitenkaan välitä eri olioita komponenttien välillä (esim. komponentista _App_ komponenttiin _Yhteensa_) erillisinä propsina, vaan suoraan taulukkona:

```react
const App = () => {
  // const-määrittelyt

  return (
    <div>
      <Otsikko kurssi={kurssi} />
      <Sisalto osat={osat} />
      <Yhteensa osat={osat} />
    </div>
  )
}
```

#### 1.5

Viedään muutos vielä yhtä askelta pidemmälle, eli tehdään kurssista ja sen osista yksi Javascript-olio. Korjaa kaikki mikä menee rikki.

```react
const App = () => {
  const kurssi = {
    nimi: 'Half Stack -sovelluskehitys',
    osat: [
      {
        nimi: 'Reactin perusteet',
        tehtavia: 10
      },
      {
        nimi: 'Tiedonvälitys propseilla',
        tehtavia: 7
      },
      {
        nimi: 'Komponenttien tila',
        tehtavia: 14
      }
    ]
  }

  return (
    <div>
      ...
    </div>
  )
}
```

### lisää reactia ###

#### 1.6 unicafe osa1

Monien firmojen tapaan nykyään myös [Unicafe](https://www.unicafe.fi/#/9/4) kerää asiakaspalautetta. Tee Unicafelle verkossa toimiva palautesovellus. Vastausvaihtoehtoja olkoon vain kolme: _hyvä_, _neutraali_ ja _huono_.

Sovelluksen tulee näyttää jokaisen palautteen lukumäärä. Sovellus voi näyttää esim. seuraavalta:

![]({{ "/images/teht/4c.png" | absolute_url }})

Huomaa, että sovelluksen tarvitsee toimia vain yhden selaimen käyttökerran ajan, esim. kun selain refreshataan, tilastot saavat hävitä.

#### 1.7 unicafe osa2

Laajenna sovellusta siten, että se näyttää palautteista statistiikkaa, keskiarvon (hyvän arvo 1, neutraalin 0, huonon -1) ja sen kuinka monta prosenttia palautteista on ollut positiivisia:

![]({{ "/images/teht/4d.png" | absolute_url }})

#### 1.8 unicafe osa3

Refaktoroi sovelluksesi siten, että se koostuu monista komponenteista. Pidä tila kuitenkin sovelluksen _juurikomponentissa_.

Tee sovellukseen ainakin seuraavat komponentit:
- _Button_ vastaa yksittäistä palautteenantonappia
- _Statistics_ huolehtii tilastojen näyttämisestä
- _Statistic_ huolehtii yksittäisen tilastorivin, esim. keskiarvon näyttämisestä

#### 1.9* unicafe osa4

Muuta sovellusta siten, että numeeriset tilastot näytetään ainoastaan jos palautteita on jo annettu:

![]({{ "/images/teht/5.png" | absolute_url }})


#### 1.10* unicafe osa5

Jos olet määritellyt jokaiselle napille oman tapahtumankäsittelijän, refaktoroi sovellustasi siten, että kaikki napit käyttävät samaa tapahtumankäsittelijäfunktiota samaan tapaan kuin materiaalin luvussa [funktio joka palauttaa funktion](/osa1/#funktio-joka-palauttaa-funktion)

Pari vihjettä. Ensinnäkin kannattaa muistaa, että olion kenttiin voi viitata pistenotaation lisäksi hakasulkeilla, eli

```js
const olio = {
  a: 1,
  b: 2
}

olio['c'] = 3

console.log(olio.a)     // tulostuu 1

console.log(olio['b'])  // tulostuu 2

const apu = 'c'
console.log(olio[apu])  // tulostuu 3
```

Myös ns. [Computed property names](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer) voi olla tässä tehtävässä hyödyksi.

#### 1.11 unicafe osa6

Toteuta tilastojen näyttäminen HTML:n [taulukkona](https://developer.mozilla.org/en-US/docs/Learn/HTML/Tables/Basics) siten, että saat sovelluksesi näyttämään suunnilleen seuraavanlaiselta

![]({{ "/images/teht/6a.png" | absolute_url }})


Muista pitää konsoli koko ajan auki. Jos saat konsoliin seuraavan warningin

![]({{ "/assets/teht/7.png" | absolute_url }})

tee tarvittavat toimenpiteet jotta saat warningin katoamaan. Googlaa tarvittaessa virheilmoituksella.

**Huolehdi nyt ja jatkossa, että konsolissa ei näy mitään warningeja!**

#### 1.12* anekdootit osa1

Ohjelmistotuotannossa tunnetaan lukematon määrä [anekdootteja](http://www.comp.nus.edu.sg/~damithch/pages/SE-quotes.htm) eli pieniä "onelinereita", jotka kiteyttävät alan ikuisia totuuksia.

Laajenna seuraavaa sovellusta siten, että siihen tulee nappi, jota painamalla sovellus näyttää _satunnaisen_ ohjelmistotuotantoon liittyvän anekdootin:

```react
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      selected: 0
    }
  }

  render() {
    return (
      <div>
        {this.props.anecdotes[this.state.selected]}
      </div>
    )
  }
}

const anecdotes = [
  'If it hurts, do it more often',
  'Adding manpower to a late software project makes it later!',
  'The first 90 percent of the code accounts for the first 90 percent of the development time...The remaining 10 percent of the code accounts for the other 90 percent of the development time.',
  'Any fool can write code that a computer can understand. Good programmers write code that humans can understand.',
  'Premature optimization is the root of all evil.',
  'Debugging is twice as hard as writing the code in the first place. Therefore, if you write the code as cleverly as possible, you are, by definition, not smart enough to debug it.'
]

ReactDOM.render(
  <App anecdotes={anecdotes} />,
  document.getElementById('root')
)
```

Google kertoo, miten voit generoida Javascriptilla sopivia satunnaisia lukuja. Muista, että voit testata esim. satunnaislukujen generointia konsolissa.

Sovellus voi näyttää esim. seuraavalta:

![]({{ "/images/teht/2.png" | absolute_url }})

#### 1.13* anekdootit osa2

Laajenna sovellusta siten, että näytettävää anekdoottia on mahdollista äänestää:

![]({{ "/images/teht/3.png" | absolute_url }})

**Huom:** jos päätät tallettaa kunkin anekdootin äänet komponentin tilassa olevan olion kenttiin tai taulukkoon, saatat tarvita päivittäessäsi tilaa oikeaoppisesti olion tai taulukon _kopioimista_.

Olio voidaan kopioida esim. seuraavasti:

```js
this.state.pisteet = { 0: 1, 1: 3, 2: 4, 3: 2}

const kopio = {...pisteet}
kopio[2] += 1   // kasvatetaan olion kentän 2 arvoa yhdellä
```

ja taulukko esim. seuraavasti:

```js
this.state.pisteet = [1, 4, 6, 3]

const kopio = [...pisteet]
kopio[2] += 1   // kasvatetaan taulukon paikan 2 arvoa yhdellä
```

#### 1.14* anekdootit osa3

Ja sitten vielä lopullinen versio, joka näyttää eniten ääniä saaneen anekdootin:

![]({{ "/images/teht/3b.png" | absolute_url }})

Jos suurimman äänimäärän saaneita anekdootteja on useita, riittää että niistä näytetään yksi.

Tämä saattaa olla jo hieman haastavampi. Taulukolta löytyy monia hyviä metodeja, katso lisää [Mozillan dokumentaatiosta](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array).

Youtubessa on kohtuullisen hyvä [johdatus funktionaaliseen javascript-ohjelmointiin](https://www.youtube.com/watch?v=BMUiFMZr7vk&list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84). Kolmen ensimmäisen osan katsominen riittää hyvin tässä vaiheessa.

### Tehtävien palautus

Palauta tehtävät [palautussovellukseen](https://studies.cs.helsinki.fi/fs-stats/).


## Osa 2

deadline 5.2. klo 23:59

Osassa on 19 tehtävää, joista "pakollisia" on 13. On suositeltavaa että etenet seuraavaan osaan vasta kun olet tehnyt kaikki pakolliset tehtävät. Palautuksen tekemisen jälkeen et voi enää palauttaa osan tehtäviä.

Osa sisältää kolme tehtäväsarjaa, joissa yksittäistä ohjelmaa laajennetaan pala palalta. Ohjelmien lopullisen version palauttaminen riittää, voit toki halutessasi tehdä commitin jokaisen tehtävän jälkeisestä tilanteesta, mutta se ei ole välttämätöntä.

<div class='important'>

<p>Ennen kun teet tehtäviä, on enemmän kuin suositeltavaa, että käyt huolellisesti läpi <a href='https://fullstack-hy.github.io/osa2/'>osan 2 materiaalin</a>. Tehtävien tekeminen ilman materiaalin lukemista tapahtuu täysin omalla vastuulla.</p>

</div>
### Kokoelmien renderöinti

#### 2.1 kurssien sisältö

Viimeistellään nyt tehtävien 1.1-1.5 kurssin sisältöjä renderöivän ohjelman koodi. Voit ottaa tarvittaessa pohjaksi mallivastauksen koodin.

Muutetaan komponentti _App_ seuraavasti:

```react
const App = () => {
  const kurssi = {
    nimi: 'Half Stack -sovelluskehitys',
    osat: [
      {
        nimi: 'Reactin perusteet',
        tehtavia: 10,
        id: 1
      },
      {
        nimi: 'Tiedonvälitys propseilla',
        tehtavia: 7,
        id: 2
      },
      {
        nimi: 'Komponenttien tila',
        tehtavia: 14,
        id: 3
      }
    ]
  }

  return (
    <div>
      <Kurssi kurssi={kurssi} />
    </div>
  )
}
```

Määrittele sovellukseen yksittäisen kurssin muotoilusta huolehtiva komponentti _Kurssi_.

Sovelluksen komponenttirakenne voi olla esim. seuraava

<pre>
App
  Kurssi
    Otsikko
    Sisalto
      Osa
      Osa
      ...
</pre>

ja renderöityvä sivu voi näyttää esim. seuraavalta:

![]({{ "/images/teht/8.png" | absolute_url }})

Tässä vaiheessa siis tehtävien yhteenlaskettua lukumäärää ei vielä tarvita.

Sovelluksen täytyy luonnollisesti toimia _riippumatta kurssissa olevien osien määrästä_, eli varmista että sovellus toimii jos lisäät tai poistat kurssin osia.

Varmista, että konsolissa ei näy mitään virheilmoituksia!

#### 2.2 tehtävien määrä

Ilmoita myös kurssin yhteenlaskettu tehtävien lukumäärä

![]({{ "/images/teht/9.png" | absolute_url }})

#### 2.3* reduce

Jos et jo niin tehnyt, laske koodissasi tehtävien määrä taulukon metodilla [reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

#### 2.4 monta kurssia

Laajennetaan sovellusta siten, että kursseja voi olla _mielivaltainen määrä_:

```react
const App = () => {
  const kurssit = [
    {
      nimi: 'Half Stack -sovelluskehitys',
      id: 1,
      osat: [
        {
          nimi: 'Reactin perusteet',
          tehtavia: 10,
          id: 1
        },
        {
          nimi: 'Tiedonvälitys propseilla',
          tehtavia: 7,
          id: 2
        },
        {
          nimi: 'Komponenttien tila',
          tehtavia: 14,
          id: 3
        }
      ]
    },
    {
      nimi: 'Node.js',
      id: 2,
      osat: [
        {
          nimi: 'Routing',
          tehtavia: 3,
          id: 1
        },
        {
          nimi: 'Middlewaret',
          tehtavia: 7,
          id: 2
        }
      ]
    }
  ]

  return (
    <div>
      // ...
    </div>
  )
}
```

Sovelluksen ulkoasu voi olla esim seuraava:

![]({{ "/images/teht/10.png" | absolute_url }})

#### 2.5 erillinen moduuli

Määrittele komponentti _Kurssi_ omana moduulinaan, jonka komponentti _App_ importtaa. Voit sisällyttää kaikki kurssin alikomponentit samaan moduuliin.

### Lomakkeet

#### 2.6 puhelinluettelo osa 1

Toteutetaan yksinkertainen puhelinluettelo. **Aluksi luetteloon lisätään vaan nimiä.**

Voit ottaa sovelluksesi pohjaksi seuraavan:

```react
import React from 'react';

class App extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      persons: [
        { name: 'Arto Hellas' }
      ],
      newName: ''
    }
  }

  render() {
    return (
      <div>
        <h2>Puhelinluettelo</h2>
        <form>
          <div>
            nimi: <input />
          </div>
          <div>
            <button type="submit">lisää</button>
          </div>
        </form>
        <h2>Numerot</h2>
        ...
      </div>
    )
  }
}

export default App
```

Tilassa oleva kenttä _newName_ on tarkoitettu lomakkeen kentän kontrollointiin.

Joskus tilan muuttujia ja tarvittaessa muitakin voi olla hyödyllistä renderöidä debugatessa komponenttiin, eli voi tilapäisesti lisätä komponentin metodin _render_ palauttamaan koodiin esim. seuraavan:

```html
<div>
  debug: {this.state.newName}
</div>
```

Muista myös osan 1 luku [React-sovellusten debuggaus](#React-sovellusten-debuggaus), erityisesti [react developer tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi) on välillä todella kätevä tilan komponentin tilan muutosten seuraamisessa.

Sovellus voi näyttää tässä vaiheessa seuraavalta

![]({{ "/images/teht/11.png" | absolute_url }})

Huomaa, React developer toolsin käyttö!

**Huom:**
* voit käyttää kentän _key_ arvona henkilön nimeä
* muista estää lomakkeen lähetyksen oletusarvoinen toiminta!

#### 2.7 puhelinluettelo osa 2

Jos lisättävä nimi on jo sovelluksen tiedossa, estä lisäys. Taulukolla on lukuisia sopivia [metodeja](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) tehtävän tekemiseen.

Voit antaa halutessasi virheilmoituksen esim. komennolla _alert()_. Se ei kuitenkaan ole tarpeen.

#### 2.8 puhelinluettelo osa 3

Lisää sovellukseen mahdollisuus antaa henkilöille puhelinnumero. Tarvitset siis lomakkeeseen myös toisen _input_-elementin (ja sille oman muutoksenkäsittelijän):

```html
<form>
  <div>
    nimi: <input />
  </div>
  <div>
    numero: <input />
  </div>
  <div>
    <button type="submit">lisää</button>
  </div>
</form>
```

Sovellus voi näyttää tässä vaiheessa seuraavalta. Kuvassa myös [react developer tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi):in tarjoama näkymä komponentin _App_ tilaan:

![]({{ "/assets/teht/12.png" | absolute_url }})

#### 2.9* puhelinluettelo osa 4

Tee lomakkeeseen hakukenttä, jonka avulla näytettävien nimien listaa voidaan rajata:

![]({{ "/assets/teht/12c.png" | absolute_url }})

Rajausehdon syöttämisen voi hoitaa omana lomakkeeseen kuulumattomana _input_-elementtinä. Kuvassa rajausehdosta on tehty _caseinsensitiivinen_ eli ehto _arto_ löytää isolla kirjaimella kirjoitetun Arton.

**Huom:** Kun toteutat jotain uutta toiminnallisuutta, on usein hyötyä 'kovakoodata' sovellukseen jotain sisältöä, esim.

```js
constructor(props) {
  super(props)
  this.state = {
    persons: [
      { name: 'Arto Hellas', number: '040-123456' },
      { name: 'Martti Tienari', number: '040-123456' },
      { name: 'Arto Järvinen', number: '040-123456' },
      { name: 'Lea Kutvonen', number: '040-123456' }
    ],
    newName: '',
    newNumber: '',
    filter: ''
  }
}
```

Näin vältytään turhalta manuaaliselta työltä, missä testaaminen edellyttäisi myös testiaineiston syöttämistä käsin soveluksen lomakkeen kautta.

Kurssin seuraavasta osasta alkaen alamme määrittelemään sovelluksemme _testejä_ jotka tietyissä tapauksissa hoitavat kovakoodatun apusyötteen roolia.

#### 2.10 puhelinluettelo osa 5

Jos koko sovelluksesi on tehty yhteen komponenttiin, refaktoroi sitä eriyttämällä sopivia komponentteja. Pidä kuitenkin edelleen kaikki tila juurikomponentissa.

Riittää että erotat sovelluksesta kaksi kompoenttia. Hyviä kandidaatteja ovat esim. filtteröintilomake, yksittäisten henkilön tietojen esittäminen ja uuden henkilön lisäävä lomake.

### Datan hakeminen palvelimelta

#### 2.11 puhelinluettelo osa 6

Talleta sovelluksen alkutila projektin juureen sijoitettavaan tiedostoon _db.json_

```json
{
  "persons": [
    {
      "name": "Arto Hellas",
      "number": "040-123456",
      "id": 1
    },
    {
      "name": "Martti Tienari",
      "number": "040-123456",
      "id": 2
    },
    {
      "name": "Arto Järvinen",
      "number": "040-123456",
      "id": 3
    },
    {
      "name": "Lea Kutvonen",
      "number": "040-123456",
      "id": 4
    }
  ]
}
```

Käynnistä json-server porttiin 3001 ja varmista selaimella osoitteesta <http://localhost:3001/persons>, että palvelin palauttaa henkilölistan.

Jos saat virheilmoituksen

```bash
events.js:182
      throw er; // Unhandled 'error' event
      ^

Error: listen EADDRINUSE 0.0.0.0:3001
    at Object._errnoException (util.js:1019:11)
    at _exceptionWithHostPort (util.js:1041:20)
```

on portti 3001 jo jonkin muun sovelluksen, esim. jo käynnissä olevan json-serverin käytössä. Sulje toinen sovellus tai jos se ei onnistu, vaihda porttia.

Muuta sovellusta siten, että datan alkutila haetaan _axios_-kirjaston avulla palvelimelta. Hoida datan hakeminen [lifecyclemetodissa](/osa2#komponenttien-lifecycle-metodit) _componentWillMount_.

#### 2.12* maiden tiedot

Rajapinta [https://restcountries.eu](https://restcountries.eu) tarjoaa paljon eri maihin liittyvää tietoa koneluettavassa muodossa REST-apina.

Tee sovellus, jonka avulla voit tarkastella eri maiden tietoja. Sovelluksen kannattaa hakea tiedot endpointista [all](https://restcountries.eu/#api-endpoints-all).

Sovelluksen käyttöliittymä on yksinkertainen. Näytettävä maa haetaan kirjoittamalla hakuehto etsintäkenttään.

Jos ehdon täyttäviä maita on liikaa (yli 10), kehoitetaan tarkentamaan hakuehtoa

![]({{ "/assets/teht/13.png" | absolute_url }})

Jos maita on alle kymmenen, mutta yli 1 näytetään hakuehdon täyttävät maat

![]({{ "/assets/teht/14.png" | absolute_url }})

Kun ehdon täyttäviä maita on enää yksi, näytetään maan lippu sekä perustiedot:

![]({{ "/assets/teht/15.png" | absolute_url }})

**Huom:** riittää että sovelluksesi toimii suurimmalle osalle maista. Jotkut maat kuten _Sudan_ voivat tuottaa ongelmia, sillä maan nimi on toisen maan _South Sudan_ osa. Näistä corner caseista ei tarvitse välittää.

#### 2.13* maiden tiedot klikkaamalla

**Älä juutu tähän tehtävään!**

Paranna edellisen tehtävän maasovellusta siten, että kun sivulla näkyy useiden maiden nimiä, riittää maan nimen klikkaaminen tarkentamaan haun siten, että klikatun maan tarkemmat tiedot saadaan näkyviin.

Huomaa, että saat "nimestä" klikattavan kiinnittämällä nimen sisältävään elementtiin, esim. diviin klikkaustenkuuntelijan:

```
<div onClick={...}>
  {country.name}
</div>
```

### palvelimella olevan datan päivittäminen

#### 2.14 puhelinluettelo osa 7

Palataan jälleen puhelinluettelon pariin.

Tällä hetkellä luetteloon lisättäviä uusia numeroita ei synkronoida palvelimelle. Korjaa tilanne.

#### 2.15 puhelinluettelo osa 8

Siirrä palvelimen kanssa kommunikoinnista vastaava toiminnallisuus omaan moduuliin osan 2 [esimerkin](/osa2/#palvelimen-kanssa-tapahtuvan-kommunikoinnin-erist%C3%A4minen-omaan-moduuliin) tapaan.

#### 2.16 puhelinluettelo osa 9

Tee ohjelmaan mahdollisuus yhteystietojen poistamiseen. Poistaminen voi tapahtua esim. nimen yhteyteen liitetyllä napilla. Poiston suorittaminen voidaan varmistaa käyttäjältä [window.confirm](https://developer.mozilla.org/en-US/docs/Web/API/Window/confirm)-metodilla:

![]({{ "/assets/teht/16.png" | absolute_url }})

Palvelimelta tiettyä henkilöä vastaava resurssi tuhotaan tekemällä HTTP DELETE -pyyntö resurssia vastaavaan _URL_:iin, eli jos poistaisimme esim. käyttäjän, jonka _id_ on 2, tulisi tapauksessamme tehdä HTTP DELETE osoitteeseen _localhost:3001:persons/2_. Pyynnön mukana ei lähetetä mitään dataa.

[Axios](https://github.com/axios/axios)-kirjaston avulla HTTP DELETE -pyyntö tehdään samaan tapaan kuin muutkin pyynnöt.

#### 2.17* puhelinluettelo osa 10

Muuta toiminnallisuutta siten, että jos jo olemassaolevalle henkilölle lisätään numero, korvaa lisätty numero aiemman numeron.

Jos henkilön tiedot löytyvät jo luettelosta, voi ohjelma kysyä käyttäjältä varmistuksen korvataanko numero

![]({{ "/images/teht/16a.png" | absolute_url }})

### tyylit

#### 2.18 puhelinluettelo osa 11

Toteuta osan 2 esimerkin [parempi virheilmoitus](/osa2/#parempi-virheilmoitus) tyyliin ruudulla muutaman sekunnin näkyvä ilmoitus, joka kertoo onnistuneista operaatioista (henkilön lisäys ja poisto, sekä numeron muutos):

![]({{ "/assets/teht/17.png" | absolute_url }})


#### 2.19* puhelinluettelo osa 12

Jos poistat jonkun henkilön toisesta selaimesta hieman ennen kun yrität _muuttaa henkilön numeroa_ toisesta selaimesta, tapahtuu virhetilanne:

![]({{ "/assets/teht/18.png" | absolute_url }})


Korjaa ongelma osan 2 esimerkin [promise ja virheet](/osa2/#promise-ja-virheet) tapaan. Loogisin korjaus lienee henkilön lisääminen uudelleen palvelimelle. Toinen vaihtoehto on ilmottaa käyttäjälle, että muutettavaksi yritettävän henkilön tiedot on jo poistettu.

### Tehtävien palautus

Palauta tehtävät [palautussovellukseen](https://studies.cs.helsinki.fi/fs-stats/).

## Osa 3

Deadline 12.2. klo 23:59

Osassa on 22 tehtävää, joista "pakollisia" on 15. On suositeltavaa että etenet seuraavaan osaan vasta kun olet tehnyt kaikki pakolliset tehtävät. Palautuksen tekemisen jälkeen et voi enää palauttaa osan tehtäviä.

Tämän osan tehtävissä teemme backendin edellisen osan puhelinluettelosovellukseen. Lopullisen version palauttaminen riittää, voit toki halutessasi tehdä commitin jokaisen tehtävän jälkeisestä tilanteesta, mutta se ei ole välttämätöntä.

**HUOM tämän osan tehtäväsarja kannattaa tehdä omaan git-repositorioon, suoraan repositorion juureen! Jos et tee näin, joudut ongelmiin tehtävässä 3.10**.

**HUOM2** Windows-käyttäjien ei kannata tehdä tehtävää sellaiseen hakemistoon, jonka nimessä on välilyönti, muuten osassa 4 on luvassa ongelmia. Myöskään missään tämän tehtävän hakemiston yläpuolella olevassa hakemistossa ei saa esiintyä välilyöntiä.

<div class='important'>

<p>Ennen kun teet tehtäviä, on enemmän kuin suositeltavaa, että käyt huolellisesti läpi <a href='https://fullstack-hy.github.io/osa3/'>osan 3 materiaalin</a>. Tehtävien tekeminen ilman materiaalin lukemista tapahtuu täysin omalla vastuulla.</p>

</div>

### Expressin alkeet

#### 3.1 puhelinluettelon backend osa 1

Vielä uusi **HUOMAUTUS:** tämän osan tehtäväsarja kannattaa tehdä omaan git-repositorioon, suoraan repositorion juureen! Jos et tee näin, joudut ongelmiin tehtävässä 3.10

**Vahva suositus:** kun teet backendin koodia, pidä koko ajan silmällä mitä palvelimen koodia suorittavassa konsolissa tapahtuu.

Tee Node-sovellus, joka tarjoaa osoitteessa <http://localhost:3001/api/persons> kovakoodatun taulukon puhelinnumerotietoja:

![]({{ "/assets/teht/19.png" | absolute_url }})

<div class='important'>

Koska nyt ei ole kyse fronendista ja Reactista, sovellusta <strong>ei luoda</strong> create-react-app:illa vaan komennolla <em>npm init</em> osan 3 luvun <a href="../osa3#node.js">Node.js</a> tapaan.

</div>

Huomaa, että Noden routejen määrittelyssä merkkijonon _api/persons_ kenoviiva käyttäytyy kuten mikä tahansa muu merkki.

Sovellus pitää pystyä käynnistämään komennolla _npm start_.

Komennolla _npm run watch_ käynnistettäessa sovelluksen tulee käynnistyä uudelleen kun koodiin tehdään muutoksia.

#### 3.2 puhelinluettelon backend osa 2

Tee sovelluksen osoitteeseen <http://localhost:3001/info> suunnilleen seuraavanlainen sivu

![]({{ "/assets/teht/20.png" | absolute_url }})

eli sivu kertoo pyynnön tekohetken sekä sen kuinka monta puhelinluettelotietoa sovelluksen muistissa olevassa taulukossa on.

#### 3.3 puhelinluettelon backend osa 3

Toteuta toiminnallisuus yksittäisen puhelinnumerotiedon näyttämiseen. Esim. id:n 5 omaavan numerotiedon url on <http://localhost:3001/api/persons/5>

Jos id:tä vastaavaa puhelinnumerotietoa ei ole, tulee palvelimen vastata asianmukaisella statuskoodilla.

#### 3.4 puhelinluettelon backend osa 4

Toteuta toiminnallisuus, jonka avulla puhelinnumerotieto on mahdollista poistaa numerotiedon yksilöivään URL:iin tehtävällä HTTP DELETE -pyynnöllä.

Testaa toiminnallisuus Postmanilla tai Visual Studio Coden REST clientillä

#### 3.5 puhelinluettelon backend osa 5

Laajenna backendia siten, että uusia puhelintietoja on mahdollista lisätä osoitteeseen <http://localhost:3001/api/persons> tapahtuvalla HTTP POST -pyynnöllä.

Generoi uuden puhelintiedon tunniste funktiolla [Math.random](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random). Käytä riittävän isoa arvoväliä jotta arvottu id on riittävän suurella todennäköisyydellä sellainen, joka ei ole jo käytössä.

#### 3.6* puhelinluettelon backend osa 6

Tee uuden numeron lisäykseen virheiden käsittely. Pyyntö ei saa onnistua, jos
- nimi tai numero puuttuu
- lisättävä nimi on jo luettelossa

Vastaa asiaankuuluvalla statuskoodilla, liitä vastaukseen mukaan myös tieto, joka kertoo virheen syyn, esim:

```js
{ error: 'name must be unique' }
```

### lisää middlewareja

#### 3.7 puhelinluettelon backend osa 7

Lisää sovellukseesi loggausta tekevä middleware [morgan](https://github.com/expressjs/morgan). Konfiguroi se logaamaan konsoliin _tiny_-konfiguraation mukaisesti.

Morganin ohjeet eivät ole ehkä kaikkein selvimmät ja joudut kenties miettimään hiukan. Toisaalta juuri koskaan dokumentaatio ei ole aivan itsestäänselvää, joten kryptisempiäkin asioita on hyvä oppia tulkitsemaan.

Morgan asennetaan kuten muutkin kirjastot, eli komennolla _npm install_ ja sen käyttöönotto tapahtuu kaikkien middlewarejen tapaan komennolla _app.use_

#### 3.8* puhelinluettelon backend osa 8

Konfiguroi morgania siten, että se näyttää myös HTTP-pyyntöjen mukana tulevan datan:

![]({{ "/assets/teht/21.png" | absolute_url }})

Tämä tehtävä on kohtuullisen haastava vaikka koodia ei tarvitakkaan paljoa.

Pari vihjettä:
- [creating new tokens](https://github.com/expressjs/morgan#creating-new-tokens)
- [JSON.stringify](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)

### yhteys frontendiin ja vienti tuotantoon

#### 3.9 puhelinluettelon backend osa 9

Laita backend toimimaan edellisessä osassa tehdyn puhelinluettelon frontendin kanssa muilta osin, paitsi mahdollisen puhelinnumeron muutoksen osalta jonka vastaava toiminnallisuus toteutetaan backendiin vasta tehtävässä 3.17.

Joudut todennäköisesti tekemään frontendiin erinäisiä pieniä muutoksia ainakin backendin oletettujen urlien osalta. Muista pitää selaimen konsoli koko ajan auki. Jos jotkut HTTP-pyynnöt epäonnistuvat, kannattaa katsoa _Network_-välilehdeltä mitä tapahtuu. Pidä myös silmällä mitä palvelimen konsolissa tapahtuu. Jos et tehnyt edellistä tehtävää, kannattaa POST-pyyntöä käsittelevässä tapahtumankäsittelijässä tulostaa konsoliin mukana tuleva data eli _request.body_.

#### 3.10 puhelinluettelon backend osa 10

Vie sovelluksen backend internetiin, esim. Herokuun.

**Huom1** komento _heroku_ toimii laitoksen koneilla ja fuksikannettavilla 9.2. alkaen. Jos et jostain syystä saa asennettua herokua koneellesi, voit käyttää komentoa [npx heroku-cli](https://www.npmjs.com/package/heroku-cli).

**Huom2** eihän hakemisto _build_ ole gitignoroituna projektissasi?

Testaa selaimen ja postmanin tai VS Code REST clientin avulla, että internetissä oleva backend toimii.

**PRO TIP:** kun deployaat sovelluksen herokuun, kannattaa ainakin alkuvaiheissa pitää **KOKO AJAN** näkyvillä herokussa olevan sovelluksen loki antamalla komento <code>heroku logs -t</code>:

![]({{ "/assets/teht/22.png" | absolute_url }})

Tee repositorion juureen tiedosto README.md ja lisää siihen linkki internetissä olevaan sovellukseesi.

#### 3.11 puhelinluettelo full stack

Generoi frontendistä tuotantoversio ja lisää se internetissä olevaan sovellukseesi osan 3 [tapaa noudatellen](/osa3/#staattisten-tiedostojen-tarjoaminen-backendistä)

Huolehdi myös, että frontend toimii edelleen myös paikallisesti.

### mongoosen alkeet

<div class='important'>
Älä laita tietokannan salasanaa Githubiin!
</div>

#### 3.12 tietokanta komentoriviltä

Luo sovellukselle pilvessä oleva mongo mlabin avulla.

Tee projektihakemistoon tiedosto _mongo.js_, jonka avulla voit lisätä tietokantaan puhelinnumeroja sekä listata kaikki kannassa olevat numerot.

**Huom** jos/kun laitat tiedoston Githubiin, älä laita tietokannan salasanaa mukaan!

Ohjelma toimii siten, että jos sille annetaan käynnistäessä kaksi komentoriviparametria, esim:

```bash
node mongo.js Joulupukki 040-1234556
```

Ohjelma tulostaa

```bash
lisätään henkilö Joulupukki numero 040-1234556 luetteloon
```

ja lisää uuden yhteystiedon tietokantaan. Huomaa, että jos nimi sisältää välilyöntejä, on se annettava hipsuissa:

```bash
node mongo.js "Arto Vihavainen" 040-1234556
```

Jos komentoriviparametreja ei anneta, eli ohjelma suoritetaan komennolla

```bash
node mongo.js
```

tulostaa ohjelma tietokannassa olevat numerotiedot:

<pre>
puhelinluettelo:
Pekka Mikkola 040-1234556
Arto Vihavainen 045-1232456
Tiina Niklander 040-1231236
</pre>

Saat selville ohjelman komentoriviparametrit muuttujasta [process.argv](https://nodejs.org/docs/latest-v8.x/api/process.html#process_process_argv)

**HUOM: älä sulje tietokantayhteyttä väärässä kohdassa**. Esim. seuraava koodi ei toimi

```ja
Person
  .find({})
  .then(persons=> {
    // ...
  })

mongoose.connection.close()
```

Koodin suoritus nimittäin etenee siten, että heti operaation _Person.find_ käynnistymisen jälkeen suoritetaan komento _mongoose.connection.close()_ ja tietokantayhteys katkeaa välittömästi. Näin ei koskaan päästä siihen pisteeseen, että _Person.find_-operaation valmistumisen käsittelevää _takaisinkutsufunktiota_ kutsuttaisiin.

Oikea paikka tietokantayhteyden sulkemiselle on takaisinkutsufunktion loppu:

```ja
Person
  .find({})
  .then(persons=> {
    // ...
    mongoose.connection.close()
  })
```

**HUOM2** jos määrittelet modelin nimeksi _Person_, muuttaa mongoose sen monikkomuotoon _people_, jota se käyttää vastaavan kokoelman nimenä.

### backend ja tietokanta

Seuraavat tehtävät saattavat olla melko suoraviivaisia, tosin jos frontend-koodissasi sattuu olemaan bugeja tai epäyhteensopivuutta backendin kanssa, voi seurauksena olla myös mielenkiintoisia bugeja.

#### 3.13 puhelinluettelo ja tietokanta, osa 1

Muuta backendin kaikkien puhelintietojen näyttämistä siten, että se hakee näytettävät puhelintiedot tietokannasta.

Varmista, että frontend toimii muutosten jälkeen.

Tee tässä ja seuraavissa tehtävissä mongoose-spesifinen koodi omaan moduuliin samaan tapaan kuin osan 3 luvussa [tietokantamäärittelyjen eriyttäminen omaksi moduuliksi](/osa3#tietokantamäärittelyjen-eriyttäminen-omaksi-moduuliksi)

#### 3.14* puhelinluettelo ja tietokanta, osa 2

Osan 3 materiaalissa määriteltiin metodi _formatNote_ jonka avulla tietokannasta haettu muistiinpano formatoidaan HTTP-pyyntöjen vastauksiin sopivaan muotoon:

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

Refaktoroi koodiasi siten, että määrittelet formatoinnin suorittavan metodin mongoose skeeman [staattisena metodina](http://mongoosejs.com/docs/guide.html#statics), jolloin voit käyttää sitä koodista seuraavasti:

```js
persons.map(Person.format)
```

muotoillessa taulukossa _persons_ olevat oliot tai yksittäsen olion _person_ muotoilussa seuraavasti:

```js
Person.format(person)
```

Tehtävän tekeminen edellyttää luovaa manuaalin lukemista. Älä juutu tähän ainakaan aluksi liian pitkäksi aikaa!

#### 3.15 puhelinluettelo ja tietokanta, osa 3

Muuta backendiä siten, että uudet numerot tallennetaan tietokantaan. Tässä vaiheessa voit olla välittämättä siitä, onko tietokannassa jo henkilöä jolla on sama nimi kuin lisättävällä.

Varmista, että frontend toimii muutosten jälkeen.

### lisää operaatiota

**HUOM:** vaikka et jostain syystä käsittelisikään promiseihin liittyviä virhetilanteita, on viisasta rekisteröidä promiseille virheenkäsittelijä, joka tulostaa virheen syyn konsoliin:

```js
.catch(error => {
  console.log(error)
  // ...
})
```

näin vältyt monilta ikäviltä yllätyksiltä. Ja muistathan pitää _koko ajan_ silmällä mitä konsolissa tapahtuu...

#### 3.16 puhelinluettelo ja tietokanta, osa 4

Mutta backendiä siten, että numerotietojen poistaminen päivittyy tietokantaan.

Varmista, että frontend toimii muutosten jälkeen.

#### 3.17* puhelinluettelo ja tietokanta, osa 5

Jos frontendissä annetaan numero henkilölle, joka on jo olemassa, päivittää frontend tiedot uudella tekemällä HTTP PUT -pyynnön henkilön tietoja vastaavaan url:iin.

Laajenna backendisi käsittelemään tämä tilanne.

Varmista, että frontend toimii muutosten jälkeen.

#### 3.18* puhelinluettelo ja tietokanta, osa 6

Päivitä myös polkujen _api/persons/:id_ ja _info_ käsittely, ja varmista niiden toimivuus suoraan selaimella, postmanilla tai VS Coden REST clientillä.

Selaimella tarkastellen yksittäisen numerotiedon tulisi näyttää seuraavalta:

![]({{ "/images/teht/22b.png" | absolute_url }})

### loppuhuipennus

#### 3.19* puhelinluettelo ja tietokanta, osa 7

Huolehdi, että backendiin voi lisätä yhdelle nimelle ainoastaan yhden numeron. Frontendin nykyisestä versiosta ei duplikaatteja voi luoda, mutta suoraan Postmanilla tai VS Coden REST clientillä se onnistuu.

Jos HTTP POST -pyyntö yrittää lisätä nimeä, joka on jo puhelinluettelossa, tulee vastata sopivalla statuskoodilla ja lisätä vastaukseen asianmukainen virheilmoitus.

Tehtävän voi tehdä muutamallakin eri tekniikalla. Koska todennäköisesti tarvitset tehtävässä useampaa tietokantaoperaatiota, tulee huomioida operaatioiden asynkroninen luonne:

```js
Person
  .find({name: nameToBeAdded})
  .then(result => {
    // jatka koodia täällä
  })

// tänne ei kannata koodia kirjoittaa...
```

Operaatioita ei siis voi kirjoittaa "peräkkäin". Eräs tapa siisteyttää suoraviivaista "sisäkkäisten callbackien" käyttöä on materiaalissakin esitetty promisejen ketjutus.

Tämä tehtävä saattaa olla jossain määrin hankala, älä juutu tehtävään liian pitkäksi aikaa!

Tutustumme seuraavassa osassa async/await-tekniikkaan, minkä avulla tämäkin tehtävä on helppo tehdä. On kuitenkin erittäin hyödyllistä opetella operoimaan myös suoraan promisejen tasolla.

Mielenkiintoinen mutta enemmän omatoimista opiskelua edellyttävä tapa toiminnallisuuden toteuttamiseen on [mongoosen validaatioiden](http://mongoosejs.com/docs/validation.html) hyödyntäminen, tällöin riittää yhden asynkronisen operaation suorittaminen.

#### 3.20 tietokantaa käyttävä versio herokuun

Generoi päivitetystä sovelluksesta "full stack"-versio, eli tee frontendista uusi production build ja kopioi se backendin repositorioon. Varmista että kaikki toimii paikallisesti käyttämällä koko sovellusta backendin osoitteesta <https://localhost:3001>.

Pushaa uusi versio herokuun ja varmista, että kaikki toimii myös siellä.

#### 3.21* eriytetty sovelluskehitys- ja tuotantotietokanta

Eriytä sovelluskehityksessä ja herokussa käytettävät tietokannat osan 3 lukua [sovelluksen vieminen tuotantoon](/osa3#sovelluksen-vieminen-tuotantoon) noudattaen.

### ESlint

#### 3.22 lint-konfiguraatio

Ota sovellukseesi käyttöön ESlint.

### Tehtävien palautus

Palauta tehtävät [palautussovellukseen](https://studies.cs.helsinki.fi/fs-stats/).

## Osa 4

Deadline 25.2. klo 23:59

Osassa on 21 tehtävää, joista "pakollisia" on 11. On suositeltavaa että etenet seuraavaan osaan vasta kun olet tehnyt kaikki pakolliset tehtävät. Palautuksen tekemisen jälkeen et voi enää palauttaa osan tehtäviä.

Rakennamme tämän osan tehtävissä _blogilistasovellusta_, jonka avulla käyttäjien on mahdollista tallettaa tietoja internetistä löytämistään mielenkiintoisista blogeista. Kustakin blogista talletetaan sen kirjoittaja (author), aihe (title), url sekä blogilistasovelluksen käyttäjien antamien äänien määrä.

Lopullisen version palauttaminen riittää, voit toki halutessasi tehdä commitin jokaisen tehtävän jälkeisestä tilanteesta, mutta se ei ole välttämätöntä.

Blogilistasovellus muistuttaa huomattavasti syksyn ohjelmistotuotantokurssin miniprojekteissa tehtyä [ohjelmistoa](https://github.com/mluukkai/ohjelmistotuotanto2017/wiki/miniprojekti-speksi).

<div class='important'>

<p>Ennen kun teet tehtäviä, on enemmän kuin suositeltavaa, että käyt huolellisesti läpi <a href='https://fullstack-hy.github.io/osa4/'>osan 4 materiaalin</a>. Tehtävien tekeminen ilman materiaalin lukemista tapahtuu täysin omalla vastuulla.</p>

</div>

### sovelluksen alustus ja rakenne

#### 4.1 blogilista, osa 1

Saat sähköpostitse yhteen tiedostoon koodatun sovellusrungon:

```js
const http = require('http')
const express = require('express')
const app = express()
const bodyParser = require('body-parser')
const cors = require('cors')
const mongoose = require('mongoose')

const Blog = mongoose.model('Blog', {
  title: String,
  author: String,
  url: String,
  likes: Number
})

module.exports = Blog

app.use(cors())
app.use(bodyParser.json())

const mongoUrl = 'mongodb://localhost/bloglist'
mongoose.connect(mongoUrl)
mongoose.Promise = global.Promise

app.get('/api/blogs', (request, response) => {
  Blog
    .find({})
    .then(blogs => {
      response.json(blogs)
    })
})

app.post('/api/blogs', (request, response) => {
  const blog = new Blog(request.body)

  blog
    .save()
    .then(result => {
      response.status(201).json(result)
    })
})

const PORT = 3003
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```

Tee sovelluksesta toimiva _npm_-projekti. Jotta sovelluskehitys olisi sujuvaa, konfiguroi sovellus suoritettavaksi _nodemon_:illa. Voit luoda sovellukselle uuden tietokannan esim. mlabiin tai käyttää edellisen osan sovelluksen tietokantaa.

Varmista, että sovellukseen on mahdollista lisätä blogeja Postmanilla tai VS Code REST clientilla, ja että sovellus näyttää lisätyt blogit.

#### 4.2 blogilista, osa 2

Jaa sovelluksen koodi osan 4 [alun](/osa4) tapaan useaan moduuliin.

**HUOM** etene todella pienin askelin, varmistaen että kaikki toimii koko ajan. Jos yrität "oikaista" tekemällä monta asiaa kerralla, on [Murphyn lain](https://fi.wikipedia.org/wiki/Murphyn_laki) perusteella käytännössä varmaa, että jokin menee pahasti pieleen ja "oikotien" takia maaliin päästään paljon myöhemmin kuin systemaattisin pienin askelin.

Paras käytänne on commitoida koodi aina stabiilissa tilanteessa, tällöin on helppo palata aina toimivaan tilanteeseen jos koodi menee liian solmuun.

### yksikkötestaus

Tehdään joukko blogilistan käsittelyyn tarkoitettuja apufunktioita. Tee funktiot esim. tiedostoon _utils/list_helper.js_. Tee testit sopivasti nimettyyn tiedostoon hakemistoon _tests_.

**HUOM:** jos jokin testi ei mene läpi, ei kannata ongelmaa korjatessa suorittaa kaikkia testejä, vaan ainoastaan rikkinäistä testiä hyödyntäen [only](https://facebook.github.io/jest/docs/en/api.html#testonlyname-fn-timeout)-metodia.

#### 4.3 apufunktioita ja yksikkötestejä, osa 1

Määrittele ensin funktio _dummy_ joka saa parametrikseen taulukollisen blogeja ja palauttaa aina luvun 1. Tiedoston _list_helper.js_ sisällöksi siis tulee tässä vaiheessa

```js
const dummy = (blogs) => {
  // ...
}

module.exports = {
  dummy
}
```

Varmista testikonfiguraatiosi toimivuus seuraavalla testillä:

```js
const listHelper = require('../utils/list_helper')

test('dummy is called', () => {
  const blogs = []

  const result = listHelper.dummy(blogs)
  expect(result).toBe(1)
})
```

#### 4.4 apufunktioita ja yksikkötestejä, osa 2

Määrittele funktio _totalLikes_ joka saa parametrikseen taulukollisen blogeja. Funktio palauttaa blogien yhteenlaskettujen tykkäysten eli _likejen_ määrän.

Määrittele funktiolle sopivat testit. Funktion testit kannattaa laittaa _describe_-lohkoon jolloin testien tulostus ryhmittyy miellyttävästi:

![]({{ "/assets/teht/23.png" | absolute_url }})

Testisyötteiden määrittely onnistuu esim. seuraavaan tapaan:

```js
describe('total likes', () => {
  const listWithOneBlog = [
    {
      _id: '5a422aa71b54a676234d17f8',
      title: 'Go To Statement Considered Harmful',
      author: 'Edsger W. Dijkstra',
      url: 'http://www.u.arizona.edu/~rubinson/copyright_violations/Go_To_Considered_Harmful.html',
      likes: 5,
      __v: 0
    }
  ]

  test('when list has only one blog equals the likes of that', () => {
    const result = listHelper.totalLikes(listWithOneBlog)
    expect(result).toBe(5)
  })
})
```

Jos et viitsi itse määritellä testisyötteenä käytettäviä blogeja, saat valmiin listan [täältä](https://github.com/FullStack-HY/part3-notes-backend/wiki/blogs-for-testing)

Törmäät varmasti testien tekemisen yhteydessä erinäisiin ongelmiin. Pidä mielessä osassa 3 käsitellyt [debuggaukseen](osa3/#Node-sovellusten-debuggaaminen) liittyvät asiat, voit testejäkin suorittaessasi printtailla konsoliin komennolla _console.log_


#### 4.5* apufunktioita ja yksikkötestejä, osa 3

Määrittele funktio _favoriteBlog_ joka saa parametrikseen taulukollisen blogeja. Funktio selvittää millä blogilla on eniten likejä. Jos suosikkeja on monta, riittää että funktio palauttaa niistä jonkun.

Paluuarvo voi olla esim. seuraavassa muodossa:

```js
{
  title: "Canonical string reduction",
  author: "Edsger W. Dijkstra",
  likes: 12
}
```

**Huom**, että kun vertailet olioita, metodi [toEqual](https://facebook.github.io/jest/docs/en/expect.html#toequalvalue) on todennäköisesti se mitä haluat käyttää sillä [toBe](https://facebook.github.io/jest/docs/en/expect.html#tobevalue)-vertailu, joka sopii esim. lukujen ja merkkijonojen vertailuun vaatisi olioiden vertailussa, että oliot ovat samat, pelkkä saman sisältöisyys ei riitä.

Tee myös tämän ja seuraavien kohtien testit kukin oman _describe_-lohkon sisälle.

#### 4.6* apufunktioita ja yksikkötestejä, osa 4

Tämä ja seuraava tehtävä ovat jo hieman haastavampia. Tehävien tekeminen ei ole osan jatkon kannalta oleellista, eli voi olla hyvä idea palata näihin vasta kun muu osa on kahlattu läpi.

Määrittele funktio _mostBlogs_ joka saa parametrikseen taulukollisen blogeja. Funktio selvittää _kirjoittajan_, kenellä on eniten blogeja. Funktion paluuarvo kertoo myös ennätysblogaajan blogien määrän:

```js
{
  author: "Robert C. Martin",
  blogs: 3
}
```

 Jos ennätysblogaajia on monta, riittää että funktio palauttaa niistä jonkun.

#### 4.7* apufunktioita ja yksikkötestejä, osa 5

Määrittele funktio _mostLikes_ joka saa parametrikseen taulukollisen blogeja. Funktio selvittää kirjoittajan, kenen blogeilla on eniten likejä. Funktion paluuarvo kertoo myös suosikkiblogaajan likejen yhteenlasketun määrän:

```js
{
  author: "Edsger W. Dijkstra",
  votes: 17
}
```

Jos suosikkiblogaajia on monta, riittää että funktio palauttaa niistä jonkun.

### API:n testaaminen

**Huom** materiaalissa käytetään muutamaan kertaan ekspektaatiota [toContain](https://facebook.github.io/jest/docs/en/expect.html#tocontainitem) tarkastettaessa että jokin arvo on taulukossa. Kannattaa huomata, että metodi käyttää samuuden vertailuun ===-operaattoria ja olioiden kohdalla tämä ei ole useinkaan se mitä halutaan ja parempi vaihtoehto onkin [toContainEqual](https://facebook.github.io/jest/docs/en/expect.html#tocontainequalitem).

#### 4.8 blogilistan testit, osa 1

Tee API-tason testit blogilistan osoitteeseen /api/blogs tapahtuvalle HTTP GET -pyynnölle.

Kun testi on valmis, refaktoroi operaatio käyttämään promisejen sijaan async/awaitia.

Huomaa, että joudut tekemään koodiin osan 4 materiaalin tyylin joukon muutoksia (mm. testausympäristön määrittely), jotta saat järkevästi määriteltyä API-tason testejä.

**Huom** testien kehitysvaiheessa ei yleensä kannata suorittaa joka kerta kaikkia testejä, vaan keskittyä yhteen testiin kerrallaan. On useita tapoja, joilla voidaan rajoittaa jestin suorittamia testejä. Esim. komennolla

```bash
npx jest -t 'blogs are returned'
```

voidaan suorittaa ainoastaan ne testit, joiden nimessä esiintyy _blogs are returned_.

Yksittäisen testitiedoston sisällä olevien testien suoritusta voidaan kontrolloida metodeilla _skip_ ja _only_ [ks. manuaali](https://facebook.github.io/jest/docs/en/api.html).

Voimme esim. laittaa koko edellisessä tehtäväsarjassa tehdyt testit ison describen sisälle ja määritellä ne [skipattavaksi](https://facebook.github.io/jest/docs/en/api.html#describeskipname-fn):

```js
describe.skip('list helpers', () => {
  test('dummy is called', () => {
    const blogs = []

    const result = listHelper.dummy(blogs)
    expect(result).toBe(1)
  })

  describe('total likes', () => {
    test('of empty list is 0', () => {
      const result = listHelper.totalLikes(emptyList)
      expect(result).toBe(0)
    })

    // ...
  })
})
```

tällöin komennolla _npm test_ suoritettaessa tiedoston testejä ei suoriteta ollenkaan.

Kun testit ovat stabiilissa tilassa, tulee skiptit ja onlyt poistaa.

#### 4.9 blogilistan testit, osa 2

Tee testit blogin lisäämiselle, eli osoitteeseen /api/blogs tapahtuvalle HTTP POST -pyynnölle.

Kun testi on valmis, refaktoroi operaatio käyttämään promisejen sijaan async/awaitia.

#### 4.10* blogilistan testit, osa 3

Tee testi joka varmistaa, että jos kentälle _likes_ ei anneta arvoa, asetetaan sen arvoksi 0. Muiden kenttien sisällöstä ei tässä tehtävässä vielä välitetä.

Laajenna ohjelmaa siten, että testi menee läpi.

#### 4.11* blogilistan testit, osa 4

Tee testit blogin lisäämiselle, eli osoitteeseen /api/blogs tapahtuvalle HTTP POST -pyynnölle, joka varmistaa, että jos uusi blogi ei sisällä kenttiä _title_ ja _url_, pyyntöön vastataan statuskoodilla _400 Bad request_

Laajenna toteutusta siten, että testit menevät läpi.

### Varoitus

Jos huomaat kirjoittavasi sekaisin async/awaitia ja _then_-kutsuja, on 99% varmaa, että teet jotain väärin. Käytä siis jompaa kumpaa tapaa, älä missään tapauksessa "varalta" molempia.

### Lisää toiminnallisuutta ja testejä

#### 4.12* blogilistan laajennus, osa 1

Refaktoroi projektin testit siten, että ne eivät enää ole riippuvaisia siitä, että HTTP GET -operaatioiden testit suoritetaan ennen uusien blogien lisäämisen testaamista. Määrittele myös sopivia apumetodeja, joiden avulla saat poistettua testeistä copypastea:

Testit voivat tämän tehtävän jälkeen noudattaa esim. osan 4 luvun [Testien refaktorointi](/osa4#testien-refaktorointi) tyyliä

```js
const helper = require('./test_helper')

// ...

test('a valid blog can be added', async () => {
  const newBlog = {
    // ....
  }

  const blogsBefore = await helper.blogsInDb()

  await api
    .post('/api/blogs')
    .send(newBlog)
    .expect(201)
    .expect('Content-Type', /application\/json/)

  const blogsAfter = await helper.blogsInDb()

  expect(blogsAfter.length).toBe(blogsBefore.length+1)
  expect(blogsAfter).toContainEqual(newBlog)
})
```

#### 4.13 blogilistan laajennus, osa 2

Toteuta sovellukseen mahdollisuus yksittäisen blogin poistoon.

Käytä async/awaitia.

Määrittele ensin toiminnallisuutta testaavat testit ja tämän jälkeen toteuta toiminnallisuus. Noudata operaation HTTP-rajapinnan suhteen [RESTful](/osa3#rest)-käytänteitä.

Saat toteuttaa ominaisuudelle testit jos haluat. Jos et, varmista ominaisuuden toimivuus esim. Postmanilla.

#### 4.14* blogilistan laajennus, osa 3

Toteuta sovellukseen mahdollisuus yksittäisen blogin muokkaamiseen.

Käytä async/awaitia.

Tarvitsemme muokkausta lähinnä _likejen_ lukumäärän päivittämiseen. Toiminnallisuuden voi toteuttaa samaan tapaan kuin muistiinpanon päivittäminen toteutettiin [osassa 3](/osa3#loput-operaatiot).

Saat toteuttaa ominaisuudelle testit jos haluat. Jos et, varmista ominaisuuden toimivuus esim. Postmanilla.

### Blogilistan käyttäjät

Seuraavien tehtävien myötä Blogilistalle luodaan käyttäjienhallinnan perusteet. Varminta on seurata melko tarkkaan osan 4 luvusta [Käyttäjien hallinta ja monimutkaisempi tietokantaskeema](/osa4#käyttäjien-hallinta-ja-monimutkaisempi-tietokantaskeema) alkavaa tarinaa. Toki luovuus on sallittua.

### Varoitus vielä kerran

Jos huomaat kirjoittavasi sekaisin async/awaitia ja _then_-kutsuja, on 99% varmaa, että teet jotain väärin. Käytä siis jompaa kumpaa tapaa, älä missään tapauksessa "varalta" molempia.

#### 4.15 blogilistan laajennus, osa 4

Tee sovellukseen mahdollisuus luoda käyttäjiä tekemällä HTTP POST -pyyntö osoitteeseen _api/users_. Käyttäjillä on käyttäjätunnus, salasana ja nimi sekä totuusarvoinen kenttä, joka kertoo onko käyttäjä täysi-ikäinen.

Älä talleta tietokantaan salasanoja selväkielisenä vaan käytä osan 4 luvun [Käyttäjien luominen](/osa4#käyttäjien-luominen) tapaan _bcrypt_-kirjastoa.

Tee järjestelmään myös mahdollisuus katsoa kaikkien käyttäjien tiedot sopivalla HTTP-pyynnöllä.

Käyttäjien lista voi näyttää esim. seuraavalta:
![](https://raw.githubusercontent.com/mluukkai/mluukkai.github.io/master/assets/teht/24.png)

#### 4.16* blogilistan laajennus, osa 5

Laajenna käyttäjätunnusten luomista siten, että salasanan tulee olla vähintään 3 merkkiä pitkä ja käyttäjätunnus on järjestelmässä uniikki. Jos täysi-ikäisyydelle ei määritellä luotaessa arvoa, on se oletusarvoisesti true.

Luomisoperaation tulee palauttaa sopiva statuskoodi ja kuvaava virheilmoitus, jos yritetään luoda epävalidi käyttäjä.

Tee testit, jotka varmistavat, että virheellisiä käyttäjiä ei luoda, ja että virheellisen käyttäjän luomisoperaatioon vastaus on järkevä statuskoodin ja virheilmoituksen osalta.

#### 4.17 blogilistan laajennus, osa 6

Laajenna blogia siten, että blogiin tulee tieto sen lisänneestä käyttäjästä.

Muokkaa blogien lisäystä osan 4 luvun [populate](/osa4/#populate) tapaan siten, että blogin lisäämisen yhteydessä määritellään blogin lisääjäksi _joku_ järjestelmän tietokannassa olevista käyttäjistä (esim. ensimmäisenä löytyvä). Tässä vaiheessa ei ole väliä kuka käyttäjistä määritellään lisääväksi. Toiminnallisuus viimeistellään tehtävässä 4.19.

Muokaa kaikkien blogien listausta siten, että blogien yhteydessä näytetään lisääjän tiedot:

![](https://raw.githubusercontent.com/mluukkai/mluukkai.github.io/master/assets/teht/25.png)

ja käyttäjien listausta siten että käyttäjien lisäämät blogit ovat näkyvillä

![]({{ "/assets/teht/26.png" | absolute_url }})

#### 4.18 blogilistan laajennus, osa 7

Toteuta osan 4 luvun [Kirjautuminen](/osa4#kirjautuminen) tapaan järjestelmään token-perustainen autentikointi.

#### 4.19 blogilistan laajennus, osa 8

Muuta blogien lisäämistä siten, että se on mahdollista vain, jos lisäyksen tekevässä HTTP POST -pyynnössä on mukana validi token. Tokenin haltija määritellään blogin lisääjäksi.

#### 4.20* blogilistan laajennus, osa 9

Osan 4 [esimerkissä](osa4/#kirjautuminen) token otetaan headereista apufunktion _getTokenFrom_ avulla.

Jos käytit samaa ratkaisua, refaktoroi tokenin erottaminen [middlewareksi](osa3/#middlewaret), joka ottaa tokenin _Authorization_-headerista ja sijoittaa sen _request_-olion kenttään _token_.

Eli kun rekisteröit middlewaren ennen routeja tiedostossa _index.js_

```js
app.use(middleware.tokenExtractor)
```

pääsevät routet tokeniin käsiksi suoraan viittaamalla _request.token_:

```js
blogsRouter.post('/', async (request, response) => {
  // ..
  const decodedToken = jwt.verify(request.token, process.env.SECRET)
  // ..
})
```

#### 4.21* blogilistan laajennus, osa 9

Muuta blogin poistavaa operaatiota siten, että poisto onnistuu ainoastaan jos poisto-operaation tekijä (eli se kenen token on pyynnön mukana) on sama kuin blogin lisääjä.

Jos poistoa yritetään ilman tokenia tai väärän käyttäjän toimesta, tulee operaation palauttaa asiaan kuuluva statuskoodi.

Huomaa, että jos haet blogin tietokannasta

```js
const blog = await Blog.findById(...)
```

ei kenttä _blog.user_ ole tyypiltään merkkijono vaan _object_. Eli jos haluat verrata kannasta haetun olion id:tä merkkijonomuodossa olevaan id:hen, ei normaali vertailu toimi. Kannasta haettu id tulee muuttaa vertailua varten merkkijonoksi:

```js
if ( blog.user.toString() === userid.toString() ) ...
```

## Osa 5

Deadline 4.3. klo 23:59

Osassa on 21 tehtävää, joista "pakollisia" on 11. On suositeltavaa että etenet seuraavaan osaan vasta kun olet tehnyt kaikki pakolliset tehtävät.  Palautuksen tekemisen jälkeen et voi enää palauttaa osan tehtäviä.

Teemme nyt edellisen osan tehtävissä tehtyä bloglist-backendia käyttävän frontendin. Voit ottaa tehtävien pohjaksi [Githubista](https://github.com/FullStack-HY/bloglist-frontend) olevan sovellusrungon. Sovellus olettaa, että backend on käynnissä koneesi portissa 3003.
Lopullisen version palauttaminen riittää, voit toki halutessasi tehdä commitin jokaisen tehtävän jälkeisestä tilanteesta, mutta se ei ole välttämätöntä.

Tämän osan alun tehtävät käytännössä kertaavat kaiken oleellisen tämän kurssin puitteissa Reactista läpikäydyn asian ja voivat siinä mielessä olla kohtuullisen haastavia, erityisesti jos edellisen osan tehtävissä toteuttamasi backend toimii puutteellisesti. Saattaakin olla varminta siirtyä käyttämään osan 4 mallivastauksen backendia.

Muista tehtäviä tehdessäsi kaikki debuggaukseen liittyvät käytänteet, erityisesti konsolin tarkkailu.

<div class='important'>

<p>Ennen kun teet tehtäviä, on enemmän kuin suositeltavaa, että käyt huolellisesti läpi <a href='https://fullstack-hy.github.io/osa5/'>osan 5 materiaalin</a>. Tehtävien tekeminen ilman materiaalin lukemista tapahtuu täysin omalla vastuulla.</p>

</div>

### Varoitus

Jos huomaat kirjoittavasi sekaisin async/awaitia ja _then_-kutsuja, on 99.9% varmaa, että teet jotain väärin. Käytä siis jompaa kumpaa tapaa, älä missään tapauksessa "varalta" molempia.

### kirjautuminen ja blogien luonti

#### 5.1 blogilistan frontend, osa 1

Ota tehtävien pohjaksi [Githubissa](https://github.com/FullStack-HY/bloglist-frontend) olevan sovellusrungo kloonaamalla se sopivaan paikkaan komennolla

```bash
git clone git@github.com:FullStack-HY/bloglist-frontend.git
```

Jos kloonaat projektin olemassaolevan git-reposition sisälle, *poista kloonatun sovelluksen git-konfiguraatio*

```bash
cd bloglist-frontend   // eli mene ensin kloonatun repositorion hakemistoon
rm -rf .git
```

Sovellus käynnistyy normaaliin tapaan, mutta joudut ensin asentamaan sovelluksen riippuvuudet:

```bash
npm install
npm start
```

Toteuta frontendiin kirjautumisen mahdollistava toiminnallisuus. Kirjautumisen yhteydessä backendin palauttama _token_ tallennetaan sovelluksen tilan kenttään _user_.

Jos käyttäjä ei ole kirjautunut, sivulla näytetään _pelkästään_ kirjautumislomake:

![]({{ "/assets/teht/27.png" | absolute_url }})

Kirjautuneelle käyttäjälle näytetään kirjautuneen käyttäjän nimi sekä blogien lista

![]({{ "/assets/teht/28.png" | absolute_url }})

Tässä vaiheessa kirjautuneiden käyttäjien tietoja ei vielä tarvitse muistaa local storagen avulla.

**HUOM** Voit tehdä kirjautumislomakkeen ehdollisen renderöinnin esim. seuraavasti:

```react
render() {
  if (this.state.user === null) {
    return (
      <div>
        <h2>Kirjaudu sovellukseen</h2>
        <form>
          //...
        </form>
      </div>
    )
  }

  return (
    <div>
      <h2>blogs</h2>
      {this.state.blogs.map(blog =>
        <Blog key={blog._id} blog={blog} />
      )}
    </div>
  )
}
```

#### 5.2 blogilistan frontend, osa 2

Tee kirjautumisesta "pysyvä" local storagen avulla. Tee sovellukseen myös mahdollisuus uloskirjautumiseen

![]({{ "/assets/teht/29.png" | absolute_url }})

Uloskirjautumisen jälkeen selain ei saa muistaa kirjautunutta käyttäjää reloadauksen jälkeen.

#### 5.3 blogilistan frontend, osa 3

Laajenna sovellusta siten, että kirjautunut käyttäjä voi luoda uusia blogeja:

![]({{ "/assets/teht/30.png" | absolute_url }})

Bloginluomislomakkeesta voi tehdä oman komponenttinsa, joka hallitsee lomakkeen kenttien sisältöä tilansa avulla. Kaiken blogin luomiseen liittyvän tilan voi toki tallettaa myös _App_-komponenttiin.

#### 5.4* blogilistan frontend, osa 4

Toteuta sovellukseen notifikaatiot, jotka kertovat sovelluksen yläosassa onnistuneista ja epäonnistuneista toimenpiteistä. Esim. blogin lisäämisen yhteydessä voi antaa seuraavan notifikaation

![]({{ "/assets/teht/32.png" | absolute_url }})

epäonnistunut kirjautuminen taas johtaa notifikaatioon

![]({{ "/assets/teht/31.png" | absolute_url }})

Notifikaation tulee olla näkyvillä muutaman sekunnin ajan. Värien lisääminen ei ole pakollista.

### komponenttien näyttäminen vain tarvittaessa

#### 5.5 blogilistan frontend, osa 5

Tee blogin luomiseen käytettävästä lomakkeesta ainoastaan tarvittaessa näytettävä osan 5 luvun [Kirjautumislomakkeen näyttäminen vain tarvittaessa](/osa5#kirjautumislomakkeen näyttäminen-vain-tarvittaessa) tapaan. Voit halutessasi hyödyntää osassa 5 määriteltyä komponenttia _Togglable_.

#### 5.6* blogilistan frontend, osa 6

Laajenna blogien listausta siten, että klikkaamalla blogin nimeä, sen täydelliset tiedot aukeavat

![]({{ "/assets/teht/33.png" | absolute_url }})

Uusi klikkaus blogin nimeen pienentää näkymän.

Napin _like_ ei tässä vaiheessa tarvitse tehdä mitään.

Kuvassa on myös käytetty hieman CSS:ää parantamaan sovelluksen ulkoasua.

Tyylejä voidaan määritellä osan 5 tapaan helposti [inline](https://react-cn.github.io/react/tips/inline-styles.html)-tyyleinä seuraavasti:

```react
class Blog extends React.Component {
  // ...

  render() {
    // ..

    const blogStyle = {
      paddingTop: 10,
      paddingLeft: 2,
      border: 'solid',
      borderWidth: 1,
      marginBottom: 5
    }

    return (
      <div style={blogStyle}>
        ...
      </div>
    )
  }
}
```

### Varoitus vielä kerran

Jos huomaat kirjoittavasi sekaisin async/awaitia ja _then_-kutsuja, on 99.9% varmaa, että teet jotain väärin. Käytä siis jompaa kumpaa tapaa, älä missään tapauksessa "varalta" molempia.

#### 5.7* blogilistan frontend, osa 7

Toteuta like-painikkeen toiminnallisuus. Like lisätään backendiin blogin yksilöivään urliin tapahtuvalla _PUT_-pyynnöllä.

Koska backendin operaatio korvaa aina koko blogin, joudut lähettämään operaation mukana blogin kaikki kentät, eli jos seuraavaa blogia liketetään

```js
{
  _id: "5a43fde2cbd20b12a2c34e91",
  user: {
    _id: "5a43e6b6c37f3d065eaaa581",
    username: "mluukkai",
    name: "Matti Luukkainen"
  },
  likes: 0,
  author: "Joel Spolsky",
  title: "The Joel Test: 12 Steps to Better Code",
  url: "https://www.joelonsoftware.com/2000/08/09/the-joel-test-12-steps-to-better-code/"
},
```

tulee palvelimelle tehdä PUT-pyyntö osoitteeseen _/api/blogs/5a43fde2cbd20b12a2c34e91_ ja sisällyttää pyynnön mukaan seuraava data:

```js
{
  user: "5a43e6b6c37f3d065eaaa581",
  likes: 1,
  author: "Joel Spolsky",
  title: "The Joel Test: 12 Steps to Better Code",
  url: "https://www.joelonsoftware.com/2000/08/09/the-joel-test-12-steps-to-better-code/"
}
```

#### 5.8* blogilistan frontend, osa 8

Järjestä sovellus näyttämään blogit _likejen_ mukaisessa suuruusjärjestyksessä. Järjestäminen onnistuu taulukon metodilla [sort](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort).

#### 5.9* blogilistan frontend, osa 9

Lisää nappi blogin poistamiselle.

Toteuta myös poiston tekevä logiikka. Laajenna backendiä siten, että ne blogit, joihin ei liity lisääjää, ovat kaikkien kirjautuneiden käyttäjien poistettavissa.

Ohjelmasi voi näyttää esim. seuraavalta:

![]({{ "/assets/teht/34.png" | absolute_url }})

Kuvassa näkyvä poiston varmistus on helppo toteuttaa funktiolla
[window.confirm](https://developer.mozilla.org/en-US/docs/Web/API/Window/confirm).

#### 5.10* blogilistan frontend, osa 10

Näytä poistonappi ainoastaan jos kyseessä on kirjautuneen käyttäjän lisäämä blogi _tai_ blogi, jolle ei ole määritelty lisääjää.

### PropTypet

#### 5.11 blogilistan frontend, osa 11

Määrittele joillekin sovelluksesi komponenteille PropTypet.

### komponenttien testaaminen

**HUOM:** jos jokin testi on rikki, ei kannata ongelmaa korjatessa suorittaa kaikkia testejä, vaan ainoastaan rikkinäistä testiä hyödyntäen [only](https://facebook.github.io/jest/docs/en/api.html#testonlyname-fn-timeout)-metodia.

**HUOM2:** älä aliarvioi testissä tapahtuvan _console.logauksen_ hyödyllisyyttä! Normaalissa koodauksessa console.log on elintärkeä, testauksessa se on välillä suorastaan välttämätön sillä testejä suorittaessa et saa mistään muualta feedbackiä.

Testejä suorittaessasi voit käyttää console.log-komentoja testeissä ja _sovelluksen koodissa_.

#### 5.12 blogilistan testit, osa 1

Lisää sovellukseesi tilapäisesti seuraava komponentti

```react
import React from 'react'

const SimpleBlog = ({ blog, onClick }) => (
  <div>
    <div>
      {blog.title} {blog.author}
    </div>
    <div>
      blog has {blog.likes} likes
      <button onClick={onClick}>like</button>
    </div>
  </div>
)

export default SimpleBlog
```

Tee testi, joka varmistaa, että komponentti renderöi blogin titlen, authorin ja likejen määrän.

Lisää komponenttiin tarvittaessa testausta helpottavia CSS-luokkia.

#### 5.13 blogilistan testit, osa 2

Tee testi, joka varmistaa, että jos komponentin _like_-nappia painetaan kahdesti, komponentin propsina saamaa tapahtumankäsittelijäfunktiota kutsutaan kaksi kertaa.

#### 5.14* blogilistan testit, osa 3

Tee oman sovelluksesi komponentille _Blog_ testit, jotka varmistavat, että oletusarvoisesti blogista on näkyvissä ainoastaan nimi ja kirjoittaja, ja että klikkaamalla niitä saadaan näkyviin myös muut osat blogin tiedoista.

**HUOM:** tee testissä klikkaus _ennen_ kuin haet tarkastettavan elementin muuttujaan,
eli tee komennot tässä järjestyksessä

```js
it('after clicking name the details are displayed', () => {
  // haetaan klikattava osa komponentista
  const nameDiv = ...
  nameDiv.simulate('click')

  // haetaan tarkastettava, eli detaljit sisältävä osa komponentista
  const contentDiv = ...
  expect(contentDiv...)
})
```

**väärä** järjestys on siis seuraava

```js
it('DOES NOT WORK', () => {
  const nameDiv = ...
  const contentDiv = ...

  // klikataan liian myöhään
  nameDiv.simulate('click')

  expect(contentDiv...)
})
```

### integraatiotestaus

#### 5.15 blogilistan testit, osa 4

Tee sovelluksesi integraatiotesti, joka varmistaa, että jos käyttäjä ei ole kirjautunut järjestelmään, näyttää sovellus ainoastaan kirjautumislomakkeen, eli yhtään blogia ei vielä renderöidä.

#### 5.16* blogilistan testit, osa 5

Tee myös testi, joka varmistaa, että kun käyttäjä on kirjautuneena, blogit renderöityvät sivulle.

**Vihje 1:**

Kirjautuminen kannattaa toteuttaa manipuloimalla testeissä local storagea. Jos määrittelet testeille mock-localstoragen osan 5 materiaalia seuraten, voit käyttää testikoodissa local storagea seuraavasti:

```js
const user = {
  username: 'tester',
  token: '1231231214',
  name: 'Teuvo Testaaja'
}

localStorage.setItem('loggedBlogAppUser', JSON.stringify(user))
```

**Vihje 2:**

Jotta mockin palauttamat blogit renderöityvät, kannattaa komponentti _App_ luoda _describe_-lohkossa. Voit noudattaa tämän ja edellisen tehtävän organisoinnissa esim. seuraavaa tapaa:

```js
describe('<App />', () => {
  let app

  describe('when user is not logged', () => {
    beforeEach(() => {
      // luo sovellus siten, että käyttäjä ei ole kirjautuneena
    })

    it('only login form is rendered', () => {
      app.update()
      // ...
    })
  })

  describe('when user is logged', () => {
    beforeEach(() => {
      // luo sovellus siten, että käyttäjä on kirjautuneena
    })

    it('all notes are rendered', () => {
      app.update()
      // ...
    })
  })
})
```

### Redux-Unicafe

Tehdään seuraavissa tehtävissä hieman muokattu redux-versio osan 1 tehtävien Unicafe-sovelluksesta. Sovellus voi näyttää esim. seuraavalta:

![]({{ "/assets/teht/35.png" | absolute_url }})

Haluttu toiminnallisuus lienee ilmeinen.

Voit ottaa halutessasi tehtävän pohjaksi koodin [täältä](https://github.com/FullStack-HY/FullStack-Hy.github.io/wiki/unicafe-redux)

#### 5.17 unicafe revisited, osa 1

Ennen sivulla näkyvää toiminnallisuutta, toteutetaan storen edellyttämä toiminnallisuus.

Storeen täytyy tallettaa erikseen lukumäärä jokaisentyyppisestä palautteesta. Storen hallitsema tila on siis muotoa:

```js
{
  good: 5,
  ok: 4,
  bad: 2
}
```

Seuraavassa on runko reducerille:

```js
const initialState = {
  good: 0,
  ok: 0,
  bad: 0
}

const counterReducer = (state = initialState, action) => {
  console.log(action)
  switch (action.type) {
    case 'GOOD':
      return state
    case 'OK':
      return state
    case 'BAD':
      return state
    case 'ZERO':
      return state
  }
  return state
}

export default counterReducer
```

ja sen testien runko

```js
import deepFreeze from 'deep-freeze'
import counterReducer from './reducer'

describe('unicafe reducer', () => {
  const initialState = {
    good: 0,
    ok: 0,
    bad: 0
  }

  it('should return a proper initial state when called with undefined state', () => {
    const state = {}
    const action = {
      type: 'DO_NOTHING'
    }

    const newState = counterReducer(undefined, action)
    expect(newState).toEqual(initialState)
  })

  it('good is incremented', () => {
    const action = {
      type: 'GOOD'
    }
    const state = initialState

    deepFreeze(state)
    const newState = counterReducer(state, action)
    expect(newState).toEqual({
      good: 1,
      ok: 0,
      bad: 0
    })
  })
})
```

Testit olettavat että reduceri on talletettu tiedostoon _reducer.js_.

**Toteuta reducer ja tee sille testit.**

Varmista testeissä _deep-freeze_-kirjaston avulla, että kyseessä on _puhdas funktio_. Huomaa, että valmiin ensimmäisen testin on syytä mennä läpi koska redux olettaa, että reduceri palauttaa järkevän alkutilan kun sitä kutsutaan siten että ensimmäinen parametri, eli aiempaa tilaa edustava _state_ on _undefined_.

Aloita laajentamalla reduceria siten, että molemmat testeistä menevät läpi. Lisää tämän jälkeen loput testit ja niiden toteuttava toiminnallisuus.

Osan 2 luvun [Muistiinpanon tärkeyden muutos](osa2/#Muistiinpanon-tärkeyden-muutos) olion kopiointiin liittyvät asiat saattavat olla hyödyksi.

#### 5.18 unicafe revisited, osa 2

Toteuta sitten sovelluksen koko sovellus.

Älä unohda, että joudut huolehtimaan siitä, että sovellus uudelleenrenderöi itsensä storen muutosten yhteydessä esim. seuraavasti:

```bash
const render = () => {
  ReactDOM.render(<App />, document.getElementById('root'))
}

render()
store.subscribe(render)
```

### redux-anekdootit

Toteutetaan osan lopuksi versio toisesta ensimmäisen osan tehtävästä, anekdoottien äänestyssovelluksesta. Voit ottaa ratkaisusi pohjaksi repositoriossa <https://github.com/FullStack-HY/redux-anecdotes> olevan projektin.


Jos kloonaat projektin olemassaolevan git-reposition sisälle, *poista kloonatun sovelluksen git-konfiguraatio*

```bash
cd redux-anecdotes   // eli mene ensin kloonatun repositorion hakemistoon
rm -rf .git
```

Sovellus käynnistyy normaaliin tapaan, mutta joudut ensin asentamaan sovelluksen riippuvuudet:

```bash
npm install
npm start
```

Sovelluksen lopullisen version tulisi näyttää seuraavalta:

![]({{ "/assets/teht/36.png" | absolute_url }})

#### 5.19 anekdootit, osa 1

Toteuta mahdollisuus anekdoottien äänestämiseen. Äänien määrä tulee tallettaa redux-storeen.

#### 5.20* anekdootit, osa 2

Huolehdi siitä, että anekdootit pysyvät äänten mukaisessa suuruusjärjetyksessä.

#### 5.21* anekdootit, osa 3

Tee sovellukseen mahdollisuus uusien anekdoottien lisäämiselle.


## Osa 6

Deadline 11.3. klo 23:59

Osassa on 23 tehtävää, joista **mikään ei ole pakollinen**. Palautuksen tekemisen jälkeen et voi enää palauttaa osan tehtäviä.

<div class='important'>

<p>Ennen kun teet tehtäviä, on enemmän kuin suositeltavaa, että käyt huolellisesti läpi <a href='https://fullstack-hy.github.io/osa6/'>osan 6 materiaalin</a>. Tehtävien tekeminen ilman materiaalin lukemista tapahtuu täysin omalla vastuulla.</p>

</div>

Seuraavissa tehtävissä parannellaan edellisen osan anekdoottisovellusta. Ota ratkaisusi pohjaksi repositoriossa <https://github.com/FullStack-HY/redux-anecdotes-v2> oleva koodi.

Jos kloonaat projektin olemassaolevan git-reposition sisälle, *poista kloonatun sovelluksen git-konfiguraatio*

```bash
cd redux-anecdotes-v2   // eli mene ensin kloonatun repositorion hakemistoon
rm -rf .git
```

Sovellus käynnistyy normaaliin tapaan, mutta joudut ensin asentamaan sovelluksen riippuvuudet:

```bash
npm install
npm start
```

### yhdistetyt reducerit

#### 6.1 ESlint

Ota projektiin käyttöön ESlint. Määrittele haluamasi kaltainen konfiguraatio.

#### 6.2 paremmat anekdootit, osa 1

Sovelluksen komponenteissa viitataan suoraan actioneihin:

```react
class AnecdoteForm extends React.Component {
  handleSubmit = (e) => {
    e.preventDefault()
    const content = e.target.anecdote.value
    this.props.store.dispatch({
      type: 'CREATE',
      content
    })

  }
  // ...
}
```

Tämä ei ole hyvä tapa. Eriytä action-olioiden luominen [action creator](https://redux.js.org/docs/basics/Actions.html#action-creators) -funktioihin. Sijoita creatorit tiedostoon _src/reducers/anecdoteReducer.js_.

#### 6.3 paremmat anekdootit, osa 2

Sovelluksessa on valmiina komponentin _Notification_ runko:

```react
class Notification extends React.Component {
  render() {
    const style = {
      border: 'solid',
      padding: 10,
      borderWidth: 1
    }
    return (
      <div style={style}>
        render here notification...
      </div>
    )
  }
}
```

Laajenna komponenttia siten, että se renderöi redux-storeen talletetun viestin, eli renderöitävä komponentti muuttuu muodoon:

```react
return (
  <div style={style}>
    {this.props.store.getState()...}
  </div>
)
```

Joudut siis muuttamaan/laajentamaan sovelluksen olemassaolevaa reduceria. Tee toiminnallisuutta varten oma reduceri ja siirry käyttämään sovelluksessa yhdistettyä reduceria osan 6 materiaalin tapaan.

Tässä vaiheessa sovelluksen ei vielä tarvitse osata käyttää _Notification_ komponenttia järkevällä tavalla, riittää että sovellus toimii ja näyttää _notificationReducerin_ alkuarvoksi asettaman viestin.

#### 6.4 paremmat anekdootit, osa 3

Laajenna sovellusta siten, että se näyttää _Notification_-komponentin avulla viiden sekunnin ajan kun sovelluksessa äänestetään tai luodaan uusia anekdootteja:

![]({{ "/assets/teht/37.png" | absolute_url }})

Notifikaation asettamista ja poistamista varten kannattaa kannattaa toteuttaa [action creatorit](https://redux.js.org/docs/basics/Actions.html#action-creators).

#### 6.5 paremmat anekdootit, osa 4

Toteuta sovellukseen näytettävien muistiinpanojen filtteröiminen

![]({{ "/assets/teht/38.png" | absolute_url }})

Säilytä filtterin tila redux storessa, eli käytännössä kannattaa jälleen luoda uusi reduceri ja action creatorit.

Tee filtterin ruudulla näyttämistä varten komponentti _Filter_. Voit ottaa sen pohjaksi seuraavan

```bash
class Filter extends React.Component {
  handleChange = (event) => {
    // input-kentän arvo muuttujassa event.target.value
  }
  render() {
    const style = {
      marginBottom: 10
    }

    return (
      <div style={style}>
        filter <input onChange={this.handleChange}/>
      </div>
    )
  }
}
```

### connect

#### 6.6 paremmat anekdootit, osa 5

Sovelluksessa välitetään _redux store_ tällä hetkellä kaikille komponenteille propseina.

Ota käyttöön kirjasto [react-redux](https://github.com/reactjs/react-redux) ja muuta komponenttia _Notification_ niin, että se pääsee käsiksi tilaan _connect_-funktion välityksellä.

Huomaa, että toimiakseen _connect_ edellyttää että sovelukselle on määriteltävä [Provider](https://github.com/reactjs/react-redux/blob/master/docs/api.md#provider-store).


#### 6.7 paremmat anekdootit, osa 6

Tee sama komponentille _Filter_ ja _AnecdoteForm_.

#### 6.8 paremmat anekdootit, osa 7

Muuta myös _AnecdoteList_ käyttämään connectia.

Poista turhaksi staten propseina tapahtuva välittäminen, eli pelkistä _App_ muotoon:

```react
class App extends React.Component {
  render() {
    return (
      <div>
        <h1>Programming anecdotes</h1>
        <Notification />
        <AnecdoteForm />
        <AnecdoteList />
      </div>
    )
  }
}
```

#### 6.9 paremmat anekdootit, osa 8

Välitä komponentille _AnecdoteList_ connectin avulla ainoastaan yksi stateen liittyvä propsi, filtterin tilan perusteella näytettävät anekdootit samaan tapaan kuin materiaalin luvussa [Presentational/Container revisited](osa6/#Presentational/Container-revisited).

Komponentin _AnecdoteList_ metodi _render_ siis typistyy suunnilleen seuraavaan muotoon

```react
class AnecdoteList extends React.Component {
  // ...
  render() {
    return (
      <div>
        <h2>Anecdotes</h2>
        <Filter />
        {this.props.anecdotesToShow.map(anecdote =>
          <div key={anecdote.id}>
            ...
          </div>
        )}
      </div>
    )
  )
}
```

### redux ja backend

#### 6.10 anekdootit ja backend, osa 1

Hae sovelluksen käynnistyessä anekdootit json-serverillä toteutetusta backendistä.

Backendin alustavan sisällön saat esim. [täältä](https://github.com/FullStack-HY/redux-anecdotes-v2/wiki).

#### 6.11 anekdootit ja backend, osa 2

Muuta uusien anekdoottien luomista siten, että anekdootit talletetaan backendiin.

#### 6.12 anekdootit ja backend, osa 3

Muuta myös äänestäminen siten, että anekdootit talletetaan backendiin. Jos teet talletuksen HTTP PUT -operaatiolla, niin muista että joudut korvaamaan tallettaessa koko olion.

### thunk

#### 6.13 anekdootit ja backend, osa 4

Muuta redux-storen alustus tapahtumaan _redux-thunk_-kirjaston avulla toteutettuun asynkroniseen actioniin.

#### 6.14 anekdootit ja backend, osa 5

Muuta myös uuden anekdootin luominen ja äänestäminen tapahtumaan _redux-thunk_-kirjaston avulla toteutettuihin asynkronisiin actioneihin.

#### 6.15 anekdootit ja backend, osa 6

Notifikaatioiden tekeminen on nyt hieman ikävää, sillä se edellyttää kahden actionin tekemistä ja _setTimeout_-funktion käyttöä:

```js
this.props.notifyWith(`you voted '${anecdote.content}'`)
setTimeout(() => {
  this.props.clearNotification()
}, 10000)
```

Tee asynkrooninen action creator, joka mahdollistaa notifikaation antamisen seuraavasti:

```js
this.props.notify(`you voted '${anecdote.content}'`, 10)
```

eli ensimmäisenä parametrina on renderöitävä teksti ja toisena notifikaation näyttöaika sekunneissa.

Ota paranneltu notifikaatiotapa käyttöön sovelluksessasi.

### router

Jatketaan anekdoottien parissa. Ota seuraaviin tehtäviin pohjaksi repositoriossa <https://github.com/mluukkai/routed-anecdotes> oleva reduxiton anekdoottisovellus.

Jos kloonaat projektin olemassaolevan git-reposition sisälle, *poista kloonatun sovelluksen git-konfiguraatio*

```bash
cd routed-anecdotes   // eli mene ensin kloonatun repositorion hakemistoon
rm -rf .git
```

Sovellus käynnistyy normaaliin tapaan, mutta joudut ensin asentamaan sovelluksen riippuvuudet:

```bash
npm install
npm start
```

#### 6.16 routed anecdotes, osa 1

Lisää sovellukseen React Router siten, että _Menu_-komponentissa olevia linkkejä klikkailemalla saadaan säädeltyä näytettävää näkymää.

Sovelluksen juuressa, eli polulla _/_ näytetään anekdoottien lista:

![]({{ "/assets/teht/40.png" | absolute_url }})

Pohjalla oleva _Footer_-komponentti tulee näyttää aina.

Uuden anekdootin luominen tapahtuu esim. polulla _create_:

![]({{ "/assets/teht/41.png" | absolute_url }})

Huom: jos saat seuraavan virheilmoituksen

![]({{ "/assets/teht/39.png" | absolute_url }})

pääset siitä eroon sisällyttämällä kaiken Router-elementin sisälle tulevaan _div_-elementtiin:

```bash
<Router>
  <div>
    ...
  </div>
</Router>
```

#### 6.17 routed anecdotes, osa 2

Toteuta sovellukseen yksittäisen anekdootin tiedot näyttävä näkymä:

![]({{ "/assets/teht/42.png" | absolute_url }})

Yksittäisen anekdootin sivulle navigoidaan klikkaamalla anekdootin nimeä

![]({{ "/assets/teht/43.png" | absolute_url }})

#### 6.18 routed anecdotes, osa 3

Luomislomakkeen oletusarvoinen toiminnallisuus on melko hämmentävä, sillä kun lomakkeen avulla luodaan uusi muistiinpano, mitään ei näytä tapahtuvan.

Paranna toiminnallisuutta siten, että luomisen jälkeen siirrytään automaattisesti kaikkien anekdoottien näkymään _ja_ käyttäjälle näytetään 10 sekunnin ajan onnistuneesta lisäyksestä kertova notifikaatio:

![]({{ "/assets/teht/44.png" | absolute_url }})

### inline-tyylit

Parannellaan edellisen tehtäväsarjan ulkoasua inlinetyylien avulla.

#### 6.19 styled anecdotes, osa 1

Tee notifikaatioista tyylikkäämpi:

![]({{ "/assets/teht/45.png" | absolute_url }})

Notifikaatiosi ei tarvitse näyttää samanlaiselta, tyyli on vapaa.

Googlaile tarvittaessa apua. Hyödyllisiä avainsanoja ovat ainakin _border_, _margin_ ja _padding_. [w3schoolsin](https://www.w3schools.com/css/default.asp) sivulta löytyy paljon esimerkkejä tyyleihin liittyen.

#### 6.20 styled anecdotes, osa 2

Paranna menun ulkoasua esim. seuraavasti

![]({{ "/assets/teht/46.png" | absolute_url }})

Kuten edellisessä tehtävässä, nytkin tyyli on vapaa.

Jos haluat erotella aktiivisena olevan sivun linkin tyylin menussa, kannattaa vaihtaa käytössä oleva komponentti [Link](https://github.com/ReactTraining/react-router/blob/master/packages/react-router-dom/docs/api/Link.md) sen edistyksellisempään versioon, eli komponenttiin [NavLink](https://github.com/ReactTraining/react-router/blob/master/packages/react-router-dom/docs/api/NavLink.md).

NavLink toimii Link-komponentin tavoin mutta sisältää muutamia käteviä lisäominaisuuksia kuten attribuutin [activeStyle](https://github.com/ReactTraining/react-router/blob/master/packages/react-router-dom/docs/api/NavLink.md#activestyle-object), jonka kanssa useimmiten käytetään attribuuttia [exact](https://github.com/ReactTraining/react-router/blob/master/packages/react-router-dom/docs/api/NavLink.md#exact-bool).

### UI-framework

Viimeistele anekdoottisovellus lisäämällä siihen tyylejä Bootstrapin tai jonkun muun UI-frameworkin avulla.

#### 6.21 styled anecdotes, osa 3

Ota käyttöön bootstrap (tai valitsemasi framework) ja renderöi anekdoottien lista tyylikkäämmin, esim. bootstrapissa [ListGroup](https://react-bootstrap.github.io/components/list-group/)-komponentin tai Semanticissa [Tablen](https://react.semantic-ui.com/collections/table) avulla:

![]({{ "/images/teht/47b.png" | absolute_url }})

#### 6.22 styled anecdotes, osa 4

Tutustu [bootstrapin](https://react-bootstrap.github.io/layout/grid/) tai [semanticin](https://react.semantic-ui.com/collections/grid) grideihin ja muuta niiden avulla sovelluksen _about_-sivua siten, että oikeassa reunassa näytetään jonkun kuuluisan tietojenkäsittelijän kuva:

![]({{ "/assets/teht/48.png" | absolute_url }})

#### 6.23 styled anecdotes, osa 5

Lisää vielä vapaavalintaisia tyylejä valitsemallasi UI frameworkilla. Voit merkata tehtävän, jos käytät aikaa vapaavalitaisten tyylien lisäämiseen noin 30 minuuttia.

## osa 7

Deadline 11.3. klo 23:59

Osassa on 23 tehtävää, arvostelussa tehtävien maksimiin tässä osassa lasketaan kuitenkin ainoastaan 15.

Tämän osan tehtävissä jatketaan osissa 4 ja 5 tehtyä Bloglist-sovellusta. Suurin osa tämän osan tehtävistä on toisistaan riippumattomia "featureita", eli tehtäviä ei tarvitse tehdä järjestyksessä, voit jättää osan aivan hyvin toteuttamatta.

Voit otaa pohjaksi oman sovelluksesi sijaan myös mallivastauksen koodin.

Useimmat tämän osan tehtävistä vaativat olemassaolevan koodin refaktoroimista. Tämä on tilanne käytännössä aina sovelluksia laajennettaessa, eli vaikka refaktorointi voi olla hankalaa ja ikävääkin, on kyseessä oleellinen taito.

Hyvä neuvo refaktorintiin niinkuin uudenkin koodin kirjoittamiseen on _pienissä askelissa eteneminen_, koodia ei kannata hajottaa totaalisesti refaktorointia tehdessä pitkäksi aikaa, se on käytännössä varma resepti hermojen menettämiseen.

<div class="important">
  Jos aiot tehdä tehtävät 7.8-7.11 eli siirtää sovelluksen tilanhallinnan reduxin vastuulle, saattaa olla helpompi tehdä reduxiin siirtymiseen vaadittava refaktorointi ennen muiden tehtävien tekemistä.
</div>

### 7.1 käyttäjien näkymä

Tee sovellukseen näkymä, joka näyttää kaikkin käyttäjiin liittyvät perustietot:

![]({{ "/assets/teht/53.png" | absolute_url }})

### 7.2 yksittäisen käyttäjän näkymä, osa 1

Tee sovellukseen yksittäisen käyttäjän näkymä, jolta selviää mm. käyttäjän lisäämät blogit

![]({{ "/assets/teht/54.png" | absolute_url }})

Näkymään päästään klikkaamalla nimeä kaikkien käyttäjien näkymästä

![]({{ "/assets/teht/55.png" | absolute_url }})

### 7.3 yksittäisen käyttäjän näkymä osa, 2

Merkkaa tämä tehtävä tehdyksi jos toteuttamasi yksittäisen käyttäjän näkymä toimii oikein myös siinä tilanteessa että menet urliin suoraan tai refreshaat selaimen ollessasi käyttäjän näkymässä.

### 7.4 blogin näkymä

Toteuta sovellukseen oma näkymä yksittäisille blogeille. Näkymä voi näyttää seuraavalta

![]({{ "/assets/teht/49.png" | absolute_url }})

Näkymään päästään klikkaamalla blogin nimeä kaikkien blogien näkymästä

![]({{ "/assets/teht/50.png" | absolute_url }})

Tämän tehtävän jälkeen tehtävässä 5.6 toteutettua toiminnallisuutta ei enää tarvita, eli kaikkien blogien näkymässä yksittäisten blogien detaljien ei enää tarvitse avautua klikatessa.

### 7.5 navigointi

Tee sovellukseen navigaatiomenu

![]({{ "/assets/teht/56.png" | absolute_url }})

### 7.6 kommentit, osa 1

Tee sovellukseen mahdollisuus blogien kommentointiin:

![]({{ "/assets/teht/51.png" | absolute_url }})

Kommentit ovat anonyymejä, eli ne eivät liity järjestelmän käyttäjiin.

Tässä tehtävässä riittää, että frontend osaa näyttää blogilla olevat backendin kautta lisätyt kommentit.

Sopiva rajapinta kommentin luomiseen on osoitteeseen _api/blogs/:id/comments_ tapahtuva HTTP POST -pyyntö.

### 7.7 kommentit, osa 2

Laajenna sovellusta siten, että kommentointi onnistuu frontendista käsin:

![]({{ "/assets/teht/52.png" | absolute_url }})

### 7.8 redux, osa 1

Siirry käyttämään React-komponenttien tilan eli _staten_ sijaan Reduxia.

Muuta tässä tehtävässä notifikaatio käyttämään Reduxia.

### 7.9 redux, osa 2

Siirrä kaikkien käyttäjien tietojen talletus Reduxiin. Varmista, että sekä kaikkien käyttäjien että yksittäisen käyttäjän näkymät toimivat edelleen.

Tässä tehtävässä saattaa olla hyödyksi käyttää metodin _connect_ toista parametria
[ownPropsia](https://github.com/reactjs/react-redux/blob/master/docs/api.md#inject-todos-of-a-specific-user-depending-on-props) joka on dokumentaation hienoisesta kryptisyydestä huolimatta [aika simppeli](https://stackoverflow.com/questions/41198842/what-is-the-use-of-the-ownprops-arg-in-mapstatetoprops-and-mapdispatchtoprops) asia.

### 7.10 redux, osa 4

Siirrä myös blogien tietojen talletus Reduxiin.

Uuden blogin luomislomakkeen tilaa voit halutessasi hallita edelleen reactin tilan avulla.

Tämä ja seuraava osa ovat kohtuullisen työläitä, mutta erittäin opettavaisia.

### 7.11 redux, osa 3

Siirrä myös kirjautuneen käyttäjän tietojen talletus Reduxiin.

### 7.12 tyylit, osa 1

Tee sovelluksesi ulkoasusta tyylikkäämpi jotain kurssilla esiteltyä tapaa käyttäen

### 7.13 tyylit, osa 2

Jos käytät tyylien lisäämiseen yli tunnin aikaa, merkkaa myös tämä tehtävä tehdyksi.

### 7.14 ESLint

Konfiguroi frontend käyttämään Lintiä

### 7.15 Webpack

Tee sovellukselle sopiva webpack-konfiguraatio

### 7.16 backendin testaus

Tee backendille lisää testejä. Voit merkitä rastin kun olet käyttänyt testien tekemiseen noin 45 minuuttia.

### 7.17 frontendin testaus

Tee frontendille tai reducereille testejä. Voit merkitä rastin kun olet käyttänyt testien tekemiseen noin 45 minuuttia.

### 7.18 snapshot-testaus

Ota sovellukseessasi käyttöön [snapshot testing](https://facebook.github.io/jest/docs/en/snapshot-testing.html)

### 7.19 headless-testaus

Tee Puppeteeria tai haluamaasi kirjastoa käyttäviä headless-testejä, testaa ainakin paria toiminnallisuutta.

### 7.20 Tyypitarkastuksia

Lisää sovellukseen tyyppitarkastuksia Proptypeinä, Flown avulla tai Typescriptillä

### 7.21 Internet

Deployaa sovellus internetiin

### 7.22 Jatkuva tuotantoonvienti

Toteuta sovelluksellesi esim. [Travis CI](https://travis-ci.org/):n avulla jatkuva tuotantoonvienti, eli mekanismi, missä koodin pushaaminen githubiin aiheuttaa testien läpimennessä uuden version käynnistämisen internettiin.

### 7.23 Kurssipalaute

Anna kurssille palautetta osoitteessa <https://ilmo.cs.helsinki.fi/kurssit/servlet/Valinta>
