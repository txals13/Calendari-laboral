# Calendari laboral

App d'una sola pàgina per registrar hores, viatges i vacances contra el calendari
laboral signat. Sense servidor, sense macros: obre `index.html` i ja funciona.

## Com s'usa

- **Calendari** — toca un dia per marcar-lo (HQ, estranger, nacional, previst,
  vacances, lliure disposició), posar-hi les hores fetes i una destinació.
  «Aplica fins a» repeteix la marca a tot un rang de dates.
- **Resum** — saldo de l'any i saldo fins avui, vacances restants, hores per mes,
  detall mensual, i el preu de l'hora amb la comparativa com a autònom.
- **Viatges** — dies i hores per destinació, i les estades amb el seu període.
  Un dia fora és un dia amb estat **Estranger** o **Nacional**: un dia a la seu
  no hi compta encara que tingui un codi de destinació (p. ex. «PRODEC»). Si un
  informe no té cap dia, l'app t'ho diu en comptes de treure un PDF buit.
- **Nòmina** — meritació bruta, exempció de l'art. 7.p, Seguretat Social, IRPF i
  net estimat del mes, més la bossa d'hores de descans i el total de l'any.
- **⚙️** — bossa de vacances, romanent de l'any anterior, lliure disposició,
  objectiu manual, i exportació/importació de dades.

## Desfer i refer

El botó **↶** de la cinta de dalt (o **Ctrl+Z** fora d'un camp de text) desfà
l'última acció, i es pot prémer fins a 50 cops seguits. **↷** (o **Ctrl+Y**, o
**Ctrl+Maj+Z**) torna a fer el que acabes de desfer. Si hi passes per sobre,
diuen què faran, i en prémer-los avisen a baix: «Desfet: 31 dies», «Refet:
ajustos», «Desfet: net real de setembre».

Si després de desfer fas un canvi nou, el que havies desfet ja no es pot refer:
com a qualsevol editor, el canvi nou obre un altre camí.

- **Una acció és un pas**, encara que toqui molts dies: aplicar un estat a tot un
  mes o a una selecció de divendres es desfà d'un sol cop.
- Es desfà el que fas tu, **no el que arriba d'un altre dispositiu**. Si mentre
  tant el mòbil ha sincronitzat un dia, desfer el teu canvi de l'ordinador no el
  toca.
- **El que desfàs també es sincronitza.** El valor restaurat porta una marca de
  temps nova, i un dia que no existia abans queda esborrat amb làpida: així
  l'altre dispositiu no el torna a posar.
- També desfà una importació de JSON i el «esborra l'any».
- Les dues llistes viuen en memòria: en tancar o recarregar l'app, es buiden.

## Nòmina

Port del simulador `2026_Nomina_Prodec_v3.xlsx`. Tots els paràmetres són a
**Nòmina → Paràmetres de nòmina**, amb els valors inicials del full ⚙️ Config:
salari i pagues, dietes nacionals i internacionals, increment d'hores extres,
quotes de la Seguretat Social, cotització de solidaritat, i els interruptors de
l'art. 7.p. Els percentatges s'escriuen en tant per u (0,047 = 4,70 %).

