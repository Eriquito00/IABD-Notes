# Consultes avançades Neo4j
## Agregacions i WITH

Cypher agrupa automàticament pels camps no agregats (no cal `GROUP BY`). `WITH` encadena etapes i permet filtrar resultats agregats.

```cypher

MATCH (c:Client)-[:HA_COMPRAT]->(p:Producte)

RETURN p.nom, count(c) AS compres

ORDER BY compres DESC

LIMIT 5;

```  

Funcions: `count()`, `sum()`, `avg()`, `min()`, `max()`, `collect()`.
## Camins

```cypher

// Clients a 1, 2 o 3 salts de l'Aina

MATCH (:Client {nom:'Aina'})-[:SEGUEIX*1..3]->(b)

RETURN DISTINCT b.nom;

```

```cypher

// Camí amb menys salts entre dos clients

MATCH p = shortestPath(

  (a:Client {client_id:'C001'})-[:SEGUEIX*..6]-(b:Client {client_id:'C050'})

)

RETURN length(p) AS salts;

```  

**Important:** el camí amb menys salts **no sempre** és el de menys minuts. Si hi ha costos a les relacions, cal sumar-los i comparar. Evita recorreguts `*` sense límit.
## Carregar CSV
`LOAD CSV WITH HEADERS` llegeix un CSV des d'una URL pública. Els camps arriben com a **text**: converteix-los amb `toInteger()`, `toFloat()`, `date()` o `toBoolean()`.

```cypher

LOAD CSV WITH HEADERS FROM 'https://exemple.cat/clients.csv' AS fila

MERGE (c:Client {client_id: fila.client_id})

SET c.nom = fila.nom;

```

*L'URL és només un exemple de sintaxi.* Carrega primer les restriccions, després els nodes i finalment les relacions.
## Índexs i PROFILE

```cypher

CREATE CONSTRAINT client_id IF NOT EXISTS

FOR (c:Client) REQUIRE c.client_id IS UNIQUE;

  

PROFILE MATCH (c:Client {client_id:'C001'})

RETURN c.nom;

```

- La restricció evita identificadors duplicats i crea l'índex corresponent.
- `PROFILE` mostra el pla real i els accessos a la base de dades (*db hits*).