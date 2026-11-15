# Lab 4 Teil C: What-If-Simulation und Prognose

| | |
|---|---|
| **Dauer** | 15–25 Minuten (im Rahmen von Lab 4) |
| **Ausgangspunkt** | Ihre Datei `AW_<Projekt>_<Kürzel>.pbix` aus Lab 1 |
| **Ergebnis** | Seite „Simulation“ mit zwei Schiebereglern und einer Umsatzprognose, erneut veröffentlicht |

## Ihr Auftrag

Ihr Auftraggeber fragt: **Was wäre, wenn …?** Bauen Sie eine Simulationsseite mit **zwei** What-If-Parametern, die zu Ihrem Projekt passen, zum Beispiel:

| Projekt | Parameter 1 | Parameter 2 |
|---|---|---|
| A Vertrieb Europa | Preisänderung % | Mengenänderung % |
| B Bikes | Preisänderung % | Kostenänderung % |
| C Händler | Händlerrabatt % | Anzahl neue Händler |
| D Kunden | Kundenzuwachs % | Bestellwert-Änderung % |
| E Promotionen | Rabatt % | Mengeneffekt % |

**Prognose:** Ergänzen Sie auf derselben Seite ein Liniendiagramm `Umsatz` nach Tag (`DimDate[FullDateAlternateKey]`, ohne Datumshierarchie) für 2020 und lassen Sie Power BI die nächsten **90 Tage** vorhersagen.

## Abnahme

- [ ] Zwei numerische Bereichsparameter mit sinnvollen Grenzen und Standardwert 0
- [ ] Measure für den simulierten Umsatz **und** die simulierte Marge
- [ ] Visual „Ist gegen Simulation“ und eine Karte mit der Differenz
- [ ] Ein Satz als Textfeld: Ab welchem Rabatt wird die Marge negativ?
- [ ] Liniendiagramm mit **Prognose** über 90 Tage und Konfidenzband (Analysebereich, Lupensymbol > **Prognose**)

## Test: Funktioniert es?

Referenzwerte für Projekt A (Region Europe, Jahr 2020, Ist-Umsatz 5.464.395, Kosten 3.209.389). Für andere Projekte gelten die Regeln in der rechten Spalte sinngemäß.

| Was Sie einstellen | Erwartetes Ergebnis |
|---|---|
| Beide Schieberegler auf **0** | `Umsatz simuliert` = `Umsatz` = **5.464.395**, Differenz **0**. Ist und Simulation liegen deckungsgleich. |
| Preisänderung **+10 %**, Menge 0 | `Umsatz simuliert` **6.010.835** (Ist × 1,1), Differenz **+546.440**, `Marge simuliert` **2.801.446** (Kosten bleiben gleich) |
| Preisänderung 0, Mengenänderung **+10 %** | `Umsatz simuliert` ebenfalls **6.010.835**, aber `Marge simuliert` nur **2.480.507**, weil die Kosten mitwachsen |
| Preisänderung **−41 %** | `Marge simuliert` noch knapp **positiv** |
| Preisänderung **−42 %** | `Marge simuliert` **negativ**. Die Grenze liegt bei der Ist-Marge von **41,3 %**. Das ist die Antwort für Ihr Textfeld. |
| Jahr wechseln auf 2019 | Simulation rechnet auf dem neuen Ist-Wert (1.969.424) weiter, die Regler behalten ihren Wert |
| Im **Service** einen Regler bewegen | Werte ändern sich im Browser ohne Desktop. Andere Personen sehen Ihre Reglerstellung nicht. |

| Prognose: Liniendiagramm mit Filter `DimDate[CalendarYear]` = 2020, Analysebereich > **Prognose** > Länge **90** Punkte, Konfidenzintervall **95 %** | Hinter dem 31.12.2020 läuft eine Prognoselinie mit grauem Konfidenzband weiter, etwa auf dem Niveau der letzten Monate (Projekt A: rund 18.000 bis 21.000 pro Tag) |
| Jahresfilter entfernen | Die Prognose knickt nach unten ab, weil der Januar 2021 nur Einzelbestellungen enthält. Deshalb das letzte, unvollständige Stück immer ausfiltern. |
| Bericht veröffentlichen und im Service öffnen | Prognose und Schieberegler funktionieren im Browser |

**Fehlerbilder:** Der Eintrag **Prognose** fehlt im Analysebereich: Die X-Achse ist eine Datumshierarchie oder kategorisch. Feld-Menü > statt **Datumshierarchie** das Feld selbst wählen, X-Achse auf **Fortlaufend** stellen. Die Simulation ändert sich nicht: Das Measure verwendet den Spaltenwert statt des erzeugten Measures `<Parameter> Wert`. Werte springen um das Hundertfache: Der Parameter liefert bereits 0,1 statt 10, dann entfällt `/ 100`. Der Regler reicht nicht bis −42 %: Untergrenze des Parameters auf −50 setzen.

<details>
<summary>Hinweise</summary>

- **Modellierung** > **Neuer Parameter** > **Numerischer Bereich**. Power BI legt Tabelle, Measure und Datenschnitt an.
- Arbeiten Sie in Ihrer Datei `AW_<Projekt>_<Kürzel>.pbix` aus Lab 1. Der Parameter legt eine neue Tabelle an, das geht nur in Desktop.
- Den Namen des automatisch erzeugten Measures sehen Sie in der neuen Parametertabelle.

</details>

<details>
<summary>Lösungsweg (Beispiel Projekt A)</summary>

```dax
Umsatz simuliert =
[Umsatz]
    * ( 1 + [Preisänderung Wert] / 100 )
    * ( 1 + [Mengenänderung Wert] / 100 )

Marge simuliert =
[Umsatz simuliert]
    - SUM ( FactInternetSales[TotalProductCost] ) * ( 1 + [Mengenänderung Wert] / 100 )
```

</details>
