# Arquitectura Lambda

Va néixer per **unificar en una mateixa infraestructura el processament batch i el processament en streaming**

Les dades que entren al sistema es dupliquen i flueixen paral·lelament per dos camins:

- **Capa Batch (camí lent)**: Guarda les dades en tal com arriben de forma immutable i recalcula periòdicament el conjunt històric per generar vistes d'alta precisió.
- **Capa Streaming / Speed (camí ràpid)**: Tracta només els esdeveniments recents mitjançant algorismes incrementals per oferir respostes immediates a costa d'assumir una lleugera pèrdua de precisió.
- **Capa Serving**: Fusiona i indexa els resultats de les dues capes per respondre a les consultes amb informació completa i actualitzada.
