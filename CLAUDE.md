# Zahlenstrahl

Interaktiver Zahlenstrahl für den Unterricht, **primär fürs iPad**: mit zwei Fingern verschieben, mit zwei Fingern zoomen.
Beim Hineinzoomen erscheinen Dezimalzahlen, beim Herauszoomen immer größere Zahlen, ohne Grenze.

## Technik

- Seite ist `index.html`, dazu `icon.svg` (Browser-Tab) und `apple-touch-icon.png` (Home-Bildschirm, 180 x 180). Canvas, kein Framework, keine externen Schriften (Systemschrift des iPads).
- Rechnet mit BigInt, damit der Strahl auch nach 80 Nachkommastellen exakt bleibt. Bildmitte = `cN * 10^(-P)`, Zoom = `L` (log10 Pixel pro 1).
- Wichtig: `pixelWert()` kann `cN` und `P` neu skalieren. Deshalb nie `cN += pixelWert(...)` schreiben, sondern `mitteVersetzen()` nutzen.
- Zahlen in deutscher Schreibweise: Komma, schmale Leerzeichen, Zahlwörter bis Dezilliarde, danach Zehnerpotenzen.
- Bei sehr langen Zahlen zeigen die Striche nur das Zahlenende, die ganze Zahl steht über der Bildmitte.
- Schalter: Hintergrund Weiß/Schwarz, Minuszahlen (Standard aus, ein Strahl beginnt bei 0), Zurück zu 0 bis 10. Gemerkt wird in localStorage.
- Knöpfe Weniger/Mehr Striche: `dichte` von -2 bis 2, relativ zur automatischen Stufe (Reihe 1, 5, 10, 50 ...), gilt also bei jedem Zoom. Bei 0 bis 10: -2 = 0 und 10, -1 = 0, 5, 10, 0 = alle Einer, +1 = Halbe, +2 = zusätzlich Zehntel-Striche. Wird nicht gespeichert und bleibt bei „Zurück zu 0 bis 10“ erhalten.
- Punkte zum Raten: Knopf „Punkt setzen“, dann Tippen mit einem Finger setzt einen roten Punkt mit Buchstabe (A, B, C ...). Antippen zeigt oder verbirgt die Zahl, Ziehen verschiebt, „Alles löschen“ entfernt alle. Der Punkt selbst passt seine Größe an den Strichabstand an (`punktRadius`), die Nachbarstriche bleiben sichtbar. Punkte rasten ein (`rasterWert`), sind exakt gespeichert und bleiben beim Zoomen an ihrer Zahl. Sie werden nicht gespeichert.
- Laserpointer: Knopf an, dann folgt ein schlichter roter Punkt (ohne Schein) frei einem Finger über den ganzen Bildschirm, zum Beispiel für Bögen. Er zieht keine Spur, bleibt nach dem Loslassen stehen und hat eine feste Bildschirmposition, bewegt sich also beim Zoomen nicht mit den Zahlen. Schließt sich mit „Punkt setzen“ gegenseitig aus. Solange er an ist, reagieren Punkte nicht auf Tippen.
- Stift: Knopf an, dann zeichnet ein Finger oder Apple Pencil rote Linien. Linien hängen an ihrer Zahl (`strichStart`: Anker exakt, Weg in Pixeln) und strecken sich beim Zoomen nur waagerecht. Rückgängig nimmt die letzte Linie weg, „Alles löschen“ entfernt Linien und Punkte. Nicht gespeichert. Laser, Stift und Punkt setzen schließen sich gegenseitig aus (`werkzeug`).
- Stiftfarben Rot, Blau, Grün: erscheinen, solange Stift oder Rechensprung an ist.
- Rechensprung: Tippen auf Start, dann auf Ziel. Es entsteht ein Bogen mit Pfeil und Text wie +3, der nächste Sprung beginnt am Ziel. Eingerastet wie Punkte, gespeichert in `striche` (Rückgängig funktioniert also auch dafür).
- Knopf Zahlen schaltet durch: alle, Anfang und Ende, Anfang Mitte Ende. Die Anzeige „Von Strich zu Strich“ hat einen eigenen Knopf (Einheit ausblenden/einblenden) und hängt nicht vom Zahlen-Knopf ab.
- Knöpfe ausblenden: lässt nur einen kleinen Knopf „Knöpfe“ oben rechts stehen, Werkzeuge bleiben aktiv.
- iPad-Gefühl (Skill mobile-native): Knöpfe werden schon beim Drücken kleiner und dunkler (`button:active`, braucht den leeren `touchstart`-Empfänger auf `document`, sonst zeigt Safari das nicht). `theme-color` folgt dem Hintergrund (Weiß/Schwarz). Beim Ziehen eines Punkts zählt der Abstand zum Griff (`tipp.versatz`), der Punkt springt nicht zum Finger.
- Zoom der Seite ist absichtlich gesperrt (`user-scalable=no`), weil der Strahl das Zoomen selbst macht.

## Hosting und Veröffentlichen

- Hauptadresse: GitHub Pages unter https://edutoolz.github.io/zahlenstrahl/ (Branch `main`, Ordner `/`). Nach `git push` ist die Änderung in ein bis zwei Minuten online.
- Zusätzlich gibt es das Artefakt auf claude.ai, privat, bis Joshua es dort teilt. Link und Slug stehen in `CLAUDE.local.md` (nicht im Repo, weil das Repo öffentlich ist). Es aktualisiert sich nicht von selbst, sondern muss bei Bedarf neu veröffentlicht werden.
- GitHub: Repo `edutoolz/zahlenstrahl` (öffentlich), Branch `main`, Remote `git@github-edutoolz:edutoolz/zahlenstrahl.git`. Nach Änderungen committen und `git push`. Umgezogen von `klassestehnken-dev` am 2026-11-07.
- Für Commits in diesem Repo ist die GitHub-noreply-Adresse eingestellt, damit keine private E-Mail-Adresse öffentlich wird.
- Lokal testen: Vorschau `zahlenstrahl` aus `.claude/launch.json` (Port 8768).
