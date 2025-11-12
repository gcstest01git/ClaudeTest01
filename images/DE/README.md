# Deutsche Bilder / German Images

Dieser Ordner enthält **deutschsprachige** Screenshots, Diagramme und Visualisierungen.

## Verwendung

Verwende Bilder aus diesem Ordner wenn:
- Screenshots deutsche UI-Texte enthalten
- Diagramme deutsche Beschriftungen haben
- Erklärungen in deutscher Sprache enthalten sind

## Namenskonvention

`{komponente}-{zweck}-de.{format}`

Beispiele:
- `order-management-de.png`
- `stripe-checkout-de.png`
- `admin-panel-de.png`
- `stripe-error-handling-de.png`

## Referenzierung in AsciiDoc

**In deutschen Dokumenten:**

```asciidoc
image::DE/order-management-de.png[Bestellverwaltung,700,align=center]
```

## Wichtig

- Jedes Bild in diesem Ordner sollte eine englische Entsprechung in `images/EN/` haben
- Dateinamen sollten zwischen DE und EN übereinstimmen (nur das Suffix `-de` vs `-en` unterscheidet sich)
- Screenshots sollten in hoher Auflösung (mindestens 1920px Breite) gespeichert werden
