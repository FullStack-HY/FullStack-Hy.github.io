---
layout: page
title: tehtävät
inheader: yes
permalink: /tehtävät/
---

# tehtävät

* [osa0](#osa-0) deadline 28.1. klo 23:59
* [osa1](#osa-1) deadline 28.1. klo 23:59
* [osa2](#osa-2) deadline 4.2. klo 23:59
* [osa3](#osa-3) deadline 11.2. klo 23:59

## Palauttaminen

Olethan lukenut huolellisesti kurssimateriaalin osan 0 luvun [Suoritustapa](/osa0/#Suoritustapa)?

Tehtävät palautetaan GitHubin kautta ja merkitsemällä tehdyt tehtävät maanantaina 15.1. avautuvaan [palautussovellukseen](https://studies.cs.helsinki.fi/fs-stats/).

Jos palautat eri osien tehtäviä samaan repositorioon, käytä järkevää hakemistojen nimentää.

Tehtävät palautetaan yksi osa kerrallaan. Kun olet palauttanut osan tehtävät, et voi enää palauttaa saman osan tekemättä jättämiäsi tehtäviä.

GitHubiin palautettuja tehtäviä tarkastetaan pistokokein. Jos GitHubista löytyy kurssin mallivastausten koodia tai useammalta opiskelijalta löytyy samaa koodia, käsitellään tilanne yliopiston [vilppikäytäntöjen](http://blogs.helsinki.fi/alakopsaa/opettajalle/epailen-opiskelijaa-vilpista-mita-tehda/) mukaan.

Suurin osa tehtävistä on moniosaisia, samaa ohjelmaa pala palalta rakentavia kokonaisuuksia. Tälläisissä tehtäväsarjoissa ohjelman lopullisen version palauttaminen riittää, voit toki halutessasi tehdä commitin jokaisen tehtävän jälkeisestä tilanteesta, mutta se ei ole välttämätöntä.

## Osa 0

Deadline 28.1. klo 23:59

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

Tee kaavio tilanteesta, missä käyttäjä menee selaimella osoitteeseen <https://fullstack-exampleapp.herokuapp.com/spa> eli muistiinpanojen [single page app](../osa1/#single-page-app)-versioon

#### 0.6 Uusi muistiinpano SPA:ssa

Tee kaavio tilanteesta, missä käyttäjä luo uuden muistiinpanon single page -versiossa.

### Tehtävien palautus

Palauta tehtävät [palautussovellukseen](https://studies.cs.helsinki.fi/fs-stats/).

## Osa 1

Deadline 28.1. klo 23:59

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

Monien firmojen tapaan nykyään myös [Unicafe](https://www.unicafe.fi/#/9/4) kerää asiakaspalautetta. Tee Unicafelle verkossa toimiva palautesovellus. Vastausvaihtoehtoja olkoon vain kolme: _hyvä_, neutraali ja _huono_.

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

Myös ns. [Computed property names](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer) voi olla tässä tehtävässä tehtävässä hyödyksi.

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

#### 1.14* anekdootit osa3

Ja sitten vielä lopullinen versio, joka näyttää eniten ääniä saaneen anekdootin:

![]({{ "/images/teht/3b.png" | absolute_url }})

Tämä saattaa olla jo hieman haastavampi. Taulukolta löytyy monia hyviä metodeja, katso lisää [Mozillan dokumentaatiosta](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array).

Youtubessa on kohtuullisen hyvä [johdatus funktionaaliseen javascript-ohjelmointiin](https://www.youtube.com/watch?v=BMUiFMZr7vk&list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84). Kolmen ensimmäisen osan katsominen riittää hyvin tässä vaiheessa.

### Tehtävien palautus

Palauta tehtävät [palautussovellukseen](https://studies.cs.helsinki.fi/fs-stats/).


## Osa 2

deadline 4.2. klo 23:59

Osassa on 22 tehtävää, joista pakollisia on 13. Voit edetä osaan 3 kun olet tehnyt kaikki pakolliset tehtävät. Palautuksen tekemisen jälkeen et voi enää palauttaa osan tehtäviä.

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

Määrittele sovellukseen yksittäisen muotoilusta huolehtiva komponentti _Kurssi_.

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

Joskus tilan muuttujia ja tarvittaessa muitakin voi olla hyödyllistä renderöidä debugatessa komponenttiin, eli voi lisätä tilapäisesti lisätä komponentin metodin _render_ palauttamaan koodiin esim. seuraavan:

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

Käynnistä json-server porttiin 3001 ja varmista selaimella osoitteesta <http://localhost:3001>, että palvelin palauttaa henkilölistan.

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

Tee sovellus, jonka avulla toit tarkastella eri maiden tietoja. Sovelluksen kannattaa hakea tiedot endpointista [all](https://restcountries.eu/#api-endpoints-all).

Sovelluksen käyttöliittymä on yksinkertainen. Näytettävä maa haetaan kirjoittamalla hakuehto etsintäkenttään.

Jos ehdon täyttäviä maita on liikaa (yli 10), kehoitetaan tarkentamaan hakuehtoa

![]({{ "/assets/teht/13.png" | absolute_url }})

Jos maita on alle kymmenen, mutta yli 1 näytetään hakuehdon täyttävät maat

![]({{ "/assets/teht/14.png" | absolute_url }})


Kun ehdon täyttäviä maita on enää yksi, näytetään maan lippu sekä perustiedot:

![]({{ "/assets/teht/15.png" | absolute_url }})

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

Siirrä palvelimen kanssa kommunikoinnista vastaava toiminnallisuus omaan moduuliin osan 2 [esimerkin](#palvelimen-kanssa-tapahtuvan-komunikoinnin-eristäminen-omaan-moduuliin) tapaan.

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

Toteuta osan 2 esimerkin [parempi virheilmoitus](osa2/#parempi-virheilmoitus) tyyliin ruudulla muutaman sekunnin näkyvä ilmoitus, joka kertoo onnistuneista operaatioista (henkilön lisäys ja poisto, sekä numeron muutos):

![]({{ "/assets/teht/17.png" | absolute_url }})


#### 2.19* puhelinluettelo osa 12

Jos poistat jonkun henkilön toisesta selaimesta hieman ennen kun yrität _muuttaa henkilön numeroa_ toisesta selaimesta, tapahtuu virhetilanne:

![]({{ "/assets/teht/18.png" | absolute_url }})


Korjaa ongelma osan 2 esimerkin [promise ja virheet](osa2/#promise-ja-virheet) tapaan. Loogisin korjaus lienee henkilön lisääminen uudelleen palvelimelle. Toinen vaihtoehto on ilmottaa käyttäjälle, että muutettavaksi yritetävän henkilön tiedot on jo poistettu.

### Tehtävien palautus

Palauta tehtävät [palautussovellukseen](https://studies.cs.helsinki.fi/fs-stats/).

## Osa 3

Deadline 11.2. klo 23:59

Osassa on 22 tehtävää, joista pakollisia on 15. Voit edetä osaan 4 kun olet tehnyt kaikki pakolliset tehtävät. Palautuksen tekemisen jälkeen et voi enää palauttaa osan tehtäviä.

Tämän osan tehtävissä teemme backendin edellisen osan puhelinluettelosovellukseen. Lopullisen version palauttaminen riittää, voit toki halutessasi tehdä commitin jokaisen tehtävän jälkeisestä tilanteesta, mutta se ei ole välttämätöntä.

**HUOM tämän osan tehtäväsarja kannattaa tehdä omaan git-repositorioon, suoraan repositorion juureen! Jos et tee näin, joudut ongelmiin tehtävässä 3.10**.

<div class='important'>

<p>Ennen kun teet tehtäviä, on enemmän kuin suositeltavaa, että käyt huolellisesti läpi <a href='https://fullstack-hy.github.io/osa3/'>osan 3 materiaalin</a>. Tehtävien tekeminen ilman materiaalin lukemista tapahtuu täysin omalla vastuulla.</p>

</div>

### Expressin alkeet

#### 3.1 puhelinluettelon backend osa 1

Vielä uusi **HUOMAUTUS:** tämän osan tehtäväsarja kannattaa tehdä omaan git-repositorioon, suoraan repositorion juureen! Jos et tee näin, joudut ongelmiin tehtävässä 3.10

**Vahva suositus:** kun teet backendin koodia, pidä koko ajan silmällä mitä palvelimen koodia suorittavassa konsolissa tapahtuu.

Tee Node-sovellus, joka tarjoaa osoitteessa <http://localhost:3001/api/persons> kovakoodatun taulukon puhelinnumerotietoja:

![]({{ "/assets/teht/19.png" | absolute_url }})

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

Tee uuden numeron lisäykseen virheiden käsittely, pyyntö ei saa onnistua, jos
- jos nimi tai numero puuttuu
- lisättävälle nimelle on jo numero luettelossa

Vastaa asiaankuuluvalla statuskoodilla, liitä vastaukseen mukaan myös tieto, joka kertoo virheen syyn, esim:

```js
{ error: 'name must be unique' }
```

### lisää middlewareja

#### 3.7 puhelinluettelon backend osa 7

Lisää sovellukseesi loggausta tekevä middleware [morgan](https://github.com/expressjs/morgan). Konfiguroi se logaamaan konsoliin _tiny_-konfiguraation mukaisesti.

Morganin ohjeet eivät ole ehkä kaikkein selvimmät ja joudut kenties miettimään hiukan. Toisaalta juuri koskaan dokumentaatio ei ole aivan itsestäänselvää, joten kryptisempiäkin asioita on hyvä oppia tulkitsemaan.

Morgan asetaan kuten muutkin kirjastot, eli komennolla _npm install_ ja sen käyttöönotto tapahtuu kaikkien middlewarejen tapaan komennolla _app.use_

#### 3.8* puhelinluettelon backend osa 8

Konfiguroi morgania siten, että se näyttää myös HTTP-pyyntöjen mukana tulevan datan:

![]({{ "/assets/teht/21.png" | absolute_url }})

Tämä tehtävä on kohtuullisen haastava vaikka koodia ei tarvitakkaan paljoa.

Pari vihjettä:
- [creating new tokens](https://github.com/expressjs/morgan#creating-new-tokens)
- [JSON.stringify](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)

### yhteys frontendiin ja vienti tuotantoon

#### 3.9 puhelinluettelon backend osa 9

Laita backend toimimaan edellisessä osassa tehdyn puhelinluettelon frontendin kanssa.

Joudut tekemään erinäisiä pieniä muutoksia. Muista pitää selaimen konsoli koko ajan auki. Jos jotkut HTTP-pyynnöt epäonnistuvat, kannattaa katsoa _Network_-välilehdeltä mitä tapahtuu. Pidä myös silmällä mitä palvelimen konsolissa tapahtuu. Jos et tehnyt edellistä tehtävää, kannattaa POST-pyyntöä käsittelevässä tapahtumankäsittelijässä tulostaa konsoliin mukana tuleva data eli _request.body_.

#### 3.10 puhelinluettelon backend osa 10

Vie sovelluksen backend internetiin, esim. Herokuun.

Testaa selaimen ja postmanin tai VS Code REST clientin avulla, että internetissä oleva backend toimii.

**PRO TIP:** kun deployaat sovelluksen herokuun, kannattaa ainakin alkuvaiheissa pitää **KOKO AJAN** näkyvillä herokussa olevan sovelluksen loki antamalla komento <code>heroku logs -t</code>:

![]({{ "/assets/teht/22.png" | absolute_url }})

Tee repositorion juureen tiedosto README.md ja lisää siihen linkki internetissä olevaan sovellukseesi.

#### 3.11 puhelinluettelo full stack

Generoi frontendistä tuotantoversio ja lisää se internetissä olevaan sovellukseesi osan 3 [tapaa noudatellen](/osa3/#staattisten-tiedostojen-tarjoaminen-backendistä)

Huolehdi myös, että frontend toimii edelleen myös paikallisesti.

### mongoosen alkeet

#### 3.12 tietokanta komentoriviltä

Luo sovellukselle pilvessä oleva mongo mlabin avulla.

Tee projektihakemistoon tiedosto _mongo.js_, jonka avulla voit lisätä tietokantaan puhelinnumeroja sekä listata kaikki kannassa olevat numerot.

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
node mongo.js 'Arto Vihavainen' 040-1234556
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

### backend ja tietokanta

Seuraavat tehtävät saattavat olla melko suoraviivaisia, tosin jos frontend-koodissasi sattuu olemaan bugeja tai epäyhteensopivuutta backendin kanssa, voi seurauksena olla myös mielenkiintoisia bugeja.

#### 3.13 puhelinluettelo ja tietokanta, osa 1

Muuta backendin kaikkien puhelintietojen näyttämistä siten, että se hakee näytettävät puhelintiedot tietokannasta.

Varmista, että frontend toimii muutosten jälkeen.

Tee tässä ja seuraavissa tehtävissä mongoose-spesifinen koodi omaan moduuliin samaan tapaan kuin osan 3 luvussa [tietokantamäärittelyjen eriyttäminen omaksi moduuliksi](osa3#tietokantamäärittelyjen-eriyttäminen-omaksi-moduuliksi)

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

Refaktoroi koodiasi siten, että määrittelet formatoinnin suorittavan metodin mongoose skeeman [staattisena metodina](http://mongoosejs.com/docs/guide.html#statics), jollin voit käyttää sitä koodista seuraavasti:

```js
persons.map(Person.format)
```

muotoillessa taulukossa _persons_ olevat oliot tai yksittäsen olion _person_ muotoilussa seuraavasti:

```js
Person.format(person)
```

Tehtävän tekeminen edellyttää luovaa manuaalin lukemista. Älä juutu tähän ainakaan aluksi liian pitkäksi aikaa!

#### 3.15 puhelinluettelo ja tietokanta, osa 3

Mutta backendiä siten, että uudet numerot tallennetaan tietokantaan. Tässä vaiheessa voit olla välittämättä siitä, onko tietokannassa jo henkilöä jolla on sama nimi kuin lisättävällä.

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

Huolehdi, että backendiin voi lisätä yhdelle nimelle ainoastaan yhden numeron. Fronendin nykyisestä versiosta ei duplikaatteja voi luoda, mutta suoraan Postmanilla tai VS Coden REST clientillä se onnistuu.

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
