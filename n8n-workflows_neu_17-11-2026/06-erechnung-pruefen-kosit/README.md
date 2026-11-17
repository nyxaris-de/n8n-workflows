# 06 · E-Rechnung prüfen (KoSIT)

Prüft eingehende **XRechnung** (XML) und **ZUGFeRD/Factur-X** (PDF mit eingebettetem XML) technisch gegen **EN 16931** – mit dem offiziellen Validator der KoSIT, lokal im eigenen Docker-Netz. Je Rechnung entsteht ein deutscher Prüfbericht, dazu eine Zeile im Monatsprotokoll.

```
Start / täglich 07:00 → Einstellungen → Dateien einlesen → KoSIT prüfen → Prüfbericht + Protokoll
```

## Was er macht

- Liest neue PDF- und XML-Dateien aus dem Eingangsordner; bereits geprüfte (Name + Änderungszeit) werden übersprungen.
- Holt aus PDFs den eingebetteten XML-Datensatz. Ein PDF ohne Datensatz ist **keine E-Rechnung** und wird nur protokolliert.
- Ergebnis je Datei: `gültig`, `abgelehnt` (mit Regel-Codes wie `BR-DE-…`) oder `nicht geprüft`. Ist der Validator nicht erreichbar, wird die Datei beim nächsten Lauf erneut versucht.
- Ablage: `pruefberichte/JJJJ-MM/<Datei>_Pruefbericht.html` und `protokoll/erechnung-JJJJ-MM.csv`.

## Grenzen

- **Technische Prüfung, kein Steuer- oder Rechtsurteil.** „Abgelehnt“ ist ein Hinweis – ob die Rechnung so angenommen wird, entscheiden Buchhaltung oder Kanzlei.
- Dateien werden weder verschoben noch gelöscht.
- Inhaltlich (richtiger Betrag, richtiger Lieferant) prüft der Validator nichts – nur, ob der Datensatz der Norm entspricht.

## Einrichten

1. [KoSIT-Validator](https://github.com/itplr-kosit/validator) mit der [XRechnung-Konfiguration](https://github.com/itplr-kosit/validator-configuration-xrechnung) im Daemon-Modus starten, nur im eigenen Netz erreichbar.
2. Im Knoten **Einstellungen** Ordner und `validatorUrl` anpassen.
3. `NODE_FUNCTION_ALLOW_BUILTIN=fs,path,zlib` setzen und die Ordner in den n8n-Container einbinden.

**Credentials:** keine.
