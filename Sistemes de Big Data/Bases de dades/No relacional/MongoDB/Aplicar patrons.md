# Aplicar patrons
### Patró atribut
Per exemple si tenim un producte amb diferent preu segons el país, doncs crearem una array de preus segons el país.

```JSON
{
  producte: "ps5",
  preus: [

    { pais: "es", preu: 549.99, moneda: "eur" },

    { pais: "uk", preu: 479.99, moneda: "gbp" },

    { pais: "us", preu: 499.99, moneda: "usd" } ]

}
```

### Patró de versionat de documents
Ens permet poder tenir dues versions d'un document.

![](../../../../Imatges/Pasted%20image%2020261029173939.png)

### Patró de versionat d'esquema
Podem tenir diferents versions d'un esquema de document.

```JSON
V1

{
  _id: ObjectId("12345"),
  nom: "Francesc Barragan",
  email: "fbarragan@sapalomera.cat",
  telefon: "610 01 23 45"
}
```

```JSON
V2

{
  _id: ObjectId("12345"),
  schema_version: 2,
  nom: "Francesc Barragan",
  contacte: {
    email: "fbarragan@sapalomera.cat",
    telefon: "610 01 23 45",
    twitter: "@francescbarragan" }
}
```
### Patró polimòrfic
Documents amb similitud però són diferents.

```JSON
{
  tipus: "actor",
  nom: "Penélope Cruz",
  naixement: ISODate("1974-04-28"),
  lloc: "Alcobendas",
  actor: { pellicules: 80, premis: ["Oscar"] } 
}

{ 
  tipus: "director",
  nom: "Pedro Almodóvar",
  naixement: ISODate("1949-09-25"),
  lloc: "Calzada de Calatrava",
  director: { productora: "El Deseo" } 
}
```
### Patró subconjunt
Creant un document amb les dades que més es consulten i després crear un document de detalls amb la resta de dades.

![](../../../../Imatges/Pasted%20image%2020261029174442.png)
### Patró calculat
Volem evitar fer constantment recàlculs de dades de lectura.

### Patró bucket
En comptes de fer un document per cada mesura podem fer un únic document amb les dades de la mesura per cada minut, hora o dia segons la necessitat del programa.

```JSON
{ 
  sensor_id: 12345,
  timestamp: ISODate("2025-01-31T10:00:00Z"),
  temp: 40 
},
{ 
  sensor_id: 12345,
  timestamp: ISODate("2025-01-31T10:01:00Z"),
  temp: 40 
}


NOU DOC

{ 
  sensor_id: 12345,
  data_inici: ISODate("2025-01-31T10:00:00Z"),
  data_fi:    ISODate("2025-01-31T10:59:59Z"),
  mesures: [
    { 
      timestamp: ISODate("2025-01-31T10:00:00Z"),
      temp: 40 
    },
    { 
      timestamp: ISODate("2025-01-31T10:01:00Z"),
      temp: 40 
    }
  ],
  quantitat: 2,
  suma: 80 
}
```
### Altres patrons i casos d'ús

![](../../../../Imatges/Pasted%20image%2020261029173614.png)