# Modelar relacions
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