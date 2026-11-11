# Kapaplan

Einfache Web-App zur Kapazitätsplanung für Projektmanager (PM) in einer Kongress-Agentur.

- Eine einzige Datei: `index.html` (HTML, CSS und JavaScript inline)
- Keine Frameworks, keine CDNs, keine externen Requests
- Keine Cookies, kein Tracking, kein Backend – es werden keine Daten übertragen
- Einstellungen und PM-Kürzel werden lokal im Browser gespeichert (localStorage) und bleiben nach dem Neuladen erhalten
- Projekte liegen nur im Arbeitsspeicher; Speichern über **Export als JSON-Datei**, Laden über **Import JSON-Datei**

> Achtung: Beim Neuladen oder Schließen der Seite gehen die Projekte verloren. Vorher exportieren.
> Die gespeicherten Einstellungen gelten nur für den jeweiligen Browser. Auf andere Geräte lassen sie sich per JSON-Export und -Import übertragen.

## Bedienung

1. Seite **Einstellungen** (einmalig):
   - **Team**: Kürzel, Kapazität und durchschnittliche Meeting-Stunden pro Monat je PM, dazu der Puffer in %.
   - **Projekttypen**: Bezeichnung und Basisstunden.
   - **Erweiterte Einstellungen** (eingeklappt): Faktoren der Kundenklassen, Besonderheiten, Phasenverteilung.
   - Jede Änderung wird automatisch im Browser gespeichert.
2. Seite **Planung**:
   - Mit **+ Projekt** ein Projekt anlegen und aufklappen: Name, Eventdatum, Projekttyp, Kunde (A/B/C) und Projektstart wählen.
   - Unter **Projektmanager** steht der Hauptansprechpartner als erste Zeile mit seinem Anteil; mit **+ Weiterer PM** zusätzliche PM zuordnen und deren Rolle und Gewicht festlegen.
   - Der Zeitraum der Auslastung wird direkt über der Heatmap eingestellt.
3. Heatmap, Hinweise und Projektübersicht werden bei jeder Änderung sofort neu berechnet. Ein Klick auf eine Heatmap-Zelle zeigt die enthaltenen Projekte.

Zum Ausprobieren: **Beispieldaten laden**.

### Berechnung

- Projektaufwand = Basisstunden × Faktor der Kundenklasse × (1 + Summe der Zuschläge der gewählten Besonderheiten)
- Der Aufwand wird gemäß Phasenverteilung auf die Monate verteilt, jeder Phasenanteil gleichmäßig auf die Monate der Phase. Phasen können mit halben Monaten beginnen oder enden; angeschnittene Monate werden anteilig belastet.
- **Projektstart „Kurzfristig“** (3 bis 6 Monate vor dem Event): Der Gesamtaufwand bleibt gleich. Stunden, die vor dem Projektstart lägen, werden gleichmäßig auf die Monate vom Projektstart bis zum Monat vor dem Event verteilt.
- **Mehrere PM je Projekt**:
  - *Hauptansprechpartner*: trägt den Projektaufwand abzüglich der Themen-Anteile.
  - *Thema / Teilbereich*: übernimmt den eingestellten Prozentanteil des Aufwands in jedem Monat (Standard 20 %, bei mehreren Themen entsprechend erhöhen).
  - *Nur vor Ort (Aufbau, Durchführung, Abbau)*: zusätzliche Stunden im Eventmonat (Standard 30 h), sie kommen zum Projektaufwand hinzu.
- Regelmäßige Meetings (h/Monat je PM) werden in jedem Monat fest addiert.
- Verplanbare Kapazität = Kapazität × (1 − Puffer)
- Auslastung % = Stunden / verplanbare Kapazität
- Farben: Grün unter 80 %, Gelb 80–100 %, Rot über 100 %

## Veröffentlichen mit GitHub Pages

### 1. Repository anlegen

1. Auf [github.com](https://github.com) anmelden.
2. Oben rechts auf **+** → **New repository** klicken.
3. Einen Namen vergeben, z. B. `kapaplan`.
4. Sichtbarkeit **Public** wählen (bei privaten Repositories ist GitHub Pages nur in kostenpflichtigen Tarifen verfügbar).
5. Auf **Create repository** klicken.

### 2. Dateien hochladen

1. Im Repository auf **Add file** → **Upload files** klicken.
2. `index.html` und `README.md` in das Fenster ziehen.
3. Unten auf **Commit changes** klicken.

Die Datei `index.html` muss im Hauptverzeichnis des Repositories liegen.

Alternativ per Git:

```bash
git clone https://github.com/<benutzername>/<repository>.git
cd <repository>
# index.html und README.md in diesen Ordner kopieren
git add index.html README.md
git commit -m "Kapaplan hinzufügen"
git push origin main
```

### 3. GitHub Pages aktivieren

1. Im Repository auf **Settings** klicken.
2. In der linken Leiste **Pages** wählen.
3. Unter **Build and deployment** bei **Source** die Option **Deploy from a branch** wählen.
4. Bei **Branch** den Branch `main` und den Ordner `/ (root)` wählen.
5. Auf **Save** klicken.

### 4. Seite aufrufen

1. Ein bis zwei Minuten warten.
2. Die Seite **Settings → Pages** neu laden. Oben steht die Adresse der Seite:

   ```
   https://<benutzername>.github.io/<repository>/
   ```

3. Die Adresse im Browser öffnen.

### 5. Aktualisieren

Eine neue Version von `index.html` in den Branch `main` hochladen oder pushen. GitHub Pages veröffentlicht die Änderung automatisch nach kurzer Zeit.

## Lokal nutzen

`index.html` per Doppelklick im Browser öffnen. Ein Webserver ist nicht nötig.

## Datenschutz

Es werden keine Daten übertragen. Einstellungen und PM-Kürzel werden nur lokal im Browser gespeichert, Projekte gar nicht. Export und Import laufen ausschließlich lokal im Browser.
