<img src="assets/brand/banner.svg" alt="Carbura" width="100%">

<br>

**English** · [Español](README.md)

# How a Carb is written

A **Carb** is a short investigation with figures and sources. It is written in
Markdown: a header holding the data and a body holding the text. The note on the
board, the whole page, the colours and the charts all come out of that file.

This page is the complete specification. Hand it to a language model along with
your data and it has everything it needs to give you back a file that publishes
without touching anything. It is written for that: **every accepted value is
listed**, not described.

## Where it goes

There are two ways to publish, and both read exactly the same format:

1. **Paste it.** At `/panel/nueva`, press **In Markdown**, paste the whole file
   and publish. This is what you want when somebody, or something, handed you a
   file.
2. **Field by field.** Same screen, **By fields**.

You can move between them without losing anything: switching to Markdown writes
the file out of whatever is in the fields, and **Read it into the fields** does
the reverse.

Publishing needs permission, which an admin grants in the accounts tab. Admins
have it always.

## The shape of the file

Two parts separated by `---`.

```markdown
---
titulo: The coffee country that buys its coffee
gancho: The harvest fell to a tenth while imports multiplied by 34
seccion: Food
icono: cafe
acento: "#2E313D"
autor: "heb"
portada:
  valor: "-72.4%"
  etiqueta: Local production in a decade
  nota: and still falling
  sentido: baja
claves:
  - valor: "280,000"
    etiqueta: Quintals harvested in 1990
  - valor: "324,944"
    etiqueta: Quintals imported in 2020
    nota: it was 9,378 in 1990
    sentido: sube
series:
  - forma: circulo
    titulo: Where the food comes from
    unidad: percentage
    nota: A line saying what to look at.
    puntos:
      - { x: Imported, y: 87, destacado: true }
      - { x: Local, y: 13 }
fuentes:
  - titulo: Local coffee production has fallen 72.4% in a decade
    url: https://sincomillas.com/la-produccion-local-de-cafe-ha-caido-72-4-en-una-decada/
---

In 1990 Puerto Rico harvested 280,000 quintals of coffee and imported 9,378. In
2020 it harvested 28,943 and imported 324,944.

It is a clean example of an uncomfortable rule: replacing local production with
imports costs nothing while the outside price is low, and presents the whole
bill the year it stops being low.
```

That publishes as it stands. Copy it and change it.

**The field names are in Spanish and stay in Spanish.** `titulo`, `gancho`,
`acento`, and the rest are the keys the parser reads, in every language. Only
the values you write are yours to translate.

## The header fields

| Field | Required? | What it is |
|---|---|---|
| `titulo` | yes | The headline. It has to fit two lines of a note, so keep it short. |
| `gancho` | yes | One line giving a reason to open it. Do not repeat the title. |
| `seccion` | yes | Free text: Food, Energy, Housing, Foreign trade, Work… |
| `acento` | yes | One of the eight colours below, **in quotes**. |
| `autor` | yes | A reserved alias, in quotes. See below. |
| `icono` | no | A filled Ionicon name. Defaults to `analytics`. |
| `portada` | yes | The one figure that sums the Carb up. Exactly one. |
| `claves` | no | Up to four more figures, in a grid. Defaults to none. |
| `series` | no | The charts. Defaults to none. |
| `fuentes` | no | Where the figures come from. **Always include at least one.** |

`slug` and `idioma` are also accepted in the header and fill those fields when
you paste the file. Without them, you choose both on screen.

### `acento`: these eight only

Any other colour is rejected when the file is read. **Always in quotes**:
without them YAML reads the `#` as the start of a comment and swallows the whole
line.

| Value | Name |
|---|---|
| `"#2992FF"` | Blue |
| `"#FF6F61"` | Salmon |
| `"#445ED9"` | Electric |
| `"#7C5CFF"` | Violet |
| `"#1FA97C"` | Emerald |
| `"#F5A623"` | Amber |
| `"#2E313D"` | Ink |
| `"#16171B"` | Midnight |

### `autor`: a reserved alias

It has to be a name that already exists on the platform, because the byline is
tied to accounts: a Carb cannot be signed by somebody who is not there. Current
bylines: `heb`, `400`, `tornado`.

Unless you are an admin, it also has to be **one of your own aliases**. You
cannot publish under somebody else's name.

### A figure

`portada` is one. `claves` is a list of them.

```yaml
valor: "-72.4%"          # required. Already formatted, in quotes.
etiqueta: Production     # required. What that figure is.
unidad: MW               # optional. Appears after the label.
nota: and still falling  # optional. A small line underneath.
sentido: baja            # optional. sube | baja | llano.
```

`sentido` takes the Spanish words: `sube` for rising, `baja` for falling,
`llano` for neither. It only changes the colour. Use it when the direction is
the news.

