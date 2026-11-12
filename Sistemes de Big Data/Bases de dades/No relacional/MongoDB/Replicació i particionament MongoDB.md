# Replicació i particionament MongoDB
## Replicació (Replica Set)

Un **replica set** és un conjunt de servidors que mantenen còpies de les mateixes dades per garantir l'**alta disponibilitat**.

- **Primari (PRIMARY):** rep les escriptures i, per defecte, les lectures. Només n'hi ha un.
- **Secundaris (SECONDARY):** copien les operacions del primari. Poden passar a ser primaris.
- **Àrbitre (ARBITER):** només vota en les eleccions; no desa dades ni pot ser primari.
- **Retardat:** replica amb retard per ajudar a recuperar errors.
- **Ocult:** no és visible per als clients; útil per a còpies o analítica.

Si cau el primari, els nodes elegibles escullen un altre primari per **majoria de vots**. Durant l'elecció no es poden fer escriptures.

### Oplog

L'**oplog** és una col·lecció _capped_ que registra les operacions (`insert`, `update`, `delete`) amb un timestamp. Els secundaris llegeixen les operacions pendents i les apliquen per sincronitzar-se.

Com que té una mida limitada, les entrades antigues s'acaben sobreescrivint.

### Ordres bàsiques del replica set

```JS
rs.status()                // Estat dels nodes
rs.conf()                  // Configuració
rs.add("mongo4:27017")     // Afegir un membre
rs.remove("mongo4:27017")  // Eliminar un membre
rs.stepDown()              // Fer que el primari deixi de ser-ho
rs.printSecondaryReplicationInfo()
db.hello()                 // Informació del node actual
```

En un desplegament de 3 nodes amb Docker, els tres processos `mongod` han d'utilitzar el mateix `--replSet`, per exemple `iabdrs`.

```JS
docker compose -f docker-compose-replicaset.yml up -d
docker exec mongo1 sh /scripts/rs-init.sh
docker exec -it mongo1 mongosh
```

## Preferències de lectura (Read Preference)

Indiquen **des de quin node es llegeixen les dades**.

|Mode|Com funciona|
|---|---|
|`primary`|Només el primari (per defecte).|
|`primaryPreferred`|Primari; si no és possible, secundari.|
|`secondary`|Només secundaris.|
|`secondaryPreferred`|Secundaris; si no n'hi ha, primari.|
|`nearest`|Node amb menor latència.|

```JS
db.getMongo().setReadPref("secondary")
db.getMongo().getReadPref()
```

> [!warning] Consistència eventual  
> Les lectures als secundaris poden retornar dades desactualitzades perquè la replicació no és immediata.

## Write Concern i Read Concern

### Write Concern

Determina **quants nodes han de confirmar una escriptura**.

- `w: 0`: no espera confirmació.
- `w: 1`: confirma el primari.
- `w: "majority"`: confirma la majoria (amb 3 nodes votants, normalment 2 confirmacions).
- `j: true`: espera el registre al _journal_.
- `wtimeout`: temps màxim d'espera, en mil·lisegons.

```JS
db.pruebas.insertOne(
  { num: 1002 },
  { writeConcern: { w: "majority", wtimeout: 5000 } }
)
```

### Read Concern

Determina **quin nivell de consistència han de tenir les dades llegides**.

|Nivell|Significat|
|---|---|
|`local`|Dades del node, encara que no s'hagin replicat.|
|`available`|Similar a `local`; pot incloure documents orfes en sharding.|
|`majority`|Dades confirmades per la majoria.|
|`linearizable`|Lectura que reflecteix les escriptures anteriors confirmades, al primari.|
|`snapshot`|Vista coherent de les dades, útil en transaccions.|

**Rollback:** si el primari cau abans que una escriptura es repliqui prou, aquesta es pot desfer. `w: "majority"` ajuda a evitar-ho.

## Eleccions i tolerància a fallades

Quan els nodes detecten que el primari no respon mitjançant els _heartbeats_, els secundaris elegibles poden iniciar una elecció. Es té en compte la connectivitat, l'actualització de les dades i la prioritat.

|Nodes amb vot|Majoria|Fallades tolerades|
|---|---|---|
|3|2|1|
|4|3|1|
|5|3|2|
|7|4|3|

És convenient tenir un **nombre senar de vots**. Afegir un quart vot a un conjunt de tres no augmenta el nombre de fallades tolerades.

## Particionament (Sharding)

El **sharding** reparteix les dades d'una col·lecció entre diversos servidors (_shards_) per aconseguir **escalat horitzontal**. A diferència de la replicació, cada shard guarda només una part de la col·lecció.

### Components

- **Shards:** emmagatzemen una part de les dades; normalment cadascun és un replica set.
- **Config servers:** guarden les metadades sobre el repartiment de dades.
- **`mongos`:** router que rep les consultes i les envia als shards corresponents.
- **Driver:** connecta l'aplicació als routers.

### Shard Key

És el camp que determina **com es distribueixen els documents**. Una bona clau ha de tenir:

- **Alta cardinalitat:** molts valors diferents.
- **Bona distribució:** evitar concentrar les insercions en un únic shard (_hotspotting_).
- **Relació amb les consultes:** permet que `mongos` consulti només els shards necessaris.

Escollir una mala shard key pot provocar desequilibris; canviar-la després (_resharding_) és costós.

### Particionament per rangs i per hash

|Per rangs `{ login: 1 }`|Per hash `{ login: "hashed" }`|
|---|---|
|Valors propers es guarden junts.|Distribueix segons el hash del valor.|
|Eficient per a consultes per rang.|Reparteix millor les insercions.|
|Pot generar _hotspotting_.|Les consultes per rang poden consultar tots els shards.|

## Chunks i balanceig

Un **chunk** és un rang de valors de la shard key i és la unitat de dades que MongoDB pot moure entre shards.

- `MinKey` i `MaxKey`: extrems dels rangs.
- **Balancer:** redistribueix chunks entre shards.
- **Split:** divideix un chunk en dos.

```JS
sh.status()                            // Estat del clúster
db.usuarios.getShardDistribution()     // Distribució de documents
sh.getBalancerState()                  // Estat del balancer
sh.startBalancer()                     // Activar-lo

sh.splitAt("iabd.usuarios", { login: "usuario10000" })
sh.splitFind("iabd.usuarios", { login: "usuario10000" })

db.adminCommand({
  moveChunk: "iabd.usuarios",
  find: { login: "usuario5000" },
  to: "iabdshard1"
})
```

## Configuració bàsica del sharding

Les ordres `sh.` es fan des de **`mongos`**, no directament des d'un shard.

```JS
docker exec -it router1 mongosh
```

```JS
// Registrar els shards
sh.addShard("iabdshard1/mongo-shard1a:27017,mongo-shard1b:27017")
sh.addShard("iabdshard2/mongo-shard2a:27017,mongo-shard2b:27017")

// Habilitar el sharding i particionar col·leccions
sh.enableSharding("iabd")
sh.shardCollection("iabd.usuarios", { login: 1 })
sh.shardCollection("iabd.usuarioshash", { login: "hashed" })

sh.status()
```

> [!tip] Diferència clau  
> **Replicació:** còpies de les mateixes dades per garantir disponibilitat.  
> **Sharding:** distribució de dades diferents entre servidors per guanyar capacitat i rendiment. Es poden combinar: cada shard pot ser un replica set.