# Modelat Documental MongoDB

El modelatge a MongoDB seria equivalent als models entitat relació dels models relacionals. A MongoDB es fan a nivell de document directament, no com a SQL, a SQL fem un modelatge a nivell de taules, que serien les col·leccions de MongoDB, però a MongoDB es fan a nivell de document.
## Com fer modelatge a BD documentals
1. Definir la càrrega
2. Modelar les relacions
3. Aplicar patrons

Els documents de Mongo poden ser polimòrfics, per tant, un document pot tenir certs camps que altre no en té.
## Definir la càrrega: Simplicitat o rendiment?
Quan hem de fer un modelatge hem de decidir si volem rendiment o simplicitat. Escollirem un o altre segons el volum de les dades. Si en tenim poc nivell de dades prioritzarem simplicitat, si treballem amb gran quantitat de dades triarem rendiment.

| Fase | Simplicitat                       | Simplicitat i rendiment                                           | Rendiment                                              |
| ---- | --------------------------------- | ----------------------------------------------------------------- | ------------------------------------------------------ |
| 1    | Operacions mes frequents          | Majoria d'operacions · mida de les dades · quantitat d'operacions | Totes les operacions · mida · quantitat · qualificació |
| 2    | Embeure sempre que sigui possible | Embeure i relacionar                                              | Embeure i relacionar                                   |
| 3    | Pocs patrons, els impresindibles  | Alguns patrons més                                                | Tots els necessaris (calculat, galleda, subconjunt…)   |
## Modelar relacions
Existeixen dues tècniques per relacionar documents:
- Referències: 
	1. Es desa el \_id d'un document en un altre. 
	2. Cal una segona consulta amb un $lookup. 
	3. Evita la duplicació de dades.
- Dades embegudes: 
	1. Subdocuments dins d'un camp o array. 
	2. Una sola consulta i una sola escriptura atòmica. 
	3. Límit de 16 MB per document.

Embeure ofereix més rendiment amb una única operació llegim o escrivim totes les dades relacionades.
### Referències manuals
Crearem un document d'usuari i un document de contacte amb el id de l'usuari.
### Dades embegudes
Crearem un document d'usuari i posarem tota la informació d'aquest usuari dins del mateix document.
#### Relacions 1:1 
Seria per exemple un usuari té una adreça doncs posarem l'adreça a aquest document.
#### Relacions 1:N
Té 3 tipus:
- 1: pocs: Crearem una array amb els registres.
- 1: molts: Si tenim uns quants portarem l'array al que tindrà més registres, per exemple una editorial té molts llibres, portaríem la id de les editorials als llibres.
- 1: moltíssims: Si tenim uns quants portarem l'array al que tindrà més registres, per exemple una aplicació que genera logs, portaríem la id de l'aplicació a cada log.
#### Relacions N:M
Té 3 tipus:
- Crear un document per cadascun i un intermedi que tingui els 2 ids.
- Crear els dos documents i els 2 tenen les dues arrays.
- Crear els dos documents i escollir un que tingui menys registres i que es quedi amb la array.

## Aplicar patrons

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
## Validació d'esquemes
Per crear una validació de documents a MongoDB podem fer el següent:

```JSON
validador = { 
	validator: { 
	    $jsonSchema: {
			bsonType: "object",
			required: ["nom", "dataNaix", "contacte"],
			title: "Validador de persones",
			properties: {
				nom: { 
					bsonType: "string",
					description: "cadena obligatòria"
				},
				cognoms: { bsonType: "string" },
				dataNaix: { bsonType: "date" },
				aficions: {
					bsonType: "array",
					uniqueItems: true,
					items: { bsonType: "string" } 
				},
				casat: { bsonType: "bool" },
				fills: {
					bsonType: "int",
					minimum: 0,
					maximum: 49 
				},
				contacte: {
					bsonType: "object", 
					required: ["email"],
					properties: {
						twitter: { bsonType: "string" },
						email:   { bsonType: "string" }
					} 
				},
				dataCreacio: { bsonType: "timestamp" }
			} 
		} 
	} 
}
```