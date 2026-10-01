# SQLServer

SQLServer és una base de dades OLTP que és una base de dades que és:
- Insert, Update, Delete i Select
- Normalitzada

Però per fer operacions massives de SELECT, és a dir, consultes utilitzem sistemes ETL per poder passar i transformar les dades a una base de dades OLAP que és una base de dades amb les següents característiques:
- Només operacions SELECT
- Desnormalitzada
- Es prioritza VELOCITAT.

SQLServer treballa amb OLTP, però amb una aplicació que es diu SQL Server Integration Services instal·lada a Visual Studio ens permet transformar amb ETL a una base de dades OLAP per tenir a una mateixa base de dades els dos sistemes.