# Els 5 pilars del Big Data

Les 5 V del Big Data. Però no s'ha d'oblidar que **s'ha d'evitar la sobreenginyeria**.

![](../../Imatges/Pasted%20image%2020260915181007.png)

Per donar resposta als dos reptes principals de la infraestructura de Big Data (**volum** i **velocitat**), qualsevol arquitectura ha de complir 5 pilars bàsics:

- **Escalabilitat**: Capacitat d'augmentar de manera senzilla tant ampliar el sistema amb més màquines com amb més emmagatzematge a la màquina.
- **Tolerància a fallades**: Garantir la disponibilitat del sistema i evitar la pèrdua de dades si falla qualsevol màquina.
- **Dades distribuïdes**: Emmagatzemar la informació repartida entre diferents nodes o màquines per evitar punts únics de fallada (**SPOF**).
- **Processament distribuït**: Executar els càlculs entre múltiples màquines per reduir els temps d'execució.
- **Localització de la dada**: Apropar els processos a la dada emmagatzemada per minimitzar el tràfic de xarxa i la latència.

Seguint els marcs **WAF** (AWS i Azure Well-Architected Framework), es busca evitar la sobreenginyeria mitjançant principis com:

- **Elasticitat**: Dissenyar per escalar tant cap amunt com cap avall (fins i tot a zero en entorns _serverless_).
- **Baix acoblament**: Ús de microserveis, cues de missatges i APIs REST per poder substituir components sense afectar la resta.
- **Decisions reversibles**: Aplicar el principi de les dues portes de Jeff Bezos per desfer decisions errònies fàcilment.
- **Seguretat i confiabilitat**: Aplicar el principi del mínim privilegi i el model de responsabilitat compartida, tenint present la cita de Werner Vogels (_"Everything fails, all the time"_).

## Tipus de dades

Hi ha tipus de dades segons la nostra necessitat que es classifiquen entre:
- Dades calentes: dades que es consulten constantment
- Dades tèbies: dades que es consulten, però no diàriament
- Dades fredes: dades que no es solen utilitzar.