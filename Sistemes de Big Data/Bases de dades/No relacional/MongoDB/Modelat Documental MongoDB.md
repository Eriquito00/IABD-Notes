# Modelat Documental MongoDB

El modelatge a MongoDB seria equivalent als models entitat relació dels models relacionals. A MongoDB es fan a nivell de document directament, no com a SQL, a SQL fem un modelatge a nivell de taules, que serien les col·leccions de MongoDB, però a MongoDB es fan a nivell de document.
## Com fer modelatge a BD documentals
1. [Definir la carrega](Definir%20la%20carrega.md)
2. [Modelar relacions](Modelar%20relacions.md)
3. [Aplicar patrons](Aplicar%20patrons.md)

Els documents de Mongo poden ser polimòrfics, per tant, un document pot tenir certs camps que altre no en té.
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