# 04 · Freigabe vor Zahlung

Keine Rechnung gilt als **zahlbar**, bevor die zuständige Person sie freigegeben hat. Der Ablauf schickt je Standort einen Link, der auf eine Bestätigungsseite führt: Betrag, Lieferant, Hinweise – erst dort wird freigegeben oder zurückgewiesen. Jede Entscheidung landet im Protokoll.

```
Täglich 08:00 → Freigaben abgleichen → Mail mit Link an Zuständige
Link (GET)    → Bestätigungsseite
Entscheidung (POST) → Status „zahlbar“ oder „zurückgewiesen“ → Protokoll + Liste zahlbar.csv
```

## Was er macht

- Liest neue Rechnungen (PDF/XML) aus den Standortordnern unter `/data/buchhaltung/`.
- Ordnet jede Rechnung der zuständigen Person zu (`KONFIG.zustaendig`).
- Erinnert, wenn eine Rechnung länger als drei Tage liegt.
- Rechnungen im Ordner `_angehalten` bekommen **keinen** Link – erst klären, dann zurückschieben.
- Schreibt `_freigabe/Freigaben.html` (Übersicht), `_freigabe/zahlbar.csv` und `_protokoll/freigaben-JJJJ-MM.csv`.

## Grenzen

- **Kein Bankzugang, keine Zahlung.** Der Ablauf setzt nur einen Status.
- Der Token im Link ist zufällig (128 Bit), ersetzt aber **keine Anmeldung**. Die Webhooks gehören ins eigene Netz oder hinter ein VPN, nicht offen ins Internet.
- Ein Ordner je Standort, eine zuständige Person je Ordner.

## Einrichten

1. Webhook-Pfad `BITTE-EIGENEN-PFAD-EINSETZEN` in beiden Webhook-Knoten und in `linkBasis` ersetzen.
2. `KONFIG` in **Freigaben abgleichen** und **Seite aufbauen** gleich halten.
3. Die Code-Knoten lesen und schreiben Dateien: `NODE_FUNCTION_ALLOW_BUILTIN=fs,path,crypto` setzen und `/data/buchhaltung` in den Container einbinden.
4. Workflow aktivieren.

**Credentials:** SMTP.
