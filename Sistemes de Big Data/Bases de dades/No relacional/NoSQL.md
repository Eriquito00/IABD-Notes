# NoSQL
El terme de NoSQL surt perquè els SGBD relacionals no es van dissenyar pensant en l'escalabilitat ni en la flexibilitat que demanen les modificacions freqüents.

Avui dia el món accelera molt i a les bases de dades relacionals els hi costava molt fer aquests canvis i portar-los a producció. NoSQL significa Not Only SQL. Les db relacionals segueixen els principis ACID.

Hi ha quatre tipus de sistemes de bases de dades NoSQL:
- **Clau-Valor**: Les més simples, cada element es desa com a parell clau valor com un diccionari. Exemple Redis. Cas d'us memória Caché.
- **Documentals**: Cada clau s'associa a un document que pot contenir parells, arrays i documents. Exemple MongoDB. Cas d'ús app web amb valors personalitzables.
- **Grafs**: Emmagatzemament informació sobre xarxes, com ara connexions socials. Exemple Neo4j.
- **Basats en columnes**: Optimitzats per a consultes sobre grans conjunts de dades; desen per columnes en lloc de per files. Exemple Cassandra.

També ha aparegut un nou terme que és el NewSQL, aquest trenca el teorema de CAP i compleix les 3 regles.

Les bases de dades no relacionals en comptes de ACID utilitzen BASE.
- BA: Basicaly Available, sempre respondra encara que no tingui conexio amb les repliques.
- S: Soft state, l'estat de les dades poden variar amb el temps sense necesitat de l'usuari.
- E: Eventually consistent, el sistema garantitza que si no s'actualitza una dada les repliques es sincronitzaran i acabaran mostrant el mateix valor.