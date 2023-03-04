---
mainImage: ../../../images/part-0.svg
part: 0
letter: a
lang: fi
---

<div class="content">

Kurssilla tutustutaan JavaScriptilla tapahtuvaan moderniin websovelluskehitykseen. Pääpaino on React-kirjaston avulla toteutettavissa single page -sovelluksissa, ja niitä tukevissa Node.js:llä toteutetuissa REST ja GraphQL-rajapinnoissa.

Kurssilla käsitellään myös sovellusten testaamista, konfigurointia ja suoritusympäristöjen hallintaa sekä NoSQL-tietokantoja.

### Oletetut esitiedot

Osallistujilta edellytetään vahvaa ohjelmointirutiinia, web-ohjelmoinnin ja tietokantojen perustuntemusta, git-versionhallintajärjestelmän peruskäytön hallintaa, kykyä pitkäjänteiseen työskentelyyn sekä valmiutta omatoimiseen tiedonhakuun ja ongelmanratkaisuun.

Osallistuminen ei kuitenkaan edellytä kurssilla käsiteltävien tekniikoiden tai JavaScript-kielen hallintaa.

### Kurssimateriaali

Kurssimateriaali on tarkoitettu luettavaksi osa kerrallaan "alusta loppuun". Materiaalin seassa on tehtäviä, jotka on sijoiteltu siten, että kunkin tehtävän tekemiseen pitäisi olla riittävät tekniset valmiudet sitä edeltävässä materiaalissa. Voit siis tehdä tehtäviä sitä mukaan kun niitä tulee materiaalissa vastaan. Voi myös olla, että koko osan tehtävät kannattaa tehdä vasta sen jälkeen, kun olet ensin lukenut osan alusta loppuun kertaalleen. Useissa osissa tehtävät ovat samaa ohjelmaa laajentavia pienistä osista koostuvia kokonaisuuksia. Muutamia tehtävien ohjelmia kehitetään eteenpäin useamman osan aikana.

Materiaali perustuu muutamien osasta osaan vaihtuvien esimerkkiohjelmien asteittaiseen laajentamiseen. Materiaali toiminee parhaiten, jos kirjoitat samalla koodin myös itse ja teet koodiin myös pieniä modifikaatioita. Materiaalin käyttämien ohjelmien koodin eri vaiheiden tilanteita on tallennettu GitHubiin.

### Suoritustapa

Kurssin tämä versio koostuu kahdeksasta osasta, joista ensimmäinen on historiallisista syistä numero nolla.

