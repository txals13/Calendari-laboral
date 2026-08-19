# Calendari laboral

App d'una sola pàgina per registrar hores, viatges i vacances contra el calendari
laboral signat. Sense servidor, sense macros: obre `index.html` i ja funciona.

## Com s'usa

- **Calendari** — toca un dia per marcar-lo (HQ, estranger, nacional, previst,
  vacances, lliure disposició), posar-hi les hores fetes i una destinació.
  «Aplica fins a» repeteix la marca a tot un rang de dates.
- **Resum** — saldo de l'any i saldo fins avui, vacances restants, hores per mes
  i detall mensual.
- **Viatges** — dies i hores per destinació, i les estades amb el seu període.
- **⚙️** — bossa de vacances, romanent de l'any anterior, lliure disposició,
  objectiu manual, i exportació/importació de dades.

## Sincronització entre dispositius

El punt de la barra de dalt és l'estat: ○ només aquí · ● verd connectat ·
◐ cal tornar a autoritzar · ● vermell error. Toca'l per sincronitzar.

Connecta amb Google Drive des de ⚙️ i les dades viatgen en un únic fitxer,
`Calendari laboral/calendari_laboral_data.json`. Puja sol dos segons després de
cada canvi, i baixa en obrir l'app, en tornar-hi i cada cinc minuts.

**Es fusiona dia a dia, no fitxer sencer.** Pots marcar l'agost al mòbil i el
setembre a l'ordinador sense que l'un esborri l'altre: per a cada data guanya la
versió amb la marca de temps més alta. Esborrar un dia hi deixa una làpida durant
90 dies perquè l'altre dispositiu no el ressusciti.

> **Cal servir-la des d'un origen autoritzat.** Fa servir el mateix client OAuth
> que el Field Service Log, registrat per a `https://txals13.github.io`. Publicada
> en qualsevol repositori d'aquest usuari a GitHub Pages funciona sense tocar res
> a Google Cloud. **Obrint `index.html` del disc, o des de `localhost`, l'accés a
> Google no arrenca** — la resta de l'app sí, amb les dades només en local.

L'autorització de Google dura una hora i no es refresca sola: passat aquest
temps el punt es posa ◐ i amb un toc es renova. Mentrestant els canvis es desen
aquí i pugen quan tornis a autoritzar; no es perd res.

Sense connectar, les dades viuen només al `localStorage` d'aquell navegador;
«Exporta còpia (JSON)» serveix per traslladar-les a mà o guardar-ne una còpia.

### Regles que convé saber

- Un **rang** només toca dies amb jornada. Per registrar un cap de setmana o un
  festiu treballat, obre aquell dia tot sol.
- Si no canvies les hores que et proposa, dins d'un rang **cada dia agafa la seva
  pròpia jornada** (8,25 · 6,00 · 7,00 · 6,50) en comptes d'un número fix.
- L'**objectiu** de l'any surt de les hores teòriques del calendari menys les
  hores dels dies marcats com a vacances o lliure disposició. Per al 2026:
  1.890,00 − 144,00 = **1.746,00 h**, tal com diu el calendari signat.

## El calendari de cada any

`CAL` (a dalt de tot del `<script>`) guarda **una lletra per dia natural**, de
l'1 de gener al 31 de desembre:

| Lletra | h/dia | Què és |
|---|---|---|
| `P` | 8,25 | jornada partida (dl-dj) |
| `V` | 6,00 | divendres |
| `I` | 7,00 | intensiva d'estiu |
| `C` | 6,50 | jornada curta (24 i 31 de desembre) |
| `F` | 0 | festiu |
| `B` | 0 | pont — compta com a vacances |
| `X` | 0 | no laborable |

El 2026 està transcrit del calendari signat i quadra amb tots els totals impresos:
168 `P` + 41 `V` + 35 `I` + 2 `C` = **1.890,00 h**, amb 16 festius, 3 ponts i 100
dies no laborables.

### Afegir un any nou

Afegeix una entrada a `CAL` amb 365 lletres (366 si és de traspàs), una per dia.
La comprovació ràpida és que les hores de cada mes coincideixin amb les del
calendari signat; el Resum les mostra a la columna «Teòr.».

Si un any no hi és, l'app se'l fabrica amb el patró per defecte (dl-dj 8,25,
divendres 6,00, caps de setmana lliures) **sense festius ni intensiva**, així que
els números seran aproximats fins que hi posis el calendari real.
