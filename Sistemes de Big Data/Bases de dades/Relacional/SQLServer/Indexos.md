# Indexos

Els índexs fan un recorregut des del SELECT fins a la taula més eficaç, ja que té certes dades i no cal consultar-les totes, per tant, és útil indexar les columnes que més s'utilitzen a les consultes. El recorregut és per exemple si indexem CodiPostal: 

SELECT (de CodiPostal) -> ÍNDEX -> RESPOSTA

Si en general hi ha una taula que s'utilitza moltíssim podem crear una optimització de memòria que bàsicament és que ens carrega aquesta taula a la memòria RAM, però clar aquesta taula no pot contenir claus foranes (Foreign Keys).

Això és perquè les claus foranes fan que es faci una consulta per comprovar que realment existeixi, per això quan es fan càrregues massives de dades se segueix un procediment per fer més ràpidament la carrega:
- Desactivar claus foranes.
- Fer la càrrega massiva de dades.
- Activar claus foranes.
- La base de dades farà comprovacions que existeixin i si no llençarà una advertència que no existeix.

Les eines que utilitzem per poder fer optimització de la base de dades SQL són les que s'instal·len per defecte quan instal·lem SQL Server Management Studio, que ve amb una eina que podem fer traça que bàsicament fa un registre durant el temps que vulguem de totes les sentencies SQL que es fan sobre una base de dades amb l'eina SQL Server Profiler, després podem pasar-li a altra eina que ens proposa canvis al nostre disseny de la base de dades d'acord amb aquest traç.