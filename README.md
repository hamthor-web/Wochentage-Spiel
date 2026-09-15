# Wochentage-Spiel – drei Spielewelten

Gemeinsame Startauswahl für Freizeit & Abenteuer, Sport & Spiel und Märchen & Zauber.

## Auf GitHub veröffentlichen

1. Dieses ZIP auf deinem Rechner entpacken.
2. Den **Inhalt** des Ordners Wochentage-Spiel-GitHub in dein GitHub-Repository hochladen. Die index.html muss direkt im Hauptverzeichnis liegen.
3. In den Repository-Einstellungen unter **Pages** die Veröffentlichung aus einem Branch auswählen (Deploy from a branch).
4. Deinen Hauptbranch (meist main) und **/(root)** wählen und speichern.
5. Nach der Veröffentlichung die dort angezeigte Website-Adresse öffnen.

Offizielle Anleitung: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site

## Dateien

- index.html: gemeinsame Startseite
- spiele/: die drei fertigen Spiele mit eingebetteten Bildern und Sounds
- bilder/: Vorschaubilder für die Startauswahl
- .nojekyll: statische Dateien direkt ausliefern

Es sind keine Installation, kein Build-Schritt und keine zusätzlichen Bibliotheken nötig. Testversionen sind nicht enthalten. Die Spiele werden erst beim Auswählen geladen.

Beim Wechsel der Spielewelt wird nach Bestätigung das laufende Spiel beendet. Spielstände werden nicht dauerhaft gespeichert. Diese Fassung enthält keinen Service Worker und keine garantierte Offline-Installation.

Zum Aktualisieren die entsprechenden Dateien ersetzen. Die Original-Bild- und Tondateien müssen nicht zusätzlich hochgeladen werden.
