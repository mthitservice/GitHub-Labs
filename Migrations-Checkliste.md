# Migrations-Checkliste: Report Server > Power BI Service

## Vor der Migration

- [ ] Berichtsbestand erfassen: Welche Berichte, wer nutzt sie, wie oft?
- [ ] Datenquellen je Bericht notieren (Server, Datenbank, Authentifizierung)
- [ ] Gibt es für jede Quelle eine Gateway-Verbindung? Schreibweise identisch?
- [ ] Q&A-Visuals vorhanden? Microsoft stellt Q&A im Dezember 2026 ein. Ersetzen statt migrieren.
- [ ] R- oder Python-Visuals, benutzerdefinierte Visuals: im Tenant freigegeben?
- [ ] Paginierte Berichte (.rdl), Excel-Dateien und KPIs aus dem Portal: eigener Migrationspfad
- [ ] Können mehrere Berichte ein gemeinsames Semantikmodell nutzen?

## Migration

- [ ] PBIX-Kopie im aktuellen Power BI Desktop öffnen und speichern
- [ ] Datenquellen ggf. auf Parameter (`Server`, `Datenbank`) umstellen
- [ ] In den vorgesehenen Arbeitsbereich veröffentlichen
- [ ] Gateway-Verbindung zuordnen
- [ ] Geplante Aktualisierung und Fehlerbenachrichtigung einrichten
- [ ] Große Faktentabellen: inkrementelle Aktualisierung prüfen

## Sicherheit und Verteilung

- [ ] RLS-Rollen vorhanden? Mitglieder (Entra-ID-Gruppen) über die Admins zuordnen lassen
- [ ] Mit „Als Rolle testen“ prüfen
- [ ] Verteilung über App bzw. Org-App mit Zielgruppen
- [ ] Build-Berechtigung für Gruppen, die eigene Berichte auf dem Modell bauen

## Nach der Migration

- [ ] Zahlen zwischen Report Server und Service vergleichen
- [ ] Abos neu einrichten (Abos werden nicht migriert)
- [ ] Nutzer informieren: neuer Link, App, Teams-Tab
- [ ] Beschreibungen im Modell pflegen: Grundlage für Copilot
- [ ] Bericht im Report Server als „abgelöst“ kennzeichnen und nach Frist entfernen
