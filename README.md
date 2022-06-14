![logo_ironhack_blau 7](https://user-images.githubusercontent.com/23629340/40541063-a07a0a8a-601a-11e8-91b5-2f13e4e6b441.png)

# LAB | Vue.js WikiCountries

## Introducció

Després de passar massa temps a GitHub, heu trobat un [conjunt de dades JSON de països](https://ih-countries-api.herokuapp.com/countries) i vau decidir utilitzar-lo per crear la vostra Viquipèdia de països!

<p align="center"><img src="https://education-team-2020.s3.eu-west-1.amazonaws.com/web-dev/labs/lab-wiki-countries-1.gif" alt="Exemple - LAB acabat"/></p>

## Configuració

- Bifurca aquest repo
- Clona aquest repo
- Obriu el LAB i comenceu:

  ```bash
   $ cd lab-vue-wiki-countries-cat
   $ yarn install
   $ yarn start
  ```

## Submissió

- En finalitzar, executeu les ordres següents:

  ```bash
  $ git add .
  $ git commit -m "done"
  $ git push origin main # or master if you are working from a master
  ```

- Creeu una sol·licitud d'extracció perquè els vostres TA puguin comprovar el vostre treball.

## Començant

Netegeu el component `App.vue` perquè tingui l'estructura següent dins de les etiquetes de plantilla

```vue
<!-- src/App.js -->
<template>
  <div class="app">
  </div>
</template>
```

<br/>

## Instruccions

### Iteració 0 | Vue Instal·lació del router

Recordeu instal·lar el router vue:

```shell
$ yarn install vue-router
```

I configureu l'encaminador al vostre fitxer `src/router.js` :

```js
// src/router.js
import { createRouter, createWebHistory } from 'vue-router';

const routes = [
  {
    path: '/',
    name: 'root',
    component: () => import(/* webpackChunkName: 'index' */ './pages/index.vue')
  },
  {
    path: '/list',
    name: 'list',
    component: () => import(/* webpackChunkName: 'list' */ './pages/CountriesList.vue')
    children: [
      {
        path: '/details',
        name: 'details',
        component: () => import(/* webpackChunkName: 'details' */ './pages/CountriesDetails.vue')
      },
    ]
  }
];

const router = createRouter({
  history: createWebHistory('/'),
  routes,
  scrollBehavior() {
    document.getElementById('app').scrollIntoView();
  }
});
```

### Instal·lació de Bootstrap

Utilitzarem [Bootstrap V4](https://getbootstrap.com/) per al disseny :+1:

```shell
$ yarn install bootstrap
```

Per fer que els estils Bootstrap estiguin disponibles a tota l'aplicació, importeu el full d'estils a `index.js` :

```javascript
// src/main.js

import 'bootstrap/dist/css/bootstrap.css';
```

## Instruccions

### Iteració 1.1 | Crea components

En aquesta iteració, ens centrarem en el disseny general. Abans de començar, dins de la carpeta `src` , creeu la carpeta `components` . Allà creareu almenys 3 components:

- `Navbar` de navegació: mostra la barra de navegació bàsica amb el nom del LAB

- `CountriesList` : mostra la llista d'enllaços amb els noms dels països. Cada enllaç hauria de ser un `router-link` `vue-router-dom` que utilitzarem per *enviar* el codi del país ( `alpha3Code` ) a través de l'URL.

- `CountryDetails` : és el component que renderem a través de la `Route` del `vue-router` i *rebrà* el codi de país ( `alpha3Code` ) a través de l'URL.

  En realitat, aquest és l'identificador del país (exemple: `/ESP` per a Espanya, `/FRA` per a França).

Per ajudar-vos amb l'estructura dels components, us hem donat un exemple de pàgina dins `example.html` .

Si voleu posar-hi estil, refresqueu la vostra memòria a Bootstrap als [documents](https://getbootstrap.com/docs/4.0) o consulteu com hem abordat l'estil a l' `example.html` .

### Iteració 1.2 | Component de la barra de navegació

La manera més senzilla de definir un component a vue és escriure una funció JavaScript, també component de funció. La barra de navegació hauria de mostrar el títol *LAB - WikiCountries* .

### Iteració 1.3 | Component CountryList

Aquest component hauria de mostrar una llista d' `router-link` que s'utilitzen per activar el canvi d'URL del navegador. Si feu clic a un component `router-link` , s'activarà la `Route` corresponent que mostrarà el component de detalls del país.

### Iteració 1.4 | Configuració del component CountryDetails i `router-view`

Ara que la nostra llista de països està preparada, hauríem de crear la pàgina `CountryDetails` . `CountryDetails` mostra els detalls del país segons l'enllaç que hem fet clic. Aquest component s'ha de mostrar/renderitzar dinàmicament amb el `<router-vue />`.

```vue
<!-- Example -->

<router-view>
```

Els components representats amb Vue.js poden llegir la consulta amb `this.$route` . Podem utilitzar-ho per obtenir la informació procedent de la barra d'URL del navegador, per exemple, el codi `alpha3Code` del país.

**NOTA:** Per a la imatge petita de la bandera, podeu utilitzar l' `alpha2Code` en minúscula i incrustar-lo a l'URL tal com es mostra a continuació:

- França: <https://flagpedia.net/data/flags/icon/72x54/fr.png>
- Alemanya: <https://flagpedia.net/data/flags/icon/72x54/de.png>
- Brasil: <https://flagpedia.net/data/flags/icon/72x54/br.png>
- etc.

----

### Iteració 2 | Enllaçant-ho tot

Un cop hagis creat els components, l'estructura dels elements que representarà el teu `App.js` hauria de semblar una mica així:

```vue
<div class="app">
  <Navbar />

  <div className="container">
    <div className="row">
      <CountriesList :countries="countries" />
      <router-view>
    </div>
  </div>
</div>
```

----

### Iteració 3 | Estableix l'estat quan es munta el component

La nostra aplicació `App.vue` hauria d'incorporar els `countries` al mètode de dades vue, conservant les dades procedents del fitxer `src/countries.json` .

----

### Iteració 4 | Bonificació | Obteniu dades de països d'una API

En lloc de confiar en les dades estàtiques procedents d'un fitxer `json` , fem alguna cosa més interessant i extreurem les dades d'una API real.

Fem una sol·licitud `GET` a l'URL <https://ih-countries-api.herokuapp.com/countries> i utilitzem les dades retornades de la resposta com a llista dels països. Podeu utilitzar `fetch` o `axios` per fer la sol·licitud.

Hauríeu d'utilitzar el ganxo `mounted()` per configurar el ganxo del cicle de vida que només s'executa una vegada i fa una sol·licitud a l'API. La sol·licitud hauria de passar primer quan es carregui l'aplicació, per tant, penseu en quan i des d'on hauríem de fer la sol·licitud a l'API.

----

### Iteració 5 | Bonificació | Obteniu dades d'un país d'una API

Utilitzant el component `CountriesDetails` del conjunt de ganxos `mounted()` . Hauria de fer una sol·licitud a l'API RestCountries i obtenir les dades del país específic. Podeu construir el punt final de sol·licitud mitjançant l' `alpha3Code` del país. Exemple:

- Estats Units: <https://ih-countries-api.herokuapp.com/countries/USA>
- Japó: <https://ih-countries-api.herokuapp.com/countries/JPN>
- Índia: <https://ih-countries-api.herokuapp.com/countries/IND>

L'efecte s'hauria d'executar després de la representació inicial i cada vegada que canviï el paràmetre URL amb l' `alpha3Code` .

<br/>

**Feliç codificació!** :heart: