# Hinweise zu den Änderungen in BIRT 4.3

## Hintergrund

Bei der Umstellung vorhandener Plug-ins, die die Chart-API nutzen, auf die Version 4.3 von BIRT mussten einige Anpassungen durchgeführt werden, damit die Plug-ins wieder korrekt funktionierten.

## Anpassungen

### Hintergrund des Charts

Früher wurde ein Chart immer auf einem weißen Hintergrund gezeichnet, nach der Umstellung war der Hintergrund grau.
Ein erster Versuch bestand darin, den Hintergrund des Diagramms auf weiß zu setzen:

```java
chart.getPlot().setBackground(ColorDefinitionImpl.WHITE());
```

Allerdings war der Hintergrund des Diagrammtitels danach immer noch grau. Deshalb hat es sich als zweckmäßig erwiesen, gleich den ganzen Hintergrund der Zeichenfläche auf weiß zu setzen:
```java
canvas.setBackground(Display.getCurrent().getSystemColor(SWT.COLOR_WHITE));
```

### Setzen der Schriftart

Die Beschriftung der Achsen wurde in der neuen Version mit unleserlichen Hieroglyphen durchgeführt. Abhilfe schuf, an den entsprechenden Elementen die Schriftart explizit zu setzen:

```java
private static final String FONT_NAME = "Microsoft Sans Serif";

....getCaption().getFont().setName(FONT_NAME);
```

### Darstellung der x-Achse

Obwohl die x-Achse ausdrücklich auf LINEAR_LITERAL gesetzt wurde, wurde sie weiterhin nicht korrekt dargestellt. Hier lag das Geheimnis darin, die standardmäßige Einstellung auf CategoryAxis ausdrücklich auszuschalten:

```java
xAchse.setCategoryAxis(false);
```

### Einfärbung von Linien

Für die explizite Festlegung der Linienfarbe muß an der zugehörigen LineSeries die Verwendung der Palette ausgeschaltet werden, ansonsten werden die Farben aus der Palette verwendet.

```java
final LineSeries ls = (LineSeries) LineSeriesImpl.create();
ls.setPaletteLineColor(false);
ls.getLineAttributes().setColor(ColorDefinitionImpl.create(255, 0, 0));
```

Das ist eine Teständerung.
