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



## Hallazgos: Centralización de Identidad Visual y Espaciado

### :root

Se definieron variables CSS en el selector `:root` para centralizar:

- **Colores:** `--color-primario`, `--color-secundario`, `--color-fondo`, `--color-texto`
- **Tipografía:** `--fuente-principal`la cual use roboto
- **Espaciados:** `--espacio-s` (8px), `--espacio-m` (16px), `--espacio-l` (24px)

### Tipografía

Se importó la fuente Roboto desde Google Fonts mediante `<link>` en el `<head>`
del HTML, antes de la hoja de estilos, y se aplicó con `var(--fuente-principal)`.

### Uso de var() en padding y margin

Los valores de `padding` y `margin` de los elementos `header`, `main`, `article`,
`aside` y `footer` se reemplazaron por `var(--espaciado-pequeño)`, `var(--espaciado-medio)` y
`var(--espaciado-grande)`.

### Ventajas encontradas

- **Mantenimiento:** cambiar un color o espaciado se hace en un solo lugar y se refleja en toda la página.
- **Consistencia:** todos los elementos usan los mismos valores.
- **Legibilidad:** nombres como `--color-primario` explican el propósito del valor.
- **Escalabilidad:** permite crear nuevos temas redefiniendo solo las variables.

Antes de usar variables, un cambio de identidad visual exigía buscar y editar
cada valor repetido. Con `:root` y `var()`, el cambio es inmediato y con menos
riesgo de errores.



## Hallazgos: Pseudoclases y Pseudoelementos

### 1. Diferencia entre pseudoclase y pseudoelemento

Una pseudoclase (como :hover, :focus o :active) hace que un elemento cambie
de estilo según lo que hace el usuario, por ejemplo cuando pasa el cursor o
hace clic. Un pseudoelemento (como ::before o ::after) no reacciona a nada,
sino que agrega contenido decorativo antes o después de un elemento desde el CSS.

### 2. Importancia de :focus para la accesibilidad

Hay personas que no usan mouse y navegan con la tecla Tab. La pseudoclase
:focus muestra un contorno en el enlace donde está el teclado, así saben
en qué parte de la página están. Sin ella, esos usuarios se perderían.

### 3. Por qué usar ::before y ::after para lo decorativo

Los adornos, como la insignia de oferta o el punto del menú, no son contenido
real de la página. Si los pongo desde el CSS, el HTML queda limpio y
semántico, sin etiquetas vacías que solo sirven de decoración, y es más
fácil cambiarlos o quitarlos desde un solo lugar.



## Hallazgos: Arquitectura BEM y Modularidad

### 1. ¿Qué significa BEM y cómo se escribe?

BEM significa **Bloque, Elemento, Modificador** (en inglés _Block, Element, Modifier_). Es una forma de nombrar las clases de CSS para que el nombre ya diga qué es cada cosa y a qué pertenece.

- **Bloque:** el componente completo e independiente. Ejemplo: `.card`.
- **Elemento:** una parte que solo tiene sentido dentro de su bloque. Se escribe con doble guion bajo `__`. Ejemplo: `.card__title`, `.card__price`.
- **Modificador:** una variación del bloque o del elemento. Se escribe con doble guion medio `--`. Ejemplo: `.card--destacada`, `.btn--secundario`.

Visualmente se escribe así: `bloque__elemento--modificador`.

En mi catálogo lo apliqué así:

```html
<article class="card card--destacada">
  <h3 class="card__title">Producto 1 - pantalon liso</h3>
  <p class="card__price">Precio: $40.000</p>
  <a href="#" class="btn btn--destacado">Comprar</a>
</article>
```

### 2. ¿Por qué dividir el CSS en varios archivos facilita el trabajo colaborativo?

Antes tenía todo mezclado en un solo `style.css`. Ahora separé los componentes en `css/tarjeta.css`, `css/botones.css` y `css/menu-lateral.css`, y los conecté con `@import`.

Esto ayuda al trabajo en equipo porque:

- Cada persona puede trabajar en un componente distinto (uno en las tarjetas, otro en los botones) sin editar el mismo archivo al mismo tiempo.
- Se reducen los conflictos al hacer `merge` en Git, porque los cambios quedan en archivos diferentes.
- Es más fácil encontrar algo: si hay un problema con un botón, se abre `botones.css` y no hay que buscar entre cientos de líneas.
- Un componente se puede reutilizar o modificar sin afectar a los demás.

### 3. ¿Cómo evita BEM la guerra de especificidad (Cascada)?

En CSS, cuando dos reglas apuntan al mismo elemento, gana la que tiene más especificidad. Con selectores anidados como `.producto h2` (una clase más una etiqueta) la especificidad sube, y para sobrescribir esa regla después hay que escribir.





## Hallazgos: Arquitectura Total con Flexbox

### 1. Control del pie de página

Utilicé `min-height: 100vh` para que la página ocupara como mínimo toda la altura de la pantalla. También usé `flex-direction: column` para organizar el encabezado, el contenido y el pie de página uno debajo del otro. Esto ayudó a que el footer quedara al final de la página.

### 2. Uso de gap en la navegación

Utilicé `gap` para separar los elementos de los menús de navegación. Así pude dejar un espacio entre los enlaces sin tener que colocar márgenes a cada uno.

### 3. Organización de las tarjetas con Flexbox

Con BEM pude organizar mejor las clases de las tarjetas, con las Custom Properties reutilicé los colores y espacios que ya tenía definidos, y con Flexbox organicé los elementos de cada producto. También utilicé `flex-grow: 1` para que las descripciones ocuparan el espacio disponible y los botones quedaran mejor alineados, aunque los textos fueran de diferentes tamaños.