Materiaalissa osasta <i>n</i> osaan <i>n+1</i> eteneminen ei ole mielekästä ennen kuin riittävä osaaminen osan <i>n</i> asioista on saavutettu. Kurssilla sovelletaankin pedagogisin termein <i>tavoiteoppimista</i>, [engl. mastery learning](https://en.wikipedia.org/wiki/Mastery_learning) ja on tarkoitus, että etenet seuraavaan osaan vasta, kun riittävä määrä edellisen osan tehtävistä on tehty.

Oletuksena on, että teet kunkin osan tehtävistä <i>ainakin ne</i> jotka eivät ole merkattu tähdellä. Myös tähdellä merkatut tehtävät vaikuttavat arvosteluun, mutta niiden tekemättä jättäminen ei aiheuta liian suuria esteitä seuraavan osan (tähdellä merkkaamattomien) tehtävien tekemiseen.

Osien **deadlinet** ovat maanantaisin klo 23:59.

| osa         | deadline&nbsp; &nbsp; |
| ----------- | :-------------------: |
| osat 0 ja 1 |       ma 30.1.        |
| osa 2       |        ma 6.2.        |
| osa 3       |       ma 13.2.        |
| osa 4       |       ma 20.2.        |
| osa 5       |       ma 27.2.        |
| osat 6 ja 7 |       ma 13.3.        |

Tämän kurssin eri osiin jo tehtyjen palautusten ajankäyttöstatistiikan näet [tehtävien palautussovelluksesta](https://study.cs.helsinki.fi/stats/courses/fullstack2023/).

### Ohjausta tehtäviin

Tehtäviin on tarjolla apua kurssikanavalla Discordissa <a target='_blank' href='https://study.cs.helsinki.fi/discord/join/fullstack'>https://study.cs.helsinki.fi/discord/join/fullstack</a> sekä Telegramissa <a target='_blank' href='https://t.me/fullstackcourse'>https://t.me/fullstackcourse</a>

Huom: kaikki epäasialliset, halventavat ja jotain ihmisryhmää syrjivät kommentit kanavalla ovat kiellettyjä ja tälläisten kommenttien esittäjät poistetaan kanavalta.

### Arvosteluperusteet

Kurssin oletusarvoinen laajuus on 5 opintopistettä. Arvosana määräytyy kaikkien tehtyjen tehtävien perusteella, myös tähdellä merkityt "vapaavalintaiset" tehtävät siis vaikuttavat arvosanaan. Noin 50% deadlineen mennessä tehdyistä tehtävistä tuo arvosanan 1 ja 80% arvosanan 5. Kurssin lopussa on koe, joka on suoritettava hyväksytysti. Koe ei kuitenkaan vaikuta arvosanaan.

Koe pidetään [Moodlessa](https://moodle.helsinki.fi/course/view.php?id=57509) 10-12.3. Koekeen suorittamiseen on aikaa 2 tuntia alotushetkestä alkaen.

Kurssilta on mahdollisuus ansaita lisäopintopisteitä. Jos teet 87.5% kurssin tehtävistä, saat yhden lisäopintopisteen. Tekemällä 95% tehtävistä saat 2 lisäopintopistettä.

Arvosana/opintopisterajat:

| tehtäviä | opintopisteitä | arvosana |
| -------- | :------------: | :------: |
| 138      |       7        |    5     |
| 127      |       6        |    5     |
| 116      |       5        |    5     |
| 105      |       5        |    4     |
| 94       |       5        |    3     |
| 83       |       5        |    2     |
| 72       |       5        |    1     |

Suoritukseen edellytetään tehtävien lisäksi hyväksytysti suoritettu koe.

### Kurssisuorituksen laajentaminen avoimessa yliopistossa

Kurssista on tarjolla [avoimessa yliopistossa](https://fullstackopen.com/) deadlinettomana, laajennettuna versiona (laajuus 5-14 opintopistettä), missä voit laajentaa halutessasi kurssisuoritustasi.

### Tehtävien palauttaminen

Tehtävät palautetaan GitHubin kautta ja merkitsemällä tehdyt tehtävät [palautussovellukseen](https://study.cs.helsinki.fi/stats/courses/fullstack2032/).

Jos palautat eri osien tehtäviä samaan repositorioon, käytä järkevää hakemistojen nimentää. Voit toki tehdä jokaisen osan omaankin repositorioon, kaikki käy. Jos käytät privaattirepositoriota tehtävien palautukseen, liitä repositoriolle collaboratoriksi <i>mluukkai</i>

Tehtävät palautetaan **yksi osa kerrallaan**. Kun olet palauttanut osan tehtävät, et voi enää palauttaa saman osan tekemättä jättämiäsi tehtäviä.

GitHubiin palautettuja tehtäviä tarkastetaan plagiaattitunnistusjärjestelmän avulla. Jos GitHubista löytyy kurssin mallivastausten koodia tai useammalta opiskelijalta löytyy samaa koodia, käsitellään tilanne yliopiston [vilppikäytäntöjen](https://guide.student.helsinki.fi/fi/artikkeli/mita-ovat-vilppi-ja-plagiointi) mukaan.

Suurin osa tehtävistä on moniosaisia, samaa ohjelmaa pala palalta rakentavia kokonaisuuksia. Tällaisissa tehtäväsarjoissa ohjelman lopullisen version palauttaminen riittää, voit toki halutessasi tehdä commitin jokaisen tehtävän jälkeisestä tilanteesta, mutta se ei ole välttämätöntä.

### Full stack -harjoitustyö

Avoimen yliopiston tarjonnassa on 5, 7, 10 opintopisteen laajuinen Full Stack -harjoitustyö, johon voit halutessasi osallistua suoritettuasi tämän kurssin vähintään 5 opintopisteen laajuisena.

Harjoitustyössä toteutetaan vapaavalintainen sovellus Reactilla ja/tai Nodella. Myös React Nativella toteutettu mobiilisovellus on mahdollinen.

Harjoitustyön opintopistemäärä määrittyy käytettyjen työtuntien mukaan, yksi opintopiste vastaa 17.5 tuntia. Työ arvostellaan skaalalla hyväksytty/hylätty.

Harjoitustyö on mahdollista tehdä myös pari- tai ryhmätyönä.

Lisää tietoa harjoitustyöstä avoimen yliopiston [sivulla](https://studies.helsinki.fi/opintotarjonta/cur/otm-85bb770f-ef2f-4bde-984d-5c753bf6a442/CSM141093/Full_Stack_websovelluskehitys_harjoitusty%C3%B6).

### Alkutoimet

Tällä kurssilla suositellaan Chrome-selaimen käyttöä, sillä se tarjoaa parhaan välineistön web-sovelluskehitystä ajatellen.

Kurssin tehtävät palautetaan GitHubiin, joten Git tulee olla asennettuna ja sitä on syytä osata käyttää. Ohjeita Gitin käyttämiseen löytyy muun muassa [täältä](https://github.com/mluukkai/ohjelmistotekniikka-kevat2019/blob/master/tehtavat/viikko1.md#gitin-alkeet).

Asenna myös joku järkevä web-devausta tukeva tekstieditori, enemmän kuin suositeltava valinta on [Visual Studio Code](https://code.visualstudio.com/).

Älä koodaa nanolla, Notepadilla tai Geditillä. Myöskään NetBeans ei ole omimmillaan Web-devauksessa ja se on myös turhan raskas verrattuna esim. Visual Studio Codeen.

Asenna koneeseesi heti myös [Node.js](https://nodejs.org/en/). Materiaali on tehty versiolla 18.13. Materiaalin esimerkit toiminevat vanhemmillakin Noden versiolla mutta jos haluat pelata varman päälle, asenna sama versio. Asennusohjeita löytyy [Node.js:n sivulta](https://nodejs.org/en/download/package-manager/).

Noden myötä koneelle asentuu myös [npm](https://www.npmjs.com/get-npm) (alunperin lyhennelmä <i>Node Package Manager</i> -nimelle), jota tulemme tarvitsemaan kurssin aikana aktiivisesti. Tuoreen Noden kera asentuu myös [npx](https://www.npmjs.com/package/npx), jota tarvitaan myös muutaman kerran.

### Typoja materiaalissa

Jos löydät kirjoitusvirheen tai jokin asia on ilmaistu epäselvästi tai kielioppisääntöjen vastaisesti, tee <i>pull request</i> repositoriossa <https://github.com/FullStack-HY/FullStack-Hy.github.io/> olevaan kurssimateriaaliin. Esim. tämän sivun Markdown-muotoinen lähdekoodi löytyy repositoryn alta osoitteesta <https://github.com/FullStack-HY/FullStack-Hy.github.io//blob/source/src/content/0/fi/osa0a.md>

Materiaalin jokaisen osan alalaidassa on linkki <em>Ehdota muutosta materiaalin sisältöön</em>, jota klikkaamalla pääset suoraan editoimaan sivun lähdekoodia.

</div>