**Les hores extres surten de `hores fetes − hores previstes`.** Les fetes són el
teu fitxatge (inici/fi/pausa; si no en poses, val el camp d'hores). Les previstes
són **un camp editable a cada dia**: el calendari només hi posa el valor inicial,
i si l'escrius tu mana el teu número. La nòmina no depèn, doncs, que el calendari
sigui correcte.

Cada dia pots marcar si les extres es **paguen** (+10 %) o van **a la bossa de
descans** (×1,5 en cap de setmana o festiu, ×1 en laborable).

La bossa es gasta de dues maneres:

- **un dia sencer**, amb l'estat **Descans c.**: gasta la jornada del dia;
- **les hores que vulguis d'un dia normal**, amb el camp **Gasto de la bossa
  d'hores** de la fitxa. «Cobreix el que falta» hi posa la diferència entre la
  jornada i el que has fet, i la pots canviar.

Exemple: el dilluns fas 8,75 h d'una jornada de 8,25 i marques les extres **en
descans** → +0,5 h a la bossa. El divendres fas 5,5 h d'una jornada de 6 i hi
poses 0,5 h de la bossa → la bossa torna a zero i el divendres no surt com a
hores de menys. A la casella del calendari hi veus +0,5 i −0,5; si gastes més
del que tens, surt en vermell i la fitxa et diu quantes hores no queden cobertes.

La bossa es compta **dia a dia**, així que el que fas un dilluns ja ho pots
gastar el divendres de la mateixa setmana. Imputació FIFO —primer les hores més
antigues— i caducitat a final del mes N + 2: les fetes el setembre valen fins al
30 de novembre. El saldo es veu al xip «bossa» del calendari (a final de mes) i
al Resum (avui, amb les primeres que caduquen).

Al **Resum**, les **hores extres** que tornes en descans es descompten del mes en
què les vas fer: amb el dilluns i el divendres de l'exemple, l'indicador passa de
0,50 a 0,00 i diu «fetes 0,50 · −0,50 tornades en descans». Una hora de bossa no
sempre és una hora d'extra: les d'un dissabte entren ×1,5, així que tornar-ne 1,5 h
en descompta 1. Les extres pagades segueixen comptant. L'indicador de la bossa
dóna el saldo d'avui i, si has planificat dies més endavant, on et deixen.

El **saldo** d'hores no canvia pel fet de fer servir la bossa: la mitja hora de
més del dilluns i la de menys del divendres ja s'hi compensen soles.

El **registre de jornada** exportable (PDF i .xlsx) porta una columna **Bossa**
amb +0,50 el dilluns i −0,50 el divendres, i una nota que explica que un dia amb
menys hores i un − a la bossa és una jornada completa compensada.

**Quan es cobra.** Les hores extres no es cobren el mes que es fan: surten a la
nòmina **dos mesos després** (paràmetre *Hores extres: mesos de retard*). Les del
setembre, a la nòmina de novembre; les de novembre, a la de gener de l'any
següent. Per això l'estimació de cada mes és el que t'hauria d'entrar al banc
**aquell** mes —el salari del mes més les extres de fa dos mesos—, i la
meritació diu de quin mes són («H. extres de setembre»). Les dietes tenen el seu
propi retard, per defecte 0: si també arriben tard, canvia'l.

El **preu de l'hora** del Resum no fa servir el retard: mesura què et paguen per
les hores que fas, així que les extres de novembre hi compten al novembre.

**Net real.** Sota el bloc D de cada mes hi ha un camp per escriure el que t'ha
entrat al banc. Surt la diferència amb l'estimació, també al KPI de dalt, i al
final de la pestanya hi ha la taula de l'any mes a mes. Una diferència aïllada
és un mes estrany; la mateixa cada mes sol ser un paràmetre (tipus d'IRPF,
dietes que es cobren amb retard). El net real es sincronitza mes a mes entre
dispositius, com els dies.

**Mesos d'alta parcial.** El període de contracte es posa als paràmetres (inici i,
si escau, fi). El mes en què entres o surts no cobra sencer: el salari, la base de
cotització i el topall van prorratejats, i les pagues extres pels dies d'alta de
l'any. Els mesos anteriors a l'alta no meriten res.

El **divisor del mes** és un paràmetre. Per defecte **30**, que és la convenció
espanyola per a salari mensual: el mes de salari són 30 dies tant si el mes real
en té 28 com 31. Posant-hi 0 es fan servir els dies reals del mes. Amb l'alta el
24 d'agost (8 dies naturals):

| Divisor | Salari d'agost |
|---|---|
| 30 (per defecte) | 895,24 € |
| 30,4167 (365/12) | 882,97 € |
| dies reals (31) | 866,36 € |

En un mes sencer el factor es limita a 1, així que un mes de 31 dies amb divisor
30 no cobra de més.

L'**IRPF** té dos modes. `AUTO` reprodueix l'algoritme de retencions de l'AEAT a
partir de les retribucions previsibles de l'any: els mesos que tinguis registrats
més una estimació des de l'inici del contracte. Amb pocs mesos entrats el tipus
pot sortir 0 % perquè encara no arriba al mínim exclòs de retenir — l'app t'ho
avisa. `MANUAL` aplica el percentatge que hi posis.

> Validat contra el full: el setembre dóna brut 3.357,14 → SS −254,59 →
> IRPF −508,61 → **net 2.593,94 €**, i el tipus automàtic amb els cinc mesos
> registrats dóna **15,15 %** amb previsibles de 26.967,50 €, idèntic al llibre.

⚠️ És una **estimació**. La liquidació definitiva es fa a la declaració de la
renda; contrasta-la amb el teu assessor.

## Preu de l'hora i comparativa amb l'autònom

