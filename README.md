<img src="assets/brand/banner.svg" alt="Carbura" width="100%">

<br>

# Cómo se escribe una investigación

Una investigación de Carbura es **un archivo Markdown**. No hay panel, ni base
de datos, ni formulario. Se escribe un archivo, se manda, y sale publicado con
sus cifras, sus gráficas y su firma.

Esta página explica el formato entero. Si prefieres copiar antes que leer, mira
cualquiera de los que ya están en `web/content/temas/`.

## Dónde va

```
content/temas/<nombre>.<idioma>.md
```

El nombre es la dirección: `cafe.es.md` se publica en `/temas/cafe`. Usa
minúsculas, sin acentos y con guiones, porque acaba en un enlace que alguien
va a pegar en un mensaje.

El idioma es `es` o `en`. Un mismo `<nombre>` con los dos sufijos es la misma
investigación en dos idiomas, y quien la abra recibirá la que le toque según su
navegador. Si solo existe una, se enseña esa y la página avisa de que está en
el otro idioma.

**Traducir es reescribir.** No hay ninguna obligación de que las dos versiones
tengan las mismas frases, ni el mismo número de párrafos, ni las mismas notas al
pie de una gráfica. Lo que sí tienen que compartir son las cifras.

## La forma de un archivo

Dos partes separadas por `---`: la **cabecera**, que lee la máquina para
dibujar, y el **cuerpo**, que lee una persona.

```markdown
---
titulo: El país del café que compra café
gancho: La cosecha cayó a la décima parte mientras la importación se multiplicó
seccion: Alimentos
icono: cafe
acento: "#2E313D"
autor: heb
publicado: 2026-08-08
portada:
  valor: "-72,4%"
  etiqueta: Producción local en una década
  nota: y sigue bajando
  sentido: baja
---

En 1990 Puerto Rico cosechó 280.000 quintales de café e importó 9.378.
```

### Cabecera: lo obligatorio

| Campo | Qué es |
|---|---|
| `titulo` | El titular. Cabe en dos líneas de una nota, así que corto. |
| `gancho` | Una línea que dé una razón para entrar. No repitas el título. |
| `seccion` | La familia: Alimentos, Energía, Vivienda, Comercio exterior… |
| `acento` | El color, en hexadecimal y **entre comillas**. Sin ellas, YAML se traga el `#` como comentario y el archivo se cae. |
| `autor` | El identificador de quien firma. Tiene que existir en `lib/autores.ts` del sitio. |
| `publicado` | `aaaa-mm-dd`. Ordena el tablero. |
| `portada` | La cifra que resume la investigación entera. Ver abajo. |

Y uno opcional que casi siempre quieres poner:

| Campo | Qué es |
|---|---|
| `icono` | Un nombre de [Ionicons](https://ionic.io/ionicons) relleno: `cafe`, `boat`, `flash`, `home`. Si no lo pones, sale `analytics`. |

### Cifras

`portada` es una cifra. `claves` es una lista de cifras, hasta cuatro, que salen
en rejilla debajo del titular.

```yaml
claves:
  - valor: "280.000"
    etiqueta: Quintales cosechados en 1990
    nota: la referencia de la que se viene
  - valor: "20.000"
    etiqueta: Quintales estimados en 2024
    nota: menos de una décima parte
    sentido: baja
```

`valor` va **entre comillas y ya formateado**. Escríbelo como quieres leerlo:
`"-72,4%"`, `"$8,8 M"`, `"3.184.835"`. Nadie va a formatearlo por ti, y así el
punto decimal es el del idioma en el que estás escribiendo.

`sentido` acepta `sube`, `baja` o `llano`, y solo cambia el color. Úsalo cuando
la dirección sea la noticia, no en todas.

## Gráficas

Van en `series`, y **tú eliges la forma**. Esa elección es parte del argumento,
no un adorno que se decide luego.

```yaml
series:
  - forma: circulo
    titulo: De dónde sale lo que se come
    unidad: porcentaje
    nota: Una frase que diga qué hay que mirar.
    puntos:
      - { x: Importado, y: 87, destacado: true }
      - { x: Local, y: 13 }
```

### Las cuatro formas, y cuándo usar cada una

| `forma` | Para qué sirve | Cuándo se convierte en mentira |
|---|---|---|
| `barras` | Comparar entre dos y seis cosas distintas. Es la que menos supone y la que sale si no pones nada. | Casi nunca. Es la opción segura. |
| `lineas` | Una misma medida a lo largo del tiempo. | Entre categorías. Una línea de «Petróleo» a «Gas» dibuja un camino entre ellos que no existe. |
| `area` | Igual que `lineas`, cuando lo que importa es el tamaño acumulado y no el punto exacto. | Con series que bajan a negativo: el relleno deja de significar nada. |
| `circulo` | Un reparto de un todo. | **Cuando las partes no suman el total.** Un círculo con tres trozos de cosas sueltas miente sobre el conjunto. |

`destacado: true` marca el punto que sostiene el argumento. Los demás salen más
apagados. Si destacas todos, no has destacado ninguno.

`unidad` sale bajo el título, en versalitas. `nota` va debajo del dibujo y es
donde se dice **qué hay que mirar**, que casi nunca es evidente.

## Fuentes

No son opcionales. Una investigación sin fuentes es una opinión con números.

```yaml
fuentes:
  - titulo: La producción local de café ha caído 72,4% en una década
    url: https://sincomillas.com/la-produccion-local-de-cafe-ha-caido-72-4-en-una-decada/
```

Pon el título tal como lo publicó quien lo publicó, no un resumen tuyo.

## El cuerpo

Markdown normal: párrafos, `**negrita**`, listas, `## títulos`, enlaces.

Los enlaces internos a otra investigación se escriben con su dirección:

```markdown
Eso explica por qué [los techos de la isla](/temas/solar-techos) llevan
una década absorbiendo casi toda la capacidad nueva.
```

Dos cosas que el sistema hace por ti y conviene saber:

**El HTML crudo no pasa.** Si escribes `<script>` o `<div>`, sale como texto.
No es una limitación temporal: el modelo es que esto lo publique gente, y el día
que alguien de fuera escriba, una etiqueta suya dentro de un Markdown sería una
sesión robada.

**Las fechas sin comillas se convierten en fechas.** `publicado: 2026-08-08`
está bien. Si alguna vez necesitas una fecha como texto en otro campo, ponla
entre comillas.

## Cómo escribir el texto

Lo que separa una investigación de una ficha:

**Empieza por el hecho, no por el contexto.** «En 1990 Puerto Rico cosechó
280.000 quintales de café e importó 9.378» es una primera frase. «El café ha
sido históricamente importante para la economía de la isla» no lo es.

**Di qué significa la cifra, no la repitas.** Si el gráfico ya dice 87%, el
párrafo no gana nada diciendo 87%. Gana diciendo que casi todo entra por el
mismo muelle.

**Enlaza con las otras investigaciones.** Los fletes explican el precio de la
comida; la factura de la luz explica los techos con placas. Una cifra sola es un
dato, y dos que se tocan son un argumento.

**Termina en la consecuencia.** No en un resumen de lo que acabas de decir.

## Antes de mandarla

- El archivo se llama `<nombre>.<idioma>.md` y el nombre no lleva acentos.
- `acento` está entre comillas.
- Cada `valor` está formateado como se lee.
- Cada `forma` es la que corresponde a esos datos, sobre todo si es `circulo`.
- Hay al menos una fuente y los títulos son los de verdad.
- El cuerpo no repite las cifras que ya salen arriba.

Para verlo antes de mandarlo:

```bash
cd web && npm run dev
```

Y abre `http://localhost:3000`.

---

Developed by Carbura  
At Santurce, PR  
Explained how an investigation is written in Markdown.

## Los ejemplos

En `ejemplos/` hay cinco archivos reales, publicados, uno por cada forma de
gráfica:

| Archivo | Forma | Por qué esa |
|---|---|---|
| `poblacion.es.md` | `barras` | Tres cantidades distintas comparadas. |
| `farmaceutica.es.md` | `circulo` | Cuatro trozos que suman el 100% de las exportaciones. |
| `fletes.es.md` | `area` | Tres fechas de la misma medida, y lo que importa es el salto. |
| `cafe.es.md` | `barras` | Cuatro cantidades de dos años distintos. |
| `cafe.en.md` | — | El mismo, traducido. Compáralos: no son la misma frase dos veces. |

Cópialos y cámbialos. Es más rápido que empezar de cero y evita las tres o
cuatro trampas de YAML que están explicadas arriba.
