<img src="assets/brand/banner.svg" alt="Carbura" width="100%">

<br>

# Cómo se escribe un Carb

Un **Carb** es una investigación corta con cifras y fuentes. Se escribe en
Markdown: una cabecera con los datos y un cuerpo con el texto. De ese archivo
salen la nota del tablero, la página entera, los colores y las gráficas.

Esta página es la especificación completa. Si se la pasas a un modelo de
lenguaje junto con tus datos, tiene todo lo que necesita para devolverte un
archivo que se publica sin tocar nada. Está escrita para eso: **cada valor
admitido está enumerado**, no descrito.

## Dónde va

Hay dos formas de publicar, y las dos leen exactamente el mismo formato:

1. **Pegarlo.** En `/panel/nueva`, pulsa **En Markdown**, pega el archivo entero
   y publica. Es lo que quieres si el archivo te lo dio otra persona o un modelo.
2. **Campo a campo.** En la misma pantalla, con **Por campos**.

Puedes ir de una a otra sin perder nada: pasar a Markdown escribe el archivo con
lo que haya en los campos, y **Leerlo y pasar a campos** hace lo contrario.

Para publicar hace falta permiso, que un administrador concede en la pestaña de
cuentas. Los administradores lo tienen siempre.

## La forma del archivo

Dos partes separadas por `---`.

```markdown
---
titulo: El país del café que compra café
gancho: La cosecha cayó a la décima parte mientras la importación se multiplicó
seccion: Alimentos
icono: cafe
acento: "#2E313D"
autor: "heb"
portada:
  valor: "-72,4%"
  etiqueta: Producción local en una década
  nota: y sigue bajando
  sentido: baja
claves:
  - valor: "280.000"
    etiqueta: Quintales cosechados en 1990
  - valor: "324.944"
    etiqueta: Quintales importados en 2020
    nota: eran 9.378 en 1990
    sentido: sube
series:
  - forma: circulo
    titulo: De dónde sale lo que se come
    unidad: porcentaje
    nota: Una frase que diga qué hay que mirar.
    puntos:
      - { x: Importado, y: 87, destacado: true }
      - { x: Local, y: 13 }
fuentes:
  - titulo: La producción local de café ha caído 72,4% en una década
    url: https://sincomillas.com/la-produccion-local-de-cafe-ha-caido-72-4-en-una-decada/
---

En 1990 Puerto Rico cosechó 280.000 quintales de café e importó 9.378. En 2020
cosechó 28.943 e importó 324.944.

Es un ejemplo limpio de una regla incómoda: la sustitución de producción local
por importación no cuesta nada mientras el precio de fuera es bajo, y presenta
la factura entera el año que deja de serlo.
```

Eso de arriba se publica tal cual. Cópialo y cámbialo.

## Los campos de la cabecera

| Campo | ¿Hace falta? | Qué es |
|---|---|---|
| `titulo` | sí | El titular. Cabe en dos líneas de una nota, así que corto. |
| `gancho` | sí | Una línea que dé una razón para entrar. No repitas el título. |
| `seccion` | sí | Texto libre: Alimentos, Energía, Vivienda, Comercio exterior, Trabajo… |
| `acento` | sí | Uno de los ocho colores de abajo, **entre comillas**. |
| `autor` | sí | Un alias reservado, entre comillas. Ver más abajo. |
| `icono` | no | Nombre de un Ionicon relleno. Por defecto `analytics`. |
| `portada` | sí | La cifra que resume el Carb entero. Una sola. |
| `claves` | no | Hasta cuatro cifras más, en rejilla. Por defecto ninguna. |
| `series` | no | Las gráficas. Por defecto ninguna. |
| `fuentes` | no | De dónde salen las cifras. **Pon siempre al menos una.** |

`slug` e `idioma` también se aceptan en la cabecera y rellenan esos campos al
pegar el archivo. Si no están, se eligen en la pantalla.

### `acento`: solo estos ocho

Cualquier otro color se rechaza al leer el archivo. **Entre comillas siempre**:
sin ellas, YAML lee el `#` como el principio de un comentario y se traga la
línea entera.

| Valor | Nombre |
|---|---|
| `"#2992FF"` | Azul |
| `"#FF6F61"` | Salmón |
| `"#445ED9"` | Eléctrico |
| `"#7C5CFF"` | Violeta |
| `"#1FA97C"` | Esmeralda |
| `"#F5A623"` | Ámbar |
| `"#2E313D"` | Tinta |
| `"#16171B"` | Medianoche |

### `autor`: un alias reservado

Tiene que ser un nombre que ya exista en la plataforma, porque la firma está
atada a las cuentas: un Carb no puede estar firmado por alguien que no está.
Firmas actuales: `heb`, `400`, `tornado`.

Si no eres administrador, además tiene que ser **un alias tuyo**. No se puede
publicar con el nombre de otro.

### Una cifra

`portada` es una. `claves` es una lista de ellas.

```yaml
valor: "-72,4%"        # obligatorio. Ya formateado, entre comillas.
etiqueta: Producción    # obligatorio. Qué es esa cifra.
unidad: MW              # opcional. Sale detrás de la etiqueta.
nota: y sigue bajando   # opcional. Una línea pequeña debajo.
sentido: baja           # opcional. sube | baja | llano.
```

`valor` es **texto y va formateado como quieres leerlo**: `"-72,4%"`, `"$8,8 M"`,
`"3.184.835"`, `"1.456"`. Nadie lo formatea por ti, y eso es lo que deja que el
separador decimal sea el del idioma en el que escribes.

