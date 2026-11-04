# css

## Especificidad en CSS

Para saber qué regla gana cuando dos chocan, el navegador le da puntos a cada selector:

- Etiqueta (a, p, h2): 1 punto
- Clase (.btn-comprar): 10 puntos
- ID (#btn-destacado): 100 puntos

Por ejemplo, `header nav ul li a` suma 5 puntos porque tiene 5 etiquetas, y `.btn-comprar` suma 10 porque es una clase.

En el ejercicio le puse al botón del primer producto una clase y un ID, cada uno con un color distinto. Aunque la regla del ID estaba antes en el CSS, el botón se vio con el color del ID, porque 100 puntos le ganan a 10.

Para arreglarlo quité las reglas con ID y usé solo clases (`.btn-comprar` y `.btn-destacado`). Como ahora las dos valen 10 puntos, gana la que está más abajo en el archivo, y así no tuve que usar `!important`.

## Hallazgos: El Modelo de Caja

La diferencia entre margin y padding:

-El padding es el espacio dentro de la caja, entre el contenido y el borde; el fondo del elemento también cubre esa zona.
-El margin es el espacio fuera de la caja, que separa un elemento de los demás y no tiene color de fondo.

### 2. Por qué usar box-sizing: border-box

Por defecto el navegador suma el padding y el borde al ancho declarado, así
que una caja de 300px con padding de 20px termina midiendo 340px y puede desbordar a su contenedor. Con border-box el padding y el borde se incluyen
dentro del ancho, y la caja mantiene el tamaño que definí.

### 3. Efecto del margen en elementos hermanos

Al aplicar un margen grande a un elemento, este empuja a sus hermanos y los aleja, porque el margen crea una zona libre alrededor de la caja. Los demas se desplazan para respetar esa distancia.
