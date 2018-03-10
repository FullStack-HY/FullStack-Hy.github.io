---
layout: page
title: osa 5
inheader: yes
permalink: /osa5/
---

## Osan 5 oppimistavoitteet

- React
  - child
  - ref
  - PropTypes
- Frontendin testauksen alkeet
  - enzyme
  - shallow ja full DOM -rendering
- Redux
  - Flux-pattern
  - Storage, reducerit, actionit
  - reducereiden testaus, deep-freeze
- React+redux
  - Storagen välittäminen komponenteille propseilla ja kontekstissa
- Javascript
  - computed property name
  - Spread-operaatio
  - Reduxin edellyttämästä funktionaalisesta ohjelmoinnista
    - puhtaat funktiot
    - immutable

## Kirjautuminen React-sovelluksesta

Kaksi edellistä osaa keskittyivät lähinnä backendin toiminnallisuuteen. Edellisessä osassa backendiin toteutettua käyttäjänhallintaa ei ole tällä hetkellä tuettuna frontendissa millään tavalla.

Frontend näyttää tällä hetkellä olemassaolevat muistiinpanot ja antaa muuttaa niiden tilaa. Uusia muistiinpanoja ei kuitenkaan voi lisätä, sillä osan 4 muutosten myötä backend edellyttää, että lisäyksen mukana on käyttäjän identiteetin varmistava token.

Toteutetaan nyt osa käyttäjienhallinnan edellyttämästä toiminnallisuudesta frontendiin. Aloitetaan käyttäjän kirjautumisesta. Oletetaan vielä tässä osassa, että käyttäjät luodaan suoraan backendiin.

Sovelluksen yläosaan on nyt lisätty kirjautumislomake, myös uuden muistiinpanon lisäämisestä huolehtiva lomake on siirretty sivun yläosaan:

![]({{ "/assets/5/1.png" | absolute_url }})

Komponentin _App_ koodi näyttää seuraavalta:

```react
import React from 'react'
import noteService from './services/notes'

class App extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      notes: [],
      newNote: '',
      showAll: true,
      error: null,
      username: '',
      password: '',
      user: null
    }
  }

  componentDidMount() {
    noteService.getAll().then(notes =>
      this.setState({ notes })
    )
  }

  addNote = (event) => {
    event.preventDefault()
    const noteObject = {
      content: this.state.newNote,
      date: new Date(),
      important: Math.random() > 0.5
    }

    noteService
      .create(noteObject)
      .then(newNote => {
        this.setState({
          notes: this.state.notes.concat(newNote),
          newNote: ''
        })
      })
  }

  toggleImportanceOf = (id) => {
    // ...
  }

  login = (event) => {
    event.preventDefault()
    console.log('login in with', this.state.username, this.state.password)
  }

  handleNoteChange = (event) => {
    this.setState({ newNote: event.target.value })
  }

  handlePasswordChange = (event) => {
    this.setState({ password: event.target.value })
  }

  handleUsernameChange = (event) => {
    this.setState({ username: event.target.value })
  }

  toggleVisible = () => {
    this.setState({ showAll: !this.state.showAll })
  }

  render() {
    // ...

    return (
      <div>
        <h1>Muistiinpanot</h1>

        <Notification message={this.state.error} />

        <h2>Kirjaudu</h2>

        <form onSubmit={this.login}>
          <div>
            käyttäjätunnus
            <input
              type="text"
              value={this.state.username}
              onChange={this.handleUsernameChange}
            />
          </div>
          <div>
            salasana
            <input
              type="password"
              value={this.state.password}
              onChange={this.handlePasswordChange}
            />
          </div>
          <button type="submit">kirjaudu</button>
        </form>

        <h2>Luo uusi muistiinpano</h2>

        <form onSubmit={this.addNote}>
          <input
            value={this.state.newNote}
            onChange={this.handleNoteChange}
          />
          <button type="submit">tallenna</button>
        </form>

        <h2>Muistiinpanot</h2>

        // ...

      </div >
    )
  }
}

export default App
```

Sovelluksen tämänhetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part2-notes/tree/part5-1), tagissa _part5-1_.

