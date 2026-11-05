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
- Punkte zum Raten: Knopf „Punkt setzen“, dann Tippen mit einem Finger setzt einen roten Punkt mit Buchstabe (A, B, C ...). Antippen zeigt oder verbirgt die Zahl, Ziehen verschiebt, „Alles löschen“ entfernt alle. Punkte rasten ein (`rasterWert`), sind exakt gespeichert und bleiben beim Zoomen an ihrer Zahl. Sie werden nicht gespeichert.
- Laserpointer: Knopf an, dann folgt ein schlichter roter Punkt (ohne Schein) frei einem Finger über den ganzen Bildschirm, zum Beispiel für Bögen. Er zieht keine Spur, bleibt nach dem Loslassen stehen und hat eine feste Bildschirmposition, bewegt sich also beim Zoomen nicht mit den Zahlen. Schließt sich mit „Punkt setzen“ gegenseitig aus. Solange er an ist, reagieren Punkte nicht auf Tippen.
- Stift: Knopf an, dann zeichnet ein Finger oder Apple Pencil rote Linien. Linien hängen an ihrer Zahl (`strichStart`: Anker exakt, Weg in Pixeln) und strecken sich beim Zoomen nur waagerecht. Rückgängig nimmt die letzte Linie weg, „Alles löschen“ entfernt Linien und Punkte. Nicht gespeichert. Laser, Stift und Punkt setzen schließen sich gegenseitig aus (`werkzeug`).

## Hosting und Veröffentlichen

- Artefakt auf claude.ai, privat, bis Joshua es dort teilt. Link und Slug stehen in `CLAUDE.local.md` (nicht im Repo, weil das Repo öffentlich ist). Änderungen: `index.html` bearbeiten und neu veröffentlichen.
- GitHub: Repo `klassestehnken-dev/zahlenstrahl` (öffentlich), Branch `main`, Remote `git@github-konto:klassestehnken-dev/zahlenstrahl.git`. Nach Änderungen committen und `git push`.
- Für Commits in diesem Repo ist die GitHub-noreply-Adresse eingestellt, damit keine private E-Mail-Adresse öffentlich wird.
- Lokal testen: Vorschau `zahlenstrahl` aus `.claude/launch.json` (Port 8768).
