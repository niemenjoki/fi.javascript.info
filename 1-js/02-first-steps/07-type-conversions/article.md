# Tyyppimuunnokset

Useimmiten operaattorit ja funktiot muuntavat niille annetut arvot automaattisesti oikeaan tyyppiin.

Esimerkiksi `alert` muuntaa minkä tahansa arvon automaattisesti merkkijonoksi näyttääkseen sen. Matemaattiset operaatiot muuntavat arvot luvuiksi.

On myös tilanteita, joissa arvo täytyy muuntaa odotettuun tyyppiin erikseen.

```smart header="Olioista ei vielä puhuta"
Tässä kappaleessa emme käsittele olioita. Toistaiseksi puhumme vain primitiiveistä.

Myöhemmin, kun olemme oppineet olioista, näemme kappaleessa <info:object-toprimitive>, miten oliot sopivat kuvioon.
```

## Merkkijonomuunnos

Merkkijonomuunnos tapahtuu, kun tarvitsemme arvon merkkijonomuodossa.

Esimerkiksi `alert(value)` tekee sen näyttääkseen arvon.

Voimme myös kutsua funktiota `String(value)` muuntaaksemme arvon merkkijonoksi:

```js run
let value = true;
alert(typeof value); // boolean

*!*
value = String(value); // nyt value on merkkijono "true"
alert(typeof value); // string
*/!*
```

Merkkijonomuunnos on useimmiten itsestään selvä. Arvosta `false` tulee `"false"`, arvosta `null` tulee `"null"` jne.

## Lukumuunnos

Lukumuunnos tapahtuu matemaattisissa funktioissa ja lausekkeissa automaattisesti.

Esimerkiksi kun jakolaskua `/` käytetään muihin kuin lukuihin:

```js run
alert( "6" / "2" ); // 3, merkkijonot muunnetaan luvuiksi
```

Funktiolla `Number(value)` voimme muuntaa arvon `value` luvuksi erikseen:

```js run
let str = "123";
alert(typeof str); // string

let num = Number(str); // muuttuu luvuksi 123

alert(typeof num); // number
```

Erillistä muunnosta tarvitaan yleensä, kun luemme arvon merkkijonopohjaisesta lähteestä, kuten tekstilomakkeesta, mutta odotamme, että siihen syötetään luku.

Jos merkkijono ei ole kelvollinen luku, muunnoksen tulos on `NaN`. Esimerkiksi:

```js run
let age = Number("mielivaltainen merkkijono luvun sijaan");

alert(age); // NaN, muunnos epäonnistui
```

Lukumuunnoksen säännöt:

| Arvo |  Muuttuu... |
|-------|-------------|
|`undefined`|`NaN`|
|`null`|`0`|
|<code>true&nbsp;ja&nbsp;false</code> | `1` ja `0` |
| `string` | Tyhjämerkit (välilyönnit, sarkaimet `\t`, rivinvaihdot `\n` jne.) poistetaan alusta ja lopusta. Jos jäljelle jäävä merkkijono on tyhjä, tulos on `0`. Muuten luku "luetaan" merkkijonosta. Virhe antaa tulokseksi `NaN`. |

Esimerkkejä:

```js run
alert( Number("   123   ") ); // 123
alert( Number("123z") );      // NaN (virhe luettaessa lukua kohdassa "z")
alert( Number(true) );        // 1
alert( Number(false) );       // 0
```

Huomaa, että `null` ja `undefined` käyttäytyvät tässä eri tavoin: `null` muuttuu nollaksi, kun taas `undefined` muuttuu arvoksi `NaN`.

Useimmat matemaattiset operaattorit tekevät myös tällaisen muunnoksen. Näemme sen seuraavassa kappaleessa.

## Totuusarvomuunnos

Totuusarvomuunnos on muunnoksista yksinkertaisin.

Se tapahtuu loogisissa operaatioissa (myöhemmin tutustumme ehtotesteihin ja muihin vastaaviin asioihin), mutta sen voi tehdä myös erikseen kutsumalla `Boolean(value)`.

Muunnossääntö:

- Arvot, jotka ovat intuitiivisesti "tyhjiä", kuten `0`, tyhjä merkkijono, `null`, `undefined` ja `NaN`, muuttuvat arvoksi `false`.
- Muut arvot muuttuvat arvoksi `true`.

Esimerkiksi:

```js run
alert( Boolean(1) ); // true
alert( Boolean(0) ); // false

alert( Boolean("hei") ); // true
alert( Boolean("") ); // false
```

````warn header="Huomaa: merkkijono, jossa on nolla `\"0\"`, on `true`"
Jotkin kielet (nimittäin PHP) käsittelevät merkkijonoa `"0"` arvona `false`. JavaScriptissa ei-tyhjä merkkijono on kuitenkin aina `true`.

```js run
alert( Boolean("0") ); // true
alert( Boolean(" ") ); // välilyöntejä, myös true (mikä tahansa ei-tyhjä merkkijono on true)
```
````

## Yhteenveto

Kolme yleisimmin käytettyä tyyppimuunnosta ovat muunnokset merkkijonoksi, luvuksi ja totuusarvoksi.

**`Merkkijonomuunnos`** -- Tapahtuu, kun tulostamme jotain. Voidaan tehdä funktiolla `String(value)`. Primitiivisten arvojen muunnos merkkijonoksi on yleensä itsestään selvä.

**`Lukumuunnos`** -- Tapahtuu matemaattisissa operaatioissa. Voidaan tehdä funktiolla `Number(value)`.

Muunnos noudattaa sääntöjä:

| Arvo |  Muuttuu... |
|-------|-------------|
|`undefined`|`NaN`|
|`null`|`0`|
|<code>true&nbsp;/&nbsp;false</code> | `1 / 0` |
| `string` | Merkkijono luetaan "sellaisenaan", ja tyhjämerkit (välilyönnit, sarkaimet `\t`, rivinvaihdot `\n` jne.) molemmista päistä jätetään huomiotta. Tyhjästä merkkijonosta tulee `0`. Virhe antaa tulokseksi `NaN`. |

**`Totuusarvomuunnos`** -- Tapahtuu loogisissa operaatioissa. Voidaan tehdä funktiolla `Boolean(value)`.

Noudattaa sääntöjä:

| Arvo |  Muuttuu... |
|-------|-------------|
|`0`, `null`, `undefined`, `NaN`, `""` |`false`|
|mikä tahansa muu arvo| `true` |


Useimmat näistä säännöistä on helppo ymmärtää ja muistaa. Merkittävät poikkeukset, joissa ihmiset yleensä tekevät virheitä, ovat:

- `undefined` on lukuna `NaN`, ei `0`.
- `"0"` ja pelkistä välilyönneistä koostuvat merkkijonot, kuten `"   "`, ovat totuusarvona tosia.

Olioita ei käsitellä tässä. Palaamme niihin myöhemmin kappaleessa <info:object-toprimitive>, joka on omistettu kokonaan olioille, kun olemme oppineet lisää JavaScriptin perusasioita.
