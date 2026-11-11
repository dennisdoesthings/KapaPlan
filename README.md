# Kapaplan

Einfache Web-App zur Kapazitätsplanung für Projektmanager (PM) in einer Kongress-Agentur.

- Eine einzige Datei: `index.html` (HTML, CSS und JavaScript inline)
- Keine Frameworks, keine CDNs, keine externen Requests
- Keine Cookies, kein Tracking, kein Backend – es werden keine Daten übertragen
- Einstellungen und PM-Kürzel werden lokal im Browser gespeichert (localStorage) und bleiben nach dem Neuladen erhalten
- Projekte liegen nur im Arbeitsspeicher; Speichern über **Export als JSON-Datei**, Laden über **Import JSON-Datei**

> Achtung: Beim Neuladen oder Schließen der Seite gehen die Projekte verloren. Vorher exportieren. Die Seite zeigt einen Hinweis, solange Projekte nicht gesichert sind, und der Browser fragt vor dem Verlassen nach.
> Die gespeicherten Einstellungen gelten nur für den jeweiligen Browser. Auf andere Geräte lassen sie sich per JSON-Export und -Import übertragen.

## Bedienung

1. Seite **Einstellungen** (einmalig):
   - **Team**: Kürzel, Kapazität sowie Meetings & Grundlast pro Monat je PM, dazu der Puffer in %.
   - **Größenklassen**: S, M, L, XL mit Basisstunden, dazu ein allgemeiner Zuschlag in % auf alle Projekte (Standard 0).
   - **Erweiterte Einstellungen** (eingeklappt): Aufwandsfaktoren, Grenzen des Gesamtfaktors und Monatsprofil je Komplexität.
   - Jede Änderung wird automatisch im Browser gespeichert.
2. Seite **Planung**:
   - Mit **+ Projekt** ein Projekt anlegen und aufklappen: Name, Eventdatum, Größenklasse, Komplexität und Projektstart wählen.
   - Die Komplexität kann direkt gewählt oder über den Score aus zehn Kriterien (je 0 bis 2 Punkte) bestimmt werden.
   - Zutreffende **Aufwandsfaktoren** ankreuzen. 2 Punkte bei Ausstellung, Logistik, Sicherheit, Schnittstellen, Kundenstruktur oder Änderungsdynamik kreuzen den passenden Faktor automatisch an.
   - Unter **Projektmanager** steht der Hauptansprechpartner als erste Zeile mit seinem Anteil; mit **+ Weiterer PM** zusätzliche PM zuordnen und deren Rolle und Gewicht festlegen.
   - Der Zeitraum der Auslastung wird direkt über der Heatmap eingestellt.
3. Heatmap, Hinweise und Projektübersicht werden bei jeder Änderung sofort neu berechnet. Ein Klick auf eine Heatmap-Zelle zeigt die enthaltenen Projekte.

Zum Ausprobieren: **Beispieldaten laden**.

### Berechnung

- **Gesamtstunden** = Basisstunden der Größenklasse × Gesamtfaktor × (1 + allgemeiner Zuschlag)
- **Gesamtfaktor** = 1 + Summe der gewählten Zu- und Abschläge, begrenzt auf 0,7 bis 1,5
- **Komplexität** (einfach, mittel, komplex) bestimmt nur die Verteilung, nicht die Stunden. Score 0–5 = einfach, 6–10 = mittel, ab 11 = komplex (16–20 gilt als sehr komplex und nutzt das komplexe Profil).
- **Monatsprofil**: Die Gesamtstunden werden je Komplexität auf die Zeiträume vor, im und nach dem Veranstaltungsmonat verteilt; der Anteil eines Zeitraums gleichmäßig auf seine Monate.

  | Zeitraum | einfach | mittel | komplex |
  |---|---|---|---|
  | Mehr als 6 Monate vorher (Monat −12 bis −7) | 2 % | 4 % | 7 % |
  | 6–4 Monate vorher | 5 % | 8 % | 10 % |
  | 3 Monate vorher | 8 % | 10 % | 11 % |
  | 2 Monate vorher | 15 % | 17 % | 18 % |
  | 1 Monat vorher | 28 % | 28 % | 27 % |
  | VA-Monat | 34 % | 27 % | 20 % |
  | Monat danach | 8 % | 6 % | 7 % |

- **Projektstart „Kurzfristig“** (3 bis 6 Monate vor dem Event): Der Gesamtaufwand bleibt gleich. Stunden, die vor dem Projektstart lägen, werden gleichmäßig auf die Monate vom Projektstart bis zum Monat vor dem Event verteilt.
- **Mehrere PM je Projekt**:
  - *Hauptansprechpartner*: trägt den Projektaufwand abzüglich der Themen-Anteile.
  - *Thema / Teilbereich*: übernimmt den eingestellten Prozentanteil des Aufwands in jedem Monat (Standard 20 %).
  - *Nur vor Ort (Aufbau, Durchführung, Abbau)*: zusätzliche Stunden im Eventmonat (Standard 30 h), sie kommen zum Projektaufwand hinzu.
- **Verfügbare Kapazität** = (Kapazität − Meetings & Grundlast) × (1 − Puffer)
- **Auslastung %** = Veranstaltungsstunden / verfügbare Kapazität
- Farben: Grün unter 80 %, Gelb 80–100 %, Rot über 100 %

Alle Stundenwerte und Prozentsätze sind Planungsannahmen und in den Einstellungen änderbar.

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
