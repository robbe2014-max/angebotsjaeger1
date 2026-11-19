# Angebotsjäger – Preisvergleich Lidl · REWE · ALDI SÜD

Web-App (PWA) für Windows, Android und iPhone. Kostenlos, privat, Umkreis 5 km um die Adresse in `config.json`.
Ein Cloud-Job holt 2× täglich die Wochenangebote (marktguru), vergleicht den Preis pro kg/l und schickt bei neuem
Bestpreis für Produkte der **Watchlist** eine Push-Nachricht.

## Einrichtung (einmalig, ca. 15 Min.)
1. Kostenloses GitHub-Konto, neues **privates** Repo, diesen Ordner hochladen.
2. Repo → Settings → Pages → Branch `main`, Ordner `/docs`. (Bei privaten Repos braucht Pages ggf. GitHub Pro; sonst Repo öffentlich stellen – es enthält keine Geheimnisse, nur Angebote.)
3. `python worker/gen_vapid.py` ausführen. Den **PUBLIC**-Key in `docs/config.js` eintragen. Den **PRIVATE**-Key als GitHub-Secret `VAPID_PRIVATE_KEY` speichern, außerdem `VAPID_EMAIL` (deine Mail).
4. Seite auf jedem Gerät öffnen → installieren (iPhone: Teilen → Zum Home-Bildschirm) → „Benachrichtigungen aktivieren“ → angezeigten Text kopieren.
5. Secret `PUSH_SUBSCRIPTIONS` = JSON-Liste aller kopierten Texte, z. B. `[{...Handy...},{...PC...}]`.
6. Actions → „Angebote abgleichen“ → Run workflow. Danach läuft es automatisch.

## Benutzung
- Produkt suchen → Märkte nach Preis pro kg/l sortiert, ★ = Bestpreis.
- Neue Watchlist-Produkte/Preisgrenzen: `config.json` bearbeiten (geht auch im GitHub-Webeditor am Handy).
- Lokal testen: `python worker/check.py --demo --no-push`, dann `python -m http.server -d docs`.

## Grenzen (ehrlich)
- marktguru hat keine offizielle Schnittstelle; ändert sich die Seite, meldet die App „Fehler beim Abruf“ und die Anpassung in `worker/check.py` ist nötig.
- Angebote sind PLZ-basiert; der 5-km-Filter prüft per OpenStreetMap, ob der Markt in der Nähe existiert (nicht die tagesaktuelle Filiale).
- Nur Produkte, die in `watchlist` oder `zusatz_suchbegriffe` stehen, werden geladen und sind durchsuchbar.
- Nutzungsbedingungen der Datenquellen beachten; nur privat und mit geringer Abruffrequenz nutzen.
