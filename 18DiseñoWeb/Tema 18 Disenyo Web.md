# Tema 18 Diseño Web. HTML5 y CSS3

## HTML5

La estructura básica de un documento HTML5 es:

~~~ html
<!DOCTYPE html>
<html>
  <head>
  ...
  </head>
  <body>
  ...
  </body>
</html>
~~~

El elemento `<!DOCTYPE html>` indica que estamos usando la última versión del lenguaje HTML.

El elemento `<html>` contiene el 
documento.

El elemento `<head>` contiene metadatos acerca del documento, por ejemplo su título, enlaces a otros documentos o el
encoding:

~~~ html
<head>
  <title>Welcome</title>
  <link rel="stylesheet" href="./assets/main.css">
  <meta charset="utf-8">
</head>
~~~

En el ejemplo de arriba, el elemento `<link>` enlaza el documento a una hoja de estilos (el tipo de relación lo determina 
el atributo `rel`) que se encuentra en la ruta contenida en el atributo `href`.

El elemento `<body>` contiene el cuerpo del documento.
### Elementos

HTML5 (a diferencia de sus antecesores) da estructura al contenido mediante el uso de elementos con significado semántico.

Hay varios tipos de elemento:

- para layout: `div` y `span`
- basados en texto (tienen contenido semántico): `h1` a `h6`, `p`
- para dar estructura (tienen contenido semántico): `header`, `footer`, `section`, `article`, `aside`, `nav`

### Layout

Los elementos para dar layout al contenido son `div` y `span`. Se trata de contenedores genéricos que no tienen
significado semántico.

`<div>` se usa para dividir el documento en bloques de una o más líneas. Por defecto los bloques se apilan uno encima de
otro, pero usando CSS3 se pueden posicionar donde queramos, permitiendo crear layouts muy complejos.

`<span>` se usa para contener partes de una línea a las que se quiere dar un estilo concreto.

La mayoría de elementos en HTML5 son, desde el punto de vista de su presentación en pantalla, o bien block-level o bien
inline-level.

Los block-level, como `<div>`:

- empiezan en una nueva línea
- se apilan uno encima de otro
- ocupan toda la anchura disponible
- se pueden anidar
- pueden contener elementos del tipo inline-level

Los inline-level, como `<span>`:

- no empiezan en una nueva línea
- se pueden alinear uno al lado del otro
- utilizan solo la anchura de su contenido
- se pueden anidar
- *no* pueden contener elementos del tipo block-level
### Hiperenlaces

Los hiperenlaces permiten enlazar un recurso con otro. En HTML5 se crean con el elemento anchor, `<a>`, que es de
tipo inline-level. Por ejemplo:

~~~ html
<a href="https://python.org">Python website</a>
~~~

El atributo `href` (hyperlink reference) identifica el distino del hiperenlace. Al hacer clic en el contenido del anchor,
que en este caso es un texto, se nos redirige a esa URL.

También se puede enlazar a la misma página en la que está el anchor, utilizando como destino un **fragment URL**, o sea 
`#` seguido del valor del atributo `id` del elemento al que queremos enlazar. Por ejemplo:

~~~ html
<body id="top">
  ...
  <a href="#top">Back to top</a>
  ...
</body>
~~~

Otros valores de `href` (como URLs `mailto:`, URLs `tel:` o media fragments) permiten enlazar a una dirección de correo 
(necesita un cliente de correo), a un teléfono (necesita una aplicación de videoconferencia), descargar una imagen 
(necesita JavaScript)...:

~~~ html
<body id="top">
  ...
  <a href="mailto: vmas@example.org">Mail me please</a>
  ...
</body>
~~~

El browsing context (pestaña, ventana o `iframe`) donde se abrirá la URL de destino se determina con el atributo 
`target`:

