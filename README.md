# Solidabiksen Koodihaaste 2022

## Ratkaisu:  **Food Fighters League**

### Teknologiat

**Frontend**  
Valinta: [Svelte](https://svelte.dev/)  
Miksi: Mielenkiinto kyseistä frameworkkiä kohtaan.  

**CI/CD**  
Valinta: [Vercel](https://vercel.com/)  
Miksi: Ilmainen hostaus, kokemus aiemmista omista projekteista

**"Backend"**  
Valinta: Json tiedosto githubista  
Miksi: Laiskuus, ajanpuute

### Pystytys ja käynnistys

#### **Tuotantoympäristö**  

Livenä osoitteessa: [https://koodihaaste-2022.vercel.app/](https://koodihaaste-2022.vercel.app/)

#### **Kehitysympäristö**

1. Kloonaa repo koneellesi
1. Aja npm install & npm run dev ratkaisun lähdekansiossa ./foodfighters

## Ratkaisun kuvaus

Tappelevat ruoat ovat valittavissa kymmenestä eri vaihtoehdosta, pienistä mustikoista aina pitsaan asti.  

Pelilogiikka pyörii täysin käyttäjän päädyssä.

Ruokatiedot on koostettu valmiiksi fruits.json tiedostoon, hyödyntäen [Finelin](https://fineli.fi/fineli/fi/index) elintarviketietokantaa. Tiedoissa on otettu ruoan arvot / 100g. Perusarvot on pyöristetty lähimpiin lukuihin.

**Sovelluksen esimerkkikäyttö:**

1. Käyttäjä avaa sivun
1. Käyttäjä valitsee itselleen "Championit"
1. Käyttäjä voi säätää Settings-painikkeen takaa taistelun nopeutta, ja lukemaan samalla kuvakkeiden selvitykset
1. Käyttäjä painaa FIGHT! - painiketta, joka käynnistää tappelun
1. Tulokset ilmestyvät BATTLE LOG - osioon sitä mukaa kun tappelu etenee
1. Tappelun päätyttyä pelin voi käynnistää uudestaan samoilla tai uusilla valinnoilla

### **Featuret**

- 10 Food Championiä mistä valita
- Rajattomat pelikerrat
- Game speed - Säädä pelin kellon nopeutta haluamaasi välillä 10ms-1s
- Darkmode - Nyt myös tyylit yökukkujille, joilla on refers-color-scheme: dark käytössä
- Battle log type - Haluatko taistelulogin koko pötkönä vai rajattuna
- Automatic scrolling -  Rullataanko viimeisimpään tapahtumaan automaattisesti

## [Koodihaaste 2022 Ohjeistus](https://koodihaaste.solidabis.com/intro)

**Tehtävänanto**:

Tehtävänäsi on toteuttaa ruokarähinä teemainen taistelu, jossa ruoat taistelevat keskenään erilaisilla statseilla.

Tehtävän toteutusta varten sinun tulee hakea eri ruokien ravintosisällöt ja toteuttaa näistä statsit, joiden perusteella ruoat taistelevat keskenään.

Toteutuksessa käytettävät teknologiat ovat vapaasti päätettävissäsi, ja voit toteuttaa tehtävän frontend-, backend- tai fullstack-toteutuksena (kts. palkintoluokat)

**Sovelluksessa tulee olla seuraavat toiminnot:**

1. Eri ravintosisältöjen haku ulkoisesta lähteestä (esim. Fineli API tai jokin muu vastaava kuten CSV-tiedosto)
1. Ravintosisältöjen muuntaminen hahmoluokkiin
1. Logiikan toteutus kahden ruokahahmon väliseen kaksintaisteluun
1. Tulosten esitys tekstimuotoisena rajapinta vastauksena tai visuaalisesti Frontendilla

**Statsit**:

Perus statsit täytyy löytyä ja olla kaavan mukaan. On kuitenkin lupa lisätä taisteluihin satunnaisuutta.