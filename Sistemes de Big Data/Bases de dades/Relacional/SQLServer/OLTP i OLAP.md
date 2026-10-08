# OLTP i OLAP

SQLServer és una base de dades OLTP que és una base de dades que és:
- Insert, Update, Delete i Select
- Normalitzada

Però per fer operacions massives de SELECT, és a dir, consultes utilitzem sistemes ETL per poder passar i transformar les dades a una base de dades OLAP que és una base de dades amb les següents característiques:
- Només operacions SELECT
- Desnormalitzada
- Es prioritza VELOCITAT.

SQLServer treballa amb OLTP, però amb una aplicació que es diu SQL Server Integration Services instal·lada a Visual Studio ens permet transformar amb ETL a una base de dades OLAP per tenir a una mateixa base de dades els dos sistemes.

Sempre s'ha d'intentar fer els scripts SQL idempotents, que significa que sempre dona el mateix resultat, això vol dir que sempre hem de fer "CREATE TABLE IF NOT EXISTS" perquè així no passin coses rares en cas que ja existeixin.

Les bases de dades tenen un fitxer de transaccions que són les transaccions que s'han executat, això fa que entre backups si la base de dades falla, quan la recuperem restaurarem el backup i amb el fitxer de transaccions executarem totes les transaccions i tornarem a tenir la base de dades en el mateix estat que quan ha caigut.

> [!NOTE]
> A Python existeix una llibreria que es diu "faker" que ens permet crear dades massives vàlides aleatòries per provar les nostres bases de dades.