Al **Resum**, al final, hi ha dues targetes. La primera dóna el preu de l'hora
**real**: al numerador hi va tot el que cobres —salari, pagues extres, hores
extres pagades i dietes— i al denominador tot el temps que hi poses, **viatges i
desplaçaments inclosos**. No és el preu de conveni (salari fix contra 1.746 h
teòriques), sinó el que et surt a la pràctica. Vacances, lliure disposició i
descans compensat queden fora del divisor: es cobren, però no són hores fetes.

Es calcula sobre el **període registrat**, no sobre l'any sencer: si has entrat
l'agost, comparar cinc mesos de sou contra dotze de despeses no voldria dir res.
La targeta et diu quins mesos i quants dies fitxats hi entren.

La segona resol el camí invers: partint d'aquell net, hi suma la quota
d'autònoms, les despeses del negoci i l'IRPF fins a trobar què hauries de
facturar, i ho reparteix entre les hores que de debò pots cobrar. La quota i les
despeses van **prorratejades als mateixos mesos** del període.

| Supòsit | Per defecte | Què vol dir |
|---|---|---|
| SS a càrrec de l'empresa | 0,32 | tant per u sobre el brut |
| Quota d'autònoms | 400 €/mes | tram mitjà de la quota per ingressos reals |
| Despeses del negoci | 3.000 €/any | gestoria, assegurances, eines, despeses no facturades |
| Hores facturables | 0,80 | administració, ofertes i buits entre clients no es cobren |

Els quatre són **supòsits editables** a *Nòmina → Paràmetres de nòmina*, no valors
del conveni. Sense cap dia fitxat encara, la targeta cau al preu de conveni i
t'avisa que ho fa.

⚠️ És una comparació, no un pressupost. L'IRPF de l'autònom hi va al mateix tipus
que la teva nòmina i, com que la base seria més alta, el tipus real pujaria.
Tampoc hi entren l'atur, la baixa per malaltia, les vacances pagades ni la
indemnització, que d'assalariat tens i d'autònom no: **pren el número com a
mínim, no com a sostre.** Les dietes compensen despeses i no són sou; la targeta
et diu quants €/h en són, perquè si com a autònom factures hotel i desplaçaments
a part, els has de restar del preu.

## Sense connexió

`sw.js` és el service worker: guarda l'app i la serveix quan no hi ha xarxa, així
que un cop l'has obert, torna a obrir en un avió o en un client sense cobertura.
L'estratègia és **xarxa primer amb 3 segons de paciència**: amb cobertura sempre
reps l'última versió publicada, i si la xarxa triga o no hi és, tira de la còpia.
No et quedes encallat en una versió antiga.

Les peticions a Google (autenticació i Drive) **no s'intercepten mai**: van sempre
a la xarxa i no es guarden. Sense connexió pots fitxar i consultar-ho tot, però la
sincronització esperarà a tenir cobertura.

> El worker ha de ser un fitxer servit per http(s). Registrar-lo des d'una URL
> `blob:` el navegador ho rebutja, i si la crida porta un `.catch()` buit et
> quedes sense worker i sense assabentar-te'n.

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

**Si no et deixa connectar**, mira l'adreça del navegador. Ha de ser
`https://txals13.github.io/Calendari-laboral/`. Si hi diu `file:///C:/...` estàs
obrint el fitxer del disc i Google no hi pot autenticar: el punt de sincronització
es posa **⊘** i t'ho explica. Passa fàcilment a l'ordinador, on és temptador obrir
l'`index.html` d'OneDrive amb doble clic.

> **Cal servir-la des d'un origen autoritzat.** Fa servir el mateix client OAuth
> que el Field Service Log, registrat per a `https://txals13.github.io`. Publicada
> en qualsevol repositori d'aquest usuari a GitHub Pages funciona sense tocar res
> a Google Cloud. **Obrint `index.html` del disc, o des de `localhost`, l'accés a
> Google no arrenca** — la resta de l'app sí, amb les dades només en local.

L'autorització de Google dura una hora i no es refresca sola: passat aquest
temps el punt es posa ◐ i amb un toc es renova. Mentrestant els canvis es desen
aquí i pugen quan tornis a autoritzar; no es perd res.

Sense connectar, les dades viuen només en aquell navegador, a **IndexedDB**
(base `cal`, magatzem `kv`, clau `db`). No a `localStorage`: la seva quota és
d'uns 5 MB **per origen**, i totes les apps servides des de
`txals13.github.io` se la reparteixen, així que la primera que cresqués faria
fallar les escriptures de les altres. Hi queden només el testimoni, el tema i
la preferència de privadesa. Si tenies dades de la versió anterior, es mouen
soles el primer cop que obres l'app i la còpia vella s'allibera.

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
