# Tipus de processament
## Processament Batch (en lots)

Processar en Batch és bàsicament processar amb un inici i final temporal de dades, això té un punt dèbil que és bàsicament que **no es pot treballar en temps real**.

Un exemple de feina i on es pot aplicar el processament Batch és a aplicacions que no necessitem processar en temps real. Els passos de Batch serien:
1. Analitzar un procés de dades de l'inici del projecte fins ahir
2. Donar un anàlisi de processament de l'inici fins al dia 1/1/2027.
3. L'endemà analitzar les dades de l'inici fins al dia 2/1/2027.

Un exemple de processament streaming seria per exemple un supermercat que vol un recompte de diners que ha fet en un dia.
## Processament Streaming

Processar nova informació a mesura que arriba sense un final temporal, és a dir, mentre van arribant les dades es van processant, per tant, tot i que no es poden obtenir les dades en temps real, ja que sempre hi ha una mica de latència, però seria per altres utilitats.

Un exemple de processament streaming seria per exemple una fàbrica 24 hores que vol saber l'estat del maquinari.

## Diferències entre Batch i Streaming

| Aspecte  |            Batch             |         Streaming         |
| :------: | :--------------------------: | :-----------------------: |
| Entrada  |       Conjunt de dades       |       Flux continu        |
|  Temps   |        Inici i final         |   Sense final temporal    |
| Resposta |    Pot tenir més latencia    |      Baixa latencia       |
| Us tipic | Informes i calculs historics | Esdeveniments i anomalies |
Exemples diferents entre Batch i Streaming

| Cas                                                  | Processament |
| :--------------------------------------------------- | :----------- |
| Informe mensual de vendes                            | Batch        |
| Detectar una temperatira perillosa                   | Streaming    |
| Recalcular cada nit l'historic                       | Batch        |
| Detectar una transaccio sospitosa mentre es produeix | Streaming    |
| Calcular les notes finals d'un curs                  | Batch        |
| Mostra els usuaris conectats ara                     | Streaming    |