`valor` is **text, formatted the way you want it read**: `"-72.4%"`, `"$8.8 M"`,
`"3,184,835"`, `"1,456"`. Nothing formats it for you, and that is what lets the
decimal separator follow the language you are writing in.

## The charts

Each entry in `series` is one chart. **You choose the shape**, and that choice is
part of the argument.

```yaml
series:
  - forma: barras       # optional; barras if absent
    titulo: ...          # sits above
    unidad: ...          # optional, in small caps under the title
    nota: ...            # optional, under the drawing
    puntos:
      - { x: Label, y: 1234, destacado: true }
      - { x: Another, y: 567 }
```

`x` is text, `y` is a number, `destacado` is optional and marks the point
carrying the argument. Highlight all of them and you have highlighted none.

### The four shapes, and where each one lies

| `forma` | Use for | Where it turns false |
|---|---|---|
| `barras` | Comparing two to six distinct things. The safe one. | Almost nowhere. |
| `lineas` | One measure across time. | Across categories. A line from "Oil" to "Gas" draws a path between them that does not exist. |
| `area` | Like `lineas`, when accumulated size matters more than each point. | Series going negative: the fill stops meaning anything. |
| `circulo` | A share of one whole. | **When the parts do not add up to the total.** Three loose quantities in a ring lie about the whole. |

Before writing `circulo`, add the `y` values up. If they do not reach the total,
it is `barras`.

Any other value is rejected when the file is read. It does not draw wrong: it
does not get in.

## Sources

```yaml
fuentes:
  - titulo: However the publisher titled it
    url: https://...
```

Optional in the schema, not in practice. A Carb without sources is an opinion
with numbers. Use the original headline, not your own summary of it.

## The body

Ordinary Markdown: paragraphs, `**bold**`, lists, `## headings`, links. Links to
another Carb go by its address:

```markdown
That explains why [the island's rooftops](/temas/solar-techos) have spent a
decade absorbing nearly all the new capacity.
```

**Raw HTML does not get through.** `<script>`, `<div>`, `<img>`: all of it comes
out as text. This is not a temporary limitation, it is the decision: people
publish these, and somebody else's tag inside a Markdown file would be a stolen
session.

## Two languages

A Carb can exist in Spanish and in English. They are two files with the same
`slug` and different `idioma`, and **translating is rewriting**: the sentences
do not have to correspond, nor the paragraph count, nor a chart's footnote. What
does have to match is the figures.

If only one exists, that one is served and the page says it is in the other
language.

## The errors you will get back

They appear when you press **Read it into the fields**, or when you publish.

| Message | What happened |
|---|---|
| No header | The `---` block is missing. |
| Missing from the header: … | One of the five required fields is absent. If it says `acento` and you can see it there, it is unquoted and YAML ate it. |
| The accent … is not in the palette | A colour that is not one of the eight. |
| That chart shape does not exist | `forma` misspelled or invented. |
| You cannot sign with an alias you have not reserved | `autor` is not yours and you are not an admin. |
| You do not have permission to publish | An admin has to grant it. |

The messages come back in Spanish. That is where they are written today.

## If you are handing this to a model

Give it this whole file and then your data. What makes it work, and therefore
what is worth saying out loud:

- **The eight colours and the four shapes are listed above.** Do not let it
  invent either.
- **`acento` and `autor` in quotes.** This is the most frequent failure.
- **`valor` is pre-formatted text**, not a number.
- **Let the data choose the shape**, and add the `y` values up before accepting
  a circle.
- **Make it use the real sources you gave it.** If it has no source for a
  figure, it should drop the figure, not invent the source.

A warning that holds for anybody, person or model: this format accepts any
number you write into it. It does not check that the number is true. The only
thing holding a Carb up is where its figures came from, and that lives in
`fuentes`.

## Writing the text

What separates a Carb from a fact sheet:

**Open on the fact, not the context.** "In 1990 Puerto Rico harvested 280,000
quintals of coffee and imported 9,378" is a first sentence. "Coffee has
historically been important to the island's economy" is not.

**Say what the figure means; do not repeat it.** If the chart already says 87%,
the paragraph gains nothing by saying 87%. It gains by saying nearly all of it
comes through one dock.

**Link to the other Carbs.** Freight explains the price of food; the electricity
bill explains the rooftops. One figure is a datum; two that touch are an
argument.

**End on the consequence**, not on a summary of what you just said.

## The examples

`ejemplos/` holds real, published Carbs, one per chart shape. Copy them: it is
faster than starting from nothing and it avoids the YAML traps explained above.
`cafe.es.md` and `cafe.en.md` are the same Carb in both languages — compare
them, they are not the same sentences twice.

---

Developed by Carbura  
At Santurce, PR  
Wrote the English edition of the Carb format.
