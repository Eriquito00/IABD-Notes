# Estadística Univariant
Estadístics univariants que utilitzarem:
## Moda
- Moda (Mo): valor més freqüent. Si hi ha més de dues modes direm que són bimodals i si no hi ha cap direm que és una distribució amodal.

Exemple: 4,5,4,5,4,5,5,5,5,5,7,8,7 => Mo = 5
## Mediana
- Mediana (Md): És el valor central d'una llista de dades ordenades.

Exemple Imparells
Exemple: 6,7,8,9,10 => Md = 8

Exemple Parells
Exemple 6,7,8,9 => Md = 7 + 8 / 2 = 7.5
## Mitjana
- Mitjana (M): És la suma de tots els valors d'una llista dividida entre la quantitat de valors de la llista. **PERÒ ÉS AFECTADA PELS OUTLIERS PER TANT ÉS UNA VARIABLE POC ROBUSTA**.

Fórmula: 

$$ \bar{x} = \frac{\sum_{i=1}^{n} x_i}{n} $$
Exemple: \[4,5,6,7,8,9]

$$ {x} = \frac{4+5+6+7+8+9}{6}=\frac{39}{6}={6.5} $$

## Min
El valor mínim d'una llista

Exemple: \[4,5,6,7,8,9] = 4
## Max
El valor màxim d'una llista

Exemple: \[4,5,6,7,8,9] = 9
## Desviació estàndard

- Rang: Ens mostra el valor més gran i més petit per poder veure la desviació de les nostres dades.

Exemple 1: rang (200 - 60)  _pot ser que hi hagi un valor atípic_
Exemple 2: rang (75 - 55) _valor més precís i tenen un rang raonable._

Fórmula:

$$\sigma^2 = \frac{\sum_{i=1}^{N} (x_i - \bar x)^2}{N}$$
Exemple: \[3,5,6,10]

$${x}=\frac{3+5+6+10}{4}=\frac{24}{4}=\mathbf{6}$$
## Percentil
El percentil es divideix en 3 quartils:
- Q1: percentil 25: El 25% de les dades són menors o iguals.
- Q2: percentil 50 = mediana: El 50% de les dades a cada costat.
- Q3: percentil 75: El 75% de les dades són menors o iguals.

![](../../Imatges/Pasted%20image%2020261002190014.png)

Aquí podem veure un diagrama molt utilitzat a estadística que és un **"Boxplot"** que bàsicament consisteix a agafar els valors entre el Q1 i Q3 com a **"Rang interquartílic"**. Després d'aquí agafem cap avall de Q1 i cap a dalt de Q3 un 1.5 de les dades i tot el que estigui per sobre o per sota d'aquests límits es consideraran **Outliers**.

Aquí podem veure un exemple amb una llista de les dades següents:

Dades: \[60, 65, 77, 78, 80, 82, 85, 100, 105]

![](../../Imatges/Pasted%20image%2020261002190652.png)

Aquí podem veure que el Q1 és 77 i el Q3 85, entre aquestes dades el rang interquartílic és 8.

Per tant, per calcular els límits de dalt i baix és 8 x 1.5 que serà 12.

Totes les dades que siguin inferiors a 77 (Q1) menys la desviació que és 12 que són 65, per tant, menys de 65 es considera outlier.

Totes les dades que siguin superiors a 85 (Q3) més la desviació que és 12 que són 97, per tant, més de 97 es considera outlier.

Per tant, en conclusió el 60, 100 i 105 els consideraríem outlier, però segons els requisits del model els permetrem o els treure'm.