- _self: the current browsing context. (Default)
- _blank: usually a new tab, but users can configure browsers to open a new window instead.
- _parent: the parent browsing context of the current one. If no parent, behaves as _self.
- _top: the topmost browsing context (the "highest" context that's an ancestor of the current one). If no ancestors, 
  behaves as _self.
### Codificación de carácteres especiales

[Character encoding](https://en.wikipedia.org/wiki/Character_encoding) is the process of assigning numbers to graphical characters, especially the written characters of 
human language, allowing them to be stored, transmitted, and transformed using digital computers. The numerical 
values that make up a character encoding are known as "code points" and collectively comprise a "code space", a "code 
page", or a "character map".

Un charset o character set es el conjunto de caracteres que se va a codificar. Algunos ejemplos son el charset ASCII, el
EBCDIC y el [Unicode](https://en.wikipedia.org/wiki/Unicode) (que se puede implementar por varios encodings), que 
incluyen tanto caracteres gráficos como caracteres de 
control (delete, backspace, escape, EOL, EOF).

El character encoding o encoding es la codificación concreta que se usa para codificar un charset, por ejemplo
[ASCII (7 bits)](https://en.wikipedia.org/wiki/ASCII), 
[EBCDIC (8 bits)](https://en.wikipedia.org/wiki/EBCDIC), [UTF-8](https://en.wikipedia.org/wiki/UTF-8),
[UTF-16](https://en.wikipedia.org/wiki/UTF-16), [UTF-32](https://en.wikipedia.org/wiki/UTF-32)...

UTF-8 (code unit de 8 bits) usa una anchura variable entre uno y cuatro bytes de 8 bits. UTF-16 (code unit de 16 bits) 
usa una anchura variable entre uno y dos bytes de 16 bits. UTF-32 (code unit de 32 bits) usa una anchura fija de 1 byte
de 32 bits.

Como ejemplo, la codificación del carácter A en ASCII es:

~~~
A -> 65 (1000001)
~~~

HTML4 fue la primera versión de HTML en usar encodings para tratar los caracteres internacionales de manera decente.

Para un documento HTML hay dos maneras de especificar el encoding:

- en el webserver, usando la cabecera `Content-type`

~~~
Content-Type: text/html; charset=ISO-8859-4
~~~

Esto permite al webserver alterar el encoding del documento durante la negociación de contenidos con el cliente.

- en el documento HTML5, dentro del elemento `<head>`

~~~ html
<meta charset="utf-8">
~~~

En HTML5 el encoding recomendado es [UTF-8](https://en.wikipedia.org/wiki/UTF-8).

Una manera de codificar caracteres en HTML5 sin especificar un encoding es usar **referencias de caracter**, que pueden
ser numéricas (indicando el code point en formato `&#nnnn;` o `&#xhhhh`) o de entidad (con el formato `&nombre;`, 
por ejemplo `&Aacute;`, `&nbsp;` o `&lt;`).


### Tablas

~~~ html
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Instrument</th>
    </tr>
  </thead>
  <tfoot>
    <tr>
      <th colspan="2">Musicians and... Instruments!</th>
    </tr>
  </tfoot>
  <tbody>
    <tr>
      <td>John Lennon</td>
      <td>Rhythm Guitar</td>
    </tr>
    <tr>
      <td>Paul McCartney</td>
      <td>Bass</td>
    </tr>
    <tr>
      <td>George Harrison</td>
      <td>Lead Guitar</td>
    </tr>
    <tr>
      <td>Ringo Starr</td>
      <td>Drums</td>
    </tr>
  </tbody>
</table>
~~~

~~~ css
table, tr {
  margin-top: 1px;
}

tr:nth-child(2n) {
  background-color: rgba(150, 212, 212, 0.4);
}

thead th {
  background-color: rgba(350, 212, 212, 0.4);
}

tfoot th {
  background-color: rgba(050, 212, 212, 0.4);
}
~~~

El resultado es

```{thumbnail} images/tabla_001.png
```
## CSS3

El HTML5 se usa para crear contenido. El CSS3 es el lenguaje que se usa para dar estilo visual a los documentos HTML5.

Una hoja de estilo (un fichero `.css`) contiene un grupo de reglas que el navegador web interpreta y después aplica
a la página web que está mostrando. Cada regla tiene tres componentes, selector, propiedad[es] y valor:

~~~ css
selector {propiedad: valor;}
~~~

Por ejemplo:

~~~ css
p {font-size: 12px;
   background: blue;
}
~~~

## Selectores

Hay varios tipos de selector:

- de tipo: selecciona (se aplica) todos los elementos de un tipo dado
- de clase: se aplican a todos los elementos que tienen un atributo `class` con un valor dado
- de ID: selecciona un elemento basándose en el valor de su atributo `id`. Solo puede haber un elemento con un 
  determinado ID dentro de un documento.
- universal: selecciona todos los elementos del documento
- de atributo: selecciona elementos basándose en el valor de un determinado atributo

Ejemplos:

~~~ css
div {
}
.button {
}
#top {
}
* {

}
[autoplay] {
}
[autoplay="false"] {
}
~~~

Una regla se puede aplicar a una combinación de selectores. Las combinaciones más importantes son:

- grupos de selectores. El estilo se aplica a cada uno de los selectores separados por coma.

~~~ css
sel1, sel2, sel3 {}
~~~

- hermanos adyacentes. El segundo elemento sigue directamente al primero y ambos comparten el mismo padre

~~~ css
h1 + p {
}
~~~

Esta regla se aplicará a todos los elementos `<p>` que sigan directamente a un elemento `<h1>`.

- hermanos. El segundo elemento sigue al primero (no necesariamente de forma inmediata) y ambos comparten el mismo padre.

~~~ css
h1 ~ p {
}
~~~

Esta regla se aplicará a todos los elementos `<p>` que sigan a un elemento `<h1>`.

- hijo. Selecciona los elementos que son hijos directo del primero.

~~~ css
ul > li {
}
~~~

Esta regla se aplicará a todos los elementos `<li>` que son hijos directos de un elemento `<ul>`.

- descendientes. Selecciona los elementos que son descendientes del primer elemento.

~~~ css
div span {
}
~~~

- columna

### Pseudoclases y pseudoelementos

Las **pseudoclases** permiten la selección de elementos, basada en información de estado que *no* está contenida en el 
árbol de documentos. Por ejemplo, la regla `a:visited` se aplicará a todos los elementos `<a>` que hayan sido visitados 
por el usuario.

Los **pseudoelementos** son abstracciones del árbol que representan entidades más allá de los elementos HTML. Por ejemplo, 
HTML no tiene un elemento que describa la primera línea de un párrafo ni los marcadores de una lista. Los pseudoelementos 
representan estas entidades y nos permiten asignarles reglas CSS. De este modo podemos diseñar estas entidades de forma 
independiente.

Ejemplo: La regla `p::first-line` se aplicará a la primera línea de texto de todos los elementos `<p>`.

[Índice de pseudoclases](https://developer.mozilla.org/es/docs/Web/CSS/Pseudo-classes).

[Índice de pseudoelementos](https://developer.mozilla.org/es/docs/Web/CSS/Pseudo-elements).
## Cascada, especificidad y herencia

En una hoja CSS todos los estilos/reglas se aplican en cascada, es decir, de arriba a abajo, permitiendo añadir nuevos 
estilos o sobreescribir estilos existentes. Por ejemplo:

~~~ css
p {font-size: 12px;
   background: blue;
}
p {font-size: 24px;
}
~~~

El resultado final es que los párrafos tendrán un fondo azul pero un tamaño de caracter de 24 píxeles. La cascada también
se aplica a las propiedades dentro de un estilo. Por ejemplo:

~~~ css
p {background: blue;
   background: orange;
}
~~~

Aquí el párrafo tendrá un fondo de color naranja porque la última declaración sobreescribe a las anteriores.

La cascada no es el único factor que influye en el orden en que se aplican los estilos. También hay que tener en 
cuenta la especificidad de los selectores.

Cada tipo de selector tiene una **especificidad o peso**. El selector universal, los combinadores y la pseudoclase
`:not()` no tienen peso.

Los selectores de tipo tienen una especificidad de 0-0-1, la menor de todas. Los de clase tiene una especificidad de 
0-1-0. Los de ID tienen una especificidad de 1-0-0, la mayor de todas.

```{important} La especificidad solo se aplica cuando el mismo elemento es objetivo de múltiples declaraciones. Según las reglas de 
CSS, en caso de que un elemento sea objeto de una declaración directa, esta siempre tendrá preferencia sobre las reglas 
heredadas de su ancestro.
```

También es significativo el concepto de **herencia**, que significa que algunas propiedades CSS heredan por defecto los 
valores establecidos en el elemento padre, pero otras no. Esto también puede causar una respuesta diferente a la que 
esperas. Por ejemplo, el color y el tamaño de fuente se heredan, pero la anchura, el margen, los bordes y el padding no.

[Referencia](https://developer.mozilla.org/es/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance).
## El modelo de cajas

El modelo de caja CSS  es un módulo  CSS que define cajas rectangulares, incluyendo sus rellenos y márgenes, que son 
generadas para los elementos y que se disponen de acuerdo al modelo de formato visual.

[Referencia](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model).

La propiedad `box-sizing` puede tener dos valores, `content-box` (valor por defecto) o `border-box`.
## Layout

The size and position of an element are often impacted by its containing block. Percentage values that are applied to the
width, height, padding, margin, and offset properties of an absolutely positioned element (i.e., which has its 
`position` property set to `absolute` or `fixed`) are computed from the element's containing block.

[Containing block](https://developer.mozilla.org/en-US/docs/Web/CSS/Containing_block)

[Apilamiento](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/Stacking_without_z-index)

[Stacking context](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)

The stacking context is a three-dimensional conceptualization of HTML elements along an imaginary z-axis relative to the 
user, who is assumed to be facing the viewport or the webpage. HTML elements occupy this space in priority order based on
element attributes.

En el layout hay tres aspectos que conviene conocer:

- posicionamiento
- apilamiento y solapamiento
- como flotar elementos

Por defecto (`position: static`) los elementos de bloque se muestran de arriba a abajo en el orden en el que se crean en 
el documento HTML:

~~~ html
<div>
  <b>DIV #1</b><br />position: static;</div>
<div>
  <b>DIV #2</b><br />position: static;</div>
<div>
  <b>DIV #3</b><br />position: static;</div>
<div>
  <b>DIV #4</b><br />position: static;</div>
<div>
  <b>DIV #5</b><br />position: static;</div>
~~~

~~~ css
b {
  font-family: sans-serif;
}

div {
  padding: 10px;
  border: 1px dashed;
  text-align: center;
}
~~~

El resultado es:


```{thumbnail} images/layout_001.png
```

El posicionamiento relativo, `position:relative`, funciona igual que el estático, los bloques se van colocando uno bajo 
el otro en orden de aparición, pero nos da la posibilidad de desplazar el elemento en cualquier dirección, lo cual da 
lugar a solapamientos entre bloques:

~~~ html
<div id="abs1">
  <b>DIV #1A</b><br />position: static;</div>
<div id="rel1" class="relative">
  <b>DIV #2A</b><br />position: relative;</div>
<div id="rel2" class="relative">
  <b>DIV #3A</b><br />position: relative;</div>
<div id="abs2" class="relative">
  <b>DIV #4A</b><br />position: relative;</div>
<div id="sta1" class="static">
  <b>DIV #5A</b><br />position: static;</div>
~~~

~~~ css
b {
  font-family: sans-serif;
}

div {
  padding: 10px;
  border: 1px dashed;
  text-align: center;
}

.static {
  position: static;
  height: 80px;
  background-color: #ffc;
  border-color: #996;
}

.relative {
  position: relative;
  width: 300px;
  height: 80px;
  background-color: #cfc;
  border-color: #696;
  opacity: 0.7;
}

#rel1 {
  top: -10px;
  left: 10px;
}

#rel2 {
  top: 40px;
}

#rel3 {
  top: 10px;
}

~~~

El resultado es 

```{thumbnail} images/layout_002.png
```

Vemos como los bloques con posicionamiento relativo primero se colocan como si fueran estáticos y luego se les aplica
el desplazamiento. En caso de solapamiento entre dos bloques, como norma general, va encima el último bloque en 
posicionarse y los bloques estáticos siempre van debajo.

Los bloques con posicionamiento absoluto, `position: absolute`, se colocan como si fueran estáticos, pero una vez 
posicionados son ignorados por los bloques que se posicionen posteriormente.

~~~ html
<div id="abs1b" class="absolute">
  <b>DIV #1B</b><br />position: absolute;</div>
<div id="rel1b" class="relative">
  <b>DIV #2B</b><br />position: relative;</div>
<div id="rel2b" class="relative">
  <b>DIV #3B</b><br />position: relative;</div>
<div id="abs2b" class="absolute">
  <b>DIV #4B</b><br />position: absolute;</div>
<div id="sta1b" class="static">
  <b>DIV #5B</b><br />position: static;</div>
~~~

~~~ css
b {
  font-family: sans-serif;
}

div {
  padding: 10px;
  border: 1px dashed;
  text-align: center;
}

.separator {
  margin: 5px 0px;
}

.static {
  position: static;
  height: 80px;
  background-color: #ffc;
  border-color: #996;
}

.absolute {
  position: absolute;
  width: 800px;
  background-color: #fdd;
  border-color: #900;
  opacity: 0.7;
}

.relative {
  position: relative;
  width: 300px;
  height: 80px;
  background-color: #cfc;
  border-color: #696;
  opacity: 0.7;
}

#rel1b {
  top: 25px;
  left: 600px;
}

#rel2b {
  top: 10px;
  left: 600px;
}
~~~

El resultado es 

```{thumbnail} images/layout_003.png
```

En resumen, el comportamiento por defecto cuando hay solapamiento entre dos bloques es:
- los bloques estáticos siempre van debajo
- en el resto de casos va encima el último en posicionarse

Este comportamiento se puede modificar con la propiedad `z-index` que nos permite especificar que posición ocupará
un bloque en caso de solapamiento y crear nuevos contextos de apilamiento. Cuando varios elementos se superponen, los 
elementos con mayor valor `z-index` cubren aquellos con menor valor.

~~~ html
<div id="abs1b" class="absolute">
  <b>DIV #1B</b>
  <br />position: absolute;
  <br />z-index: 5;</div>
<div id="rel1b" class="relative">
  <b>DIV #2B</b>
  <br />position: relative;
  <br />z-index: 3;</div>
<div id="rel2b" class="relative">
  <b>DIV #3B</b>
  <br />position: relative;
  <br />z-index: 2;</div>
<div id="abs2b" class="absolute">
  <b>DIV #4B</b>
  <br />position: absolute;
  <br />z-index: 1;</div>
<div id="sta1b" class="static">
  <b>DIV #5B</b>
  <br />position: static;
  <br />z-index: 8;</div>
~~~

~~~ css
b {
  font-family: sans-serif;
}

div {
  padding: 10px;
  border: 1px dashed;
  text-align: center;
}

.static {
  position: static;
  height: 80px;
  background-color: #ffc;
  border-color: #996;
}

.absolute {
  position: absolute;
  width: 800px;
  background-color: #fdd;
  border-color: #900;
  opacity: 0.7;
}

.relative {
  position: relative;
  width: 300px;
  height: 80px;
  background-color: #cfc;
  border-color: #696;
  opacity: 0.7;
}

#rel1b {
  top: 25px;
  left: 600px;
  z-index: 3;
}

#rel2b {
  top: 10px;
  left: 600px;
  z-index: 2;
}

#abs1b {
  z-index: 5;
}

#abs2b {
  z-index: 1
}

#sta1b {
  z-index: 8;
}
~~~

