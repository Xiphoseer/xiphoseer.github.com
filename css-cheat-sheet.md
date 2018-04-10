CSS Cheat-Sheet
===============

```html
<div id="content" class="foo bar">
	<h2>Hello World!</h2>
	<p style="text-align: center;">
		Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, <b>sed</b> diam voluptua.
	</p>
</div>
```

CSS Styles
----------

- Datei (.css)
- `style`-Attribut

Konzept
-------

- Zuordnung von Selektoren (Auswahl) zu Attributen (Eigenschaften)
- Alle Elemente die der Auswahl entsprechen, erhalten die Attribute
- Häufig an Kindelemente weitergegeben (da Standardwert der Eigenschaft `inherit`)

Selektoren
----------

- `div`: HTML Element [vom Typ `div` (Container)]
- `#content`: Element mit Attribute `id="content"` [Note: Jeder id-Wert soll im Dokument nur höchstens einmal vorkommen]
- `.foo`: Element mit Klasse `foo`, bspw. `class="foo bar"`, `class="foo"`
- `*`: Alle Elemente

Verkettung von Selektoren
-------------------------

- `#content h2`: Alle Überschriften 2. Levels (h2-Elemente) die irgendwo innerhalb des Element mit `id="content"` liegen
- `#content > h2`: Alle Überschriften 2. Levels, deren Elternelement die ID `content` hat
- `h2 + p`: Alle Paragraphen (p-Elemente), die direkt auf ein h2-Element folgen
- `h2 ~ p`: Alle Paragraphen (p-Elemente), die nach einem h2-Element auftreten
- `h2, h3`: Überschriften von zweiter oder dritter Ebene (h2 oder h3)

Beliebig kombinierbar: #content > h2 + p b:
	Fettschrift (b) innerhalb eines Paragraphen, der auf eine 2. Überschrift folgt, und direktes Kindelement des Element mit ID `content` ist.

Einschränkungen
---------------

Diese Suffixe werden direkt an einen Selektor angehängt:

- Attribut `abc` existiert: `[abc]`
- Attribut `abc` ist genau `xyz`: `[abc="xyz"]`
- Attribut `abc` enthält Wort `xyz`: `[abc~="xyz"]`
- Attribut `abc` enthält `xyz`: `[abc*="xyz"]`
- Attribut `abc` beginnt mit `xyz`: `[abc^="xyz"]`
- Attribut `abc` endet mit `xyz`: `[abc$="xyz"]`

- Mauszeiger über Element: `:hover`
- Wurde mal angeklickt: `:active`
- Im Fokus: `:focus`


CSS-Syntax
----------

```
<selector>
{
	<key>: <value>;
	<key>: <value>;
	:
	'
}
```

Beispiel:
- Setzt die Hintergrundfabe des Elements mit ID `content` auf Blau und die Schriftfarbe auf weiß;

```css
#content
{
	background-color: #5555ff;
	color: #ffffff;
}
```

Farben
------

- Namen: `red`, `green`, `blue`
- RGB-Hex: `#ffffff`, `#AABBCC` (Groß-, Kleinschreibung egal)
- Kurzform: `#abc` (entspricht `#aabbcc`)
- RGBA: `rgba(255,255,255,0.5)`

Längen
------

Zahl mit Einheit, e.g.: `10rem`

- Wurzel-'m'-Breite: `rem`
- % Anzeigebreite: `vw`
- % Anzeigehöhe: `vh`
- Zentimeter: `cm`
- Millimeter: `mm`

Ausserdem (mit Vorsicht)

- Pixel: `px` (Abhängig von Displayauflösung)
- 'm'-Breite: `em` (Abhängig von der Schriftgröße des Elternelement!)

Wichtige Attribute
------------------

- Hintergrundfarbe: `background-color`: [`transparent`, `inherit`, Farbe]
- Schriftfarbe: `color`: [`inherit`, Farbe]
- Rahmen: `border`: [Breite Stil Farbe]
- Rahmenbreite: `border-width`: [Länge]
- Rahmenfarbe: `border-color`: [Farbe]
- Rahmenstil: `border-style`: [`solid`, `dotted`, `dashed`]
- Abrundung: `border-radius`: [Länge]
- Schriftgröße: `font-size`: [Zahl`pt`]
- Innenabstand: `padding`: [Länge]
- Aussenabstand: `margin`: [Länge, `auto`]
- Ausrichtung: `text-align`: [`left`, `right`, `justify`, `center`]
- Dekoration: `text-decoration`: [`none`, `underline`]
- Anzeige: `display`: [`none`, `block`, `inline`, `flex`, `grid`]

Bei `margin`, `padding`, `border` auch mit `-left`, `-right`, `-top`, `-bottom`, e.g. `margin-left`

Ressourcen
----------

- https://developer.mozilla.org/de/docs/Web/CSS
- https://www.w3schools.com/css/default.asp