`sentido` solo cambia el color. Úsalo cuando la dirección sea la noticia.

## Las gráficas

Cada entrada de `series` es una gráfica. **Tú eliges la forma**, y esa elección
es parte del argumento.

```yaml
series:
  - forma: barras       # opcional; barras si falta
    titulo: ...          # sale encima
    unidad: ...          # opcional, en versalitas bajo el título
    nota: ...            # opcional, debajo del dibujo
    puntos:
      - { x: Etiqueta, y: 1234, destacado: true }
      - { x: Otra, y: 567 }
```

`x` es texto, `y` es número, `destacado` es opcional y marca el punto que
sostiene el argumento. Si destacas todos, no has destacado ninguno.

### Las cuatro formas, y cuándo cada una miente

| `forma` | Para qué | Dónde se vuelve falsa |
|---|---|---|
| `barras` | Comparar entre dos y seis cosas distintas. La opción segura. | Casi en ningún sitio. |
| `lineas` | Una misma medida a lo largo del tiempo. | Entre categorías. Una línea de «Petróleo» a «Gas» dibuja un camino entre ellos que no existe. |
| `area` | Como `lineas`, cuando importa el tamaño acumulado más que cada punto. | Con series que bajan a negativo: el relleno deja de significar nada. |
| `circulo` | Un reparto de un todo. | **Cuando las partes no suman el total.** Tres cantidades sueltas en un anillo mienten sobre el conjunto. |

Antes de poner `circulo`, suma los `y`. Si no llegan al total, es `barras`.

Cualquier otro valor se rechaza al leer el archivo. No se dibuja mal: no entra.

## Las fuentes

```yaml
fuentes:
  - titulo: Cómo lo tituló quien lo publicó
    url: https://...
```

No son opcionales en la práctica. Un Carb sin fuentes es una opinión con
números. Usa el titular original, no un resumen tuyo.

## El cuerpo

Markdown normal: párrafos, `**negrita**`, listas, `## títulos`, enlaces. Los
enlaces a otro Carb van por su dirección:

```markdown
Eso explica por qué [los techos de la isla](/temas/solar-techos) llevan una
década absorbiendo casi toda la capacidad nueva.
```

**El HTML crudo no pasa.** `<script>`, `<div>`, `<img>`: todo eso sale como
texto. No es una limitación temporal, es la decisión: esto lo publica gente, y
una etiqueta ajena dentro de un Markdown sería una sesión robada.

## Dos idiomas

Un Carb puede existir en español y en inglés. Son dos archivos con el mismo
`slug` y distinto idioma, y **traducir es reescribir**: no hace falta que las
frases se correspondan, ni el número de párrafos, ni las notas de una gráfica.
Lo que sí tiene que coincidir son las cifras.

Si solo existe uno, se sirve ese y la página avisa de que está en el otro idioma.

## Los errores que te va a devolver

Salen al pulsar **Leerlo y pasar a campos** o al publicar.

| Mensaje | Qué pasó |
|---|---|
| No hay cabecera | Falta el bloque entre `---`. |
| Falta en la cabecera: … | Falta uno de los cinco obligatorios. Si dice `acento` y tú lo ves puesto, es que va sin comillas y YAML se lo comió. |
| El acento … no está en la paleta | Un color que no es uno de los ocho. |
| Esa forma de gráfica no existe | `forma` mal escrita o inventada. |
| No puedes firmar con un alias que no has reservado | `autor` no es tuyo y no eres administrador. |
| No tienes permiso para publicar | Un administrador tiene que dártelo. |

## Si se lo vas a pasar a un modelo

Dale este archivo entero y luego tus datos. Lo que hace que funcione, y por lo
que conviene decírselo:

- **Los ocho colores y las cuatro formas están enumerados arriba.** No dejes que
  invente ninguno de los dos.
- **`acento` y `autor` entre comillas.** Es el fallo más frecuente.
- **`valor` es texto ya formateado**, no un número.
- **Que elija la forma por los datos**, y que sume los `y` antes de poner un
  círculo.
- **Que ponga las fuentes reales** que le diste. Si no tiene fuente para una
  cifra, que quite la cifra, no que invente la fuente.

Un aviso que vale para cualquiera, persona o modelo: este formato acepta
cualquier número que le escribas. No comprueba que sea cierto. Lo único que
sostiene un Carb es de dónde salieron sus cifras, y eso está en `fuentes`.

## Cómo escribir el texto

Lo que separa un Carb de una ficha:

**Empieza por el hecho, no por el contexto.** «En 1990 Puerto Rico cosechó
280.000 quintales de café e importó 9.378» es una primera frase. «El café ha
sido históricamente importante para la economía de la isla» no lo es.

**Di qué significa la cifra, no la repitas.** Si el gráfico ya dice 87%, el
párrafo no gana nada diciendo 87%. Gana diciendo que casi todo entra por el
mismo muelle.

**Enlaza con los otros Carbs.** Los fletes explican el precio de la comida; la
factura de la luz explica los techos con placas. Una cifra sola es un dato; dos
que se tocan son un argumento.

**Termina en la consecuencia**, no en un resumen de lo que acabas de decir.

---

Developed by Carbura  
At Santurce, PR  
Wrote the complete Carb format, enumerated so a model can follow it.

## Los ejemplos

En `ejemplos/` hay Carbs reales, publicados, uno por cada forma de gráfica.
Cópialos: es más rápido que empezar de cero y evita las trampas de YAML que
están explicadas arriba.
