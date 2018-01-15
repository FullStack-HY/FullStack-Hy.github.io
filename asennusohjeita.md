---
layout: page
title: 
permalink: /asennusohjeita/
---

## Asennusohjeita

Tähän dokumenttiin on koottu muutamia Noden asennusohjeita.

**HUOM:** riippumatta siitä miten asennat nyt Noden, älä asenna kurssin aikana npm:llä mitään "globaalisti", eli komennolla _npm install -g_, (käyttäen tarkenninta g) sille ei ole tarvetta ja koulun koneilla et sitä edes pysty tekemään.

### Laitoksen koneet ja fuksiläppärit

Laitoksen koneilla ja vuoden 2016 tai sitä uudemmissa fuksiläppäreissä pitäisi olla nyt Noden versio 8.9.4. joka on kurssin suoritusta varten riittävä. Koneilla on myös Visual studio code -editori. Mitään ei siis tarvitse asentaa.

Jos fuksiläppärisi on varhaisempaa mallia, ota yhteys IT-ylläpidon Jani Jaakkolaan.

### Linux

Tapoja on monia, tässä eräs

1. Lataa https://nodejs.org/en/download/ tai https://nodejs.org/en/download/current/ -sivulta Linux Binaries (x86/x64) -kohdasta 64-bittinen paketti

2. Pura paketti kotihakemistossa:
```tar xvJf ~/Downloads/node-VERSIO-linux-x64.tar.xz```

3. Lisää polkuun uuden hakemiston sisällä oleva bin-hakemisto (eli .bashrc tiedostoon, korvaa VERSIO:
```export PATH=~/node-VERSIO-linux-x64/bin:$PATH```

### Mac

Käytä [macOS installeria](https://nodejs.org/en/download/)

### Windows

Tapoja on kaksi, voit käyttää [Windows-installeria](https://nodejs.org/en/download/)

**tai** toimia seuraavasti

1. Lataa https://nodejs.org/en/download/ -sivulta Windows Binaries

2. Pura paketti haluamaasi kansioon (esim. %userprofile%\Applications\node)

3. Paina WIN+R (CMD+R) ja kirjoita

```rundll32 sysdm.cpl,EditEnvironmentVariables```

4. Valitse "Path". Paina "Edit...". Paina "New". Lisää polku (esim %userprofile%\Applications\node).