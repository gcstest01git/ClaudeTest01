## Architekturdokumentation - PR Checkliste

### Überblick
- **Komponente/Feature:** [z.B. Stripe-Integration]
- **Betroffene Kapitel:** [z.B. 05 Building Blocks]
- **Sprachen:** [ ] DE [ ] EN [ ] Beide

### Symmetrie-Checks (MUST HAVE)
- [ ] Neue `.adoc`-Dateien in **BEIDEN** `DE/` und `EN/` angelegt?
- [ ] Ordnerstruktur bleibt **identisch** zwischen DE und EN?
- [ ] **Alle Bildpfade relativ** (`../../images/DE/` oder `../../images/shared/`)?
- [ ] Beide `.adoc`-Dateien haben **gleiche Struktur** (nur Inhalt unterscheidet sich)?
- [ ] `version.properties` aktualisiert in DE/ **UND** EN/?

### Content-Checks
- [ ] Inhalte in Deutsch grammatikalisch korrekt?
- [ ] Inhalte in Englisch grammatikalisch korrekt?
- [ ] Alle Bilder vorhanden?
  - [ ] Deutsche Screenshots in `images/DE/`?
  - [ ] Englische Screenshots in `images/EN/`?
  - [ ] Sprachunabhängige Diagramme in `images/shared/`?

### Format
- [ ] AsciiDoc-Syntax korrekt?
- [ ] Commit-Messages aussagekräftig?

---

**Danke für den PR! 🎉**
