# Arquitectura Kappa

Va néixer com una **evolució i simplificació de l'Arquitectura Lambda** per eliminar la duplicitat de codi i d'infraestructura. En lloc de tenir dos camins paral·lels, l'arquitectura Kappa proposa un **únic camí basat en el processament en _streaming_**.

Totes les dades s'assimilen com un flux continu d'esdeveniments (_stream_) de forma immutable i es processen mitjançant un sol motor de processament.

![](../../Imatges/Pasted%20image%2020260921181015.png)
### Component de l'Arquitectura

Log d'Esdeveniments
- Actua com a registre històric immutable on les dades s'emmagatzemen per ordre d'arribada.
 
- Permet **reproduir les dades des del principi** si cal recalcular o aplicar noves regles de negoci.

Capa de Processament en Streaming (_Stream Processing_):    
- Unifica el camí lent i el ràpid** en un sol motor

- Processa les dades en temps real a mesura que van arribant.

- Si es necessita reprocessar dades històriques (el que abans feia la capa Batch), es torna a llegir el flux d'esdeveniments des del punt desitjat en el Log d'Emmagatzematge.

Capa Serving:
- Emmagatzema i indexa les vistes o resultats generats pel motor de processament en temps real.

- A diferència de Lambda, Serving només consulta un únic origen de dades, simplificant les consultes de l'aplicació.