# Aplicacions i practica Neo4j
## Recomanacions
Idea: **recomanar productes comprats per clients amb compres semblants**, excloent els que ja tenim.

```cypher

MATCH (jo:Client {client_id:'C001'})-[:HA_COMPRAT]->(:Producte)

      <-[:HA_COMPRAT]-(similar:Client)-[:HA_COMPRAT]->(rec:Producte)

WHERE similar <> jo

  AND NOT (jo)-[:HA_COMPRAT]->(rec)

RETURN rec.nom AS recomanacio, count(DISTINCT similar) AS punts

ORDER BY punts DESC LIMIT 5;

```

**Limitació:** *arrencada en fred*: un client sense compres no rep recomanacions amb aquest mètode.
## Detecció de frau

Diversos clients que comparteixen dispositius poden formar un **anell** sospitós.

```cypher

MATCH (c:Client {client_id:'C027'})-[:FA_SERVIR*2..10]-(altre:Client)

WHERE altre <> c

RETURN DISTINCT altre.nom;

```

**Compte:** compartir un dispositiu no demostra frau (pot ser familiar). Cal revisió humana i tenir present l'RGPD.
## Quan NO convé Neo4j?  
- Agregacions enormes de vendes → millor **Data Warehouse / Parquet**.
- Transaccions simples → habitualment **SQL**.
- Comptadors molt ràpids → **Redis**.
## Què entra a les activitats?
- **AC5074/03/08:** xarxa de transport; connexions directes, 2 trams, camí més curt, temps totals, restricció i `PROFILE`.
- **PR507410 (Sapa-Shop):** modelar i carregar dades, explorar relacions, recomanacions, detectar possibles anells de frau i analitzar `PROFILE`.
## Xuleta ràpida
| Necessites…                 | Fes servir…                        |
| --------------------------- | ---------------------------------- |
| Cercar nodes/relacions      | `MATCH`                            |
| Filtrar                     | `WHERE`                            |
| Crear / evitar duplicats    | `CREATE` / `MERGE`                 |
| Mostrar / ordenar / limitar | `RETURN` / `ORDER BY` / `LIMIT`    |
| Agregar / passar resultats  | `count()` / `WITH`                 |
| Recórrer salts / camí mínim | `*1..n` / `shortestPath()`         |
| Modificar / esborrar        | `SET` / `REMOVE` / `DETACH DELETE` |
| Comprovar rendiment         | `PROFILE`                          |