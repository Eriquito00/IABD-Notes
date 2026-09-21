# Arquitectura Lambda

Va néixer per **unificar en una mateixa infraestructura el processament batch i el processament en streaming**

Les dades que entren al sistema es dupliquen i flueixen paral·lelament per dos camins:

- **Capa Batch (camí lent)**: Guarda les dades en tal com arriben de forma immutable i recalcula periòdicament el conjunt històric per generar vistes d'alta precisió.
- **Capa Streaming / Speed (camí ràpid)**: Tracta només els esdeveniments recents mitjançant algorismes incrementals per oferir respostes immediates a costa d'assumir una lleugera pèrdua de precisió. El temps que actua el speed el posem nosaltres segons els algoritmes i eines que utilitzem.
- **Capa Serving**: Fusiona i indexa els resultats de les dues capes per respondre a les consultes amb informació completa i actualitzada.

![](../../Imatges/Pasted%20image%2020260921161853.png)

Ara amb l'arquitectura lambda diguem que l'usuari segons la consulta que tingui serving anirà a una capa o altre, però l'aplicació sempre anirà a serving i d'aquí segons si la consulta és per exemple "històric de cerques d'un usuari els últims 3 mesos" anirà al batch si és per exemple "activitat d'un usuari últims 5 minuts" anirà al speed.

L'arquitectura lambda té sentit quan necessitem baixa latència i un històric, **sobretot si necessitem diferents tractaments per batch i streaming**.

Aconseguim utilitzar a la vegada els dos tipus de processament de dades, podem tenir un històric de les dades, baixa latència, flexibilitat ja que podem obtenir els dos tipus de processament. Però tenia una gran càrrega, ja que es processava dues vegades la mateixa dada i això feia que fos un sistema molt complex i costos, per tant, va néixer una nova arquitectura que fusionava els dos tipus de processament en una mateixa línia és a dir sense processar dues vegades la mateixa dada.