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

Verkettung von Selektoren
-------------------------

- #content h2: Alle Überschriften 2. Levels (h2-Elemente) die irgendwo innerhalb des Element mit `id="content"` liegen
- #content > h2: Alle Überschriften 2. Levels, deren Elternelement die ID `content` hat
- h2 + p: Alle Paragraphen (p-Elemente), die direkt auf ein h2-Element folgen
- h2, h3: Überschriften von zweiter oder dritter Ebene (h2 oder h3)

Beliebig kombinierbar: #content > h2 + p b:
	Fettschrift (b) innerhalb eines Paragraphen, der auf eine 2. Überschrift folgt, und direktes Kindelement des Element mit ID `content` ist.

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


