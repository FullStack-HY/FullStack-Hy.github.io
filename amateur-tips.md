## Opiskelijoiden vinkkejä

Tänne saa lisätä esimerkiksi:
* Lisäyksiä materiaalin, joita ei kannata lisätä suoraan varsinaisen materiaalin sekaan
* Omia oivalluksia, jotka haluat jakaa
* JavaScriptin kummallisuuksia, joiden kanssa hakkaisit päätä seinään liian pitkään
* Ja muuta vastaavaa mille et keksi parempaa paikkaa

### Funktio, joka palauttaa funktion

Tässä mahdollisimman yksinkertainen esimerkki funktiosta, joka palauttaa funktion. Tämän kokeileminen auttoi tajuamaan tekniikan, sillä ei tarvinnut samalla miettiä setState juttuja. Voit copypastettaa ao. koodin itsellesi ja kokeilla itse.

```
/** tulostajaGeneraattori on funktio, joka palauttaa funktion.
   * Sille annetaan parametrinä string jota halutaan tulostella
   */
  const tulostajaGeneraattori = (tulostettavaSana) => {
    return () => {
      console.log(tulostettavaSana);
    }
  }

  /** Tässä alustetaan kaksi tulostajaFunktiota generaattorin avulla
   * Toinen tulostaa 'foo' ja toinen 'bar'
   */
  const fooTulostajaFunktio = tulostajaGeneraattori('foo')
  const barTulostajaFunktio = tulostajaGeneraattori('bar')

  fooTulostajaFunktio() //sama kuin console.log('foo')
  barTulostajaFunktio() //sama kuin console.log('bar')

  /** Jos tulostaa itse tulostajaFunktion niin foo tai bar ei kuitenkaan näy.
   * Seuraavat tulostavat siis täysin saman asian:
   */
  console.log(fooTulostajaFunktio)
  console.log(barTulostajaFunktio)
  ```
  
### Autentikointi testeissä
  
Voit lisätä authorization-headerin testeihin näin:
  
```
await api
    .post('/api/blogs')
    .send(newBlog)
    .set('Authorization', 'bearer eyJhbGciOiJIUzI...')
    .expect(201)
    .expect('Content-Type', /application\/json/)
```
