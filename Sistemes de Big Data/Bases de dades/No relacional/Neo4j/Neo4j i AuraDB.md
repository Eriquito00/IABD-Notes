# Neo4j i AuraDB
## AuraDB Free
Versió gratuïta de Neo4j al núvol. La presentació indica aquests límits: **200.000 nodes**, **400.000 relacions** i pausa després de **3 dies** sense activitat.
1. Entra a [console.neo4j.io](https://console.neo4j.io).
2. Crea una instància **AuraDB Free** i desa les credencials.
3. Quan estigui **Running**, entra a **Connect → Query**.

**Eines:** `Query` (executar Cypher), `Explore` (navegar pel graf), `Import` (carregar CSV locals). No publiquis mai les credencials.
## Llegir patrons Cypher

```cypher

(c:Client)                          // Node amb etiqueta

(c:Client {nom:'Aina'})             // Filtre de propietat

(a)-[:SEGUEIX]->(b)                 // Relació dirigida

(a)-[:SEGUEIX]-(b)                  // Qualsevol direcció

(a)-[:SEGUEIX*1..3]->(b)            // D'1 a 3 salts

```

**Sintaxi:** `()` nodes · `[]` relacions · `->` direcció · `{}` propietats.
## Crear i consultar

```cypher

// MERGE evita duplicar el node quan es torna a executar

MERGE (c:Client {client_id:'C001'})

SET c.nom = 'Aina';

  

MATCH (c:Client)

WHERE c.nom STARTS WITH 'A'

RETURN c.nom

ORDER BY c.nom

LIMIT 10;

```

- `CREATE`: crea sempre (pot duplicar).
- `MERGE`: busca el patró i el crea si no existeix.
- `MATCH`: cerca patrons; `WHERE`: filtra; `RETURN`: mostra.

**Bona pràctica:** fes `MERGE` dels nodes per separat i després de la relació.
## Modificar i esborrar

```cypher

MATCH (c:Client {client_id:'C001'})

SET c.vip = true;

  

MATCH (c:Client {client_id:'C001'})

REMOVE c.vip;

  

MATCH (c:Client {client_id:'C001'})

DETACH DELETE c;  // Esborra el node i les seves relacions

```

`DELETE` tot sol no pot esborrar un node que encara té relacions.