El resultado es

```{thumbnail} images/layout_004.png
```

Como vemos el valor de `z-index` no crea un stacking context en bloques con posicionamiento estático. O sea, el valor
de `z-index` se ignora en dichos bloques. Por eso, en caso de solapamiento esos bloques siempre van debajo, tengan o no 
`z-index`.

Teniendo en cuenta todo lo visto en este apartado podemos entender mejor como se posicionan los bloques cuando se muestra
una página web:

- primero se colocan secuencialmente, uno debajo del otro
- luego se reposicionan teniendo en cuenta la propiedad `position`
    - los bloques con `position: absolute` se ignoran a la hora de posicionar los bloques siguientes
    - se aplican los desplazamientos (si los hay)
- finalmente los bloques que solapan se van considerando por parejas y se decide como solapan
    - si no hay propiedad `z-index` el solapamiento se decide en base a la `position` de los bloques que solapan
    - si hay `z-index` se aplica su valor (si el bloque es estático su `z-index` se ignora)
## Flotar elementos con flexbox

[Flexible box](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox)

The Flexible Box Module, usually referred to as flexbox, was designed as a one-dimensional layout model, and as a method 
that could offer space distribution between items in an interface and powerful alignment capabilities. 

When we describe flexbox as being one dimensional we are describing the fact that flexbox deals with layout in one 
dimension at a time — either as a row or as a column. This can be contrasted with the two-dimensional model of CSS Grid 
Layout, which controls columns and rows together.

~~~ html
<div class="box">
  <div class="one">One</div>
  <div class="two">Two</div>
  <div class="three">Three</div>
</div>
~~~

~~~ css
.box {
  border: dotted 1px;
  padding: 5px;
  display: flex;
  flex-direction: row-reverse;
  flex-wrap: wrap;
}

.box div {
  border: solid 1px;
  width: 40%;
}

/* flex: flex-grow flex-shrink flex-basis */
.one {
  flex: 0 1 auto;
  background-color: #bfa;
}

.two {
  flex: 0 1 auto;
  background-color: #dfc;
}

.three {
  background-color: #efc;
  flex: 1 2 auto;
}
~~~

El resultado es

```{thumbnail} images/flexbox_001.png
```

Los elementos dentro del flexbox ocupan un 40% el ancho disponible. Como `flex-wrap: wrap` los elementos que no caben 
(en este caso el último) se pasan a la siguiente línea. Si ponemos `flex-wrap: nowrap`, entonces los elementos que no 
caben se comprimen:

```{thumbnail} images/flexbox_002.png
```
## Grid layout

[Grid layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout)
