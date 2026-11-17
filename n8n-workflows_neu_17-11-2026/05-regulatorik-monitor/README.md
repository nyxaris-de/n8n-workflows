# 05 · Regulatorik-Monitor mit eigener KI

Beobachtet **öffentliche** Quellen zur KI-Verordnung und zum Datenschutz (EUR-Lex, EU-Kommission, Bundesnetzagentur, BSI, Datenschutzkonferenz, LfDI MV). Änderungen fasst ein **eigenes Sprachmodell** (Ollama) auf Deutsch zusammen. Danach prüfen feste Regeln das Ergebnis – die KI schlägt vor, die Regeln entscheiden.

```
Täglich 06:15 → Quellen abrufen → Änderungen erkennen → KI nötig? → Ollama → Regeln prüfen → Ablegen
                                                                                └─ „hoch“ → Sofortmeldung
Montags 07:00 → Wochenbericht → Mail
Eval starten  → 6 erfundene Meldungen → dieselbe Kette → Auswertung
```

## Die Regeln

- **Frist wirklich im Text?** Nennt die KI ein Datum, das in der Quelle nicht steht, wird es gestrichen.
- **Pflicht-Schlüsselwörter:** Hochrisiko, Art. 50, Leitlinie, Bußgeld, Verschiebung heben die Meldung mindestens auf „mittel“, mit belegter Frist auf „hoch“.
- **Versteckte Anweisungen:** Text, der das Modell steuern will (Prompt Injection), wird erkannt; die KI-Zusammenfassung wird dann verworfen und die Quelle zum Selbstlesen markiert.
- Regeln **verschärfen nur** – sie stufen nie herunter, was die KI als wichtig erkannt hat. Ist die KI-Antwort unbrauchbar, gilt „mittel“ mit Hinweis, die Quelle selbst zu lesen.
- EUR-Lex ist eingetragen, aber abgeschaltet: Die Seite blockt automatische Abrufe, und Schutzmaßnahmen werden nicht umgangen.

## Evals

Sechs erfundene Mustermeldungen, darunter erfundene Fristen und eine versteckte Anweisung, laufen durch dieselbe Kette. Das Ergebnis zeigt, ob Modell und Regeln zusammen das Erwartete liefern – vor jedem Modellwechsel wiederholen.

## Grenzen

- Nur öffentliche Quellen, keine Kunden- oder personenbezogenen Daten.
- Der erste Abruf je Quelle ist der Ausgangsstand und erzeugt keine Meldung.
- Jede Meldung trägt den Link zur Quelle. **KI-Text vor der Verwendung gegen die Quelle prüfen** – der Monitor ist ein Frühwarnsystem, keine Rechtsauskunft.

## Einrichten

1. Ollama mit einem Modell (getestet mit `mistral`), `OLLAMA_URL` und `MODELL` im Knoten **Quellen** anpassen.
2. `STATE_DIR` setzen und in den Container einbinden; `NODE_FUNCTION_ALLOW_BUILTIN=fs,path`.
3. Absender und Empfänger in den Mail-Knoten eintragen.

**Credentials:** SMTP.
