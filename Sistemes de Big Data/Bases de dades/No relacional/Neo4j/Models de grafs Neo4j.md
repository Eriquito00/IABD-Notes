# Models de grafs Neo4j
## Quan utilitzar grafs?
Les bases de dades de grafs (com **Neo4j**) són útils quan el més important són les **connexions**: xarxes socials, recomanacions, detecció de frau o rutes.
- **SQL:** diversos salts acostumen a requerir diversos `JOIN` (o consultes recursives).
- **Graf:** segueix les relacions entre nodes (*index-free adjacency*). El recorregut depèn sobretot de la part del graf visitada.
## Elements del graf de propietats
| Element       | Què és?           | Exemple         |
| ------------- | ----------------- | --------------- |
| **Node**      | Entitat           | Una clienta     |
| **Etiqueta**  | Tipus de node     | `:Client`       |
| **Relació**   | Connexió dirigida | `:HA_COMPRAT`   |
| **Propietat** | Dada clau-valor   | `{nom: 'Aina'}` |
  
```cypher

(:Client {nom:'Aina'})-[:HA_COMPRAT {valoracio:5}]->(:Producte {nom:'Teclat'})

```
## De taules a grafs
- Fila d'una entitat → **node**.
- Clau forana → **relació**.
- Taula pont → **relació amb propietats**.
- Una dada com `Ciutat` pot ser **node** si la fem servir per connectar clients; si només es mostra, pot ser una propietat.

**Consell:** una comanda amb múltiples productes i dades pròpies pot ser un node `:Comanda`, no només una relació.