Kirjautumislomakkeen käsittely noudattaa samaa periaatetta kuin [osassa 2](/osa2#lomakkeet). Lomakkeen kenttiä varten on lisätty komponentin tilaan kentät _username_ ja _password_. Molemmille kentille on rekisteröity muutoksenkäsittelijä (_handleUsernameChange_ ja _handlePasswordChange_), joka synkronoi kenttään tehdyt muutokset komponentin _App_ tilaan. Kirjautumislomakkeen lähettämisestä vastaava metodi _login_ ei tee vielä mitään.

Jos lomakkeella on paljon kenttiä, voi olla työlästä toteuttaa jokaiselle kentälle oma muutoksenkäsittelijä. React tarjoaakin tapoja, miten yhden muutoksenkäsittelijän avulla on mahdollista huolehtia useista syötekentistä. Jaetun käsittelijän on saatava jollain tavalla tieto minkä syötekentän muutos aiheutti tapahtuman. Eräs tapa tähän on lomakkeen syötekenttien nimeäminen.

Lisätään _input_ elementteihin nimet _name_-attribuutteina ja vaihdetaan molemmat käyttämään samaa muutoksenkäsittelijää:

```html
<form onSubmit={this.login}>
  <div>
    käyttäjätunnus
    <input
      type="text"
      name="username"
      value={this.state.username}
      onChange={this.handleLoginFieldChange}
    />
  </div>
  <div>
    salasana
    <input
      type="password"
      name="password"
      value={this.state.password}
      onChange={this.handleLoginFieldChange}
    />
  </div>
  <button type="submit">kirjaudu</button>
</form>
```

Yhteinen muutoksista huolehtiva tapahtumankäsittelijä on seuraava:

```js
handleLoginFieldChange = (event) => {
  if (event.target.name === 'password') {
    this.setState({ password: event.target.value })
  } else if (event.target.name === 'username') {
    this.setState({ username: event.target.value })
  }
}
```

Tapahtumankäsittelijän parametrina olevan tapahtumaolion _event_ kentän _target.name_ arvona on tapahtuman aiheuttaneen komponentin _name_-attribuutti, eli joko _username_ tai _password_. Koodi haarautuu nimen perusteella ja asettaa tilaan oikean kentän arvon.

Javascriptissa on ES6:n myötä uusi syntaksi [computed property name](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer), jonka avulla olion kentän voi määritellä muuttujan avulla. Esim. seuraava koodi


```js
const field = 'name'

const object = { [field] : 'Arto Hellas' }
```

määrittelee olion <code>{ name: 'Arto Hellas'}</code>

Näin saamme eliminoitua if-lauseen tapahtumankäsittelijästä ja se pelkistyy yhden rivin mittaiseksi:

```js
handleLoginFieldChange = (event) => {
  this.setState({ [event.target.name]: event.target.value })
}
```

Kirjautuminen tapahtuu tekemällä HTTP POST -pyyntö palvelimen osoitteeseen _api/login_. Eristetään pyynnön tekevä koodi omaan moduuliin, tiedostoon _services/login.js_.

Käytetään nyt promisejen sijaan _async/await_-syntaksia HTTP-pyynnön tekemiseen:

```js
import axios from 'axios'
const baseUrl = '/api/login'

const login = async (credentials) => {
  const response = await axios.post(baseUrl, credentials)
  return response.data
}

export default { login }
```

Kirjautumisen käsittelystä huolehtiva metodi voidaan toteuttaa seuraavasti:

```js
login = async (event) => {
  event.preventDefault()
  try{
    const user = await loginService.login({
      username: this.state.username,
      password: this.state.password
    })

    this.setState({ username: '', password: '', user})
  } catch(exception) {
    this.setState({
      error: 'käyttäjätunnus tai salasana virheellinen',
    })
    setTimeout(() => {
      this.setState({ error: null })
    }, 5000)
  }
}
```

Kirjautumisen onnistuessa nollataan kirjautumislomakkeen kentät _ja_ talletetaan palvelimen vastaus (joka sisältää _tokenin_ sekä kirjautuneen käyttäjän tiedot) sovelluksen tilan kenttään _user_.

Jos kirjautuminen epäonnistuu, eli metodin _loginService.login_ suoritus aiheuttaa poikkeuksen, ilmoitetaan siitä käyttäjälle.

Onnistunut kirjautuminen ei nyt näy sovelluksen käyttäjälle mitenkään. Muokataan sovellusta vielä siten, että kirjautumislomake näkyy vain _jos käyttäjä ei ole kirjautuneena_ eli _this.state.user === null_ ja uuden muistiinpanon luomislomake vain _jos käyttäjä on kirjautuneena_, eli (eli _this.state.user_ sisältää kirjautuneen käyttäjän tiedot.

Määritellään ensin komponentin _App_ metodiin render apufunktiot lomakkeiden generointia varten:

```html
const loginForm = () => (
  <div>
    <h2>Kirjaudu</h2>

    <form onSubmit={this.login}>
      <div>
        käyttäjätunnus
        <input
          type="text"
          name="username"
          value={this.state.username}
          onChange={this.handleLoginFieldChange}
        />
      </div>
      <div>
        salasana
        <input
          type="password"
          name="password"
          value={this.state.password}
          onChange={this.handleLoginFieldChange}
        />
      </div>
      <button type="submit">kirjaudu</button>
    </form>
  </div>
)

const noteForm = () => (
  <div>
    <h2>Luo uusi muistiinpano</h2>

    <form onSubmit={this.addNote}>
      <input
        value={this.state.newNote}
        onChange={this.handleNoteChange}
      />
      <button type="submit">tallenna</button>
    </form>
  </div>
)
```

ja renderöidään ne ehdollisesti komponentin _App_ render-metodissa:

```bash
class App extends React.Component {
  // ..
  return (
    <div>
      <h1>Muistiinpanot</h1>

      <Notification message={this.state.error}/>

      {this.state.user === null && loginForm()}

      {this.state.user !== null && noteForm()}


      <h2>Muistiinpanot</h2>

      // ...

    </div>
  )
}
```

Lomakkeiden ehdolliseen renderöintiin käytetään hyväksi aluksi hieman erikoiselta näyttävää, mutta Reactin yhteydessä [yleisesti käytettyä kikkaa](https://reactjs.org/docs/conditional-rendering.html#inline-if-with-logical--operator):

```js
{this.state.user === null && loginForm()}
```

Jos ensimmäinen osa evaluoituu epätodeksi eli on [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy), ei toista osaa eli lomakkeen generoivaa koodia suoriteta ollenkaan.

Voimme suoraviivaistaa edellistä vielä hieman käyttämällä [kysymysmerkkioperaattoria](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator):

```html
return (
  <div>
    <h1>Muistiinpanot</h1>

    <Notification message={this.state.error}/>

    {this.state.user === null ?
      loginForm() :
      noteForm()
    }

    <h2>Muistiinpanot</h2>

    // ...

  </div>
)
```

Eli jos _this.state.user === null_ on [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy), suoritetaan _loginForm_ ja muussa tapauksessa _noteForm_.

Tehdään vielä sellainen muutos, että jos käyttäjä on kirjautunut, renderöidään kirjautuneet käyttäjän nimi:

```html
return (
  <div>
    <h1>Muistiinpanot</h1>

    <Notification message={this.state.error}/>

    {this.state.user === null ?
      loginForm() :
      <div>
        <p>{this.state.user.name} logged in</p>
        {noteForm()}
      </div>
    }

    <h2>Muistiinpanot</h2>

    // ...

  </div>
)
```

Ratkaisu näyttää hieman rumalta, mutta jätämme sen koodiin toistaiseksi.

Sovelluksen tämänhetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part2-notes/tree/part5-2), tagissa _part5-2_. **HUOM** koodissa on parissa kohtaa käytetty vahingossa komponentin kentästä nimeä _new_note_, oikea (seuraaviin tageihin korjattu) muoto on _newNote_,

Sovelluksemme pääkomponentti _App_ on tällä hetkellä jo aivan liian laaja ja nyt tekemämme muutokset ovat ilmeinen signaali siitä, että lomakkeet olisi syytä refaktoroida omiksi komponenteikseen. Jätämme sen kuitenkin harjoitustehtäväksi.

## Muistiinpanojen luominen

Frontend on siis tallettanut onnistuneen kirjautumisen yhteydessä backendilta saamansa tokenin sovelluksen tilaan _this.state.user.token_:

![]({{ "/images/5/1b.png" | absolute_url }})

Korjataan uusien muistiinpanojen luominen siihen muotoon, mitä backend edellyttää, eli lisätään kirjautuneen käyttäjän token HTTP-pyynnön Authorization-headeriin.

_noteService_-moduuli muuttuu seuraavasti:

```js
import axios from 'axios'
const baseUrl = '/api/notes'

let token = null

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

const setToken = (newToken) => {
  token = `bearer ${newToken}`
}

const create = async (newObject) => {
  const config = {
    headers: { 'Authorization': token }
  }

  const response = await axios.post(baseUrl, newObject, config)
  return response.data
}

const update = (id, newObject) => {
  const request = axios.put(`${baseUrl}/${id}`, newObject)
  return request.then(response => response.data)
}

export default { getAll, create, update, setToken }
```

Moduulille on määritelty vain moduulin sisällä näkyvä muuttuja _token_, jolle voidaan asettaa arvo moduulin exporttaamalla funktiolla _setToken_. Async/await-syntaksiin muutettu _create_ asettaa moduulin tallessa pitämän tokenin _Authorization_-headeriin, jonka se antaa axiosille metodin _post_ kolmantena parametrina.

Kirjautumisesta huolehtivaa tapahtumankäsittelijää pitää vielä viilata sen verran, että se kutsuu metodia <code>noteService.setToken(user.token)</code> onnistuneen kirjautumisen yhteydessä:

```js
login = async (event) => {
  event.preventDefault()
  try {
    const user = await loginService.login({
      username: this.state.username,
      password: this.state.password
    })

    noteService.setToken(user.token)
    this.setState({ username: '', password: '', user})
  } catch(exception) {
    // ...
  }
}
```

Uusien muistiinpanojen luominen onnistuu taas!

## Tokenin tallettaminen selaimen local storageen

Sovelluksessamme on ikävä piirre: kun sivu uudelleenladataan, tieto käyttäjän kirjautumisesta katoaa. Tämä hidastaa melkoisesti myös sovelluskehitystä, esim. testatessamme uuden muistiinpanon luomista, joudumme joka kerta kirjautumaan järjestelmään.

Ongelma korjaantuu helposti tallettamalla kirjautumistiedot [local storageen](https://developer.mozilla.org/en-US/docs/Web/API/Storage) eli selaimessa olevaan avain-arvo- eli [key-value](https://en.wikipedia.org/wiki/Key-value_database)-periaatteella toimivaan tietokantaan.

Local storage on erittäin helppokäyttöinen. Metodilla [setItem](https://developer.mozilla.org/en-US/docs/Web/API/Storage/setItem) talletetaan tiettyä _avainta_ vastaava _arvo_, esim:

```js
window.localStorage.setItem('nimi', 'juha tauriainen')
```

tallettaa avaimen _nimi_ arvoksi toisena parametrina olevan merkkijonon.

Avaimen arvo selviää metodilla [getItem](https://developer.mozilla.org/en-US/docs/Web/API/Storage/getItem):

```js
window.localStorage.getItem('nimi')
```

ja [removeItem](https://developer.mozilla.org/en-US/docs/Web/API/Storage/removeItem) poistaa avaimen.

Storageen talletetut arvot säilyvät vaikka sivu uudelleenladattaisiin. Storage on ns. [origin](https://developer.mozilla.org/en-US/docs/Glossary/Origin)-kohtainen, eli jokaisella selaimella käytettävällä web-sovelluksella on oma storagensa.

Laajennetaan sovellusta siten, että se asettaa kirjautuneen käyttäjän tiedot local storageen.

Koska storageen talletettavat arvot ovat [merkkijonoja](https://developer.mozilla.org/en-US/docs/Web/API/DOMString), emme voi tallettaa storageen suoraan Javascript-oliota, vaan ne on muutettava ensin JSON-muotoon metodilla _JSON.stringify_. Vastaavasti kun JSON-muotoinen olio luetaan local storagesta, on se parsittava takaisin Javascript-olioksi metodilla _JSON.parse_.

Kirjautumisen yhteyteen tehtävä muutos on seuraava:

```js
login = async (event) => {
  event.preventDefault()

  try {
    const user = await loginService.login({
      username: this.state.username,
      password: this.state.password
    })

    window.localStorage.setItem('loggedNoteappUser', JSON.stringify(user))
    noteService.setToken(user.token)
    this.setState({ username: '', password: '', user})
  } catch(exception) {
    // ...
  }
}
```

Kirjautuneen käyttäjän tiedot tallentuvat nyt local storageen ja niitä voidaan tarkastella konsolista:

![]({{ "/images/5/2a.png" | absolute_url }})

Sovellusta on vielä laajennettava siten, että kun sivulle tullaan uudelleen, esim. selaimen uudelleenlataamisen yhteydessä, tulee sovelluksen tarkistaa löytyykö local storagesta tiedot kirjautuneesta käyttäjästä. Jos löytyy, asetetaan ne sovelluksen tilaan ja _noteServicelle_.

Sopiva paikka tähän on _App_-komponentin metodi [componentDidMount](https://reactjs.org/docs/react-component.html#componentDidMount) johon tutustuimme jo [osassa 2](/osa2#komponenttien-lifecycle-metodit).

Kyseessä on siis ns. lifecycle-metodi, jota React-kutsuu heti komponentin ensimmäisen renderöinnin jälkeen. Metodissa on tällä hetkellä jo muistiinpanot palvelimelta lataava koodi. Laajennetaan koodia seuraavasti

```js
componentDidMount() {
  noteService.getAll().then(notes =>
    this.setState({ notes })
  )

  const loggedUserJSON = window.localStorage.getItem('loggedNoteappUser')
  if (loggedUserJSON) {
    const user = JSON.parse(loggedUserJSON)
    this.setState({user})
    noteService.setToken(user.token)
  }
}
```

Nyt käyttäjä pysyy kirjautuneena sovellukseen ikuisesti. Sovellukseen olisikin kenties syytä lisätä _logout_-toiminnallisuus, joka poistaisi kirjautumistiedot local storagesta. Jätämme kuitenkin uloskirjautumisen harjoitustehtäväksi.

Meille riittää se, että sovelluksesta on mahdollista kirjautua ulos kirjoittamalla konsoliin

```js
window.localStorage.removeItem('loggedNoteappUser')
```

tai local storagen tilan kokonaan nollaavan komennon

```js
window.localStorage.clear()
```

Sovelluksen tämänhetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part2-notes/tree/part5-3), tagissa _part5-3_.

## Tehtäviä

Tee nyt tehtävät [5.1-5.4](/tehtävät#osa-5)

## Kirjautumislomakkeen näyttäminen vain tarvittaessa

Muutetaan sovellusta siten, että kirjautumislomaketta ei oletusarvoisesti näytetä:

![]({{ "/assets/5/3.png" | absolute_url }})

Lomake aukeaa, jos käyttäjä painaa nappia _login_:

![]({{ "/assets/5/4.png" | absolute_url }})

Napilla _cancel_ käyttäjä saa tarvittaessa suljettua lomakkeen.

Aloitetaan eristämällä kirjautumislomake omaksi komponentikseen:

```html
const LoginForm = ({ handleSubmit, handleChange, username, password }) => {
  return (
    <div>
      <h2>Kirjaudu</h2>

      <form onSubmit={handleSubmit}>
        <div>
          käyttäjätunnus
          <input
            value={username}
            onChange={handleChange}
            name="username"
          />
        </div>
        <div>
          salasana
          <input
            type="password"
            name="password"
            value={password}
            onChange={handleChange}
          />
        </div>
        <button type="submit">kirjaudu</button>
      </form>
    </div>
  )
}
```

Reactin [suosittelemaan tyyliin](https://reactjs.org/docs/lifting-state-up.html) tila ja tilaa käsittelevät funktiot on kaikki määritelty komponentin ulkopuolella ja välitetään komponentille propseina.

Huomaa, että propsit otetaan vastaan _destrukturoimalla_, eli sen sijaan että määriteltäisiin

```html
const LoginForm = (props) => {
  return (
      <form onSubmit={props.handleSubmit}>
        <div>
          käyttäjätunnus
          <input
            value={props.username}
            onChange={props.handleChange}
            name="username"
          />
        </div>
        // ...
        <button type="submit">kirjaudu</button>
      </form>
    </div>
  )
}
```

jolloin muuttujan _props_ kenttiin on viitattava muuttujan kautta esim. _props.handleSubmit_, otetaan kentät suoraan vastaan omiin muuttujiinsa.

Nopea tapa toiminnallisuuden toteuttamiseen on muuttaa komponentin _App_ käyttämä funktio _loginForm_ seuraavaan muotoon:

```react
const loginForm = () => {
  const hideWhenVisible = { display: this.state.loginVisible ? 'none' : '' }
  const showWhenVisible = { display: this.state.loginVisible ? '' : 'none' }

  return (
    <div>
      <div style={hideWhenVisible}>
        <button onClick={e => this.setState({ loginVisible: true })}>log in</button>
      </div>
      <div style={showWhenVisible}>
        <LoginForm
          visible={this.state.visible}
          username={this.state.username}
          password={this.state.password}
          handleChange={this.handleLoginFieldChange}
          handleSubmit={this.login}
        />
        <button onClick={e => this.setState({ loginVisible: false })}>cancel</button>
      </div>
    </div>
  )
}
```

Komponentin _App_ tilaan on nyt lisätty kenttä _loginVisible_ joka määrittelee sen näytetäänkö kirjautumislomake.

Näkyvyyttä säätelevää tilaa vaihdellaan kahden napin avulla, molempiin on kirjoitettu tapahtumankäsittelijän koodi suoraan:

```react
<button onClick={e => this.setState({ loginVisible: true })}>log in</button>

<button onClick={e => this.setState({ loginVisible: false })}>cancel</button>
```

Komponenttien näkyvyys on määritelty asettamalla komponentille CSS-määrittely, jossa [display](https://developer.mozilla.org/en-US/docs/Web/CSS/display)-propertyn arvoksi asetetaan _none_ jos komponentin ei haluta näkyvän:

```html
const hideWhenVisible = { display: this.state.loginVisible ? 'none' : '' }
const showWhenVisible = { display: this.state.loginVisible ? '' : 'none' }

// ...

<div style={hideWhenVisible}>
  // nappi
</div>

<div style={showWhenVisible}>
  // lomake
</div>
```

Käytössä on taas kysymysmerkkioperaattori, eli jos _this.state.visible_ on _true_, tulee napin CSS-määrittelyksi

```css
display: 'none';
```

jos _this.state.loginVisible_ on _false_, ei _display_ saa mitään (napin näkyvyyteen liittyvää) arvoa.

Hyödynsimme mahdollisuutta määritellä React-komponenteille koodin avulla [inline](https://react-cn.github.io/react/tips/inline-styles.html)-tyylejä. Palaamme asiaan tarkemmin seuraavassa osassa.

## Komponentin lapset, eli this.props.children

Kirjautumislomakkeen näkyvyyttä ympäröivän koodin voi ajatella olevan oma looginen kokonaisuutensa ja se onkin hyvä eristää pois komponentista _App_ omaksi komponentikseen.

Tavoitteena on luoda komponentti _Togglable_, jota käytetään seuraavalla tavalla:

```html
<Togglable buttonLabel="login">
  <LoginForm
    visible={this.state.visible}
    username={this.state.username}
    password={this.state.password}
    handleChange={this.handleLoginFieldChange}
    handleSubmit={this.login}
  />
</Togglable>
```

Komponentin käyttö poikkeaa aiemmin näkemistämme siinä, että käytössä on nyt avaava ja sulkeva tagi, joiden sisällä määritellään toinen komponentti eli _LoginForm_. Reactin terminologiassa _LoginForm_ on nyt komponentin _Togglable_ lapsi.

_Togglablen_ avaavan ja sulkevan tagin sisälle voi sijoittaa lapsiksi mitä tahansa React-elementtejä, esim.:

```html
<Togglable buttonLabel="paljasta">
  <p>tämä on aluksi piilossa</p>
  <p>toinen salainen rivi</p>
</Togglable>
```

Komponentin koodi on seuraavassa:

```react
class Togglable extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      visible: false
    }
  }

  toggleVisibility = () => {
    this.setState({visible: !this.state.visible})
  }

  render() {
    const hideWhenVisible = { display: this.state.visible ? 'none' : '' }
    const showWhenVisible = { display: this.state.visible ? '' : 'none' }

    return (
      <div>
        <div style={hideWhenVisible}>
          <button onClick={this.toggleVisibility}>{this.props.buttonLabel}</button>
        </div>
        <div style={showWhenVisible}>
          {this.props.children}
          <button onClick={this.toggleVisibility}>cancel</button>
        </div>
      </div>
    )
  }
}
```

Mielenkiintoista ja meille uutta on [this.props.children](https://reactjs.org/docs/glossary.html#propschildren), jonka avulla koodi viittaa komponentin lapsiin, eli avaavan ja sulkevan tagin sisällä määriteltyihin React-elementteihin.

Tällä kertaa lapset ainoastaan renderöidään komponentin oman renderöivän koodin seassa:

```html
<div style={showWhenVisible}>
  {this.props.children}
  <button onClick={this.toggleVisibility}>cancel</button>
</div>
```

Toisin kuin "normaalit" propsit, _children_ on Reactin automaattisesti määrittelemä, aina olemassa oleva propsi. Jos komponentti määritellään automaattisesti suljettavalla eli _/>_ loppuvalla tagilla, esim.

```html
<Note
  key={note.id}
  note={note}
  toggleImportance={this.toggleImportanceOf(note.id)}
/>
```

on _this.props.children_ tyhjä taulukko.

Komponentti _Togglable_ on uusiokäytettävä ja voimme käyttää sitä tekemään myös uuden muistiinpanon luomisesta huolehtivan formin vastaavalla tavalla tarpeen mukaan näytettäväksi.

Eristetään ensin muistiinpanojen luominen omaksi komponentiksi

```react
const NoteForm = ({ onSubmit, handleChange, value}) => {
  return (
    <div>
      <h2>Luo uusi muistiinpano</h2>

      <form onSubmit={onSubmit}>
        <input
          value={value}
          onChange={handleChange}
        />
        <button type="submit">tallenna</button>
      </form>
    </div>
  )
}
```

ja määritellään lomakkeen näyttävä koodi komponentin _Togglable_ sisällä

```html
<Togglable buttonLabel="new note">
  <NoteForm
    onSubmit={this.addNote}
    value={this.state.newNote}
    handleChange={this.handleNoteChange}
  />
</Togglable>
```

## ref eli viite komponenttiin

Ratkaisu on melko hyvä, haluaisimme kuitenkin parantaa sitä erään seikan osalta.

Kun uusi muistiinpano luodaan, olisi loogista jos luomislomake menisi piiloon. Nyt lomake pysyy näkyvillä. Lomakkeen piilottamiseen sisältyy kuitenkin pieni ongelma, sillä näkyvyyttä kontrolloidaan _Togglable_-komponentin tilassa olevalla muuttujalla ja komponentissa määritellyllä metodilla _toggleVisibility_. Miten pääsemme niihin käsiksi komponentin ulkopuolelta?

Koska React-komponentit ovat Javascript-olioita, on niiden metodeja mahdollista kutsua jos komponenttia vastaavaan olioon onnistutaan saamaan viite.

Eräs keino viitteen saamiseen on React-komponenttien attribuutti [ref](https://reactjs.org/docs/refs-and-the-dom.html#adding-a-ref-to-a-class-component).

Muutetaan lomakkeen renderöivää koodia seuraavasti:

```bash
<div>
  <Togglable buttonLabel="new note" ref={component => this.noteForm = component}>
    <NoteForm
      ...
    />
  </Togglable>
</div>
```

Kun komponentti _Togglable_ renderöidään, suorittaa React ref-attribuutin sisällä määritellyn funktion:

```js
component => this.noteForm = component
```

parametrin _component_ arvona on viite komponenttiin. Funktio tallettaa viitteen muuttujaan _this.noteForm_ eli _App_-komponentin kenttään _noteForm_.

Nyt mistä tahansa komponentin _App_ sisältä on mahdollista päästä käsiksi uusien muistiinpanojen luomisen sisältävään _Togglable_-komponenttiin.

Voimme nyt piilottaa lomakkeen kutsumalla _this.noteForm.toggleVisibility()_ samalla kun uuden muistiinpanon luominen tapahtuu:


```js
addNote = (e) => {
  e.preventDefault()
  this.noteForm.toggleVisibility()

  // ..
}
```

Refeille on myös [muita käyttötarkoituksia](https://reactjs.org/docs/refs-and-the-dom.html) kuin React-komponentteihin käsiksi pääseminen.

Sovelluksen tämänhetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part2-notes/tree/part5-4), tagissa _part5-4_.

### Huomio komponenteista

Kun Reactissa määritellään komponentti

```js
class Togglable extends React.Component {
  // ...
}
```

ja otetaan se käyttöön seuraavasti

```react
<div>
  <Togglable buttonLabel="1" ref={component => this.t1 = component}>
    ensimmäinen
  </Togglable>

  <Togglable buttonLabel="2" ref={component => this.t2 = component}>
    toinen
  </Togglable>

  <Togglable buttonLabel="3" ref={component => this.t3 = component}>
    kolmas
  </Togglable>
</div>
```

syntyy _kolme erillistä komponenttiolioa_, joilla on kaikilla oma tilansa:

![]({{ "/assets/5/5.png" | absolute_url }})

_ref_-attribuutin avulla on talletettu viite jokaiseen komponenttiin muuttujiin _this.t1_, _this.t2_ ja _this.t3_.

## Tehtäviä

Tee nyt tehtävät [5.5-5.10](/tehtävät#komponenttien-näyttäminen-vain-tarvittaessa)

## PropTypes

Komponentti _Togglable_ olettaa, että sille määritellään propsina _buttonLabel_ napin teksti. Jos määrittely unohtuu

```html
<Togglable>
  buttonLabel unohtui...
</Togglable>
```

Sovellus kyllä toimii, mutta selaimeen renderöityy hämäävästi nappi, jolla ei ole mitään tekstiä.

Haluaisimmekin varmistaa että jos _Togglable_-komponenttia käytetään, on propsille "pakko" antaa arvo.

Kirjaston olettamat ja edellyttämät propsit ja niiden tyypit voidaan määritellä kirjaston [prop-types](https://github.com/facebook/prop-types) avulla. Asennetaan kirjasto

```bash
npm install --save prop-types
```

_buttonLabel_ voidaan määritellä _pakolliseksi_ string-tyyppiseksi propsiksi seuraavasti

```react
import PropTypes from 'prop-types'

class Togglable extends React.Component {
  // ...
}

Togglable.propTypes = {
  buttonLabel: PropTypes.string.isRequired
}
```

Jos propsia ei määritellä, seurauksena on konsoliin tulostuva virheilmoitus

![]({{ "/assets/5/6.png" | absolute_url }})

Koodi kuitenkin toimii edelleen, eli mikään ei pakota määrittelemään propseja PropTypes-määrittelyistä huolimatta. On kuitenkin erittäin epäprofessionaalia jättää konsoliin _mitään_ punaisia tulosteita.

Määritellään Proptypet myös _LoginForm_-komponentille:

```react
import PropTypes from 'prop-types'

const LoginForm = ({ handleSubmit, handleChange, username, password }) => {
  return (
    // ...
  )
}

LoginForm.propTypes = {
  handleSubmit: PropTypes.func.isRequired,
  handleChange: PropTypes.func.isRequired,
  username: PropTypes.string.isRequired,
  password: PropTypes.string.isRequired
}
```

Funktionaalisen komponentin proptypejen määrittely tapahtuu samalla tavalla kuin luokkaperustaisten.

Jos propsin tyyppi on väärä, esim. yritetään määritellä propsiksi _handleChange_ merkkijono, seurauksena on varoitus:

![]({{ "/assets/5/7.png" | absolute_url }})

Luokkaperustaisille komponenteille PropTypet on mahdollista määritellä myös _luokkamuuttujina_, seuraavalla syntaksilla:

```react
import PropTypes from 'prop-types'

class Togglable extends React.Component {
  static propTypes = {
    buttonLabel: PropTypes.string.isRequired
  }

  // ...
}
```

Muuttujamäärittelyn edessä oleva _static_ määrittelee nyt, että _propTypes_-kenttä on nimenomaan komponentin määrittelevällä luokalla _Togglable_ eikä luokan instansseilla. Oleellisesti ottaen kyseessä on ainoastaan Javascriptin vielä standardoimattoman [ominaisuuden](https://github.com/tc39/proposal-class-fields) mahdollistava syntaktinen oikotie määritellä seuraava:

```js
Togglable.propTypes = {
  buttonLabel: PropTypes.string.isRequired
}
```

Surffatessasi internetissä saatat vielä nähdä ennen Reactin versiota 0.16 tehtyjä esimerkkejä, joissa PropTypejen käyttö ei edellytä erillistä kirjastoa. Versiosta 0.16 alkaen PropTypejä ei enää määritelty React-kirjastossa itsessään ja kirjaston _prop-types_ käyttö on pakollista.

## Tehtäviä

Tee nyt tehtävä [5.11](/tehtävät#proptypet)

## React-sovelluksen testaus

Reactilla tehtyjen frontendien testaamiseen on monia tapoja. Aloitetaan niihin tutustuminen nyt.

Testit tehdään samaan tapaan kuin edellisessä osassa eli Facebookin [Jest](https://facebook.github.io/jest/)-kirjastolla. Jest onkin valmiiksi konfiguroitu create-react-app:illa luotuihin projekteihin.

Jestin lisäksi käytetään AirBnB:n kehittämää [enzyme](https://github.com/airbnb/enzyme)-kirjastoa.

Asennetaan enzyme komennolla:

```bash
npm install --save-dev enzyme enzyme-adapter-react-16
```

Testataan aluksi muistiinpanon renderöivää komponenttia:

```html
const Note = ({ note, toggleImportance }) => {
  const label = note.important ? 'make not important' : 'make important'
  return (
    <div className="wrapper">
      <div className="content">
        {note.content}
      </div>
      <div>
        <button onClick={toggleImportance}>{label}</button>
      </div>
    </div>
  )
}
```

Testauksen helpottamiseksi komponenttiin on lisätty sisällön määrittelevälle _div_-elementille [CSS-luokka](https://reactjs.org/docs/dom-elements.html#classname) _content_.

### shallow-renderöinti

Ennen testien tekemistä, tehdään _enzymen_ konfiguraatioita varten tiedosto _src/setupTests.js_ ja sille seuraava sisältö:

```js
import { configure } from 'enzyme'
import Adapter from 'enzyme-adapter-react-16'

configure({ adapter: new Adapter() })
```

Nyt olemme valmiina testien tekemiseen.

Koska _Note_ on yksinkertainen komponentti, joka ei käytä yhtään monimutkaista alikomponenttia vaan renderöi suoraan HTML:ää, sopii sen testaamiseen hyvin enzymen [shallow](http://airbnb.io/enzyme/docs/api/shallow.html)-renderöijä.

Tehdään testi tiedoston _src/components/Note.test.js_, eli samaan hakemistoon, missä komponentti itsekin sijaitsee.

Ensimmäinen testi varmistaa, että komponentti renderöi muistiinpanon sisällön:

```html
import React from 'react'
import { shallow } from 'enzyme'
import Note from './Note'

describe.only('<Note />', () => {
  it('renders content', () => {
    const note = {
      content: 'Komponenttitestaus tapahtuu jestillä ja enzymellä',
      important: true
    }

    const noteComponent = shallow(<Note note={note} />)
    const contentDiv = noteComponent.find('.content')

    expect(contentDiv.text()).toContain(note.content)
  })
})
```

Edellisessä osassa määrittelimme testitapaukset metodin [test](https://facebook.github.io/jest/docs/en/api.html#testname-fn-timeout) avulla. Nyt käytössä oleva _it_ viittaa samaan olioon kuin _test_, eli on sama kumpaa käytät. It on tietyissä piireissä suositumpi ja käytössä mm. Enzymen dokumentaatiossa joten käytämme it-muotoa tässä osassa.

Alun konfiguroinnin jälkeen testi renderöi komponentin metodin _shallow_ avulla:

```html
const noteComponent = shallow(<Note note={note} />)
```

Normaalisti React-komponentit renderöityvät _DOM_:iin. Nyt kuitenkin renderöimme komponentteja [shallowWrapper](http://airbnb.io/enzyme/docs/api/shallow.html)-tyyppisiksi, testaukseen sopiviksi olioiksi.

ShallowWrapper-muotoon renderöidyillä React-komponenteilla on runsaasti metodeja, joiden avulla niiden sisältöä voidaan tutkia. Esimerkiksi [find](http://airbnb.io/enzyme/docs/api/ShallowWrapper/find.html) mahdollistaa komponentin sisällä olevien _elementtien_ etsimisen [enzyme-selektorien](http://airbnb.io/enzyme/docs/api/selector.html) avulla. Eräs tapa elementtien etsimiseen on [CSS-selektorien](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors) käyttö. Liitimme muisiinpanon sisällön kertovaan div-elementtiin luokan _content_, joten voimme etsiä elementin seuraavasti:

```js
const contentDiv = noteComponent.find('.content')
```

ekspektaatiossa varmistamme, että elementtiin on renderöitynyt oikea teksti, eli muistiinpanon sisältö:

```js
expect(contentDiv.text()).toContain(note.content)
```

### Testien suorittaminen

Create-react-app:issa on konfiguroitu testit oletusarvoisesti suoritettavaksi ns. watch-moodissa, eli jos suoritat testit komennolla _npm test_, jää konsoli odottamaan koodissa tapahtuvia muutoksia. Muutosten jälkeen testit suoritetaan automaattisesti ja Jest alkaa taas odottamaan uusia muutoksia koodiin.

Jos haluat ajaa testit "normaalisti", se onnistuu komennolla

```bash
CI=true npm test
```

Mikäli testejä suoritettaessa ei löydetä tiedostossa _src/setupTests.js_ tehtyä adapterin konfigurointia, auttaa seuraavan asetuksen lisääminen tiedostoon package-lock.json:

```
  "jest": {
    ...
    "setupFiles": [
      "<rootDir>/src/setupTests.js"
    ],
    ...
  }
```

### Testien sijainti

Reactissa on (ainakin) [kaksi erilaista](https://medium.com/@JeffLombardJr/organizing-tests-in-jest-17fc431ff850) konventiota testien sijoittamiseen. Sijoitimme testit ehkä vallitsevan tavan mukaan, eli samaan hakemistoon missä testattava komponentti sijaitsee.

Toinen tapa olisi sijoittaa testit "normaaliin" tapaan omaan erilliseen hakemistoon. Valitaanpa kumpi tahansa tapa, on varmaa että se on jonkun mielestä täysin väärä.

Itse en pidä siitä, että testit ja normaali koodi ovat samassa hakemistossa. Noudatamme kuitenkin nyt tätä tapaa, sillä se on oletusarvo create-react-app:illa konfiguroiduissa sovelluksissa.

### Testien debuggaaminen

Testejä tehdessä törmäämme tyypillisesti erittäin moniin ongelmiin. Näissä tilanteissa vanha kunnon _console.log_ on hyödyllinen. Voimme tulostaa _shallow_-metodin avulla renderöityjä komponentteja ja niiden sisällä olevia elementtejä metodin [debug](http://airbnb.io/enzyme/docs/api/ShallowWrapper/debug.html) avulla:

```bash
describe.only('<Note />', () => {
  it('renders content', () => {
    const note = {
      content: 'Komponenttitestaus tapahtuu jestillä ja enzymellä',
      important: true
    }

    const noteComponent = shallow(<Note note={note} />)
    console.log(noteComponent.debug())


    const contentDiv = noteComponent.find('.content')
    console.log(contentDiv.debug())

    // ...
  })
})
```

Konsoliin tulostuu komponentin generoima html:

```bash
console.log src/components/Note.test.js:16
  <div className="wrapper">
    <div className="content">
      Komponenttitestaus tapahtuu jestillä ja enzymellä
    </div>
    <div>
      <button onClick={[undefined]}>
        make not important
      </button>
    </div>
  </div>

console.log src/components/Note.test.js:20
  <div className="content">
    Komponenttitestaus tapahtuu jestillä ja enzymellä
  </div>
```

### Nappien painelu testeissä

Sisällön näyttämisen lisäksi toinen _Note_-komponenttien vastuulla oleva asia on huolehtia siitä, että painettaessa noten yhteydessä olevaa nappia, tulee propsina välitettyä tapahtumankäsittelijäfunktiota _toggleImportance_ kutsua.

Testaus onnistuu seuraavasti:

```bash
it('clicking the button calls event handler once', () => {
  const note = {
    content: 'Komponenttitestaus tapahtuu jestillä ja enzymellä',
    important: true
  }

  const mockHandler = jest.fn()

  const noteComponent = shallow(
    <Note
      note={note}
      toggleImportance={mockHandler}
    />
  )

  const button = noteComponent.find('button')
  button.simulate('click')

  expect(mockHandler.mock.calls.length).toBe(1)
})
```

Testissä on muutama mielenkiintoinen seikka. Tapahtumankäsittelijäksi annetaan Jestin avulla määritelty [mock](https://facebook.github.io/jest/docs/en/mock-functions.html)-funktio:

```js
const mockHandler = jest.fn()
```

Testi hakee renderöidystä komponentista _button_-elementin ja klikkaa sitä. Koska komponentissa on ainoastaan yksi nappi, on sen hakeminen helppoa:

```js
const button = noteComponent.find('button')
button.simulate('click')
```

Klikkaaminen tapahtuu metodin [simulate](http://airbnb.io/enzyme/docs/api/ShallowWrapper/simulate.html) avulla.

Testin ekspektaatio varmistaa, että _mock-funktiota_ on kutsuttu täsmälleen kerran:

```js
expect(mockHandler.mock.calls.length).toBe(1)
```

[Mockoliot ja -funktiot](https://en.wikipedia.org/wiki/Mock_object) ovat testauksessa yleisesti käytettyjä valekomponentteja, joiden avulla korvataan testattavien komponenttien riippuvuuksia, eli niiden tarvitsemia muita komponentteja. Mockit mahdollistavat mm. kovakoodattujen syötteiden palauttamisen sekä niiden metodikutsujen lukumäärän sekä parametrien testauksen aikaisen tarkkailun.

Esimerkissämme mock-funktio sopi tarkoitukseen erinomaisesti, sillä sen avulla on helppo varmistaa, että metodia on kutsuttu täsmälleen kerran.

### Komponentin _Togglable_ testit

Tehdään komponentille _Togglable_ muutama testi. Lisätään komponentin lapset renderöivään div-elementtiin CSS-luokka _togglableContent_:

```react
class Togglable extends React.Component {

  render() {
    const hideWhenVisible = { display: this.state.visible ? 'none' : '' }
    const showWhenVisible = { display: this.state.visible ? '' : 'none' }

    return (
      <div>
        <div style={hideWhenVisible}>
          <button onClick={this.toggleVisibility}>{this.props.buttonLabel}</button>
        </div>
        <div style={showWhenVisible} className="togglableContent">
          {this.props.children}
          <button onClick={this.toggleVisibility}>cancel</button>
        </div>
      </div>
    )
  }
}
```

Testit ovat seuraavassa

```react
import React from 'react'
import { shallow } from 'enzyme'
import Adapter from 'enzyme-adapter-react-16'
import Note from './Note'
import Togglable from './Togglable'

describe('<Togglable />', () => {
  let togglableComponent

  beforeEach(() => {
    togglableComponent = shallow(
      <Togglable buttonLabel="show...">
        <div className="testDiv" />
      </Togglable>
    )
  })

  it('renders its children', () => {
    expect(togglableComponent.contains(<div class="testDiv" />)).toEqual(true)
  })

  it('at start the children are not displayed', () => {
    const div = togglableComponent.find('.togglableContent')
    expect(div.getElement().props.style).toEqual({ display: 'none' })
  })

  it('after clicking the button, children are displayed', () => {
    const button = togglableComponent.find('button')

    button.at(0).simulate('click')
    const div = togglableComponent.find('.togglableContent')
    expect(div.getElement().props.style).toEqual({ display: '' })
  })

})
```

Ennen jokaista testiä suoritettava _beforeEach_ alustaa shallow-renderöimällä _Togglable_-komponentin muuttujaan _togglableComponent_.

Ensimmäinen testi tarkastaa, että _Togglable_ renderöi lapsikomponentin _<div class="testDiv" />_. Loput testit varmistavat, että Togglablen sisältämä lapsikomponentti on alussa näkymättömissä, eli sen sisältävään _div_-elementin liittyy tyyli _{ display: 'none' }_, ja että nappia painettaessa komponentti näkyy, eli tyyli on _{ display: '' }_. Koska Togglablessa on kaksi nappia, painallusta simuloidessa niistä pitää valita oikea, eli tällä kertaa ensimmäinen.

## Tehtäviä

Tee nyt tehtävät [5.12-14](/tehtävät#komponenttien-testaaminen)

### mount ja full DOM -renderöinti

Käyttämämme _shallow_-renderöijä on useimmista tapauksissa riittävä. Joskus tarvitsemme kuitenkin järeämmän työkalun sillä _shallow_ renderöi ainoastaan "yhden tason", eli sen komponentin, jolle metodia kutsutaan.

Jos yritämme esim. sijoittaa kaksi _Note_-komponenttia _Togglable_-komponentin sisälle ja tulostamme syntyvän _ShallowWrapper_ -olion

```bash
it('shallow renders only one level', () => {
  const note1 = {
    content: 'Komponenttitestaus tapahtuu jestillä ja enzymellä',
    important: true
  }
  const note2 = {
    content: 'shallow ei renderöi alikomponentteja',
    important: true
  }

  const togglableComponent = shallow(
    <Togglable buttonLabel="show...">
      <Note note={note1} />
      <Note note={note2} />
    </Togglable>
  )

  console.log(togglableComponent.debug())
})
```

huomaamme, että _Togglable_ komponentti on renderöitynyt, eli "muuttunut" HTML:ksi, mutta sen sisällä olevat _Note_-komponentit eivät ole HTML:ää vaan React-komponentteja.

```bash
<div>
  <div style={{...}}>
    <button onClick={[Function]}>
      show...
    </button>
  </div>
  <div style={{...}} className="togglableContent">
    <Note note={{...}} />
    <Note note={{...}} />
    <button onClick={[Function]}>
      cancel
    </button>
  </div>
</div>
```

Jos komponentille tehdään edellisten esimerkkien tapaan yksikkötestejä, _shallow_-renderöinti on useimmiten riittävä. Jos haluamme testata isompia kokonaisuuksia, eli tehdä frontendin _integraatiotestausta_, ei _shallow_-renderöinti riitä vaan on turvauduttava komponentit kokonaisuudessaan renderöivään [mount](http://airbnb.io/enzyme/docs/api/mount.html):iin.

Muutetaan testi käyttämään _shallowin_ sijaan _mountia_:

```react
import React from 'react'
import { shallow, mount } from 'enzyme'
import Note from './Note'
import Togglable from './Togglable'

it('mount renders all components', () => {
  const note1 = {
    content: 'Komponenttitestaus tapahtuu jestillä ja enzymellä',
    important: true
  }
  const note2 = {
    content: 'mount renderöi myös alikomponentit',
    important: true
  }

  const noteComponent = mount(
    <Togglable buttonLabel="show...">
      <Note note={note1} />
      <Note note={note2} />
    </Togglable>
  )

  console.log(noteComponent.debug())
})
```

Tuloksena on kokonaisuudessaan HTML:ksi renderöitynyt _Togglable_-komponentti:

```bash
<Togglable buttonLabel="show...">
  <div>
    <div style={{...}}>
      <button onClick={[Function]}>
        show...
      </button>
    </div>
    <div style={{...}} className="togglableContent">
      <Note note={{...}}>
        <div className="wrapper">
          <div className="content">
            Komponenttitestaus tapahtuu jestillä ja enzymellä
          </div>
          <div>
            <button onClick={[undefined]}>
              make not important
            </button>
          </div>
        </div>
      </Note>
      <Note note={{...}}>
        <div className="wrapper">
          <div className="content">
            mount renderöi myös alikomponentit
          </div>
          <div>
            <button onClick={[undefined]}>
              make not important
            </button>
          </div>
        </div>
      </Note>
      <button onClick={[Function]}>
        cancel
      </button>
    </div>
  </div>
</Togglable>
```

Mountin avulla renderöitäessä testi pääsee siis käsiksi periaatteessa samaan HTML-koodiin, joka todellisuudessa renderöidään selaimeen ja tämä luonnollisesti mahdollistaa huomattavasti monipuolisemman testauksen kuin _shallow_-renderöinti. Komennolla _mount_ tapahtuva renderöinti on kuitenkin hitaampaa, joten jos _shallow_ riittää, sitä kannattaa käyttää.

Huomaa, että testin käyttämä metodi [debug](http://airbnb.io/enzyme/docs/api/ReactWrapper/debug.html) ei palauta todellista HTML:ää vaan debuggaustarkoituksiin sopivan tekstuaalisen esitysmuodon komponentista. Todellisessa HTML:ssä ei mm. ole ollenkaan React-komponenttien tageja.

Jos on tarvetta tietää, mikä on testattaessa syntyvä todellinen HTML, sen saa selville metodilla [html](http://airbnb.io/enzyme/docs/api/ReactWrapper/html.html).

Jos muutamme testin viimeisen komennon muotoon

```js
console.log(noteComponent.html())
```
tulostuu todellinen HTML:

```html
<div>
  <div><button>show...</button></div>
  <div style="display: none;">
    <div class="wrapper">
      <div class="content">Komponenttitestaus tapahtuu jestillä ja enzymellä</div>
      <div><button>make not important</button></div>
    </div>
    <div class="wrapper">
      <div class="content">mount renderöi myös alikomponentit</div>
      <div><button>make not important</button></div>
    </div>
    <button>cancel</button></div>
</div>
```

Komennon _mount_ palauttamaa renderöidyn "komponenttipuun" [ReactWrapper](http://airbnb.io/enzyme/docs/api/mount.htm)-tyyppisenä oliona, joka tarjoaa hyvin samantyyppisen rajapinnan komponentin sisällön tutkimiseen kuin _ShallowWrapper_.

### Lomakkeiden testaus

Lomakkeiden testaaminen Enzymellä on jossain määrin haasteellista. Enzymen dokumentaatio ei mainitse lomakkeista sanaakaan. [Issueissa](https://github.com/airbnb/enzyme/issues/364) asiasta kuitenkin keskustellaan.

Tehdään testi komponentille _NoteForm_. Lomakkeen koodi näyttää seuraavalta

```react
const NoteForm = ({ onSubmit, handleChange, value }) => {
  return (
    <div>
      <h2>Luo uusi muistiinpano</h2>

      <form onSubmit={onSubmit}>
        <input
          value={value}
          onChange={handleChange}
        />
        <button type="submit">tallenna</button>
      </form>
    </div>
  )
}
```

Lomakkeen toimintaperiaatteena on synkronoida lomakkeen tila sen ulkopuolella olevan React-komponentin tilaan. Lomakettamme on jossain määrin vaikea testata yksistään.

Teemmekin testejä varten apukomponentin _Wrapper_, joka renderöi _NoteForm_:in ja hallitsee lomakkeen tilaa:

```react
class Wrapper extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      formInput: ''
    }
  }
  onChange = (e) => {
    this.setState({ formInput: e.target.value })
  }
  render() {
    return (
      <NoteForm
        value={this.state.formInput}
        onSubmit={this.props.onSubmit}
        handleChange={this.onChange}
      />
  )}
}
```

Testi on seuraavassa:

```react
import React from 'react'
import { mount } from 'enzyme'
import NoteForm from './NoteForm'

it('renders content', () => {
  const onSubmit = jest.fn()

  const wrapper = mount(
    <Wrapper onSubmit={onSubmit} />
  )

  const input = wrapper.find('input')
  const button = wrapper.find('button')

  input.simulate('change', { target: { value: 'lomakkeiden testaus on hankalaa' } })
  button.simulate('submit')

  expect(wrapper.state().formInput).toBe('lomakkeiden testaus on hankalaa')
  expect(onSubmit.mock.calls.length).toBe(1)
})
```

Testi luo _Wrapper_-komponentin, jolle se välittää propseina mockatun funktion _onSubmit_. Wrapper välittää funktion edelleen _NoteFormille_ tapahtuman _onSubmit_ käsittelijäksi.

Syötekenttään _input_ kirjoittamista simuloidaan tekemällä syötekenttään tapahtuma _change_ ja määrittelemällä sopiva olio, joka määrittelee syötekenttään 'kirjoitetun' sisällön.

Lomakkeen nappia tulee painaa simuloimalla tapahtumaa _submit_, tapahtuma _click_ ei lähetä lomaketta.

Testin ensimmäinen ekspektaatio tutkii komponentin _Wrapper_ tilaa metodilla [state](http://airbnb.io/enzyme/docs/api/ReactWrapper/state.html), ja varmistaa, että lomakkeelle kirjoitettu teksti on siirtynyt tilaan. Toinen ekspektaatio varmistaa, että lomakkeen lähetys on aikaansaanut tapahtumankäsittelijän kutsumisen.

## Frontendin integraatiotestaus

Suoritimme edellisessä osassa backendille integraatiotestejä, jotka testasivat backendin tarjoaman API:n läpi backendia ja tietokantaa. Backendin testauksessa tehtiin tietoinen päätös olla kirjoittamatta yksikkötestejä sillä backendin koodi on melko suoraviivaista ja ongelmat tulevatkin esiin todennäköisemmin juuri monimutkaisemmissa skenaarioissa, joita integraatiotestit testaavat hyvin.

Toistaiseksi kaikki frontendiin tekemämme testit ovat olleet yksittäisten komponenttien oikeellisuutta valvovia yksikkötestejä. Yksikkötestaus on toki tärkeää, mutta kattavinkaan yksikkötestaus ei riitä antamaan riittävää luotettavuutta sille, että järjestelmä toimii kokonaisuudessaan.

Tehdään nyt sovellukselle yksi integraatiotesti. Integraatiotestaus on huomattavasti komponenttien yksikkötestausta hankalampaa. Erityisesti sovelluksemme kohdalla ongelmia aiheuttaa kaksi seikkaa: sovellus hakee näytettävät muistiinpanot palvelimelta _ja_ sovellus käyttää local storagea kirjautuneen käyttäjän tietojen tallettamiseen.

Local storage ei ole oletusarvoiseti käytettävissä testejä suorittaessa, sillä kyseessä on selaimen tarjoama toiminnallisuus ja testit ajetaan selaimen ulkopuolella. Ongelma on helppo korjata määrittelemällä testien suorituksen ajaksi _mock_ joka matkii local storagea. Tapoja tähän on [monia](https://stackoverflow.com/questions/32911630/how-do-i-deal-with-localstorage-in-jest-tests).

Koska testimme ei edellytä local storagelta juuri mitään toiminnallisuutta, teemme tiedostoon [src/setupTests.js](https://github.com/facebookincubator/create-react-app/blob/ed5c48c81b2139b4414810e1efe917e04c96ee8d/packages/react-scripts/template/README.md#initializing-test-environment) hyvin yksinkertaisen mockin

```js
let savedItems = {}

const localStorageMock = {
  setItem: (key, item) => {
    savedItems[key] = item
  },
  getItem: (key) => savedItems[key],
  clear: savedItems = {}
}

window.localStorage = localStorageMock
```

Toinen ongelmistamme on se, että sovellus hakee näytettävät muistiinpanot palvelimelta. Muistiinpanojen haku tapahtuu heti komponentin _App_ luomisen jälkeen, kun metodi _componentDidMount_ kutsuu _noteService_:n metodia _getAll_:


```js
componentDidMount() {
  noteService.getAll().then(notes =>
    this.setState({ notes })
  )

  // ...
}
```

Jestin [manual mock](https://facebook.github.io/jest/docs/en/manual-mocks.html#content) -konsepti tarjoaa tilanteeseen hyvän ratkaisun. Manual mockien avulla voidaan kokonainen moduuli, tässä tapauksessa _noteService_ korvata testien ajaksi vaihtoehtoisella esim. kovakoodattua dataa tarjoavalla toiminnallisuudella.

Luodaan Jestin ohjeiden mukaisesti hakemistoon _src/services_ alihakemisto *\_\_mocks\_\_* (alussa ja lopussa kaksi alaviivaa) ja sinne tiedosto _notes.js_ jonka määrittelemä metodi _getAll_ palauttaa kovakoodatun listan muistiinpanoja:

```js
let token = null

const notes = [
  {
    id: "5a451df7571c224a31b5c8ce",
    content: "HTML on helppoa",
    date: "2017-12-28T16:38:15.541Z",
    important: false,
    user: {
      _id: "5a437a9e514ab7f168ddf138",
      username: "mluukkai",
      name: "Matti Luukkainen"
    }
  },
  {
    id: "5a451e21e0b8b04a45638211",
    content: "Selain pystyy suorittamaan vain javascriptiä",
    date: "2017-12-28T16:38:57.694Z",
    important: true,
    user: {
      _id: "5a437a9e514ab7f168ddf138",
      username: "mluukkai",
      name: "Matti Luukkainen"
    }
  },
  {
    id: "5a451e30b5ffd44a58fa79ab",
    content: "HTTP-protokollan tärkeimmät metodit ovat GET ja POST",
    date: "2017-12-28T16:39:12.713Z",
    important: true,
    user: {
      _id: "5a437a9e514ab7f168ddf138",
      username: "mluukkai",
      name: "Matti Luukkainen"
    }
  }
]

const getAll = () => {
  return Promise.resolve(notes)
}

export default { getAll, notes }
```

Määritelty metodi _getAll_ palauttaa muistiinpanojen listan käärittynä promiseksi metodin [Promise.resolve](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve) avulla sillä käytettäessä metodia, oletetaan sen paluuarvon olevan promise:

```js
noteService.getAll().then(notes =>
```

Olemme valmiina määrittelemään testin:

```js
import React from 'react'
import { mount } from 'enzyme'
import App from './App'
import Note from './components/Note'
jest.mock('./services/notes')
import noteService from './services/notes'

describe('<App />', () => {
  let app
  beforeAll(() => {
    app = mount(<App />)
  })

  it('renders all notes it gets from backend', () => {
    app.update()
    const noteComponents = app.find(Note)
    expect(noteComponents.length).toEqual(noteService.notes.length)
  })
})
```

Komennolla _jest.mock('./services/notes')_ otetaan juuri määritelty mock käyttöön. Loogisempi paikka komennolle olisi kenties testien määrittelyt tekevä tiedosto _src/setupTests.js_

Testin toimivuuden kannalta on oleellista metodin [app.update](http://airbnb.io/enzyme/docs/api/ReactWrapper/update.html) kutsuminen, näin pakotetaan sovellus renderöitymään uudelleen siten, että myös mockatun backendin palauttamat muistiinpanot renderöityvät.

## Testauskattavuus

[Testauskattavuus](https://github.com/facebookincubator/create-react-app/blob/ed5c48c81b2139b4414810e1efe917e04c96ee8d/packages/react-scripts/template/README.md#coverage-reporting) saadaan helposti selville suorittamalla testit komennolla

```bash
CI=true npm test -- --coverage
```

![]({{ "/assets/5/8.png" | absolute_url }})

Melko primitiivinen HTML-muotoinen raportti generoituu hakemistoon _coverage/lcov-report_. HTML-muotoinen raportti kertoo mm. yksittäisen komponenttien testaamattomat koodirivit:

![]({{ "/assets/5/9.png" | absolute_url }})

Huomaamme, että parannettavaa jäi vielä runsaasti.

Sovelluksen tämänhetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/part2-notes/tree/part5-5), tagissa _part5-5_.

## Tehtäviä

Tee nyt tehtävät [5.15 ja 5.16](/tehtävät#integraatiotestaus)

## Snapshot-testaus

Jest tarjoaa "perinteisen" testaustavan lisäksi aivan uudenlaisen tavan testaukseen, ns. [snapshot](https://facebook.github.io/jest/docs/en/snapshot-testing.html)-testauksen. Mielenkiintoista snapshot-testauksessa on se, että sovelluskehittäjän ei tarvitse itse määritellä ollenkaan testejä, snapshot-testauksen käyttöönotto riittää.

Periaatteena on verrata komponenttien määrittelemää HTML:ää aina koodin muutoksen jälkeen siihen, minkälaisen HTML:n komponentit määrittelivät ennen muutosta.

Jos snapshot-testi huomaa muutoksen komponenttien määrittelemässä HTML:ssä, voi kyseessä joko olla haluttu muutos tai vahingossa aiheutettu "bugi". Snapshot-testi huomauttaa sovelluskehittäjälle, jos komponentin määrittelemä HTML muuttuu. Sovelluskehittäjä kertoo muutosten yhteydessä, oliko muutos haluttu. Jos muutos tuli yllätyksenä, eli kyseessä oli bugi, sovelluskehittäjä huomaa sen snapshot-testauksen ansiosta nopeasti.

## End to end -testaus

Olemme tehneet sekä backendille että frontendille hieman niitä kokonaisuutena testavia integraatiotestejä. Eräs tärkeä testauksen kategoria on vielä käsittelemättä, [järjestelmää kokonaisuutena](https://en.wikipedia.org/wiki/System_testing) testaavat "end to end" (eli E2E) -testit.

Web-sovellusten E2E-testaus tapahtuu simuloidun selaimen avulla esimerkiksi [Selenium](http://www.seleniumhq.org)-kirjastoa käyttäen. Toinen vaihtoehto on käyttää ns. [headless browseria](https://en.wikipedia.org/wiki/Headless_browser) eli selainta, jolla ei ole ollenkaan graafista käyttöliittymää. Esim. Chromea on mahdollista suorittaa Headless-moodissa.

E2E testit ovat potentiaalisesti kaikkein hyödyllisin testikategoria, sillä ne tutkivat järjestelmää saman rajapinnan kautta kuin todelliset käyttäjät.

E2E-testeihin liittyy myös ikäviä puolia. Niiden konfigurointi on haastavampaa kuin yksikkö- ja integraatiotestien. E2E-testit ovat tyypillisesti myös melko hitaita ja isommassa ohjelmistossa niiden suoritusaika voi helposti nousta minuutteihin, tai jopa tunteihin. Tämä on ikävää sovelluskehityksen kannalta, sillä sovellusta koodatessa on erittäin hyödyllistä pystyä ajamaan testejä mahdollisimman usein koodin regressioiden varalta.

Palaamme end to end -testeihin kurssin viimeisessä, eli seitsemännessä osassa.

## Sovellusten tilan hallinta Reduxilla

Olemme noudattaneet sovelluksen tilan hallinnassa Reactin suosittelemaa käytäntöä määritellä tila ja sitä käsittelevät metodit [sovelluksen juurikomponentissa](https://reactjs.org/docs/lifting-state-up.html). Tilaa ja metodeja on välitetty propsien avulla niitä tarvitseville komponenteille. Tämä toimii johonkin pisteeseen saakka, mutta kun sovellusten koko kasvaa, muuttuu tilan hallinta haasteelliseksi.

### Flux-arkkitehtuuri

Facebook kehitti tilan hallinnan ongelmia helpottamaan [Flux](https://facebook.github.io/flux/docs/in-depth-overview.html#content)-arkkitehtuurin. Fluxissa sovelluksen tilan hallinta erotetaan kokonaan Reactin komponenttien ulkopuolisiin varastoihin eli _storeihin_. Storessa olevaa tilaa ei muuteta suoraan, vaan tapahtumien eli _actionien_ avulla.

Kun action muuttaa storen tilaa, renderöidään näkymät uudelleen:

![](https://facebook.github.io/flux/img/flux-simple-f8-diagram-1300w.png)

Jos sovelluksen käyttö, esim. napin painaminen aiheuttaa tarpeen tilan muutokseen, tehdään tilanmuutos actonin avulla. Tämä taas aiheuttaa uuden näytön renderöitymisen:

![](https://facebook.github.io/flux/img/flux-simple-f8-diagram-with-client-action-1300w.png)

Flux tarjoaa siis standardin tavan sille miten ja missä sovelluksen tila pidetään sekä tavalle tehdä tilaan muutoksia.

### Redux

Facebookilla on olemassa valmis toteutus Fluxille, käytämme kuitenkin saman periaatteen mukaan toimivaa, mutta hieman yksinkertaisempaa [Redux](https://redux.js.org)-kirjastoa, jota myös Facebookilla käytetään nykyään alkuperäisen Flux-toteutuksen sijaan.

Tutustutaan Reduxiin tekemällä laskurin toteuttava sovellus:

![]({{ "/assets/5/10.png" | absolute_url }})

Tehdään uusi create-react-app-sovellus ja asennetaan siihen _redux_ komennolla

```bash
npm install redux --save
```

Fluxin tapaan Reduxissa sovelluksen tila talletetaan [storeen](https://redux.js.org/basics/store).

Koko sovelluksen tila talletetaan _yhteen_ storen tallettamaan Javascript-objektiin. Koska sovelluksemme ei tarvitse mitään muuta tilaa kuin laskurin arvon, talletetaan se storeen suoraan. Jos sovelluksen tila olisi monipuolisempi, talletettaisiin "eri asiat" storessa olevan olioon erillisinä kenttinä.

Storen tilaa muutetaan [actionien](https://redux.js.org/basics/actions) avulla. Actionit ovat olioita, joilla on vähintään actionin _tyypin_ määrittelevä kenttä _type_. Sovelluksessamme tarvitsemme esimerkiksi seuraavaa actionia:

```js
{
  type: 'INCREMENT'
}
```

Jos actioneihin liittyy dataa, määritellään niille tarpeen vaatiessa muitakin kenttiä. Laskurisovelluksemme on kuitenkin niin yksinkertainen, että actioneille riittää pelkkä tyyppikenttä.

Actionien vaikutus sovelluksen tilaan määritellään [reducerin](https://redux.js.org/basics/reducers) avulla. Käytännössä reducer on funktio, joka saa parametrikseen olemassaolevan staten tilan sekä actionin ja _palauttaa_ staten uuden tilan.

Määritellään nyt sovelluksellemme reduceri:

```js
const counterReducer = (state, action) => {
  if (action.type === 'INCREMENT') {
    return state + 1
  } else if (action.type === 'DECREMENT') {
    return state - 1
  } else if (action.type === 'ZERO') {
    return 0
  }

  return state
}
```

Ensimmäinen parametri on siis storessa oleva _tila_. Reducer palauttaa uuden tilan actionin tyypin mukaan.

Muutetaan koodia vielä hiukan. Reducereissa on tapana käyttää if:ien sijaan [switch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch)-komentoa.
Määritellään myös parametrille _state_ [oletusarvoksi](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters) 0. Näin reducer toimii vaikka store -tilaa ei olisi vielä alustettu.

```js
const counterReducer = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    case 'ZERO':
      return 0
  }
  return state
}
```

Reduceria ei ole tarkoitus kutsua koskaan suoraan sovelluksen koodista. Reducer ainoastaan annetaan parametrina storen luovalle _createStore_-funktiolle:

```js
import {createStore} from 'redux'

const counterReducer = (state = 0, action) => {
  // ...
}

const store = createStore(counterReducer)
```

Store käyttää nyt reduceria käsitelläkseen _actioneja_, jotka _dispatchataan_ eli "lähetetään" storagelle sen [dispatch](https://redux.js.org/api-reference/store#dispatch(action))-metodilla:

```js
store.dispatch({type: 'INCREMENT'})
```

Storen tilan saa selville metodilla [getState](https://redux.js.org/api-reference/store#getstate()).

Esim. seuraava koodi

```js
const store = createStore(counterReducer)
console.log(store.getState())
store.dispatch({type: 'INCREMENT'})
store.dispatch({type: 'INCREMENT'})
store.dispatch({type: 'INCREMENT'})
console.log(store.getState())
store.dispatch({type: 'ZERO'})
store.dispatch({type: 'DECREMENT'})
console.log(store.getState())
```

tulostaisi konsoliin

<pre>
0
3
-1
</pre>

sillä ensin storen tila on 0. Kolmen _INCREMENT_-actionin jälkeen tila on 3, ja lopulta actionien _ZERO_ ja _DECREMENT_ jälkeen -1.

Kolmas tärkeä metodi storella on [subscribe](https://redux.js.org/api-reference/store#subscribe(listener)), jonka avulla voidaan määritellä takaisinkutsufunktioita, joita store kutsuu sen tilan muuttumisen yhteydessä.

Jos esim. lisäisimme seuraavan funktion subscribellä, tulostuisi _jokainen storen muutos_ konsoliin.

```js
store.subscribe(() => {
  const storeNow = store.getState()
  console.log(storeNow)
})
```

eli koodi

```js
const store = createStore(counterReducer)

store.subscribe(() => {
  const storeNow = store.getState()
  console.log(storeNow)
})

store.dispatch({ type: 'INCREMENT' })
store.dispatch({ type: 'INCREMENT' })
store.dispatch({ type: 'INCREMENT' })
store.dispatch({ type: 'ZERO' })
store.dispatch({ type: 'DECREMENT' })
```

aiheuttaisi tulostuksen

<pre>
1
2
3
0
-1
</pre>


Laskurisovelluksemme koodi on seuraavassa. Kaikki koodi on kirjoitettu samaan tiedostoon, jolloin _store_ on suoraan React-koodin käytettävissä. Tutustumme React/Redux-koodin parempiin strukturointitapoihin myöhemmin.

```react
import React from 'react'
import ReactDOM from 'react-dom'
import {createStore} from 'redux'

const counterReducer = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    case 'ZERO':
      return 0
  }
  return state
}

const store = createStore(counterReducer)

class App extends React.Component {
  render() {
    return(
      <div>
        <div>
          {store.getState()}
        </div>
        <button onClick={e => store.dispatch({ type: 'INCREMENT'})}>
          plus
        </button>
        <button onClick={e => store.dispatch({ type: 'DECREMENT' })}>
          minus
        </button>
        <button onClick={e => store.dispatch({ type: 'ZERO' })}>
          zero
        </button>
      </div>
    )
  }
}

const renderApp = () => {
  ReactDOM.render(<App />, document.getElementById('root'))
}

renderApp()
store.subscribe(renderApp)
```

Koodissa on pari huomionarvoista seikkaa. _App_ renderöi laskurin arvon kysymällä sitä storesta metodilla _store.getState()_. Nappien tapahtumankäsittelijät _dispatchaavat_ suoraan oikean tyyppiset actionit storelle.

Kun storessa olevan tilan arvo muuttuu, ei React osaa automaattisesti renderöidä sovellusta uudelleen. Olemmekin rekisteröineet koko sovelluksen renderöinnin suorittavan funktion _renderApp_ kuuntelemaan storen muutoksia metodilla _store.subscribe_. Huomaa, että joudumme kutsumaan heti alussa metodia _renderApp()_, ilman kutsua sovelluksen ensimmäistä renderöintiä ei koskaan tapahdu.

## Redux-muistiinpanot

Tavoitteenamme on muuttaa muistiinpanosovellus käyttämään tilanhallintaan reduxia. Katsotaan kuitenkin ensin eräitä konsepteja hieman yksinkertaistetun muistiinpanosovelluksen kautta.

Sovelluksen ensimmäinen versio seuraavassa

```react
const noteReducer = (state = [], action) => {
  if (action.type === 'NEW_NOTE') {
    state.push(action.data)
    return state
  }

  return state
}

const store = createStore(noteReducer)

store.dispatch({
  type: 'NEW_NOTE',
  data: {
    content: 'sovelluksen tila talletetaan storeen',
    important: true,
    id: 1
  }
})

store.dispatch({
  type: 'NEW_NOTE',
  data: {
    content: 'tilanmuutokset tehdään actioneilla',
    important: false,
    id: 2
  }
})

class App extends React.Component {
  render() {
    return(
      <div>
        <ul>
          {store.getState().map(note=>
            <li key={note.id}>
              {note.content} <strong>{note.important ? 'tärkeä' : ''}</strong>
            </li>
          )}
         </ul>
      </div>
    )
  }
}
```

Toistaiseksi sovelluksessa ei siis ole toiminnallisuutta uusien muistiinpanojen lisäämiseen, voimme kuitenkin tehdä sen dispatchaamalla _NEW_NOTE_-tyyppisiä actioneja koodista.

Actioneissa on nyt tyypin lisäksi kenttä _data_, joka sisältää lisättävän muistiinpanon:

```js
{
  type: 'NEW_NOTE',
  data: {
    content: 'tilanmuutokset tehdään actioneilla',
    important: false,
    id: 2
  }
}
```

### puhtaat funktiot, immutable

Reducerimme alustava versio on yksinkertainen:

```js
const noteReducer = (state = [], action) => {
  if (action.type === 'NEW_NOTE') {
    state.push(action.data)
    return state
  }

  return state
}
```

Tila on nyt taulukko. _NEW_NOTE_-tyyppisen actionin seurauksena tilaan lisätään uusi muistiinpano metodilla [push](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push).

Sovellus näyttää toimivan, mutta määrittelemämme reduceri on huono, se rikkoo Reactin reducerien [perusolettamusta](https://github.com/reactjs/redux/blob/master/docs/basics/Reducers.md#handling-actions) siitä, että reducerien tulee olla [puhtaita funktioita](https://en.wikipedia.org/wiki/Pure_function).

Puhtaat funktiot ovat sellaisia, että ne _eivät aiheuta mitään sivuvaikutuksia_ ja niiden tulee aina palauttaa sama vastaus samoilla parametreilla kutsuttaessa.

Lisäsimme tilaan uuden muistiinpanon metodilla _state.push(action.data)_ joka _muuttaa_ state-olion tilaa. Tämä ei ole sallittua. Ongelma korjautuu helposti käyttämällä metodia [concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat), joka luo _uuden taulukon_, jonka sisältönä on vanhan taulukon alkiot sekä lisättävä alkio:

```js
const noteReducer = (state = [], action) => {
  if (action.type === 'NEW_NOTE') {
    return state.concat(action.data)
  }

  return state
}
```

Reducen tilan tulee koostua muuttumattomista eli [immutable](https://en.wikipedia.org/wiki/Immutable_object) olioista. Jos tilaan tulee muutos, ei vanhaa oliota muuteta, vaan se _korvataan uudella muuttuneella oliolla_. Juuri näin toimimme uudistuneessa reducerissa, vanha taulukko korvaantuu uudella.

Laajennetaan reduceria siten, että se osaa käsitellä muistiinpanon tärkeyteen liittyvän muutoksen:

```js
{
  type: 'TOGGLE_IMPORTANCE',
  data: {
    id: 2
  }
}
```

Koska meillä ei ole vielä koodia joka käyttää ominaisuutta, laajennetaan reduceria testivetoisesti. Aloitetaan tekemällä testi actionin _NEW_NOTE_ käsittelylle.

Jotta testaus olisi helpompaa, siirretään reducerin koodi ensin omaan moduuliinsa tiedostoon _src/noteReducer.js_. Otetaan käyttöön myös kirjasto [deep-freeze](https://github.com/substack/deep-freeze), jonka avulla voimme varmistaa, että reducer on määritelty oikeaoppisesti puhtaana funktiona. Asennetaan kirjasto kehitysaikaiseksi riippuvuudeksi

```js
npm install --save-dev deep-freeze
```

Testi on seuraavassa:

```js
import noteReducer from './noteReducer'
import deepFreeze from 'deep-freeze'

describe('noteRenderer', () => {
  it('returns new state with action NEW_NOTE', () => {
    const state = []
    const action = {
      type: 'NEW_NOTE',
      data: {
        content: 'sovelluksen tila talletetaan storeen',
        important: true,
        id: 1
      }
    }

    deepFreeze(state)
    const newState = noteReducer(state, action)

    expect(newState.length).toBe(1)
    expect(newState).toContainEqual(action.data)
  })
})
```

Komento _deepFreeze(state)_ varmistaa, että reducer ei muuta parametrina olevaa storen tilaa. Jos reduceri käyttää state:n manipulointiin komentoa _push_, testi ei mene läpi

![]({{ "/assets/5/11.png" | absolute_url }})


Tehdään sitten testi actionin _TOGGLE_IMPORTANCE_ käsittelylle:

```js
it('returns new state with action TOGGLE_IMPORTANCE', () => {
  const state = [
    {
      content: 'sovelluksen tila talletetaan storeen',
      important: true,
      id: 1
    },
    {
      content: 'tilanmuutokset tehdään actioneilla',
      important: false,
      id: 2
    }]

  const action = {
    type: 'TOGGLE_IMPORTANCE',
    data: {
      id: 2
    }
  }

  deepFreeze(state)
  const newState = noteReducer(state, action)

  expect(newState.length).toBe(2)

  expect(newState).toContainEqual(state[0])

  expect(newState).toContainEqual({
    content: 'tilanmuutokset tehdään actioneilla',
    important: true,
    id: 2
  })
})
```

Reduceri laajenee seuraavasti

```js
const noteReducer = (state = [], action) => {
  switch(action.type) {
    case 'NEW_NOTE':
      return state.concat(action.data)
    case 'TOGGLE_IMPORTANCE':
      const id = action.data.id
      const noteToChange = state.find(n => n.id === id)
      const changedNote = { ...noteToChange, important: !noteToChange.important }
      return state.map(note => note.id !== id ? note : changedNote )
    default:
      return state
  }
}
```

Luomme tärkeyttä muuttaneesta muistiinpanosta kopion osasta 2 [tutulla syntaksilla](/osa2#muistiinpanon-tärkeyden-muutos) ja korvaamme tilan uudella tilalla, mihin otetaan muuttumattomat muistiinpanot ja muutettavasta sen muutettu kopio _changedNote_.

### array spread -syntaksi

Koska reducerilla on nyt suhteellisen hyvät testit, voimme refaktoroida koodia turvallisesti.

Uuden muistiinpanon lisäys luo palautettavan tilan taulukon _concat_-funktiolla. Katsotaan nyt miten voimme toteuttaa saman hyödyntämällä Javascriptin [array spread](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator) -syntaksia:

```js
const noteReducer = (state = [], action) => {
  switch(action.type) {
    case 'NEW_NOTE':
      return [...state, action.data]
    case 'TOGGLE_IMPORTANCE':
      // ...
    default:
    return state
  }
}
```

Spread-syntaksi toimii seuraavasti. Jos määrittelemme

```js
const luvut = [1, 2, 3]
```

niin <code>...luvut</code> hajottaa taulukon yksittäisiksi alkioiksi, eli voimme sijoittaa sen esim, toisen taulukon sisään:

```js
[...luvut, 4, 5]
```

ja lopputuloksena on taulukko, jonka sisältö on _[1, 2, 3, 4, 5]_.

Jos olisimme sijoittaneet taulukon toisen sisälle ilman spreadia, eli

```js
[luvut, 4, 5]
```

lopputulos olisi ollut _[ [1, 2, 3], 4, 5]_.

Samannäköinen syntaksi toimii taulukosta [destrukturoimalla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) alkioita otettaessa siten, että se _kerää_ loput alkiot:

```js
const luvut = [1, 2, 3, 4, 5, 6]

const [eka, toka, ...loput] = luvut

console.log(eka)    // tulostuu 1
console.log(toka)   // tulostuu 2
console.log(loput)  // tulostuu [3, 4, 5, 6]
```

## Tehtäviä

Tee nyt tehtävät [5.17 ja 5.18](/tehtävät#redux-unicafe)

### Lisää toiminnallisuutta ja ei-kontrolloitu lomake

Lisätään sovellukseen mahdollisuus uusien muistiinpanojen tekemiseen sekä tärkeyden muuttamiseen:

```react
const generateId = () => Number((Math.random() * 1000000).toFixed(0))

class App extends React.Component {
  addNote = (event) => {
    event.preventDefault()
    const content = event.target.note.value
    store.dispatch({
      type: 'NEW_NOTE',
      data: {
        content: content,
        important: false,
        id: generateId()
      }
    })
    event.target.note.value = ''
  }
  toggleImportance = (id) => () => {
    store.dispatch({
      type: 'TOGGLE_IMPORTANCE',
      data: { id }
    })
  }
  render() {
    return(
      <div>
        <form onSubmit={this.addNote}>
          <input name="note" />
          <button type="submit">lisää</button>
        </form>
        <ul>
          {store.getState().map(note=>
            <li key={note.id} onClick={this.toggleImportance(note.id)}>
              {note.content} <strong>{note.important ? 'tärkeä' : ''}</strong>
            </li>
          )}
         </ul>
      </div>
    )
  }
}
```

Molemmat toiminnallisuudet on toteutettu suoraviivaisesti. Huomionarvoista uuden muistiinpanon lisäämisessä on nyt se, että toisin kuin aiemmat Reactilla toteutetut lomakkeet, emme ole nyt sitoneet lomakkeen kentän arvoa komponentin _App_ tilaan. React kutsuu tälläisiä lomakkeita [ei-kontrolloiduiksi](https://reactjs.org/docs/uncontrolled-components.html).

> Ei-kontrolloiduilla lomakkeilla on tiettyjä rajoitteita (ne eivät esim. mahdollista lennossa annettavia validointiviestejä, lomakkeen lähetysnapin disabloimista sisällön perusteella ym...), meidän käyttötapaukseemme ne kuitenkin tällä kertaa sopivat.
Voit halutessasi lukea aiheesta enemmän [täältä](https://goshakkk.name/controlled-vs-uncontrolled-inputs-react/).

Muistiinpanon lisäämisen käsittelevä metodi on yksinkertainen, se ainoastaan dispatchaa muistiinpanon lisäävän actionin:

```js
addNote = (event) => {
  event.preventDefault()
  const content = event.target.note.value
  store.dispatch({
    type: 'NEW_NOTE',
    data: {
      content: content,
      important: false,
      id: generateId()
    }
  })
  event.target.note.value = ''
}
```

Uuden muistiinpanon sisältö saadaan suoraan lomakkeen syötekentästä, johon kentän nimeämisen ansiosta päästään käsiksi tapahtumaolion kautta _event.target.note.value_

Tärkeyden muuttaminen tapahtuu klikkaamalla muistiinpanon nimeä. Käsittelijä on erittäin yksinkertainen:

```js
toggleImportance = (id) => () => {
  store.dispatch({
    type: 'TOGGLE_IMPORTANCE',
    data: { id }
  })
}
```

Kyseessä on jälleen tuttu _funktio, joka palauttaa funktion_, eli kullekin muistiinpanolle generoituu käsittelijäksi funktio, jolla on muistiinpanon yksilöllinen id. Esim. jos id olisi 12345, käsittelijä olisi seuraava:

```js
() => {
  store.dispatch({
    type: 'TOGGLE_IMPORTANCE',
    data: { id: 12345 }
  })
}
```

### action creatorit

Alamme huomata, että jo näinkin yksinkertaisessa sovelluksessa Reduxin käyttö yksinkertaistaa sovelluksen ulkoasusta vastaavaa koodia melkoisesti. React-komponenttien on oikeastaan tarpeetonta tuntea reduxin actionien tyyppejä ja esitysmuotoja. Eristetään ne erilliseen olioon, jonka metodit ovat [action creatoreja](https://redux.js.org/advanced/async-actions#synchronous-action-creators):

```js
const actionFor = {
  noteCreation(content) {
    return {
      type: 'NEW_NOTE',
      data: {
        content,
        important: false,
        id: generateId()
      }
    }
  },
  importanceToggling(id) {
    return {
      type: 'TOGGLE_IMPORTANCE',
      data: { id }
    }
  }
}
```

Komponentin _App_ ei tarvitse enää tietää mitään actionien sisäisestä esitystavasta:

```js
class App extends React.Component {
  addNote = (event) => {
    event.preventDefault()
    store.dispatch(
      actionFor.noteCreation(event.target.note.value)
    )
    event.target.note.value = ''
  }
  toggleImportance = (id) => () => {
    store.dispatch(
      actionFor.importanceToggling(id)
    )
  }

  // ...
}
```

### staten välittäminen propseissa ja contextissa

Sovelluksemme on reduceria lukuunottamatta tehty samaan tiedostoon. Kyseessä ei tietenkään ole järkevä käytäntö, eli on syytä eriyttää _App_ omaan moduuliinsa.

Herää kuitenkin kysymys miten _App_ pääsee muutoksen jälkeen käsiksi _storeen_? Ja yleisemminkin, kun komponentti koostuu suuresta määrästä komponentteja, tulee olla jokin mekanismi, minkä avulla komponentit pääsevät käsiksi storeen.

Tapoja on muutama, käsitellään tässä osassa kahta helpoimmin ymmärrettävää. Parhaan tavan eli kirjaston React-redux määrittelevän [connect](https://github.com/reactjs/react-redux/blob/master/docs/api.md#connectmapstatetoprops-mapdispatchtoprops-mergeprops-options)-metodin säästämme seuraavaan osaan, sillä se on hieman abstrakti ja on kenties hyvä totutella Reduxiin aluksi ilman connectin tuomia käsitteellisiä haasteita.

Yksinkertaisin vaihtoehto on välittää store propsien avulla. Sovelluksen käynnistyspiste _index.js_ typistyy seuraavasti

```react
import React from 'react'
import ReactDOM from 'react-dom'
import { createStore } from 'redux'
import App from './App'
import noteReducer from './noteReducer'

const store = createStore(noteReducer)

const render = () => {
  ReactDOM.render(<App store={store}/>,
  document.getElementById('root'))
}

render()
store.subscribe(render)
```

Muutos omaan moduuliinsa eriytettyyn komponenttiin _App_ on pieni, storeen viitataan _propsien_ kautta <code>this.props.store</code>:

```react
import React from 'react'
import actionFor from './actionCreators'

class App extends React.Component {
  addNote = (event) => {
    event.preventDefault()
    this.props.store.dispatch(
      actionFor.noteCreation(event.target.note.value)
    )
    event.target.note.value = ''
  }
  toggleImportance = (id) => () => {
    this.props.store.dispatch(
      actionFor.importanceToggling(id)
    )
  }
  render() {
    return (
      <div>
        <form onSubmit={this.addNote}>
          <input name="note" />
          <button type="submit">lisää</button>
        </form>
        <ul>
          {this.props.store.getState().map(note =>
            <li key={note.id} onClick={this.toggleImportance(note.id)}>
              {note.content} <strong>{note.important ? 'tärkeä' : ''}</strong>
            </li>
          )}
        </ul>
      </div>
    )
  }
}

export default App
```

Jos sovelluksessa on enemmän storea tarvitsevia komponentteja, tulee _App_-komponentin välittää _store_ propseina kaikille sitä tarvitseville komponenteille.

Eriytetään uuden muistiinpanon luominen sekä muistiinpanojen lista ja yksittäisen muisiinpanon esittäminen omiksi komponenteiksi:

```react
class NoteForm extends React.Component {
  addNote = (event) => {
    event.preventDefault()
    this.props.store.dispatch(
      actionFor.noteCreation(event.target.note.value)
    )
    event.target.note.value = ''
  }
  render() {
    return(
      <form onSubmit={this.addNote}>
        <input name="note" />
        <button>lisää</button>
      </form>
    )
  }
}

const Note = ({note, handleClick}) => {
  return(
    <li onClick={handleClick}>
      {note.content} <strong>{note.important ? 'tärkeä' : ''}</strong>
    </li>
  )
}

class NoteList extends React.Component {
  toggleImportance = (id) => () => {
    this.props.store.dispatch(
      actionFor.importanceToggling(id)
    )
  }
  render() {
    return(
      <ul>
        {this.props.store.getState().map(note =>
          <Note
            key={note.id}
            note={note}
            handleClick={this.toggleImportance(note.id)}
          />
        )}
      </ul>
    )
  }
}
```

Komponenttiin _App_ ei jää enää paljoa koodia:

```react
class App extends React.Component {
  render() {
    return (
      <div>
        <NoteForm store={this.props.store} />
        <NoteList store={this.props.store} />
      </div>
    )
  }
}
```

Toisin kuin aiemmin ilman Reduxia tekemässämme React-koodissa, tapahtumankäsittelijät on nyt siirretty pois _App_-komponentista. Yksittäisen muistiinpanon renderöinnistä huolehtiva _Note_ on erittäin yksinkertainen, eikä ole tietoinen siitä, että sen propsina saama tapahtumankäsittelijä dispatchaa actionin. Tälläisiä komponentteja kutsutaan Reactin terminologiassa [presentational](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)-komponenteiksi.

_NoteList_ taas on sellainen mitä kutsutaan [container](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)-komponenteiksi, se sisältää sovelluslogiikkaa, eli määrittelee mitä _Note_-komponenttien tapahtumankäsittelijät tekevät ja koordinoi _presentational_-komponenttien, eli _Notejen_ konfigurointia.

Palaamme presentational/container-jakoon tarkemmin seuraavassa osassa.

_storen_ välittäminen sitä tarvitseviin komponentteihin propsien avulla on melko ikävää. Vaikka _App_ ei itse tarvitse storea, sen on otettava store vastaan, pystyäkseen välittämään sen edelleen komponenteille _NoteForm_ ja _NoteList_.

Tutustumme vielä tämän osan lopuksi _storen_ välittämiseen Reactin [contextin](https://reactjs.org/docs/context.html) avulla.

Manuaalin sanoin:
> In some cases, you want to pass data through the component tree without having to pass the props down manually at every level. You can do this directly in React with the powerful “context” API.

Reactin Context API on vielä kokeellinen ja se voi hävitä tulevista versiosta. Contextin käyttö ei olekaan kovin suositeltavaa. Katsomme kuitenkin mistä on kyse.

Asennetaan ensin contextin käyttöä helpottava [react-redux](https://github.com/reactjs/react-redux)-kirjasto sekä contextien määrittelyyn tarvittava _prop-types_:

```bash
npm install react-redux prop-types --save
```

Muutetaan tiedostoa _index.js_ seuraavasti:

```react
import React from 'react'
import ReactDOM from 'react-dom'
import { createStore } from 'redux'
import { Provider } from 'react-redux'
import App from './App'
import noteReducer from './noteReducer'

const store = createStore(noteReducer)

const render = () => {
  ReactDOM.render(
    <Provider store={store}>
      <App/>
    </Provider>,
  document.getElementById('root'))
}

render()
store.subscribe(render)
```

Komponentti _App_ on sijoitettu react-redux-kirjaston tarjoavan [Provider](https://github.com/reactjs/react-redux/blob/master/docs/api.md#provider-store) komponentin lapseksi. Store on annettu _Providerille_ propsina.

Provider määrittelee _storen_ saataville komponentin _App_ ja sen alikomponenttien _kontekstista_.

Koska _App_ ei tarvitse itse storea, voi sen muuttaa muotoon:

```react
class App extends React.Component {
  render() {
    return (
      <div>
        <NoteForm />
        <NoteList />
      </div>
    )
  }
}
```

_NoteList_ muuttuu seuraavasti:

```react
import React from 'react'
import PropTypes from 'prop-types'
import actionFor from '../actionCreators'
import Note from './Note'

class NoteList extends React.Component {
  toggleImportance = (id) => () => {
    this.context.store.dispatch(
      actionFor.importanceToggling(id)
    )
  }
  render() {
    return(
      <ul>
        {this.context.store.getState().map(note =>
          <Note
            key={note.id}
            note={note}
            handleClick={this.toggleImportance(note.id)}
          />
        )}
      </ul>
    )
  }
}

NoteList.contextTypes = {
  store: PropTypes.object
}
```

Muutos on siis hyvin pieni. Nyt propsien sijaan storen viite on <code>this.context.store</code>. Komponentille on myös pakko määritellä sen vastaanottaman kontekstin tyyppi:

```js
NoteList.contextTypes = {
  store: PropTypes.object
}
```

Ilman määrittelyä konteksti jää tyhjäksi.

Komponenttiin _NoteForm_ tehtävä muutos on samanlainen. Koska _Note_ ei riipu millään tavalla _storesta_, se jää muuttumattomaksi.

Tehdään sovellukseen vielä yksi parannus. Lisätään storea käyttäviin komponentteihin _NoteForm_ ja _NoteList_ seuraavan koodin sisältävät metodit _componentDidMount_ ja _componentWillUnmount_:

```react
class NoteForm extends React.Component {
  componentDidMount() {
    const { store } = this.context
    this.unsubscribe = store.subscribe(() =>
      this.forceUpdate()
    )
  }

  componentWillUnmount() {
    this.unsubscribe()
  }

  // ...
}
```

Näin komponentit rekisteröityvät kuuntelemaan storessa tapahtuvia muutoksia ja niiden tapahtuessa uudelleenrenderöimään itsensä (ja lapsikomponenttinsa) metodilla [forceUpdate](https://reactjs.org/docs/react-component.html#forceupdate).

Nyt pääsemme eroon tiedostossa _index.js_ tapahtuneesta koko sovelluksen uudelleenrenderöinnistä ja koodi yksinkertaistuu muotoon.

```react
ReactDOM.render(
  <Provider store={createStore(noteReducer)}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

Redux-sovelluksen tämänhetkinen koodi on kokonaisuudessaan [githubissa](https://github.com/FullStack-HY/redux-notes/tree/part5-6), tagissa _part5-6_.

Egghead.io:ssa on ilmaiseksi saatavilla Reduxin kehittäjän Dan Abramovin loistava tutoriaali [Getting started with Redux](https://egghead.io/courses/getting-started-with-redux). Neljässä viimeisessä videossa käytettävää _connect_-metodia käsittelemme vasta kurssin seuraavassa osassa.

## Tehtäviä

Tee nyt tehtävät [5.19-5.21](/tehtävät#redux-anekdootit)
