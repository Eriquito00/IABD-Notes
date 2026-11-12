# Rendiment MongoDB

Els **índexs** permeten trobar documents sense recórrer tota la col·lecció. Guarden valors de camps ordenats i també poden ajudar a ordenar resultats.

## `explain()` i pla d'execució

Permet comprovar com s'executa una consulta i si utilitza índexs.

```JS
db.grades.find({ student_id: 0, class_id: 7 }).explain("executionStats")
```

- **COLLSCAN:** recorre tota la col·lecció.
- **IXSCAN:** recorre les claus d'un índex.
- **FETCH:** recupera els documents a partir de l'índex.
- **SORT:** ordena els resultats en memòria.
- **IDHACK:** cerca directa per `_id` (segons el pla i la versió).
- **AND_SORTED:** intersecció de dos índexs.

Mètriques principals d'`executionStats`:

- `nReturned`: documents retornats.
- `totalKeysExamined`: claus d'índex examinades.
- `totalDocsExamined`: documents examinats.
- `executionTimeMillis`: temps d'execució.

Si `totalDocsExamined` és molt més gran que `nReturned`, la consulta pot necessitar un índex millor. `winningPlan` indica el pla escollit i `rejectedPlans`, els descartats.

## Índexs simples

Un índex simple conté un únic camp. El camp `_id` ja té un índex únic per defecte.

```JS
db.grades.createIndex({ class_id: 1 })
```

- `1`: ascendent.
- `-1`: descendent.
- En un índex simple, MongoDB el pot recórrer en ambdós sentits.
- Els camps imbricats s'indexen amb notació punt: `{ "loc.x": 1 }`.

**Compte:** els índexs acceleren lectures, però ocupen disc/RAM i alenteixen insercions i modificacions. Cal crear-los per a consultes freqüents, no indiscriminadament.

## Índexs compostos

Un índex compost conté diversos camps. **L'ordre dels camps a l'índex importa**, però l'ordre en què apareixen al filtre de la consulta no.

```JS
db.grades.createIndex({ student_id: 1, class_id: 1 })
```

Si tenim un índex `{ A: 1, B: 1, C: 1 }`, els prefixos que aprofiten bé l'índex són:

- **A**
- **A, B**
- **A, B, C**

En canvi, **B**, **C** i **B, C** no són prefixos per l'esquerra i no aprofiten l'índex de la mateixa manera.

Si ja tenim l'índex `(A, B, C)`, un índex simple `(A)` pot ser redundant.

## Regla ESR

Serveix per decidir l'ordre dels camps d'un índex compost:

- **E — Equality:** filtres d'igualtat (`state: 'AL'`).
- **S — Sort:** camps utilitzats per ordenar (`sort({ city: 1 })`).
- **R — Range:** filtres de rang ($gt, $lt, $gte, $lte).

Consulta:

```JS
db.zips.find({
  state: 'AL',
  pop: { $gt: 10000 }
}).sort({ city: 1 })
```

Índex òptim segons ESR:

```JS
db.zips.createIndex({ state: 1, city: 1, pop: 1 })
```

## Ordenació amb índexs

- Un índex pot evitar l'etapa `SORT` en memòria.
- En un índex compost, importen l'ordre dels camps i el sentit (`1` o `-1`).
- Es pot recórrer l'índex en sentit invers (**backward**) si s'inverteixen tots els sentits de l'ordenació.

Per exemple, amb `{ A: 1, B: -1, C: 1 }`, filtrant per `A` podem ordenar per `{ B: -1, C: 1 }` o per `{ B: 1, C: -1 }`.

## Altres tipus d'índexs

### Parcials

Només indexen els documents que compleixen una condició. Ocupen menys espai.

```JS
db.zips.createIndex(
  { state: 1 },
  { partialFilterExpression: { pop: { $gte: 5000 } } }
)
```

Una consulta amb `pop: { $gt: 6500 }` pot aprofitar-lo; una consulta només per `state` no garanteix la condició del parcial.

### Dispersos (`sparse`)

Indexen documents on existeix el camp, fins i tot si val `null`, i ometen els que no el tenen.

```JS
db.zips.createIndex({ state: 1 }, { sparse: true })
```

Normalment es prefereixen els **parcials** perquè permeten definir millor la condició.

### Únics

Impedeixen valors duplicats; en un índex compost, impedeixen repetir la combinació.

```JS
db.zips.createIndex({ zip: 1 }, { unique: true })
```

### Multiclau (`multikey`)

Indexen els elements dels arrays. Un array de 10 valors pot generar 10 entrades d'índex.

```JS
db.posts.createIndex({ tags: 1 })
```

En un índex compost, no es poden indexar dos camps array independents d'un mateix document.

### Especialitzats

- **Text:** cerques de text (`{ body: "text" }`).
- **Geoespacial:** proximitat (`{ loc: "2dsphere" }`).
- **Wildcard:** camps dinàmics (`{ "$**": 1 }`).
- **Hashed:** valors hash, útils per al sharding.
- **TTL:** elimina documents automàticament després d'un temps.
- **Collation:** defineix regles de comparació i ordenació de textos.

## Gestió dels índexs

```JS
db.grades.getIndexes()                         // Llistar
db.grades.createIndex({ student_id: 1 })      // Crear
db.grades.dropIndex({ student_id: 1 })        // Eliminar
db.grades.totalIndexSize()                    // Mida total
```

Per forçar un índex concret es pot utilitzar `hint()`:

```JS
db.iabd.find({ nombre: "Bob", twitter: "bob" })
  .hint({ twitter: 1 })
```

Una **consulta coberta** obté tot el necessari de l'índex i pot tenir `totalDocsExamined: 0`.

## Col·leccions limitades (`capped`)

Funcionen com un **buffer circular**: quan s'omplen, els documents nous substitueixen els més antics. Són útils per a logs i auditories.

```JS
db.createCollection("auditoria", {
  capped: true,
  size: 20480,
  max: 100
})
```

- `size`: mida màxima en bytes (obligatòria).
- `max`: nombre màxim de documents.
- Els documents no poden créixer de mida en modificar-los.
- `db.auditoria.isCapped()` comprova si és limitada.
- `sort({ $natural: -1 })` mostra l'ordre natural invers.

## Profiling i monitoratge

El **profiler** registra operacions per detectar consultes lentes. En el material es treballa en local o Docker, ja que no està disponible al clúster gratuït d'Atlas.

|Nivell|Què registra|
|---|---|
|`0`|Res (per defecte)|
|`1`|Operacions lentes|
|`2`|Totes les operacions|

```JS
db.setProfilingLevel(1, 50)  // Operacions lentes > 50 ms
db.getProfilingStatus()
db.system.profile.find().sort({ ts: -1 }).limit(10)
```

Eines de terminal:

```JS
mongotop 3    # Temps de lectura/escriptura per col·lecció
mongostat 5   # Operacions per segon del servidor
```

**Flux de treball:** detectar consulta lenta → analitzar amb `explain()` → crear o ajustar índex → tornar a mesurar.