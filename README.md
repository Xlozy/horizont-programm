# Horizont

Persönliches Werkzeug zum Wiedererinnern: einzelne Gedanken (Einsichten, Überlegungen,
Denkansätze, Perspektiven) werden eingetragen und kehren beim Öffnen der App wieder.
Kein Überblick über alles, sondern Wiederkehr dessen, was einmal wichtig erschien.

## Verhalten

- Beim Öffnen erscheint ein Gedanke. Keine Angaben, keine Knöpfe ausser +.
- Höchstens **3 neue Gedanken pro gleitender Stunde**; die drei Punkte oben zeigen den Stand.
  Bereits gezeigte bleiben zurückblätterbar.

### Gesten

| Geste | Wirkung | Taste |
|---|---|---|
| ← wischen | nächster Gedanke | → |
| → wischen | zurück | ← |
| ↑ wischen | +1 Punkt (einmal pro Durchlauf) | ↑ |
| ↓ wischen | −1 Punkt (einmal pro Durchlauf) | ↓ |
| 2× tippen | Menü: umformulieren, archivieren, Archiv, Einstellungen | M |
| halten | umformulieren | ↵ |
| + | neuer Gedanke | N |

Pro Durchlauf (eine Anzeige) ist nur ein Schritt möglich: nach +1 bewirkt ein weiteres ↑ nichts,
↓ nimmt das +1 zurück. Archivieren lässt sich über denselben Knopf rückgängig machen, bis zum
nächsten Gedanken gewischt wird.

### Wiederkehr

- Jeder Gedanke startet bei 0 Punkten; der Stand summiert sich über die Durchläufe.
- **Grundzeit** = Anzahl aktiver Gedanken ÷ gesehene Gedanken pro Tag (gemessen über die letzten
  14 Tage, anfangs 6 angenommen), mindestens 6 Stunden. Ein Gedanke mit 0 Punkten kommt so etwa
  einmal pro Umlauf durch den ganzen Bestand.
- **Wartezeit** = Grundzeit × 2^(−Punkte): +1 halbiert sie, −1 verdoppelt sie.
- Nach Ablauf der Wartezeit ist ein Gedanke fällig. Unter den fälligen wählt die App per
  gewichtetem Zufall, je weiter über der Wartezeit, desto wahrscheinlicher; nie gezeigte haben Vorrang.

## Technik

- Eine einzelne HTML-Datei (`index.html`), kein Build-Schritt, kein Framework. Schrift: Work Sans.
- Daten liegen als JSON in einem separaten Repository (`horizont-data`, Datei
  `horizont-daten.json`), eingebunden über die GitHub-Contents-API mit einem Personal Access
  Token (2× tippen → Einstellungen). Fine-grained Token mit «Contents: Read and write» genügt.
- Lokale Kopie in `localStorage`; die App funktioniert offline. Sync beim Öffnen, bei Rückkehr
  und kurz nach jeder Änderung.
- Zusammenführen pro Gedanke: Text, Punkte und Archivstatus nach neuerem `modified`,
  Anzeige-Zeitpunkte als Vereinigung. Gelöschte Gedanken bleiben als Tombstone (`deleted: true`).

## Datenformat

```json
{
  "app": "horizont",
  "version": 2,
  "updated": "ISO-Zeitstempel",
  "gedanken": {
    "g-…": {
      "text": "max. 280 Zeichen",
      "erstellt": "ISO", "modified": "ISO",
      "punkte": 0,
      "archiviert": null,
      "angezeigt": ["ISO", "…"]
    }
  }
}
```

## Verwenden

`index.html` lokal im Browser öffnen (auch per `file://`, z. B. in Safari «Zum Dock
hinzufügen») oder über GitHub Pages ausliefern und auf dem Handy zum Home-Bildschirm hinzufügen.

## Icons

`icon.svg` ist die bearbeitbare Quelle (ein Kreis aus zwei sich gegenüberstehenden Halbkreisen, ∩ über ∪, hell auf dunkel). PNG-Grössen und
`favicon.ico` sind daraus exportiert.
