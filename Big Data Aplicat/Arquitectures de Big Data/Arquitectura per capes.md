# Arquitectura per capes

Aquesta arquitectura proposa 6 fases per separar responsabilitats i que fan la seva funció i passa la següent fase.

Les capes que proposa aquesta arquitectura són les següents:
- Ingesta: recull les dades de les fonts, APIs, fitxers, sensors...
- Col·lecció: transporta les dades i les prepara per anàlisi.
- Processament: transforma, neteja, enriqueix i classifica les dades. Suporta batch, streaming o híbrid.
- Emmagatzematge: persistir grans volums de dades (datalake) per mantenir històric
- Consulta: obté valor sobre les dades sense redissenyar el pipeline.
- Visualització: interfície d'usuari per mostrar les dades per prendre decisions.


## Principis SCV

Aquests principis són com el teorema de CAP, però aplicat al flux de càlcul analític. Els principis són:
- S (Speed): temps que triga a processar-se una dada desde que entra fins el resultat analític.
- C (Consitency): nivell d'exactitud del resultat.
- V (Volume): Quantitat total de dades que el sistema és capaç de processar.

Com a màxim un sistema de Big Data podrà optimitzar 2 de 3 variables simultàniament.
- S + C: s'han de reduir les dades a processar.
- C + V: se sacrifica velocitat augmentant el temps de resposta.
- S + V: sacrifica precisió.

## El marc WAF

Prové d'AWS i Azure per optimitzar la qualitat d'arquitectura del núvol i compta amb 5 pilars:
- Excel·lència operativa: capacitat d'operar, monitorar el pipeline per millorar contínuament processos.
- Seguretat: protegir la informació mitjançant mínims privilegis, encriptació i traçabilitat.
- Fiabilitat: tolerància a fallades, redundància, escalabilitat davant de canvis i capacitat de recuperació.
- Eficiència de rendiment: utilitzar els recursos adequats ajustats a una necessitat real.
- Optimització en costos: entregar valor analític al menor cost financer possible.